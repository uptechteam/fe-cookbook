# Fast React Context API

Here we will be working with a pub-sub pattern and React refs. More info about the pub-sub pattern in React you can find [here](https://dev.to/avinash8847/publisher-subscriber-pattern-in-react-js-42h8) as well.

## Publisher-subscriber pattern

In simple words, the publisher and subscriber are unaware of each other. All the communication between them is taken through events which are emitted from the publisher and notifies subscriber. As shown bellow:

  ![Pub-sub pattern](https://github.com/uptechteam/fe-cookbook/assets/13544983/fdb7d0d5-ab28-4c5e-8c9a-6dbe234d466f)

### How to avoid unnecessarily rerenders?

We can’t use state updates as it could cause unnecessary rerenders. Also, we can’t use the `useReducer` here as its behavior is the same. So React refs will help us to solve this problem.

Here is a demo for this example:

  ![Fast context demo](https://github.com/uptechteam/fe-cookbook/assets/13544983/2db2e780-f324-4cdc-9b94-7a3927904990)

A full demo of this solution is [here](https://fast-context.vercel.app/fast).

Code for this example:

  ```

    import {
      useRef,
      createContext,
      useContext,
      useCallback,
      useSyncExternalStore,
      ReactNode
    } from "react";

    type Store = { first: string; last: string };

    function useStoreData(): {
      get: () => Store;
      set: (value: Partial<Store>) => void;
      subscribe: (callback: () => void) => () => void;
    } {
      const store = useRef({
        first: "",
        last: "",
      });

      const get = useCallback(() => store.current, []);

      const subscribers = useRef(new Set<() => void>());

      const set = useCallback((value: Partial<Store>) => {
        store.current = { ...store.current, ...value };
        subscribers.current.forEach((callback) => callback());
      }, []);

      const subscribe = useCallback((callback: () => void) => {
        subscribers.current.add(callback);
        return () => subscribers.current.delete(callback);
      }, []);

      return {
        get,
        set,
        subscribe,
      };
    }

    type UseStoreDataReturnType = ReturnType<typeof useStoreData>;

    const StoreContext = createContext<UseStoreDataReturnType | null>(null);

    function Provider({ children }: { children: ReactNode }) {
      return (
        <StoreContext.Provider value={useStoreData()}>
          {children}
        </StoreContext.Provider>
      );
    }

    function useStore<SelectorOutput>(
      selector: (store: Store) => SelectorOutput
    ): [SelectorOutput, (value: Partial<Store>) => void] {
      const store = useContext(StoreContext);
      if (!store) {
        throw new Error("Store not found");
      }

      /*

        ALTERNATIVE APPROACH

        const [state, setState] = useState(store.get());
        
        useEffect(() => {
          return store.subscribe(() => setState(store.get()));
        }, [])

      */

      const state = useSyncExternalStore(store.subscribe, () =>
        selector(store.get())
      );

      return [state, store.set];
    }

    const TextInput = ({ value }: { value: "first" | "last" }) => {
      const [fieldValue, setStore] = useStore((store) => store[value]);
      return (
        <div className="field">
          {value}:{" "}
          <input
            value={fieldValue}
            onChange={(e) => setStore({ [value]: e.target.value })}
          />
        </div>
      );
    };

    const Display = ({ value }: { value: "first" | "last" }) => {
      const [fieldValue] = useStore((store) => store[value]);
      return (
        <div className="value">
          {value}: {fieldValue}
        </div>
      );
    };

    const FormContainer = () => {
      return (
        <div className="container">
          <h5>FormContainer</h5>
          <TextInput value="first" />
          <TextInput value="last" />
        </div>
      );
    };

    const DisplayContainer = () => {
      return (
        <div className="container">
          <h5>DisplayContainer</h5>
          <Display value="first" />
          <Display value="last" />
        </div>
      );
    };

    const ContentContainer = () => {
      return (
        <div className="container">
          <h5>ContentContainer</h5>
          <FormContainer />
          <DisplayContainer />
        </div>
      );
    };

    function App() {
      return (
        <Provider>
          <div className="container">
            <h5>App</h5>
            <ContentContainer />
          </div>
        </Provider>
      );
    }

    export default App;

  ```

See the full code on [GitHub](https://github.com/MaxKalashnyk/fast-context/blob/main/src/Fast.tsx).

### Using `Set` instead of the `Array`

If we subscribe with the same function multiple times we would get multiple instances in the array which we then have to delete. `Set` can add the same value only once.

### The importance of the `useCallback`

If you return some functions from custom hooks I recommend you wrap them to useCallback as we don’t know of how this hook will be called.

### Using a new React hook `useSyncExternalStore`

We have state updates but nothing changes. So in order to make it work we have to subscribe to this state. We can create some state in the `useStore` hook and subscribe/unsubscribe in the `useEffect` hook which will be called once.

In order to simplify this solution we use a new React [hook](https://reactjs.org/docs/hooks-reference.html#usesyncexternalstore) `useSyncExternalStore`.

### Solution with TypeScript generics

Our proposed solution is not flexible enough, so in the following example, we use TypeScript [generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) for making the solution work with different state data types.

  ```

    // App.tsx

    // With generic

    import createFastContext from "./createFastContext";

    const { Provider, useStore } = createFastContext({
      first: "",
      last: "",
    });

    /* TextInput, Display, FormContainer, DisplayContainer and ContentContainer 
    are the same as in the previous example */

    function App() {
      return (
        <Provider>
          <div className="container">
            <h5>Page</h5>
            <ContentContainer />
          </div>
        </Provider>
      );
    }

    export default App;

  ```

  ```

    // createFastContext.tsx

    import {
      useRef,
      createContext,
      useContext,
      useCallback,
      useSyncExternalStore,
      ReactNode
    } from "react";

    export default function createFastContext<Store>(initialState: Store) {
      function useStoreData(): {
        get: () => Store;
        set: (value: Partial<Store>) => void;
        subscribe: (callback: () => void) => () => void;
      } {
        const store = useRef(initialState);

        const get = useCallback(() => store.current, []);

        const subscribers = useRef(new Set<() => void>());

        const set = useCallback((value: Partial<Store>) => {
          store.current = { ...store.current, ...value };
          subscribers.current.forEach((callback) => callback());
        }, []);

        const subscribe = useCallback((callback: () => void) => {
          subscribers.current.add(callback);
          return () => subscribers.current.delete(callback);
        }, []);

        return {
          get,
          set,
          subscribe,
        };
      }

      type UseStoreDataReturnType = ReturnType<typeof useStoreData>;

      const StoreContext = createContext<UseStoreDataReturnType | null>(null);

      function Provider({ children }: { children: ReactNode }) {
        return (
          <StoreContext.Provider value={useStoreData()}>
            {children}
          </StoreContext.Provider>
        );
      }

      function useStore<SelectorOutput>(
        selector: (store: Store) => SelectorOutput
      ): [SelectorOutput, (value: Partial<Store>) => void] {
        const store = useContext(StoreContext);
        if (!store) {
          throw new Error("Store not found");
        }

        const state = useSyncExternalStore(
          store.subscribe,
          () => selector(store.get()),
          () => selector(initialState)
        );

        return [state, store.set];
      }

      return {
        Provider,
        useStore,
      };
    }

  ```

See the full code on [GitHub](https://github.com/MaxKalashnyk/fast-context/blob/main/src/FastWithGeneric.tsx).
