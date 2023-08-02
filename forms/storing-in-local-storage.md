# Storing form data in localStorage

1.  [General](#general)
2.  [useLocalStorage hook](#uselocastorage-hook)
    - [How to use with react-hook-form](#how-to-use-with-react-hook-form)
    - [How to use with Formik](#how-to-use-with-formik)

## General

Storing form data in `localStorage` can be useful for scenarios where you want to persist form data even if the user refreshes the page or navigates away and returns later. It allows you to retain the form state between sessions.

## useLocalStorage hook

The `useLocalStorage` hook, which enables you to store and retrieve data in/from localStorage with ease.

```ts
import { useCallback, useState } from "react";

export const useLocalStorage = <T>(key: string, initialValue: T) => {
  // The useState hook manages the state for the stored value
  const [storedValue, setStoredValue] = useState<T>(() => {
    // On the initial render, retrieve the value from localStorage if available, or use the initial value provided
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  });

  // useCallback is used to memoize the function, preventing unnecessary re-renders
  const setValue = useCallback(
    (value: T) => {
      try {
        // Set the storedValue state and update the value in localStorage
        setStoredValue((prev) => {
          const valueToStore = value instanceof Function ? value(prev) : value;
          localStorage.setItem(key, JSON.stringify(valueToStore));
          return valueToStore;
        });
      } catch (error) {
        console.log(error);
      }
    },
    [key]
  );

  const clear = useCallback(() => {
    // Remove the value from localStorage
    localStorage.removeItem(key);
  }, [key]);

  const getValue = useCallback(() => {
    // Get the value from localStorage or use the initial value provided
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  }, [initialValue, key]);

  // Return the storedValue state and the utility functions to manage the stored value
  return { value: storedValue, setValue, clear, getValue };
};
```

### How to use with react-hook-form

```ts
import { FC, useEffect } from "react";
import { useForm } from "react-hook-form";
import { useLocalStorage } from "./useLocalStorage";
import { ControlledTextField } from "./ControlledTextField";

interface FormValues {
  name: string;
  email: string;
}

// define initial form values
const initialValues: FormValues = {
  name: "",
  email: "",
};

const FormComponent: FC = () => {
  const {
    value: storedFields,
    setValue: setStoredFields,
    clear,
  } = useLocalStorage<FormValues>("formData", initialValues); // set localStorage "key" and "initialValues"

  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitSuccessful },
  } = useForm<FormValues>({
    defaultValues: storedFields,
  });

  const formValues = watch();

  const onSubmit = (data: FormValues) => {
    console.log(data); // Data contains the form input values
    setStoredFields(data); // Set form values to local storage after submit
  };

  useEffect(() => {
    // Set form values to local storage in every onChange values (optional)
    setStoredFields(formValues);
  }, [formValues, setStoredFields]);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <ControlledTextField<FormValues>
        control={control}
        name="firstName"
        label="First Name"
        required
      />
      <ControlledTextField<FormValues>
        control={control}
        name="lastName"
        label="Last Name"
        required
      />
      <button type="submit">Submit</button>
    </form>
  );
};

export default FormComponent;
```

### How to use with Formik

```ts
import { FC, useEffect } from "react";
import { Field, useFormik } from "formik";
import { Button } from "@mui/material";
import CustomTextField from "./CustomTextField";

interface MyFormValues {
  name: string;
  email: string;
}

const initialValues: MyFormValues = {
  name: "",
  email: "",
};

const MyForm: FC = () => {
  const {
    value: storedFields,
    setValue: setStoredFields,
    clear,
  } = useLocalStorage<MyFormValues>("formData", initialValues); // set localStorage "key" and "initialValues"

  const formik = useFormik<MyFormValues>({
    initialValues: storedFields, // pass values from useLocalStorage as initialValues
    onSubmit: (values) => {
      // Handle form submission logic here
      console.log(values);
      setStoredFields(values); // Set form values to local storage after submit
    },
  });

  useEffect(() => {
    // Set formik.values to local storage in every onChange values (optional)
    setStoredFields(formik.values);
  }, [setStoredFields, formik.values]);

  return (
    <form onSubmit={formik.handleSubmit}>
      <CustomTextField name="name" label="Name" />
      <CustomTextField name="email" label="Email" />
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </form>
  );
};

export default MyForm;
```
