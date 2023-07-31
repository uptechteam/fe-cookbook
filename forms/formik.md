# Formik

1.  [Setup](#setup)
2.  [Dynamic arrays](#dynamic-arrays)
3.  [Custom form component](#custom-form-component)
4.  [useFormikContext](#useFormikContext)
5.  [Best practices](#best-practices)
6.  [Optimization](#optimization)

Formik is a popular open-source library for managing form state in React applications. It simplifies and streamlines the process of building and handling forms by providing a set of utilities and components.

[Here you can find Formik Documentation](https://formik.org/)

## Setup

1\. Install Formik

```bash
pnpm install formik
```

2\. Import required modules and define the Formik form component using the useFormik hook:

```ts
import { FC } from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import { TextField, Button } from "@mui/material";

const MyForm: FC = () => {
  const initialValues = {
    name: "",
    email: "",
    // Add more form fields if needed
  };

  const onSubmit = (values, { setSubmitting }) => {
    // Handle form submission logic here
    console.log(values);
    setSubmitting(false);
  };

  const formik = useFormik({
    initialValues,
    onSubmit,
    validate: (values) => {
      const errors = {};

      if (!values.name) {
        errors.name = "Required";
      }

      if (!values.email) {
        errors.email = "Required";
      }

      return errors;
    },
  });

  return (
    <Form>
      <TextField
        id="name"
        name="name"
        label="Name"
        value={formik.values.name}
        onChange={formik.handleChange}
        onBlur={formik.handleBlur}
        error={formik.touched.name && Boolean(formik.errors.name)}
        helperText={formik.touched.name && formik.errors.name}
      />
      <TextField
        id="email"
        name="email"
        label="Email"
        type="email"
        value={formik.values.email}
        onChange={formik.handleChange}
        onBlur={formik.handleBlur}
        error={formik.touched.email && Boolean(formik.errors.email)}
        helperText={formik.touched.email && formik.errors.email}
      />
      {/* Add more MUI components for other form fields if needed */}
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </Form>
  );
};

export default MyForm;
```

**Handle Form Field Interactions**

For each form field, use the appropriate **MUI component** (e.g., `TextField`) and bind its `value`, `onChange`, and `onBlur` attributes to Formik's corresponding functions. This ensures that **Formik** manages the form state and validation:

`value`: Set the value of the MUI component to `formik.values.fieldName`
`onChange`: Call `formik.handleChange` with the field name as an argument to update the field value.
`onBlur`: Call `formik.handleBlur` with the field name as an argument to mark the field as "touched" when it loses focus.

**Display Validation Errors**

To display validation errors, use Formik's `touched` and `errors` properties in conjunction with MUI's error and `helperText` attributes. This way, validation errors are shown when a field is touched and has an error:

```javascript
error={formik.touched.fieldName && Boolean(formik.errors.fieldName)}
helperText={formik.touched.fieldName && formik.errors.fieldName}
```

**Handle Form Submission**

For the form element, use `formik.handleSubmit` as the `onSubmit` attribute to handle form submission. This ensures that Formik triggers the `onSubmit` function we defined earlier when the form is submitted.

For further details and advanced usage, check out the official Formik documentation: https://formik.org/docs.

## Dynamic arrays

To work with dynamic arrays, we need to use the `<FieldArray>` component provided by Formik.

Here's an example of how to use `<FieldArray>`:

1\. Create the Formik form configuration using `useFormik`

```ts
import { FC } from 'react';
import { useFormik, FieldArray } from 'formik';
import { Box, TextField, Button } from '@mui/material';

const MyForm: FC = () => {
  const formik = useFormik({
    initialValues: {
      emails: [''], // Initial value with one empty email field
    },
    onSubmit: (values) => {
      // Handle form submission logic here
      console.log(values);
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <FieldArray name="emails">
        {({ push, remove }) => (
          <>
            {formik.values.emails.map((email, index) => (
              <Box key={index}>
                <TextField
                  name={`emails.${index}`}
                  label={`Email ${index + 1}`}
                  value={email}
                  onChange={formik.handleChange}
                  onBlur={formik.handleBlur}
                />
                {index > 0 && (
                  <Button type="button" onClick={() => remove(index)}>
                    Remove
                  </Button>
                )}
              </div>
            ))}
            <Button type="button" onClick={() => push('')}>
              Add Email
            </Button>
          </>
        )}
      </FieldArray>
      <Button type="submit">
        Submit
      </Button>
    </form>
  );
};
```

Use the `FieldArray` component from **Formik** to handle the array.
Pass it a `name` property with the path to the key within `values` that holds the relevant array.

The `push` function is used to add new item to the array, while the `remove` function is used to remove existing item based on their index.

More about working with dynamic arrays you can find here: https://formik.org/docs/api/fieldarray

## Custom form component

For creating custom from component, we can create a wrapper component that leverages `useField` to manage the form field's state and connect it with your custom form component. The `useField` hook allows you to connect a field in your form with Formik, providing the necessary props and validation handling.

1\. Here's a example how to use `useField` hook with Mui `TextFiled`:

```ts
import { FC } from "react";
import { useField } from "formik";
import { TextField, StandardTextFieldProps } from "@mui/material";

interface CustomTextFieldProps extends StandardTextFieldProps {
  // any custom props
}

const CustomTextField: FC<CustomTextFieldProps> = (props) => {
  const [field, meta] = useField(props);

  return (
    <TextField
      {...field}
      {...props}
      error={meta.touched && !!meta.error}
      helperText={meta.touched && meta.error}
    />
  );
};
```

2\. Use the custom text field component in your form

```ts
import { FC } from "react";
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
  const formik = useFormik({
    initialValues,
    onSubmit: (values) => {
      // Handle form submission logic here
      console.log(values);
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
     {/* you can use Field from formik */}
      <Field name="name" label="Name" component={CustomTextField} />
      {/* or just pass your Component */}
      <CustomTextField name="email" label="Email" />
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </form>
  );
};

export default MyForm;
```

## useFormikContext

`useFormikContext` is a hook that allows you to access the Formik context and form state outside of the Formik component. This can be useful when you have nested components and need to access the form state or dispatch actions from a child component without having to pass props down the component tree.

Here's an example of how to use `useFormikContext` with the `useFormik` hook:

1\. Access form state using `useFormikContext`
Child component that needs access to the form state without having to pass it as a prop. You can use `useFormikContext` in the child component to access the Formik context and form state:

```ts
import { FC } from "react";
import { useFormikContext } from "formik";
import { Box, Typography } from "@mui/material";

export const ChildComponent: FC = () => {
  const formik = useFormikContext();

  return (
    <Box>
      <Typography>
        {formik.isSubmitting ? "Submitting..." : "Form is ready for submission"}
      </Typography>
    </Box>
  );
};
```

2\. Create the form configuration using `useFormik` and add `ChildComponent`

```ts
import { FC } from "react";
import { useFormik } from "formik";
import { TextField, Button } from "@mui/material";

export const MyFormWithChildComponent: FC = () => {
  const formik = useFormik({
    initialValues: {
      email: "",
      password: "",
    },
    onSubmit: (values) => {
      // Simulate form submission
      setSubmitting(false);
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <TextField
        type="text"
        name="name"
        value={formik.values.name}
        onChange={formik.handleChange}
        onBlur={formik.handleBlur}
      />
      <TextField
        type="password"
        name="password"
        value={formik.values.name}
        onChange={formik.handleChange}
        onBlur={formik.handleBlur}
      />
      <Button type="submit" disabled={formik.isSubmitting}>
        Submit
      </Button>
      <ChildComponent />
    </form>
  );
};
```

This allows for a more modular and organized approach to handling form state and interactions in nested components.

## Best practices

Formik is a powerful form management library for React applications, and following best practices can help you get the most out of it while building robust and maintainable forms. Here are some best practices to consider when using Formik:

- **Keep Form Components Separate**: Organize your form components into separate files or components. This helps maintain a clean and modular codebase and improves code reusability.

- **Use `useFormik` Hook**: With React hooks available, prefer using the `useFormik` hook instead of the render props-based `<Formik>` component. It simplifies the code and keeps your logic concise.

- **Initialize `initialValues`:** Provide initial values for your form fields in the `initialValues` object. This ensures that your form state starts with the appropriate values.

- **Implement Validation**: Use **Yup** or other validation libraries to define validation schemas for your form fields. Proper validation ensures data integrity and provides a better user experience.

- **Defer Validation**: Consider deferring validation until the user submits the form or focuses out of a field. This approach prevents showing validation errors immediately on input and provides a smoother user experience.

- **Avoid Inline Functions**: Avoid using inline functions as event handlers in your form components. Instead, create separate functions outside the render scope and bind them to event handlers.

- **Handle Form Submission**: Use the `onSubmit` function to handle form submission. Ensure that you disable the submit button while the form is being submitted to prevent multiple submissions.

- **Use` formik.errors` and `formik.touched`**: Leverage the `formik.errors` and `formik.touched` objects to display validation errors and show visual cues for touched fields.

- **Optimize Rendering:** Minimize unnecessary re-renders of form components. Memoize or separate the components to ensure they only re-render when required.

- **Use `formik.setFieldValue`**: When modifying form field values programmatically (e.g., through other UI interactions), use `formik.setFieldValue` to ensure Formik properly manages the form state.

- **Use `formik.setFieldTouched`**: If you need to mark a field as "touched" programmatically (e.g., when a field loses focus), use formik.setFieldTouched to avoid validation errors.

- **Custom Components and Formik Context**: Utilize custom form components and Formik's context API to share form state and helper functions across your application effectively.

- **Error Boundary**: Consider wrapping your form components with an error boundary to gracefully handle errors and prevent the entire application from crashing due to a form-related error.

- **Test Your Forms**: Write unit tests for your form components and form validation to ensure they work as expected and catch potential issues early.

- **Stay Updated:** Keep your dependencies, including Formik, up to date to benefit from bug fixes, improvements, and new features.

## Optimization

Optimizing forms with Formik involves improving the performance, user experience, and maintainability of your form components. Here are some tips for optimizing forms with Formik:

- **Memoization**: Use React's `React.memo` or `useMemo` hooks to memoize expensive form components, preventing unnecessary re-renders when form state changes.

- **Field-Level Memoization**: For large forms, consider memoizing individual field components (e.g., using` React.memo` for custom field components) to avoid unnecessary re-renders for unrelated fields.

- **Form-Level Validation**: If your form has expensive validation logic, consider using a form-level validation approach rather than field-level validation. Perform validation only when needed, such as on form submission or when the user focuses out of a field.

- **Lazy Initial Values**: Use lazy initial values (e.g., passed as a function) to calculate the initial form state only when needed. This can be beneficial when initializing form data from a remote API or complex calculations.

- **Use Field Arrays Wisely**: If using field arrays (e.g., for dynamic form fields), consider using a key generator function for the name prop to ensure stable keys. Unstable keys can lead to remounting of components, affecting performance.

- **Optimized Field Components**: When creating custom field components, optimize their implementation to avoid unnecessary renders and prevent any side effects that may impact form performance.

- **Preventing Nested Re-renders**: Ensure that your custom field components prevent unnecessary nested re-renders of their children, as it can affect the overall form performance.

- **Leverage Validation Debouncing**: Use Formik's `validateOnBlur` and `validateOnChange` options to debounce validation calls, especially when validation logic involves remote data fetching or complex calculations.

- **Form Reset**: When resetting the form, prefer using `resetForm()` from Formik to set the form back to its initial state efficiently.

- **Improve Validation Schema**: If using Yup or another validation library, optimize your validation schema to be efficient and avoid overly complex validation logic.

- **Avoid Uncontrolled Components**: Stick to controlled components when using Formik. Uncontrolled components can lead to conflicts with Formik's form state management.

- **Use Yup's Conditional Validation**: If you have conditional validation logic, utilize Yup's conditional validation methods like `.when()` to handle complex validation scenarios effectively.

- **Formik Helpers**: Take advantage of Formik's helper functions (e.g., `setFieldValue`, `setFieldTouched`, etc.) to interact with the form state efficiently.

- **Testing and Profiling**: Thoroughly test your forms and use React profiling tools (e.g., React DevTools) to identify any potential performance bottlenecks.

- **Keep Form Logic Lightweight**: Avoid adding heavy computations or logic inside form components, especially during rendering. Consider moving such logic outside the component or running it asynchronously.
