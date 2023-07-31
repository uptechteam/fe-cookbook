# Validation

  - [Yup](#yup)
    - [Methods](#methods)
    - [Schemas](#schemas)
    - [Using with react hook form](#using-with-react-hook-form)
    - [Using with Formik](#using-with-formik)

## Yup

**Yup** is a JavaScript library that allows you to create schema-based validation rules for data objects.
Yup provides a simple and expressive API for defining validation schemas, making it easy to define complex validation rules.

**Documentation**: https://www.npmjs.com/package/yup

### Methods

Yup provides a variety of validation methods to cover different types of data validation scenarios. Here's an overview of some common validation methods available in Yup:

1. **`.string()`**: Validates that the value is a string.
   - **.required(message)**: Requires the value to be present and not an empty string.
   - **.min(minLength, message)**: Requires the string length to be at least minLength.
   - **.max(maxLength, message)**: Requires the string length to be at most maxLength.
   - **.matches(regex, message)**: Requires the string to match the provided regular expression.
   - **.email(message)**: Requires the string to be a valid email address.
2. **` .number()`**: Validates that the value is a number.
   - **.required(message)**: Requires the value to be present and not NaN.
   - **.min(minValue, message)**: Requires the number to be greater than or equal to minValue.
   - **.max(maxValue, message)**: Requires the number to be less than or equal to maxValue.
   - **.positive(message)**: Requires the number to be positive (greater than 0).
   - **.negative(message)**: Requires the number to be negative (less than 0).
   - **.integer(message)**: Requires the number to be an integer.
3. **`.boolean()`**: Validates that the value is a boolean.
   - **.required(message)**: Requires the value to be present and of type boolean.
4. **`.date()`**: Validates that the value is a valid date.
   - **.required(message)**: Requires the value to be present and a valid date.
5. **`.array()`**: Validates that the value is an array.
   - **.required(message)**: Requires the value to be present and an array.
   - **.min(minLength, message)**: Requires the array length to be at least minLength.
   - **.max(maxLength, message)**: Requires the array length to be at most maxLength.
   - **.of(schema, message)**: Requires each element of the array to match the provided schema.
6. **`.object()`**: Validates that the value is an object.
   - **.required(message)**: Requires the value to be present and an object.
   - **.shape(objectShape)**: Requires the object to have a specific shape, where objectShape is an object with keys matching the expected property names and corresponding validation schemas.
7. **`.oneOf(enumArray, message)`**: Requires the value to be included in the enumArray.
8. **`.notOneOf(enumArray, message)`**: Requires the value not to be included in the enumArray.
9. **`.test(name, message, testFunction)`**: Allows you to define a custom validation test using a function. The `testFunction` takes the value as an argument and should return a boolean or a promise that resolves to a boolean. Use this for more complex validation scenarios.

## Schemas

Here's a basic example of using Yup to validate an object:

1\. Install Yup. You can do this via pnpm or yarn:

```bash
pnpm install yup
# or
yarn add yup
```

2\. Define your validation schema using Yup.

```javascript
import * as Yup from "yup";
import { t } from "i18next"; // Internationalization (i18n) Library (more about library here: https://www.i18next.com/)

const productSchema = Yup.object().shape({
  name: Yup.string().required(t("validation.required")), // here we pass validation message with internationalization (optional)
  price: Yup.number()
    .min(0, "Price must be a positive number")
    .required("Price is required"),
  description: Yup.string().max(
    200,
    "Description must be at most 200 characters"
  ),
  category: Yup.string().oneOf(
    ["electronics", "clothing", "books"],
    "Invalid category"
  ),
  stock: Yup.number().when("category", {
    is: "electronics",
    then: Yup.number().min(10, "Electronics must have a minimum stock of 10"),
    otherwise: Yup.number().min(
      5,
      "Other categories must have a minimum stock of 5"
    ),
  }),
  produceDate: Yup.date().required("Product produce date is required "),
  modifiers: Yup.array().of(
    Yup.object()
      .shape({
        newPrice: Yup.lazy((value) =>
          value === ""
            ? Yup.string().required("price")
            : Yup.number()
                .typeError("acceptablePriceFormat")
                .max(MAX_PRICE, "priceMaxNumber")
                .test(
                  "maxDigitsAfterDecimal",
                  "acceptablePriceFormat",
                  (number) => {
                    return PRICE_REGEX.test(number?.toString() || "");
                  }
                )
        ),
      })
      .required()
  ),
});
```

Validation methods used in this **productSchema**:

- **name**: Requires a non-empty string for the product name.
- **price**: Requires a number that is greater than or equal to 0 for the product price.
- **description**: Limits the description to be at most 200 characters.
- **category**: Ensures the category is one of the three valid options: "electronics", "clothing", or "books".
- **stock**: The stock value depends on the category. If the category is "electronics", the stock must be at least 10; otherwise, for other categories, it must be at least 5.
- **produceDate**: Requires a valid date for the product's production date.
- **modifiers**: Expects an array of objects with specific validation rules.
  - **.of()**: Specifies that each element of the modifiers array should match the defined shape.
  - **Yup.object().shape()**: Defines the shape of each object in the modifiers array.
    - `newPrice`: The `newPrice` property within each `modifiers` object:
      - **Yup.lazy()**: Allows for conditional validation based on the current value of the field.
        - .**typeError()**: Specifies a custom error message if the value is not a valid number.
        - **.max()**: Sets the maximum allowed value for the number.
        - **.test()**: Defines a custom validation test using a function.
          - The test checks if the number matches the `PRICE_REGEX`, a regular expression for valid price format (presumably with a maximum of `MAX_PRICE` digits after the decimal point)
          - It returns `true` if the number matches the format, and an error message otherwise.

## Using with react hook form

**React Hook Form** provides a `yupResolver` function that allows you to use Yup schemas for form validation. Here's how you can do it:

1\. Install the required packages (if you haven't already):

```bash
pnpm install react-hook-form yup @hookform/resolvers
# or
yarn add react-hook-form yup @hookform/resolvers
```

2\. Define your component with the form and inputs

```ts
import { FC } from 'react';
import { useForm } from 'react-hook-form';
import { yupResolver } from '@hookform/resolvers';
import { productSchema } from './schema';

const defaultValues = {
    name: '',
    price: 0,
    description: '',
    category: 'electronics',
    stock: 10,
    produceDate: new Date().toISOString(),
    modifiers: [{ newPrice: '' }],
  };

const ProductForm: FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: yupResolver(productSchema), // Use the yupResolver to integrate the Yup schema
	defaultValues, // Pass the defaultValues object to useForm
  });

  const onSubmit = (data) => {
    // Handle form submission with validated data
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Input fields */}
      <input {...register('name')} />
      {errors.name && <span>{errors.name.message}</span>}
      <input {...register('price')} />
      {errors.price && <span>{errors.price.message}</span>}
      <textarea {...register('description')} />
      {errors.description && <span>{errors.description.message}</span>
      {/* Other input fields */}

      <button type="submit">Submit</button>
    </form>
  );
}
```

## Using with Formik

To integrate the schema from Yup with Formik using the `useFormik` hook, you'll need to use the `validationSchema` prop provided by Formik. This prop allows you to pass your Yup schema to Formik for validation. Here's how you can do it:

1\. Install the required packages (if you haven't already):

```bash
pnpm install formik yup
# or
yarn add formik yup
```

2\. Define your component with the form and inputs

```ts
import { FC } from "react";
import { useFormik } from "formik";
import { productSchema } from "./schema";

const initialValues = {
  name: "",
  price: 0,
  description: "",
  category: "electronics",
  stock: 10,
  produceDate: new Date().toISOString(),
  modifiers: [{ newPrice: "" }],
};

const ProductForm: FC = () => {
  const formik = useFormik({
    initialValues,
    validationSchema: productSchema, // Pass the Yup schema to Formik for validation
    onSubmit: (values) => {
      // Handle form submission with validated data
      console.log(values);
    },
  });

  const onSubmit = (data) => {
    // Handle form submission with validated data
    console.log(data);
  };

  return (
    <form onSubmit={formik.handleSubmit}>
      {/* Input fields */}
      <input {...formik.getFieldProps("name")} />
      {formik.touched.name && formik.errors.name && (
        <span>{formik.errors.name}</span>
      )}
      {/* Other input fields */}
      <button type="submit">Submit</button>
    </form>
  );
};
```
