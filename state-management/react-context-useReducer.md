# React Context API with the useReducer hook

React Context API and the `useReducer` hook are commonly used together to manage state at a global level in React applications. The `useReducer` hook provides a way to manage complex state and state transitions, while the Context API allows you to propagate that state to any component that needs it without passing props through intermediate components.

Here is an official [documentation](https://react.dev/learn/scaling-up-with-reducer-and-context) of this approach.

And here's an example of how you can use the `useReducer` hook with the Context API:

1. Create a folder "contexts" in your app repo.
2. Create a subfolder with the appropriate name of your context (e.g. Counter).
3. In this folder we usually store the `Counter.tsx` file and the `types.ts` file.
4. Update your `Counter.tsx` file(or any name you prefer) to define your global state and actions:

  ```
    import { createContext, useReducer, FC, useContext } from "react";
    import { CounterActions } from "./types";

    // Define the initial state
    const initialState: IState = {
      count: 0,
    };

    const CounterDispatch = createContext<ActionDispatch>(() => ({
      type: Actions.Noop,
    }));

    const CounterContext = createContext<IState>(defaultState);

    export const Reducer: React.Reducer<IState, IActionTypes> = (state, action) => {
       switch (action.type) {
        case CounterActions.Increment:
          return { ...state, count: state.count + 1 };
        case CounterActions.Decrement:
          return { ...state, count: state.count - 1 };
        default:
          return state;
      }
    };

    export const Context: FC<IContext> = ({ children }) => {
      const [state, dispatch] = useReducer<React.Reducer<IState, IActionTypes>>(
        Reducer,
        defaultState
      );

      return (
        <CounterDispatch.Provider value={dispatch as ActionDispatch}>
          <CounterContext.Provider value={state}>
            {children}
          </CounterContext.Provider>
        </CounterDispatch.Provider>
      );
    };

    export const useCounterState = (): IState => {
      const ctx = useContext<IState>(CounterContext);
      if (ctx === undefined) {
        throw new Error("useState must be used within a Provider");
      }
      return ctx;
    };

    export const useCounterDispatch = (): ActionDispatch => {
      const dispatch = useContext<ActionDispatch>(CounterDispatch);
      if (dispatch === undefined) {
        throw new Error("useDispatch must be used within a Provider");
      }
      return dispatch;
    };

  ```

5. Update your `types.ts` file:

  ```

    import { ReactNode } from "react";

    export enum CounterActions {
      Increment = "INCREMENT",
      Decrement = "DECREMENT",
      Noop = "noop",
    }

    export interface IState {
      count: number;
    }

    export interface IContext {
      children?: ReactNode;
    }

    export interface IOperation {
      type: typeof CounterActions.Increment | typeof CounterActions.Decrement;
      // payload: number; You can also add types for your payloads 
    }

    export interface INoopAction {
      type: typeof CounterActions.Noop;
    }

    export type CounterActionTypes = IOperation | INoopAction;

    export type ActionDispatch = (arg: CounterActionTypes) => CounterActionTypes;

  ```

6. Wrap your root component with prior created providers:

  ```

    import ReactDOM from 'react-dom';
    import App from './App';
    import { Context } from '~/contexts/CounterContext';

    ReactDOM.render(
      <Context>
        <App />
      </Context>,
      document.getElementById('root')
    );

  ```

  Note than you can wrap not only root component of your app but other different pages:

  ```

    <Context>
      <MyComponent />
    </Context>

  ```

7. In any component that needs access to the global state, import the `Context` and use the `useCounterState` and the `useCounterDispatch` hooks to access the state and dispatch function:

  ```

    // MyComponent.js
    import { useContext, FC } from 'react';
    import { useCounterState, useCounterDispatch } from '~/contexts/CounterContext';

    export const MyComponent: FC = () => {
      const { count } = useCounterState();
      const dispatch = useCounterDispatch();

      const handleIncrement = () => {
        dispatch({ type: CounterActions.Increment });
      };

      const handleDecrement = () => {
        dispatch({ type: CounterActions.Decrement });
      };

      return (
        <div>
          <p>Count: {count}</p>
          <button onClick={handleIncrement}>Increment</button>
          <button onClick={handleDecrement}>Decrement</button>
        </div>
      );
    };

  ```

8. More information you can find [here](https://react.dev/learn/scaling-up-with-reducer-and-context).
