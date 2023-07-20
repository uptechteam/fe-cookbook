# Leave modal

- [General information](#general-information)
- [Hook](#hook)

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
