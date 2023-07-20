# WYSIWYG

* [General information](#general-information)
* [Configuration](#configuration)

## General information

To build a WYSIWYG (What You See Is What You Get) editor component in React, you can make use of existing libraries such as tiptap, Draft.js or React-Quill. These libraries provide a set of pre-built components and utilities for building rich text editors.

In the example below we will use tiptap editor. *It could be usefl for cases if you want to get output content as HTML*.
It could look like this component below:
![tiptap editor](https://github.com/uptechteam/prism-athlete-fe/assets/13544983/194c5591-a4b3-4d1e-9a21-29a460bc21bb)

Tiptap is a modern and extensible WYSIWYG (What You See Is What You Get) editor framework for web applications. It provides a set of tools and components to build rich text editors in various frameworks, including React.

Tiptap is built on top of ProseMirror, a powerful and flexible framework for building text editors. It abstracts away the complexity of ProseMirror and provides a simpler and more intuitive API for working with rich text editing.

The official documentation you can find [here](https://tiptap.dev/installation/react).

Tiptap offers features such as:

1. Formatting: It supports various text formatting options like bold, italic, underline, strikethrough, and more.
2. Lists: It provides support for creating ordered and unordered lists.
3. Tables: It allows creating and editing tables within the editor.
4. Links and Images: Tiptap enables inserting and editing hyperlinks and images.
5. Collaborative Editing: It includes collaborative editing capabilities, allowing multiple users to work on the same document simultaneously.
6. Extensions: Tiptap offers a plugin system for adding custom functionality and extending the editor's capabilities.
7. Customization: It provides a theming system and various configuration options for customizing the editor's appearance and behavior.

 ---

## Configuration

To use Tiptap in a React application, you can follow these steps:

```plaintext
pnpm add tiptap react-tiptap @tiptap/starter-kit
```

Create a new component for your editor:

```typescript

// RichTextEditor.tsx

import { useEffect, FC } from "react";
import { EditorContent, useEditor } from "@tiptap/react";
import StarterKit from "@tiptap/starter-kit";
import Link from "@tiptap/extension-link";
import TextAlign from "@tiptap/extension-text-align";
import CharacterCount from "@tiptap/extension-character-count";
import { InputAdornment } from "@mui/material";

import MenuBar from "./MenuBar";
import { Editor, Wrap } from "./styles";

interface IProps {
  onChange: (value: string) => void;
  value: string;
  charactersLimit?: number;
  error?: string;
  initialValue?: string;
  withEmptyState?: boolean;
}

const emptyContent = "<p></p>";

const RichTextEditor: FC<IProps> = ({
  initialValue = "",
  onChange,
  value,
  withEmptyState = false,
  error = "",
  charactersLimit = 0,
}) => {
  const editor = useEditor({
    extensions: [
      StarterKit,
      Link.configure({
        autolink: false,
      }),
      TextAlign.configure({
        types: ["heading", "paragraph"],
      }),
      ...(charactersLimit
        ? [
            CharacterCount.configure({
              limit: charactersLimit,
            }),
          ]
        : []),
    ],
    content: value,
    onUpdate: ({ editor: editorEl }) => {
      const currentValue = editorEl.getHTML();
      onChange(
        withEmptyState
          ? {
              value: currentValue,
              isEmpty:
                currentValue !== emptyContent
                  ? !editorEl.getText().trim()
                  : true,
            }
          : currentValue
      );
    },
    onCreate: ({ editor: editorEl }) => {
      const currentValue = editorEl.getHTML();

      if (withEmptyState) {
        onChange({
          value: currentValue !== emptyContent ? currentValue : "",
          isEmpty:
            currentValue !== emptyContent ? !editorEl.getText().trim() : true,
        });
      }
      if (initialValue) {
        onChange(
          withEmptyState
            ? {
                value: initialValue !== emptyContent ? initialValue : "",
                isEmpty:
                  currentValue !== emptyContent
                    ? !editorEl.getText().trim()
                    : true,
              }
            : initialValue !== emptyContent
            ? initialValue
            : ""
        );
      }
    },
  });

  useEffect(() => {
    if (!editor) return;
    let { from, to } = editor.state.selection;
    editor.commands.setContent(value, false, {
      preserveWhitespace: "full",
    });
    editor.commands.setTextSelection({ from, to });
  }, [editor, value]);

  return (
    <Wrap>
      <Editor error={!!error}>
        {editor && <MenuBar editor={editor} />}
        <EditorContent className="editor__content" editor={editor} />
      </Editor>
      {charactersLimit ? (
        <div className="input-adornment-wrapper">
          <InputAdornment position="end">
            {editor?.storage.characterCount.characters() || 0}/{charactersLimit}
          </InputAdornment>
        </div>
      ) : null}
    </Wrap>
  );
};

export default RichTextEditor;

```

Here is MenuBar component:

```typescript

// MenuBar.tsx

import { Fragment, useCallback, FC } from "react";
import FormatBoldIcon from "@mui/icons-material/FormatBold";
import StrikethroughSIcon from "@mui/icons-material/StrikethroughS";
import FormatItalicIcon from "@mui/icons-material/FormatItalic";
import CodeIcon from "@mui/icons-material/Code";
import FormatListBulletedIcon from "@mui/icons-material/FormatListBulleted";
import FormatListNumberedIcon from "@mui/icons-material/FormatListNumbered";
import FormatAlignLeftIcon from "@mui/icons-material/FormatAlignLeft";
import FormatAlignCenterIcon from "@mui/icons-material/FormatAlignCenter";
import FormatAlignRightIcon from "@mui/icons-material/FormatAlignRight";
import InsertLinkIcon from "@mui/icons-material/InsertLink";
import FormatAlignJustifyIcon from "@mui/icons-material/FormatAlignJustify";
import MenuItem from "./MenuItem";

const boldIcon = <FormatBoldIcon fontSize="small" />;
const italicIcon = <FormatItalicIcon fontSize="small" />;
const strikeThroughSIcon = <StrikethroughSIcon fontSize="small" />;
const codeIcon = <CodeIcon fontSize="small" />;
const insertLinkIcon = <InsertLinkIcon fontSize="small" />;
const bulletedListIcon = <FormatListBulletedIcon fontSize="small" />;
const numberedListIcon = <FormatListNumberedIcon fontSize="small" />;
const leftAlignIcon = <FormatAlignLeftIcon fontSize="small" />;
const centerAlignIcon = <FormatAlignCenterIcon fontSize="small" />;
const rightAlignIcon = <FormatAlignRightIcon fontSize="small" />;
const justifyAlignIcon = <FormatAlignJustifyIcon fontSize="small" />;

interface IProps {
  // Should be updated according to the tiptap package types
}

const MenuBar: FC<IProps> = ({ editor }) => {
  const setLink = useCallback(() => {
    const previousUrl = editor.getAttributes("link").href;
    const url = window.prompt("URL", previousUrl);

    // cancelled
    if (url === null) {
      return;
    }

    // empty
    if (url === "") {
      editor.chain().focus().extendMarkRange("link").unsetLink().run();

      return;
    }

    // update link
    editor.chain().focus().extendMarkRange("link").setLink({ href: url }).run();
  }, [editor]);

  if (!editor) {
    return null;
  }

  const items = [
    {
      icon: boldIcon,
      title: "Bold",
      action: () => editor.chain().focus().toggleBold().run(),
      isActive: () => editor.isActive("bold"),
    },
    {
      icon: italicIcon,
      title: "Italic",
      action: () => editor.chain().focus().toggleItalic().run(),
      isActive: () => editor.isActive("italic"),
    },
    {
      icon: strikeThroughSIcon,
      title: "Strike",
      action: () => editor.chain().focus().toggleStrike().run(),
      isActive: () => editor.isActive("strike"),
    },
    {
      icon: codeIcon,
      title: "Code",
      action: () => editor.chain().focus().toggleCode().run(),
      isActive: () => editor.isActive("code"),
    },
    {
      icon: insertLinkIcon,
      title: "Link",
      action: () => setLink(),
      isActive: () => editor.isActive("link"),
    },
    {
      icon: bulletedListIcon,
      title: "Bullet List",
      action: () => editor.chain().focus().toggleBulletList().run(),
      isActive: () => editor.isActive("bulletList"),
    },
    {
      icon: numberedListIcon,
      title: "Ordered List",
      action: () => editor.chain().focus().toggleOrderedList().run(),
      isActive: () => editor.isActive("orderedList"),
    },
    {
      icon: leftAlignIcon,
      title: "Left Align",
      action: () => editor.chain().focus().setTextAlign("left").run(),
      isActive: () => editor.isActive({ textAlign: "left" }),
    },
    {
      icon: centerAlignIcon,
      title: "Center Align",
      action: () => editor.chain().focus().setTextAlign("center").run(),
      isActive: () => editor.isActive({ textAlign: "center" }),
    },
    {
      icon: rightAlignIcon,
      title: "Right Align",
      action: () => editor.chain().focus().setTextAlign("right").run(),
      isActive: () => editor.isActive({ textAlign: "right" }),
    },
    {
      icon: justifyAlignIcon,
      title: "Justify Align",
      action: () => editor.chain().focus().setTextAlign("justify").run(),
      isActive: () => editor.isActive({ textAlign: "justify" }),
    },
  ];

  return (
    <div className="editor__header">
      {items.map((item, index) => (
        <Fragment key={index}>
          <MenuItem {...item} />
        </Fragment>
      ))}
    </div>
  );
};

export default MenuBar;

```

Here is the MenuBar component:

```typescript
// MenuItem.tsx

import { FC } from "react";
import { styled } from "@mui/material/styles";

const MenuItemButton = styled("button")(({ theme }) => ({
  backgroundColor: "transparent",
  border: "none",
  borderRadius: "0.4rem",
  color: theme.palette.black,
  height: "1.75rem",
  marginRight: "0.25rem",
  padding: "0.25rem",
  width: "1.75rem",
  cursor: "pointer",

  svg: {
    fill: "rgba(21, 21, 21, .3)",
    height: "100%",
    width: "100%",
  },

  "&:hover, &.is-active": {
    backgroundColor: theme.palette.black,
    color: theme.palette.common.white,
    svg: {
      fill: theme.palette.common.white,
    },
  },
}));


interface IProps {
  // Should be updated according to the tiptap package types
}

const MenuItem: FC<IProps> = ({ icon, title, action, isActive = null }) => (
  <MenuItemButton
    className={`${isActive && isActive() ? " is-active" : ""}`}
    onClick={action}
    title={title}
    type="button"
  >
    {icon}
  </MenuItemButton>
);

export default MenuItem;

```

Here is an example of how to use this component in the react-hook-form:

```typescript
// RichTextEditorComponent.tsx

import { FC } from "react";
import { Controller, useFormContext } from "react-hook-form";
import { RichTextEditor } from "@atoms"; // it could be another path

interface IProps {
  name: string;
}

const RichTextEditorComponent:FC<IProps> = ({ name }) => {
  const { control } = useFormContext();

  return (
    <Controller
      name={name}
      control={control}
      render={({ field: { onChange, value } }) => (
        <RichTextEditor onChange={onChange} value={value} />
      )}
    />
  );
};

export default RichTextEditorComponent;
```
