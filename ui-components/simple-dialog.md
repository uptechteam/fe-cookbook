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

## Usage

```typescript
// usage of dialog system

interface config = {
 title?: string | ReactNode | JSX,  default: ''
 description?: string | ReactNode | JSX, default: ''
 actionButtonName?: string, default: 'Start' (for info modal) | 'Delete' (for danger modal)
}

danger(config).then(() => {
  // run success code (can be ignored)
}).catch(()=>{
  // run failure code (can be ignored)
})

```
