# React hook form

1.  [Setup](#setup)
2.  [Best practices](#best-practices)
3.  [Configure form fields](#configure-form-fields)
4.  [Optimization](#optimization)
5.  [Validation with Yup](#validation-with-yup)
6.  [Validation with Zod](#validation-with-zod)

## Setup

React Hook Form is a popular library for managing form state and validation in React applications. Here's a step-by-step guide to setting it up:

1\. Install React Hook Form:

```bash
pnpm install react-hook-form
```

2\. Import necessary components and functions and create and use a form:

In your desired component file (e.g., src/FormComponent.tsx), import the necessary components and functions from the React Hook Form library:

```ts
import { FC } from 'react';
import { useForm } from 'react-hook-form';

const FormComponent: FC = () => {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = (data) => {
    console.log(data); // Data contains the form input values
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="name">Name:</label>
      <input
        type="text"
        id="name"
        {...register('name', { required: 'Name is required' })}
      />
      {errors.name && <p>{errors.name.message}</p>}
      <button type="submit">Submit</button>
    </form>
  );
}

export default FormComponent;

```

The `useForm` hook is used to initialize the form state and validation rules. The `register` function is used to connect form inputs to the form state. The `handleSubmit` function is used to handle form submissions and trigger validation.

3\. Styling and enhancing your form (optional):

You can style and enhance your form just like any other React form. You can also use additional features provided by React Hook Form, such as custom validation, working with arrays, conditional field rendering, etc. The React Hook Form documentation (https://react-hook-form.com/) is an excellent resource to explore more features and examples.

## Best practices

Using React Hook Form effectively involves following best practices to ensure a maintainable, performant, and scalable codebase. Here are some best practices for using React Hook Form:

- **Minimize Re-renders**: React Hook Form is designed to minimize unnecessary re-renders. To achieve this, ensure that you use the `useForm` hook at the top level of your functional component and avoid using it within loops or conditional statements.

- ** Form Organization**: Organize your form logic and UI components separately. Keep the form-related logic in one place, and use separate components for form inputs, validation messages, etc.

- **Uncontrolled Components**: React Hook Form relies on uncontrolled components. Avoid using `defaultValue` with input fields as it may lead to unexpected behaviors.

- **Use Registered Fields**: Always use the register function to register your form inputs with React Hook Form. This is how the library keeps track of form state and validation.

- **Validation**: Use the built-in validation provided by React Hook Form. It supports various validation rules like required, minLength, pattern, etc. You can also use custom validation functions.

- **Conditional Validation**: Implement conditional validation based on the form input's value using watch and trigger functions.

- **Performance Optimization**: Use the `useForm` hooks options parameter to enable shallow form state tracking (shouldUnregister: true) for better performance with large forms.

- **Field Arrays**: For dynamic field arrays (e.g., repeating form fields), use the `useFieldArray` hook provided by React Hook Form.

- **Error Handling**: Handle errors gracefully. Show validation errors near the input fields and provide clear error messages.

- **Use Controller (Optional)**: For integrating with third-party components that require a value prop (controlled components), use the `Controller` component from React Hook Form.

- **Form Submission**: Use the `handleSubmit` function from the useForm hook to handle form submissions. Avoid using the native form submit unless necessary.

- **Disable Browser Validation**: To avoid conflicts with the built-in browser validation, remember to set the noValidate attribute on your form element.

- **Clear Errors**: Clear errors when necessary, especially if you're using conditional fields or dynamically showing/hiding inputs.

- **Test Your Forms**: Write unit tests to ensure your forms behave as expected, especially concerning validation and form submission.

- **Keep Updated**: Stay updated with the React Hook Form documentation and the library updates to leverage new features and improvements.

## Configure form fields

```ts
import { FC } from 'react';
import { useForm } from 'react-hook-form';

const  MyFormComponent: FC = () => {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = (data) => {
    console.log(data); // Data contains the form input values
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Form fields go here */}
      <label htmlFor="name">Name:</label>
      <input
        type="text"
        id="name"
        {...register('name', { required: 'Name is required' })}
      />
      {errors.name && <p>{errors.name.message}</p>}

      {/* Add more form fields here */}

      <button type="submit">Submit</button>
    </form>
  );
}

export default MyFormComponent;
```

For each form field, you'll need to use the `register` function to register the input with React Hook Form. The `register` function connects the form input to the form state and enables validation based on the specified rules.

In the example above, we've registered a field with the name 'name' and added a required validation rule. The `errors` object from the formState provides information about any validation errors.

**Handle form submissions:**

In the `onSubmit` function, you can handle the form submissions. The `handleSubmit` function from React Hook Form will handle form validation and call your `onSubmit` function if the form is valid.

In the example above, when the form is submitted, the `onSubmit` function will be called, and the data containing the form input values will be logged to the console.

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
register('field_name');

```

3\. **Shallow Form State: ** For large forms, enable the `shouldUnregister` option when using `useForm`. This option helps in reducing the number of fields being re-registered when a field is unmounted, which can improve performance.

```javascript
const { register } = useForm({
  // Enable shallow form state tracking
  shouldUnregister: true,
});
```

## Validation with Yup

To use **Yup** with **React Hook Form**, you can combine the powerful validation capabilities of Yup with the form management features of React Hook Form. Yup is a schema validation library that allows you to define validation rules for your form fields declaratively. Here's how you can use Yup with React Hook Form:

1\. Install Yup:

```bash
pnpm install @hookform/resolvers yup
```

2\.  Import necessary modules:

In your form component, import the required modules from Yup and React Hook Form.

```ts
import { FC } from 'react';
import { useForm } from 'react-hook-form';
import { yupResolver } from '@hookform/resolvers/yup';
import * as yup from 'yup';
```

3\. Define Yup schema for validation:

Create a Yup schema that defines the validation rules for your form fields.

```javascript
const schema = yup.object().shape({
  name: yup.string().required('Name is required'),
  email: yup.string().email('Invalid email').required('Email is required'),
  age: yup.number().integer().positive().required('Age is required'),
});
```

4\. Create the form component:

Use the `useForm` hook and the `yupResolver` from `@hookform/resolvers/yup` to integrate Yup validation with React Hook Form.

```ts
const MyFormComponent:FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: yupResolver(schema),
  });

  const onSubmit = (data) => {
    console.log(data); // Data contains the form input values
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="name">Name:</label>
      <input type="text" id="name" {...register('name')} />
      {errors.name && <p>{errors.name.message}</p>}

      <label htmlFor="email">Email:</label>
      <input type="text" id="email" {...register('email')} />
      {errors.email && <p>{errors.email.message}</p>}

      <label htmlFor="age">Age:</label>
      <input type="number" id="age" {...register('age')} />
      {errors.age && <p>{errors.age.message}</p>}

      <button type="submit">Submit</button>
    </form>
  );
}
```

5\. Submit the form:

When the form is submitted, the `handleSubmit` function will automatically trigger the Yup validation defined in the schema. If there are any validation errors, they will be displayed next to the respective form fields.

With Yup and React Hook Form integrated, your form will have robust validation based on the Yup schema, providing a smooth user experience.

Yup's documentation is a great resource for exploring all the available validation rules and methods: https://github.com/jquense/yup.

## Validation with Zod

Validation with Zod is similar to Yup. To use Zod with React Hook Form, you can follow these steps:


1\. Install Zod:

```bash
pnpm install @hookform/resolvers zod
```

2\. Create a Zod schema that defines the validation rules for your form fields.

```javascript
import { z } from 'zod';

const schema = z.object({
  name: z.string().nonempty('Name is required'),
  email: z.string().email('Invalid email').nonempty('Email is required'),
  age: z.number().int('Age must be an integer').nonnegative('Age is required'),
});
```

3\. Create the form component:

```ts
import { FC } from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

const MyFormComponent: FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(schema),
  });

  const onSubmit = (data) => {
    console.log(data); // Data contains the form input values
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="name">Name:</label>
      <input type="text" id="name" {...register('name')} />
      {errors.name && <p>{errors.name.message}</p>}

      <label htmlFor="email">Email:</label>
      <input type="text" id="email" {...register('email')} />
      {errors.email && <p>{errors.email.message}</p>}

      <label htmlFor="age">Age:</label>
      <input type="number" id="age" {...register('age')} />
      {errors.age && <p>{errors.age.message}</p>}

      <button type="submit">Submit</button>
    </form>
  );
}
```

Always refer to the official documentation for the latest information:
Zod: https://github.com/colinhacks/zod