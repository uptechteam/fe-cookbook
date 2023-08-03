# Textarea with Emoji picker Component

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)

## General information

An emoji picker component is a user interface element that allows users to select emojis from a list or grid of available options. It's a popular feature in modern applications, messaging platforms, and social media websites, as emojis have become an essential part of online communication, adding emotion and expression to messages.

The emoji picker typically appears as a small window or popup that contains a collection of emojis categorized into various groups like smileys and people, animals and nature, food and drink, etc. Users can scroll through the list or use search functionality to find the desired emoji and click or tap on it to insert it into the text input field, comment section, or wherever they need it.

It could look like this component below:
![emoji picker component](https://github.com/uptechteam/prism-athlete-fe/assets/13544983/5be21177-a78a-4901-b96d-d116dfb9305b)

## Setup

In the example below we use [@emoji-mart/react](https://github.com/missive/emoji-mart) library.
Install this package to your app:

```bash
pnpm install @emoji-mart/data @emoji-mart/react
# or
yarn add @emoji-mart/data @emoji-mart/react
```

## Usage

```typescript
// useModal.ts hook

import { useState, useCallback } from "react";

export const useModal = () => {
  const [showModal, setShowModal] = useState(false);
  const [payload, setPayload] = useState(null);

  const onOpen = useCallback(() => {
    setShowModal(true);
  }, [setShowModal]);

  const onClose = useCallback(() => {
    setPayload(null);
    setShowModal(false);
  }, [setShowModal]);

  return {
    showModal,
    onOpen,
    onClose,
    payload,
    setPayload,
  };
};

// styles.ts
import { styled } from "@mui/material/styles";
import { Box } from "@mui/material";

export const Wrap = styled(Box)(() => ({
  "& .link-wrapper": {
    marginTop: "8px !important",
    "& > a": {
      color: "#326164",
      display: "inline-block",
      textDecoration: "none",

      "& > svg": {
        verticalAlign: "middle",
      },

      "& > p": {
        color: "#326164",
        display: "inline-block",
        lineHeight: "12px",
        marginLeft: "6px",
        verticalAlign: "middle",
      },
    },
  },

  "& .picker-wrapper": {
    position: "relative",
    marginTop: "0 !important",
    zIndex: 6,
    "& > div": {
      position: "absolute",
      top: 0,
      left: 0,
      zIndex: 2,
    },
  },
}));

// Note! The Textarea component you can use from our template here: https://github.com/uptechteam/fe-vitejs-template/tree/develop/src/components/atoms/ControlledFields/Textarea

// TextareaWithEmojis.tsx

import { useCallback } from "react";
import Picker from "@emoji-mart/react";
import emojiPickerData from "@emoji-mart/data";
import { Box, Typography } from "@mui/material";

import { useModal } from "@hooks";
import { Textarea } from "@atoms";
import { ReactComponent as EmojisIcon } from "@assets/svgs/Emojis.svg";
import { Wrap } from "./styles";

const id = "someBlockId"; // It could be updated to another one

export interface IProps<T> {
  name: Path<T>;
  setValue: UseFormSetValue<T>;
  control: Control<T, object>;
}

export const TextareaWithEmojis = <T extends object>({
  name,
  setValue,
  control
}: IProps<T>) => {
  const {
    showModal: isShowingEmojis,
    onOpen: showEmojis,
    onClose: hideEmojis,
  } = useModal();

  const handleEmojiSelect = useCallback((emojiObject) => {
    const textAreaElement = document.getElementById(id);

    if (textAreaElement) {
      const newValue =
        textAreaElement.value.substr(0, textAreaElement.selectionStart) +
        emojiObject.native +
        textAreaElement.value.substr(textAreaElement.selectionEnd);

      setValue(name, newValue);
    }
  }, []);

  const handleClickOutside = useCallback(() => {
    hideEmojis();
  }, []);

  const handleEmojisClick = useCallback(
    (event) => {
      event.preventDefault();
      event.stopPropagation();

      if (isShowingEmojis) {
        hideEmojis();
      } else {
        showEmojis();
      }
    },
    [isShowingEmojis]
  );

  return (
    <Wrap>
      <Textarea name={name} control={control} label="Description" />
      <Box className="link-wrapper">
        <a href="#" onClick={handleEmojisClick}>
          <EmojisIcon />
          <Typography>Emoji</Typography>
        </a>
      </Box>
      <Box
        className="picker-wrapper"
        sx={{ display: isShowingEmojis ? "block" : "none" }}
      >
        <Picker
          theme="light"
          data={emojiPickerData}
          onEmojiSelect={handleEmojiSelect}
          onClickOutside={handleClickOutside}
        />
      </Box>
    </Wrap>
  );
};
```
