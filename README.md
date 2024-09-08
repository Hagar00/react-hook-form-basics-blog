# react-hook-form-basics-blog
This repository provides a comprehensive guide to using React Hook Form for handling form validation in React. Learn the fundamentals of useForm, field validation, integration with third-party components using Controller, and managing dynamic fields with useFieldArray, complete with examples and best practices.
# Basics of React Hook Form

## Introduction
React Hook Form is a lightweight library for handling form validation in React. Unlike other form libraries, it uses uncontrolled inputs (using `ref`) instead of relying on React state to control the inputs. This approach reduces the number of re-renders and makes the form more performant. Additionally, React Hook Form is minimal with zero dependencies, making it a great choice for modern web applications.

## Benefits of Using React Hook Form
1. **Uncontrolled Inputs with Ref**: This makes the forms more performant by reducing the number of re-renders.
2. **Small Size and Zero Dependencies**: It’s a lightweight library, meaning it won’t bloat your project.

## Fundamentals of the `useForm` Hook
The `useForm` Hook returns an object containing a few key properties, such as `register` and `handleSubmit`. These properties are crucial for integrating forms with React Hook Form.

```jsx
const { register, handleSubmit } = useForm();

## section 1 
•	Register: This method registers an input field with React Hook Form, making it available for validation and tracking changes.

```jsx
<input type="text" name="firstName" {...register('firstName')} />
```

## Note that each input must have a unique name property.

HandleSubmit: This method is used to handle form submission. It takes two functions as arguments:
1.	The first function is called when form validation is successful.
2.	The second function is called when validation fails.

```jsx
const onFormSubmit = (data) => console.log(data);
const onErrors = (errors) => console.error(errors);

<form onSubmit={handleSubmit(onFormSubmit, onErrors)}>
  {/* ... */}
</form>

## Basic Example

```jsx
import React from "react";
import { useForm } from "react-hook-form";

const RegisterForm = () => {
  const { register, handleSubmit } = useForm();
  const handleRegistration = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(handleRegistration)}>
      <div>
        <label>Name</label>
        <input name="name" {...register('name')} />
      </div>
      <div>
        <label>Email</label>
        <input type="email" name="email" {...register('email')} />
      </div>
      <div>
        <label>Password</label>
        <input type="password" name="password" {...register('password')} />
      </div>
      <button>Submit</button>
    </form>
  );
};
export default RegisterForm;
```

## Section 2: Validation
To validate forms with React Hook Form, pass validation parameters to the register method:

** uired: Makes the field mandatory.

**  minLength / maxLength: Sets the minimum and maximum length for string input values.

**  min / max: Sets the minimum and maximum values for numerical inputs.

** pattern: Defines a regex pattern for the input.

## Example with Validations
``` jsx
import React from "react";
import { useForm } from "react-hook-form";

const RegisterForm = () => {
  const { register, handleSubmit, formState: { errors } } = useForm();
  const handleRegistration = (data) => console.log(data);
 const registerOptions = {
    name: { required: "Name is required" },
    email: { required: "Email is required" },
    password: {
      required: "Password is required",
      minLength: {
        value: 8,
        message: "Password must have at least 8 characters"
      }
    }
  };

  return (
    <form onSubmit={handleSubmit(handleRegistration)}>
      <div>
        <label>Name</label>
        <input name="name" type="text" {...register('name', registerOptions.name)} />
        <small>{errors?.name && errors.name.message}</small>
      </div>
      <div>
        <label>Email</label>
        <input type="email" name="email" {...register('email', registerOptions.email)} />
        <small>{errors?.email && errors.email.message}</small>
      </div>
      <div>
        <label>Password</label>
        <input type="password" name="password" {...register('password', registerOptions.password)} />
        <small>{errors?.password && errors.password.message}</small>
      </div>
      <button>Submit</button>
    </form>
  );
};
export default RegisterForm;
```

## Section 3: Using Controllers for Third-Party Components
When using third-party components that don’t directly expose input elements, use the Controller component provided by React Hook Form.
Example with Controller


## Example with controller 
```
jsx
import { useForm, Controller } from "react-hook-form";
import Select from "react-select";

const { register, handleSubmit, control } = useForm();

const selectOptions = [
  { value: "student", label: "Student" },
  { value: "developer", label: "Developer" },
  { value: "manager", label: "Manager" }
];

const registerOptions = {
  role: { required: "Role is required" }
};

return (
  <form>
    <div>
      <label>Your Role</label>
      <Controller
        name="role"
        control={control}
        defaultValue=""
        rules={registerOptions.role}
        render={({ field }) => (
          <Select options={selectOptions} {...field} />
        )}
      />
      <small>{errors?.role && errors.role.message}</small>
    </div>
  </form>
);
```
## Section 4: Working with Arrays and Nested Fields
To handle arrays and nested fields, use the useFieldArray Hook. This hook provides methods such as append, remove, prepend, move, insert, and swap to manipulate array items.
Example with useFieldArray

``` jsx

import React from "react";
import { useForm, useFieldArray } from "react-hook-form";

function App() {
  const { register, control, handleSubmit } = useForm();
  const { fields, append, remove } = useFieldArray({
    control,
    name: "test"
  });

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <ul>
        {fields.map((item, index) => (
          <li key={item.id}>
            <input
              name={`test[${index}].firstName`}
              ref={register()}
              defaultValue={item.firstName}
            />
            <button type="button" onClick={() => remove(index)}>Delete</button>
          </li>
        ))}
      </ul>
      <button type="button" onClick={() => append({ firstName: "New" })}>Append</button>
      <input type="submit" />
    </form>
  );
}
export default App;
```

## Conclusion
React Hook Form simplifies the process of creating, validating, and managing forms in React applications. It improves performance by using uncontrolled components, requires minimal code, and is highly extensible, making it a great choice for both simple and complex forms.









