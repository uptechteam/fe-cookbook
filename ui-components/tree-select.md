# Tree view multi-select component

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)

## General information

It's a select component that can display hierarchical tree data.

It could look like this component below:
![tree view multi-select](https://github.com/uptechteam/fe-cookbook/assets/13544983/405379b5-67fe-47e7-a9d1-973d51063e0c)

## Setup

In the example below we use [react-dropdown-tree-select](https://dowjones.github.io/react-dropdown-tree-select/#/story/readme) library for configuring this component.
Also, we need to install the `lodash.isequal` package for coparing objects inside this component.
So install thesese packages to your app:

```bash
pnpm install react-dropdown-tree-select lodash.isequal
```

## Usage

Here is a [demo](https://codesandbox.io/p/sandbox/mutable-water-6z2yqc) of this component in CodeSandbox.
We use react-hook-form and MUI here.

```typescript
// Input data sample:

const placesOptions = [
  {
    label: 'Germany',
    value: 'Germany',
    children: [
      {
        label: 'Berlin',
        value: 'Berlin',
        parent: 'Germany',
        checked: false,
      },
      {
        label: 'Dusseldorf',
        value: 'Dusseldorf',
        parent: 'Germany',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'Great Britain',
    value: 'Great Britain',
    children: [
      {
        label: 'London',
        value: 'London',
        parent: 'Great Britain',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'HiddenCountry',
    value: 'HiddenCountry',
    children: [
      {
        label: 'Test',
        value: 'Test',
        parent: 'HiddenCountry',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'Israel',
    value: 'Israel',
    children: [
      {
        label: 'Haifa',
        value: 'Haifa',
        parent: 'Israel',
        checked: false,
      },
      {
        label: 'Kiryat Ono',
        value: 'Kiryat Ono',
        parent: 'Israel',
        checked: false,
      },
      {
        label: 'Petach Tikva',
        value: 'Petach Tikva',
        parent: 'Israel',
        checked: false,
      },
      {
        label: 'Raanana',
        value: 'Raanana',
        parent: 'Israel',
        checked: false,
      },
      {
        label: 'Ramat Gan',
        value: 'Ramat Gan',
        parent: 'Israel',
        checked: false,
      },
      {
        label: 'Tel Aviv',
        value: 'Tel Aviv',
        parent: 'Israel',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'Netherlands',
    value: 'Netherlands',
    children: [
      {
        label: 'Amsterdam',
        value: 'Amsterdam',
        parent: 'Netherlands',
        checked: false,
      },
      {
        label: 'Utrecht',
        value: 'Utrecht',
        parent: 'Netherlands',
        checked: false,
      },
      {
        label: 'UTRECHT',
        value: 'UTRECHT',
        parent: 'Netherlands',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'Poland',
    value: 'Poland',
    children: [
      {
        label: 'Warsaw',
        value: 'Warsaw',
        parent: 'Poland',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'UK',
    value: 'UK',
    children: [
      {
        label: 'London',
        value: 'London',
        parent: 'UK',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'Ukraine',
    value: 'Ukraine',
    children: [
      {
        label: 'Kyiv',
        value: 'Kyiv',
        parent: 'Ukraine',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'US',
    value: 'US',
    children: [
      {
        label: 'DC',
        value: 'DC',
        parent: 'US',
        checked: false,
      },
      {
        label: 'Miami',
        value: 'Miami',
        parent: 'US',
        checked: false,
      },
      {
        label: 'Philadelphia',
        value: 'Philadelphia',
        parent: 'US',
        checked: false,
      },
    ],
    checked: false,
  },
  {
    label: 'USA',
    value: 'USA',
    children: [
      {
        label: 'Miami',
        value: 'Miami',
        parent: 'USA',
        checked: false,
      },
      {
        label: 'WASHINGTON, DC',
        value: 'WASHINGTON, DC',
        parent: 'USA',
        checked: false,
      },
    ],
    checked: false,
  },
];

```

## Improvements

This component based on the react-dropdown-tree-select library which uses an old version of React (v16) under the hood. It could cause issues with the support of this component in the future.
