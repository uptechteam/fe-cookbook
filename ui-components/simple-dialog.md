# Simple dialog system

## What is it?

This code sets up a reusable "danger modal" component that can be used to display a styled modal dialog with buttons for resolving or rejecting an action. The danger function creates and manages the rendering of this modal based on the provided configuration and user interactions.

## Example

```typescript
import styled from "@emotion/styled";
import { Box, Button, Modal, Stack, Typography } from "@mui/material";
import { ThemeProvider as MuiThemeProvider } from "@mui/material/styles";
import { FC } from "react";
import { createRoot } from "react-dom/client";

import { theme } from "~/styles/theme";

// This line selects an HTML element with the id 'modal' and assigns it to the modalRoot constant. This element is where the modal will be rendered.
const modalRoot = document.getElementById("modal");

interface Config {
  title: string;
  description: string;
  actionButtonName: string;
}

interface DangerModalProps extends Config {
  resolve: () => void;
  reject: () => void;
}

const StyledModal = styled(Modal)`
  border-radius: 8px;
  width: 320px;
  height: fit-content;
  margin: auto;
  overflow: hidden;
`;

// example of modal
const DangerModal: FC<DangerModalProps> = ({
  resolve,
  reject,
  title,
  description,
  actionButtonName = "Ok",
}) => (
  <StyledModal open onClose={reject}>
    <Box p="20px" sx={{ background: "#d9d9d9" }}>
      <Typography variant="h2" mb="32px" textAlign="center">
        {title}
      </Typography>
      <Typography variant="h2" mb="32px" textAlign="center">
        {description}
      </Typography>
      <Stack gap="8px">
        <Button onClick={resolve} variant="contained">
          Cancel
        </Button>
        <Button onClick={reject} variant="outlined" autoFocus>
          {actionButtonName}
        </Button>
      </Stack>
    </Box>
  </StyledModal>
);

// This exports a function called danger that takes a config object as its parameter.
// This function creates a new Promise that will resolve or reject based on user interaction with the modal.
// Inside the Promise, it renders the DangerModal component with the provided configuration and attaches it to the modalRoot.
// When the user interacts with the modal, it calls resolve or reject accordingly and unmounts the modal.
export const danger = (config: Config) => {
  return new Promise((resolve, reject) => {
    const root = createRoot(modalRoot);

    root.render(
      // need to add theme provider because render of this modal outside main app
      <MuiThemeProvider theme={theme}>
        <DangerModal
          {...config}
          resolve={() => {
            resolve("success");
            root.unmount();
          }}
          reject={() => {
            reject("failure");
            root.unmount();
          }}
        />
      </MuiThemeProvider>
    );
  });
};
```

## Pros

1. Reusability:

   - The DangerModal component is reusable and can be easily integrated into different parts of the application wherever a confirmation or danger modal is needed.
   - The danger function provides a convenient way to display the modal with customizable configurations.

2. Styling:

   - The use of Emotion for styling (StyledModal) allows for a flexible and easily maintainable styling approach within the component.

3. Separation of Concerns:

   - The separation of concerns is maintained by keeping the modal logic and presentation encapsulated within the DangerModal component.

4. Promise-Based Interaction:

   - The use of promises (new Promise(...)) in the danger function allows for easy handling of asynchronous interactions. The promise is resolved or rejected based on user actions.

5. Dynamic Rendering:

   - The modal is dynamically rendered using React's createRoot and can be mounted on a specific DOM element (modalRoot), providing flexibility in where the modal is rendered within the DOM.

## Cons

1. Global DOM Dependency:

   - The reliance on document.getElementById('modal') to find the modal root assumes that there is an HTML element with the ID 'modal' in the global DOM. This might lead to issues in a more complex or dynamically changing DOM structure.

2. Limited Configuration:

   - The current configuration (Config interface) is minimal, only allowing customization of the modal title. If more advanced configurations or variations of the modal are needed, the system may need enhancements.

3. Unmounting Entire React Tree:

   - The root.unmount() method is used to unmount the entire React tree, which might have unintended consequences if other components are rendered in the same root. This approach assumes that the modal is the only content in the specified root.

4. Blocking Promise Chain:

   - The current design blocks the execution of the code until the user interacts with the modal. In some scenarios, this might be undesirable, especially in UIs where asynchronous operations should continue while awaiting user input.

## Usage

```typescript
// usage of dialog system

interface config = {
 title?: string | ReactNode | JSX,  default: ''
 description?: string | ReactNode | JSX, default: ''
 actionButtonName?: string, default: 'Start' (for info modal) | 'Delete' (for danger modal)
}

// run this code in any place of your project (only client side)
danger(config).then(() => {
  // run success code (can be ignored)
}).catch(() => {
  // run failure code (can be ignored)
})


// EXAMPLE WITH CUSTOM HOOK
export const useCustomHook = () => {
  const handleDangerAction = async () => {
    try {
      await danger(config);

      // run success code
    } catch {
      // run cancel code or ignore it
    }
  };

  return {
    handleDangerAction,
  };
};
```
