# OTP Input Component

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)

## General information

An OTP input, also known as One-Time Password input, is a security mechanism used to enhance the authentication process in various online services and applications. It is a temporary and single-use code that is valid for a short period of time, typically just a few minutes. The primary purpose of OTPs is to add an extra layer of security on top of traditional username and password-based authentication methods.

It could look like this component below:
![otp input example](https://github.com/uptechteam/fe-cookbook/assets/13544983/03959b36-017d-4027-a11c-90a8e8f603d0)

## Setup

In the example below we use [react-otp-input](https://www.npmjs.com/package/react-otp-input) library for configuring this input.
Install this package to your app:

```bash
pnpm install react-otp-input
# or
yarn add react-otp-input
```

## Usage

```typescript
// OTPInput.tsx

import { FormHelperText, styled } from '@mui/material';
import { FC, useState } from 'react';
import OtpInput from 'react-otp-input';

interface IProps {
  error: string;
}

// TODO: styles could be redefined here
const StyledInput = styled('input')(({ theme }) => ({
  height: '57px',
  width: '57px',
  borderRadius: '3.5px',
  backgroundColor: 'rgba(255, 255, 255, 0.16)',
  color: theme.palette.common.white,
  fontSize: '24px',
  fontWeight: 700,
  marginRight: theme.spacing(2),
  border: '2px solid transparent',
  '&:last-child': {
    marginRight: 0,
  },
  '&:focus, &:focus-visible': {
    borderColor: theme.palette.secondary.main,
    outline: 'none',
  },
  '&.error': {
    borderColor: theme.palette.error.main,
    '&:focus, &:focus-visible': {
      borderColor: theme.palette.error.main,
    },
  },
}));

export const OTPInput: FC<IProps> = ({ error }) => {
  const [code, setCode] = useState('');

  return (
    <>
      <OtpInput
        value={code}
        onChange={setCode}
        numInputs={5}
        inputType="number"
        renderInput={(props) => (
          <StyledInput
            {...props}
            className={error ? 'error' : ''}
            style={{ width: '57px', textAlign: 'center' }} // TODO: styles could be changed here
          />
        )}
      />
      {error && <FormHelperText error>Code is not valid</FormHelperText>}
    </>
  );
};
```
