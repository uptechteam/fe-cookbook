# Roles and permissions

- [General information](#general-information)
- [Hook](#hook)
- [HOC](#hoc)
- [Wrapper component](#wrapper-component)

## General information

Handling roles and permissions in a React app is an essential aspect of building secure and scalable applications, especially when dealing with different user types and access levels. Below are some approaches and best practices to manage roles and permissions effectively in a React app:

1. Role-Based Access Control (RBAC):
   RBAC is a widely-used approach for managing roles and permissions in applications. In this model, users are assigned specific roles (e.g., admin, manager, user) that determine their access to certain features or functionalities. The application's components and routes can be conditionally rendered based on the user's role.

2. Centralized State Management:
   Use a centralized state management library like Redux or MobX to manage user roles and permissions globally across the application. This ensures consistency and makes it easier to update roles or permissions when needed.

3. Authentication and Authorization:
   Implement a robust authentication system to verify user identities and ensure that only authenticated users can access protected parts of the application. Additionally, implement authorization logic to check if the authenticated user has the required permissions for a particular action.

4. Route-Based Access Control:
   Leverage React Router or similar routing libraries to protect routes based on user roles and permissions. You can create higher-order components (HOCs) or custom route wrappers to control access to specific routes.

5. Backend Validation:
   While access control can be enforced on the frontend, always ensure that critical operations and sensitive data access are validated on the backend as well. The frontend should act as a visual representation of the backend's access control.

6. Secure Data Fetching:
   When fetching data from the server, ensure that the backend provides only the data that the user is allowed to access. This prevents unauthorized access to sensitive information.

7. Conditional Rendering:
   Use conditional rendering techniques to show/hide components or elements based on the user's role and permissions. This helps create a personalized user experience.

8. Error Handling:
   Implement proper error handling when a user attempts to access restricted areas or perform unauthorized actions. Provide clear feedback to the user and avoid exposing sensitive information in error messages.

9. Regular Review and Updates:
   Periodically review and update the roles and permissions of users as the application evolves. Users may change roles within an organization, and new features might require updates to access control logic.

10. Testing:
    Thoroughly test your role-based access control mechanisms to ensure they work as expected. Write unit tests and end-to-end tests to cover various scenarios involving different roles and permissions.

By following these best practices, you can create a secure and scalable React app with robust role-based access control, providing a smooth and personalized user experience for different user types and access levels.

## Hook

Here's the `usePermissions` hook, assuming you have the user's permissions available in your Redux store:

```js
import { useSelector } from "react-redux";

const usePermissions = () => {
  const permissions = useSelector((state) => state.user.permissions);

  const hasPermission = (requiredPermission) => {
    return permissions.includes(requiredPermission);
  };

  return { permissions, hasPermission };
};

export default usePermissions;
```

#### Usage

```js
const MyComponent = () => {
  const { hasPermission } = usePermissions();

  // Your component logic here...

  return <div>{hasPermission("edit_posts") && <button>Edit Post</button>}</div>;
};

export default MyComponent;
```

## HOC

Here's the implementation of the `withRolesAndPermissions` HOC:

```js
const withRolesAndPermissions =
  (requiredRoles, requiredPermissions) => (WrappedComponent) => {
    const EnhancedComponent = (props) => {
      // Replace this with your actual implementation to get user roles and permissions
      const userRoles = ["admin", "manager"];
      const userPermissions = ["edit_posts"];

      const hasRole = (requiredRole) => userRoles.includes(requiredRole);
      const hasPermission = (requiredPermission) =>
        userPermissions.includes(requiredPermission);

      const isAuthorized = () => {
        if (requiredRoles && requiredRoles.length > 0) {
          return requiredRoles.some(hasRole);
        }
        if (requiredPermissions && requiredPermissions.length > 0) {
          return requiredPermissions.every(hasPermission);
        }
        return true; // No specific roles or permissions required, allow access
      };

      return isAuthorized() ? <WrappedComponent {...props} /> : null;
    };

    return EnhancedComponent;
  };

export default withRolesAndPermissions;
```

In this HOC, we define a function `withRolesAndPermissions` that takes two arrays: requiredRoles and requiredPermissions. It returns a new function that takes the component to be wrapped (WrappedComponent) and returns an enhanced component (`EnhancedComponent`).

The `EnhancedComponent` performs the role and permission checks based on the provided requiredRoles and requiredPermissions and renders the WrappedComponent only if the user has the necessary roles and permissions. If the user doesn't meet the requirements, it displays nothing.

To use this HOC in your components, follow these steps:

#### Usage

```js
const MyComponent = (props) => {
  // Your component logic here...

  return (
    <div>
      <button>Edit Post</button>
    </div>
  );
};

// Specify the required roles and/or permissions when applying the HOC
export default withRolesAndPermissions(
  ["admin", "manager"],
  ["edit_posts"]
)(MyComponent);
```

In this example, we apply the `withRolesAndPermissions` HOC to `MyComponent`, indicating that the user must have either the `admin` or `manager` role and the `edit_posts` permission to access this component. If the user doesn't meet these requirements, the HOC will render nothing instead of the wrapped component.

By using this composition component, you can easily manage roles and permissions in your React app and control access to different components or features based on the user's role and permissions.

## Wrapper component

Here's the implementation of the `RolesAndPermissionsWrapper` component:

```js
const RolesAndPermissionsWrapper = ({
  requiredRoles,
  requiredPermissions,
  children,
}) => {
  // Replace this with your actual implementation to get user roles and permissions
  const userRoles = ["admin", "manager"];
  const userPermissions = ["edit_posts"];

  const hasRole = (requiredRole) => userRoles.includes(requiredRole);
  const hasPermission = (requiredPermission) =>
    userPermissions.includes(requiredPermission);

  const isAuthorized = () => {
    if (requiredRoles && requiredRoles.length > 0) {
      return requiredRoles.some(hasRole);
    }
    if (requiredPermissions && requiredPermissions.length > 0) {
      return requiredPermissions.every(hasPermission);
    }
    return true; // No specific roles or permissions required, allow access
  };

  return isAuthorized() ? <>{children}</> : null;
};

export default RolesAndPermissionsWrapper;
```

In this Wrapper Component, we receive the `requiredRoles` and `requiredPermissions` as props. We then perform the role and permission checks based on the provided props. If the user has the necessary roles and permissions, the Wrapper Component will render the children. Otherwise, it will display the `null`.
To use this HOC in your components, follow these steps:

#### Usage

```js
const MyComponent = (props) => {
  // Your component logic here...

  return (
    <div>
      <button>Edit Post</button>
    </div>
  );
};

export default () => (
  <RolesAndPermissionsWrapper
    requiredRoles={["admin", "manager"]}
    requiredPermissions={["edit_posts"]}
  >
    <MyComponent />
  </RolesAndPermissionsWrapper>
);
```

In this example, we wrap `MyComponent` with the `RolesAndPermissionsWrapper` and specify that the user must have either the `admin` or `manager` role and the `edit_posts` permission to access the component. If the user doesn't meet these requirements, the Wrapper Component will render the `null` instead of the wrapped component.

This approach provides a more declarative way of handling permissions and allows you to specify the requirements directly where you use the component.
