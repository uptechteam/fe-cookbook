# React Context API

Context API has been introduced in React v16.3 and as the documentation states, “Context provides a way to pass data through the component tree without having to pass props down manually at every level”.

Although React Context is simple to implement and great for certain types of apps, it’s built in such a way that every time the value of the context changes, the component consumer rerenders.

Here is an official [documentation](https://react.dev/reference/react/createContext).

## Pros and cons of React Context API

### Pros

- Resourceful and practical in small applications with minimal state changes.
- Easy to understand and handle even for beginners.
- Minimal configuration, only for creating the context.
- Well documented.
- Can use a lot of local contexts to handle separate logic tasks.
- Can be easily used with async tasks.
- Out-of-the-box, which leads to smaller packages and better project maintenance.

### Cons

- Not designed to use with frequently refreshed or changed data.
- Could be more challenging to maintain in complex apps, especially if we have custom solutions and helpers.

## Example of using React Context API

Make sure you configured your browser for showing highlighted components after rerender. Here is an example of how to do it in Google Chrome DevTools:

  ![Highlighting components](https://github.com/uptechteam/fe-cookbook/assets/13544983/5a01dd6a-a69a-44e3-a6ca-e4f8dafd9a58)

In the example below we consider a simple form with a couple of inputs.

  ![Form rerendering](https://github.com/uptechteam/fe-cookbook/assets/13544983/eeecbd44-7e0c-4a42-b19c-ebf6efbab5bc)

As you can see all of the page was rerendered after some of the text inputs were changed.

A full demo for this solution is [here](https://fast-context.vercel.app/).

Every time the value of the context changes, all components’ consumers will rerender.
Here is code for this example:

  ```

    import { useState, createContext, useContext, memo } from "react";

    function useStoreData() {
      const store = useState({
        first: "",
        last: "",
      });
      return store;
    }

    type UseStoreDataReturnType = ReturnType<typeof useStoreData>;

    const StoreContext = createContext<UseStoreDataReturnType | null>(null);

    const TextInput = ({ value }: { value: "first" | "last" }) => {
      const [store, setStore] = useContext(StoreContext)!;
      return (
        <div className="field">
          {value}:{" "}
          <input
            value={store[value]}
            onChange={(e) => setStore({ ...store, [value]: e.target.value })}
          />
        </div>
      );
    };

    const Display = ({ value }: { value: "first" | "last" }) => {
      const [store] = useContext(StoreContext)!;
      return (
        <div className="value">
          {value}: {store[value]}
        </div>
      );
    };

    const FormContainer = memo(() => {
      return (
        <div className="container">
          <h5>FormContainer</h5>
          <TextInput value="first" />
          <TextInput value="last" />
        </div>
      );
    });

    const DisplayContainer = memo(() => {
      return (
        <div className="container">
          <h5>DisplayContainer</h5>
          <Display value="first" />
          <Display value="last" />
        </div>
      );
    });

    const ContentContainer = memo(() => {
      return (
        <div className="container">
          <h5>ContentContainer</h5>
          <FormContainer />
          <DisplayContainer />
        </div>
      );
    });

    function App() {
      const store = useState({
        first: "",
        last: "",
      });

      return (
        <StoreContext.Provider value={store}>
          <div className="container">
            <h5>Page</h5>
            <ContentContainer />
          </div>
        </StoreContext.Provider>
      );
    }

    export default App;

  ```

See the full code on [GitHub](https://github.com/MaxKalashnyk/fast-context/blob/main/src/Default.tsx).
