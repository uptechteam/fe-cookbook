# React hook form

1.  [Setup](#setup)
2.  [Configure form with custom fields](#configure-form-with-custom-fields)
3.  [useFormContext](#useFormContext)
4.  [Best practices](#best-practices)
5.  [Optimization](#optimization)

## Setup

React Hook Form is a popular library for managing form state and validation in React applications. Here's a step-by-step guide to setting it up:

1\. Install React Hook Form:

```bash
pnpm install react-hook-form
```

2\. Import necessary components/functions and create and use a form:

In your desired component file (e.g., src/FormComponent.tsx), import the necessary components/functions from the React Hook Form library:

```ts
import { FC } from "react";
import { useForm } from "react-hook-form";

const FormComponent: FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log(data); // Data contains the form input values
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="name">Name:</label>
      <input
        type="text"
        id="name"
        {...register("name", { required: "Name is required" })}
      />
      {errors.name && <p>{errors.name.message}</p>}
      <button type="submit">Submit</button>
    </form>
  );
};

export default FormComponent;
```

The `useForm` hook is used to initialize the form state and validation rules. The `register` function is used to connect form inputs to the form state. The `handleSubmit` function is used to handle form submissions and trigger validation.

3\. Styling and enhancing your form (optional):

You can style and enhance your form just like any other React form. You can also use additional features provided by React Hook Form, such as custom validation, working with arrays, conditional field rendering, etc. The React Hook Form documentation (https://react-hook-form.com/) is an excellent resource to explore more features and examples.

## Configure form with custom fields

Using `useController` with **MUI** (Material-UI) components is a great way to integrate controlled MUI components with React Hook Form. Here the steps how to use `useController` in app:

1\. Create custom `ControledTextFieldProps` component built using **MUI** components and integrated with React Hook Form's `useController` hook for form control.

```ts
import { Control, Path, useController } from "react-hook-form";
import {
  Box,
  TextField,
  FormHelperText,
  StandardTextFieldProps,
} from "@mui/material";

interface ControlledTextFieldProps<T extends object>
  extends StandardTextFieldProps {
  control: Control<T, object>;
  outsideError?: string;
}

export const ControlledTextField = <T extends object>({
  control,
  outsideError,
  ...textFieldProps
}: ControlledTextFieldProps<T>) => {
  const {
    field,
    fieldState: { error },
  } = useController({
    name: textFieldProps.name, // Assuming 'name' is a required prop from textFieldProps
    control,
  });

  const errorMessage = error?.message || outsideError;

  return (
    <Box>
      <TextField id={textFieldProps.name} {...field} {...textFieldProps} />
      {errorMessage && <FormHelperText error>{errorMessage}</FormHelperText>}
    </Box>
  );
};
```

The `ControlledTextField` component is a custom controlled `TextField` component that integrates with React Hook Form and Material-UI.

It takes `ControlledTextField` as props, which includes the control prop from React Hook Form and other props from `StandardTextFieldProps` in MUI (e.g., name, label, placeholder, etc.).

The `useController` hook is used to connect the controlled `TextField` component to React Hook Form by registering it with the provided name and control.

The `errorMessage` variable is determined based on whether there's an error from React Hook Form (error?.message) or an `outsideError` provided through props.

The `TextField` component from MUI is rendered with props from field and other `textFieldProps` that were passed as props to the `ControlledTextField`.

If there's an error message, it is rendered as `FormHelperText` with the error prop set to true to indicate an error state.

2\. Create your form component where you want to use the `ControlledTextField`.

```ts
import { FC } from "react";
import { useForm } from "react-hook-form";
import { ControlledTextField } from "./ControlledTextField";

interface FormValues {
  firstName: string;
  lastName: string;
  // Add more form fields here if needed
}

const MyFormComponent: FC = () => {
  const { control, handleSubmit } = useForm<FormValues>();

  const onSubmit = (data: FormValues) => {
    console.log(data); // Form data with controlled input values
  };

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

export default MyFormComponent;
```

**Handle form submissions:**

In the `onSubmit` function, you can handle the form submissions. The `handleSubmit` function from React Hook Form will handle form validation and call your `onSubmit` function if the form is valid.

When the form is submitted, the `onSubmit` function will be called, and the data containing the form input values will be logged to the console.

## useFormContext

`useFormContext` is a hook provided by React Hook Form that allows you to access the form context outside of the top-level form component. This is useful when you have deeply nested components in your application that need access to the form methods and state without passing props down through the entire component tree.

Here's how to use `useFormContext`

1\. Set up your Form:

In your top-level form component, use the `useForm` hook from React Hook Form to set up the form. This will create a form context that can be accessed by child components.:

```ts
import { FC } from "react";
import { useForm } from "react-hook-form";

const MyForm: FC = ({ children }) => {
  const { handleSubmit } = useForm();

  const onSubmit = (data) => {
    console.log(data); // Form data submitted by the child component
  };

  return <form onSubmit={handleSubmit(onSubmit)}>{children}</form>;
};

export default MyForm;
```

2\. Use `useFormContext` in Child Component:

In your child component, import `useFormContext` from React Hook Form and use it to access the form context.

```ts
import { FC } from 'react';
import { useFormContext } from 'react-hook-form';

const MyChildComponent: FC = () => {
  const { control } = useFormContext();

  return (
    <>
      <ControlledTextField>
        control={control}
        name="name"
        label="Name"
      />
      <button type="submit">Submit</button>
    </>
  );
};

export default MyChildComponent;
```

3\. Nest the `MyChildComponent` inside the `MyForm` component, so it can access the form context.

```ts
import MyForm from "./MyForm";
import MyChildComponent from "./MyChildComponent";

const App = () => {
  return (
    <MyForm>
      <MyChildComponent />
    </MyForm>
  );
};

export default App;
```

## Best practices

Using React Hook Form effectively involves following best practices to ensure a maintainable, performant, and scalable codebase. Here are some best practices for using React Hook Form:

- **Minimize Re-renders**: React Hook Form is designed to minimize unnecessary re-renders. To achieve this, ensure that you use the `useForm` hook at the top level of your functional component and avoid using it within loops or conditional statements.

- **Form Organization**: Organize your form logic and UI components separately. Keep the form-related logic in one place, and use separate components for form inputs, validation messages, etc.

- **Uncontrolled Components**: React Hook Form relies on uncontrolled components. Avoid using `defaultValue` with input fields as it may lead to unexpected behaviors.

- **Use Registered Fields**: Always use the register function to register your form inputs with React Hook Form. This is how the library keeps track of form state and validation.

- **Validation**: Use the built-in validation provided by React Hook Form. It supports various validation rules like required, minLength, pattern, etc. You can also use custom validation functions.

- **Conditional Validation**: Implement conditional validation based on the form input's value using watch and trigger functions.

- **Performance Optimization**: Use the `useForm` hooks options parameter to enable shallow form state tracking (shouldUnregister: true) for better performance with large forms.

- **Field Arrays**: For dynamic field arrays (e.g., repeating form fields), use the `useFieldArray` hook provided by React Hook Form.

- **Error Handling**: Handle errors gracefully. Show validation errors near the input fields and provide clear error messages.

- **Use Controller (Optional)**: For integrating with third-party components that require a value prop (controlled components), use the `Controller` component from React Hook Form.

- **Form Submission**: Use the `handleSubmit` function from the useForm hook to handle form submissions. Avoid using the native form submit unless necessary.

- **Disable Browser Validation**: To avoid conflicts with the built-in browser validation, remember to set the `noValidate` attribute on your form element.

- **Clear Errors**: Clear errors when necessary, especially if you're using conditional fields or dynamically showing/hiding inputs.

- **Test Your Forms**: Write unit tests to ensure your forms behave as expected, especially concerning validation and form submission.

- **Keep Updated**: Stay updated with the React Hook Form documentation and the library updates to leverage new features and improvements.

## Optimization

1\. **Use `useForm` at Top Level**: Make sure to use the `useForm` hook at the top level of your component to prevent unnecessary re-renders. Avoid using it inside loops or conditionals.

2\. **Lazy Register Fields:** If you have a large form with many fields, consider using the lazy option when registering fields with `register`. This will help improve the initial rendering performance by deferring the registration of fields that are not immediately visible.
Here are some specific tips to optimize forms with React Hook Form:

```javascript
const { register } = useForm({
  // Set lazy to true to defer field registration
  lazy: true,
});

// Register the field later when it becomes visible
register("field_name");
```

3\. **Shallow Form State:** For large forms, enable the `shouldUnregister` option when using `useForm`. This option helps in reducing the number of fields being re-registered when a field is unmounted, which can improve performance.

```javascript
const { register } = useForm({
  // Enable shallow form state tracking
  shouldUnregister: true,
});
```
