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
Install this package to your app:

```bash
pnpm install react-dropdown-tree-select
```

## Usage

```typescript
// Container.tsx

import isEqual from 'lodash.isequal';
import { Component } from 'react';
import DropdownTreeSelect, {
  DropdownTreeSelectProps,
  DropdownTreeSelectState,
} from 'react-dropdown-tree-select';

export default class Container extends Component<
  DropdownTreeSelectProps,
  DropdownTreeSelectState
> {
  constructor(props) {
    super(props);
    this.state = { data: props.data };
  }

  static getDerivedStateFromProps(props, state) {
    if (!isEqual(props.data, state.data)) {
      return {
        data: props.data,
      };
    }

    return null;
  }

  shouldComponentUpdate = (nextProps) => {
    const { data } = this.state;
    return !isEqual(nextProps.data, data);
  };

  render() {
    const { data, ...rest } = this.props;
    const { data: stateData } = this.state;
    return <DropdownTreeSelect data={stateData} {...rest} />;
  }
}

// styles.ts
import { Box } from '@mui/material';
import { styled } from '@mui/material/styles';

import CheckBoxChecked from '~/assets/svgs/CheckBoxChecked.svg';
import CheckBoxEmpty from '~/assets/svgs/CheckBoxEmpty.svg';
import CheckBoxPartial from '~/assets/svgs/CheckBoxPartial.svg';
import TriangleCollapsed from '~/assets/svgs/TriangleCollapsed.svg';
import TriangleExpanded from '~/assets/svgs/TriangleExpanded.svg';

export const Wrap = styled(Box)(() => ({
  width: '100%',
  position: 'relative',
  '& .dropdown': {
    width: '100%',
    zIndex: 5,
  },
  '& .dropdown .dropdown-content': {
    width: '100%',
  },

  '& .dropdown-select': {
    '& .tag': {
      padding: '2px 0 2px 8px',
      height: '25px',
    },
    '& .tag-remove': {
      fontSize: '0.92rem',
    },
  },

  '& .dropdown-select .dropdown-trigger': {
    width: '100%',
    padding: '4px',
    lineHeight: '20px',
    maxHeight: '200px',
    display: 'inline-block',
    overflow: 'auto',
    border: '1px solid #b9b9b9',
    borderRadius: 6,
    '& > span:after': {
      fontSize: '12px',
      color: '#555',
    },
    '&:after': {
      position: 'absolute',
      right: '12px',
      top: '9px',
    },
  },

  '& .dropdown-select .search': {
    borderBottom: 'none',
    height: '26px',
  },

  '& .dropdown-select li.node': {
    position: 'relative',
    padding: 0,
    overflow: 'hidden',

    display: 'flex',
    flexDirection: 'row-reverse',

    '&.focused': {
      backgroundColor: 'transparent',
    },
    '&:hover': {
      backgroundColor: '#f4f4f4',
    },

    '&[aria-level="1"]': { paddingLeft: '8px !important' },
    '&[aria-level="2"]': { paddingLeft: '25px !important' },
    '&[aria-level="3"]': { paddingLeft: '42px !important' },
    '&[aria-level="4"]': { paddingLeft: '59px !important' },

    '& > i.toggle': {
      flexGrow: '2',
      textAlign: 'right',
      cursor: 'pointer',
      position: 'relative',
      whiteSpace: 'pre',
      marginRight: 0,
      outline: 'none',

      '&:after': {
        content: '""',
        cursor: 'pointer',
        fontSize: 0,
        display: 'block',
        width: '8px',
        height: '5px',
        position: 'absolute',
        top: '13px',
        right: '10px',
      },

      '&.collapsed': {
        '&:after': {
          width: '5px',
          height: '8px',
          top: '10px',
          right: '11px',
          backgroundImage: `url(${TriangleCollapsed})`,
        },
      },

      '&.expanded': {
        '&:after': {
          backgroundImage: `url(${TriangleExpanded})`,
        },
      },
    },

    '&.partial': {
      '& > label > input': {
        '&:before': {
          width: '16px',
          height: '16px',
          top: '0',
          left: '1px',
          backgroundImage: `url(${CheckBoxPartial})`,
        },
      },
    },

    '& > label': {
      display: 'block',
      padding: '6px 4px',

      '& > input': {
        '&:before': {
          width: '18px',
          height: '18px',
          border: 0,
          display: 'block',
          backgroundImage: `url(${CheckBoxEmpty})`,
          transition: 'none',
          top: '-1px',
        },

        '&:checked': {
          '&:before': {
            transform: 'none',
            width: '16px',
            height: '16px',
            top: '0',
            left: '1px',
            backgroundImage: `url(${CheckBoxChecked})`,
          },
        },
      },
    },

    '&.leaf': {
      '& > i': {
        display: 'none',
      },
      '& > label': {
        flexGrow: '2',
      },
    },
  },

  '& .searchModeOn': {
    '& i.toggle': {
      display: 'block',
      opacity: 0,
    },
  },

  '& .dropdown-select .checkbox-item': {
    position: 'relative',
    width: '1rem',
    height: '1rem',
    marginRight: '9px',
    cursor: 'pointer',
    appearance: 'none',
    outline: 0,
    verticalAlign: 'middle',
    '&:checked:before': {
      height: '50%',
      transform: 'rotate(-45deg)',
      borderTopStyle: 'none',
      borderRightStyle: 'none',
      borderColor: '#2196f3',
    },
    '&:before': {
      content: '""',
      position: 'absolute',
      left: 0,
      top: 0,
      zIndex: 1,
      width: '100%',
      height: '100%',
      border: '2px solid #aaa',
      transition: 'all 0.3s ease-in-out',
    },
  },
}));

// Icons files

// CheckBoxChecked.svg
<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
    <rect width="16" height="16" rx="2" fill="#378B91"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M12.7872 4.1957L7.04256 13L3.21277 9.08698C3.21277 9.08698 2.73405 8.59788 3.21277 8.10875C3.6915 7.61962 4.17022 8.10872 4.17022 8.10872L7.04256 11.0435L11.8298 3.21744C11.8298 3.21744 12.3085 2.72822 12.7872 3.21741C13.266 3.7066 12.7872 4.1957 12.7872 4.1957Z" fill="white"/>
    <rect x="0.5" y="0.5" width="15" height="15" rx="1.5" stroke="#151515" stroke-opacity="0.3"/>
</svg>

// CheckBoxEmpty.svg
<svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
    <rect width="18" height="18" rx="2" fill="white"/>
    <rect x="0.5" y="0.5" width="17" height="17" rx="1.5" stroke="#151515" stroke-opacity="0.3"/>
</svg>

// CheckBoxPartial.svg
<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
    <rect width="16" height="16" rx="2" fill="#378B91"/>
    <rect x="5" y="5" width="6" height="6" rx="1" fill="white"/>
    <rect x="0.5" y="0.5" width="15" height="15" rx="1.5" stroke="#151515" stroke-opacity="0.3"/>
</svg>

// TriangleCollapsed.svg
<svg width="6" height="9" viewBox="0 0 6 9" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M0.5 8.5L5.5 4.5L0.5 0.5L0.5 8.5Z" fill="black" fill-opacity="0.6"/>
</svg>

// TriangleExpanded.svg
<svg width="8" height="5" viewBox="0 0 8 5" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M0 0L4 5L8 0H0Z" fill="black" fill-opacity="0.6"/>
</svg>

// types.ts

import { BoxProps } from '@mui/material';
import { DropdownTreeSelectProps } from 'react-dropdown-tree-select';
import { Control, Path, UseFormSetValue } from 'react-hook-form';

interface Option {
  label: string;
  value: string;
  checked: boolean;
}

interface ChildOption extends Option {
  parent: string;
}

export interface TreeOption extends Option {
  children: ChildOption[];
}

type ModeType = 'multiSelect' | 'hierarchical' | 'simpleSelect' | 'radioSelect';

export interface IProps<T> extends DropdownTreeSelectProps {
  name: Path<T>;
  control: Control<T, object>;
  setValue: UseFormSetValue<T>;
  options: TreeOption[];
  mode: ModeType;
  onChange: (nodes: TreeOption[]) => void;
  outsideError?: string;
  label?: string;
  wrapProps?: BoxProps;
}

// TreeSelect.tsx

import 'react-dropdown-tree-select/dist/styles.css';

import { FormHelperText } from '@mui/material';
import { Path, PathValue, useController } from 'react-hook-form';

import { InputLabel } from '../InputLabel';
import Container from './Container';
import { Wrap } from './styles';
import { IProps } from './types';

export const TreeSelect = <T extends object>({
  options = [],
  name,
  control,
  setValue,
  mode,
  onChange,
  label,
  outsideError,
  wrapProps = {},
  ...props
}: IProps<T>) => {
  const {
    field,
    fieldState: { error },
  } = useController({
    name,
    control,
  });

  const onHandleChange = (currentNode, selectedNodes) => {
    if (mode === 'multiSelect') {
      setValue(name, selectedNodes);
      if (onChange) {
        onChange(selectedNodes);
      }
    } else {
      const { locationId, value, label, parent } = currentNode;
      const nodeData = {
        id: locationId,
        value,
        label,
        parent,
      };

      const nodeSelected = selectedNodes.some(
        ({ value }) => value === currentNode.value
      );

      setValue(
        name,
        (nodeSelected ? nodeData : undefined) as PathValue<T, Path<T>>
      );
      if (onChange) {
        onChange(
          (nodeSelected ? nodeData : undefined) as PathValue<T, Path<T>>
        );
      }
    }
  };

  const errorMessage = error?.message || outsideError;

  return (
    <Wrap {...wrapProps}>
      {label && (
        <InputLabel htmlFor={name} error={!!errorMessage}>
          {label}
        </InputLabel>
      )}

      <Container
        showPartiallySelected
        keepOpenOnSelect
        keepChildrenOnSearch
        keepTreeOnSearch
        data={options}
        onChange={(currentNode, selectedNodes) => {
          field.onChange();
          onHandleChange(currentNode, selectedNodes);
        }}
        mode={mode}
        className="dropdown-select"
        {...props}
      />

      {errorMessage && <FormHelperText error>{errorMessage}</FormHelperText>}
    </Wrap>
  );
};

```

Usage this component in the react-hook-form:

```typescript
// Form.tsx
import { Button as MuiButton, Stack } from '@mui/material';
import { useForm } from 'react-hook-form';

import { TreeSelect } from '~/components/atoms';
import { TreeOption } from '~/components/atoms/ControlledFields/TreeSelect/types';

// Update this interface according to your form and move it outside this file
interface FormData {
  places: TreeOption[];
}

const defaultValues = {
  places: [],
};

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

export const Form = () => {
  const methods = useForm<FormData>({
    defaultValues,
    // resolver: yupResolver(schema), TODO: update resolver here
  });

  const { handleSubmit, control, setValue } = methods;

  const onSubmit = (data) => {
    console.log('data', data);
  };

  const handleLocationChange = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Stack width="400px">
        <Stack direction="row" mb={2}>
          <TreeSelect
            name="places"
            mode="multiSelect"
            options={placesOptions}
            onChange={handleLocationChange}
            setValue={setValue}
            control={control}
          />
        </Stack>
        <Stack direction="row" mb={2}>
          <MuiButton type="submit" variant="contained">
            Submit
          </MuiButton>
        </Stack>
      </Stack>
    </form>
  );
};

```
