# Leave modal

- [General information](#general-information)
- [Hook](#hook)
- [Another approach with the useBlocker and useCallbackPrompt hooks](#another-approach-with-the-useblocker-and-usecallbackprompt-hooks)

## General information

The functionality described here involves handling event for a custom prompt modal or using browser-native confirmation dialogs to handle user navigation attempts, such as clicking a link or using the back button, in a React application.

We need this functionality to ensure that users are aware of potential data loss or unintended actions when attempting to leave a page with unsaved changes. By displaying a prompt or confirmation dialog, we give users the opportunity to confirm their intention to leave the current page or cancel the navigation. This helps prevent accidental data loss and improves the overall user experience by providing clear communication about potential consequences of their actions.

## Hook

```typescript
import { useEffect } from "react";
import { useBeforeUnload, useLocation, useNavigate } from "react-router-dom";

interface IProps {
  needShowDialog: boolean;
  onOpenCustomModal?: () => void;
}

export const useModalBeforeLeavePage = ({
  needShowDialog,
  onOpenCustomModal,
}: IProps) => {
  const navigate = useNavigate();
  const location = useLocation();

  useEffect(() => {
    const handleBackButton = (e) => {
      if (needShowDialog) {
        // prevent back and show modal if needed
        e.preventDefault();
        onOpenCustomModal();
        window.history.pushState(null, null, window.location.pathname);
      } else {
        // go back
        navigate(-1);
      }
    };

    window.history.pushState(null, null, window.location.pathname);
    window.addEventListener("popstate", handleBackButton);

    return () => {
      window.removeEventListener("popstate", handleBackButton);
    };
  }, [location.pathname, needShowDialog, navigate]);

  // show browser default modal when we try to refresh page
  useBeforeUnload((e) => {
    if (needShowDialog) {
      e.preventDefault();
      e.returnValue = "";
    }
  });
};

// USAGE
const Container = () => {
  const [open, setOpen] = useState(false);

  useModalBeforeLeavePage({
    needShowDialog: true, // true if data in form
    onOpenCustomModal: setOpen(true),
  });

  return (
    <CustomPrompt
      open={open}
      title="Are you sure you want to leave this page?"
    />
  );
};
```

## Another approach with the useBlocker and useCallbackPrompt hooks

In the example below we define `useBlocker` and `useCallbackPrompt` hooks:

```typescript
// useBlocker.ts hook

import { useEffect, useContext } from "react";
import { UNSAFE_NavigationContext } from "react-router-dom";

// Handle the logic of blocking some action in the router prompt
export const useBlocker = (blocker, when = true) => {
  const navigator = useContext(UNSAFE_NavigationContext).navigator;

  useEffect(() => {
    if (!when) return;

    const unblock = navigator.block((tx) => {
      const autoUnblockingTx = {
        ...tx,
        retry() {
          unblock();
          tx.retry();
        },
      };

      blocker(autoUnblockingTx);
    });

    return unblock;
  }, [navigator, blocker, when]);
};
```

```typescript
// useCallbackPrompt.ts hook

import { useCallback, useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import useBlocker from "./useBlocker";

// Handle the logic of showing the router prompt
export const useCallbackPrompt = (when: boolean): {
  showModal: boolean, openModal: () => void, closeModal: () => void, confirm: () => void
} => {
  const navigate = useNavigate();
  const location = useLocation();

  const [showModal, setShowModal] = useState(false);
  const [lastLocation, setLastLocation] = useState(null);
  const [confirmedNavigation, setConfirmedNavigation] = useState(false);

  // handle blocking when user click on another route prompt will be shown
  const handleBlockedNavigation = useCallback(
    (nextLocation) => {
      if (
        !confirmedNavigation &&
        nextLocation.location.pathname !== location.pathname
      ) {
        setShowModal(true);
        setLastLocation(nextLocation);
        return false;
      }
      return true;
    },
    [confirmedNavigation, location]
  );

  const confirm = useCallback(() => {
    setShowModal(false);
    setConfirmedNavigation(true);
  }, []);

  useEffect(() => {
    if (confirmedNavigation && lastLocation) {
      navigate(lastLocation.location.pathname);
      setConfirmedNavigation(false);
    }
  }, [confirmedNavigation, lastLocation]);

  const openModal = useCallback(() => setShowModal(true), []);
  const closeModal = useCallback(() => setShowModal(false), []);

  useBlocker(handleBlockedNavigation, when);

  return { showModal, openModal, closeModal, confirm };
};
```

Here is an example of how we can use these hooks in some container component with a custom modal component:

```typescript
// USAGE
import { useCallbackPrompt } from "@hooks";
import { LeaveModal } from "@organisms";
import { useCallback, useEffect, ReactNode, FC } from "react";

interface IProps {
  children: ReactNode;
  when: boolean;
  showByTrigger: boolean;
  resetTrigger?: () => void;
  handleConfirm?: () => void;
}

export const LeaveModalContainer: FC<IProps> = ({
  when = false,
  showByTrigger = false,
  resetTrigger,
  handleConfirm,
  children,
}) => {
  const { showModal, closeModal, openModal, confirm } = useCallbackPrompt(when);

  useEffect(() => {
    if (showByTrigger) {
      openModal();
    }
  }, [showByTrigger, resetTrigger]);

  const onHandleClose = useCallback(() => {
    closeModal();
    if (resetTrigger) {
      resetTrigger();
    }
  }, [resetTrigger]);

  const onHandleConfirm = useCallback(() => {
    confirm();
    if (resetTrigger) {
      resetTrigger();
    }
    if (handleConfirm) {
      handleConfirm();
    }
  }, [resetTrigger, handleConfirm]);

  return (
    <>
      {children}
      {/* Here could be any custom modal component */}
      <LeaveModal
        isOpen={showModal}
        onClose={onHandleClose}
        onConfirm={onHandleConfirm}
      />
    </>
  );
};
```
