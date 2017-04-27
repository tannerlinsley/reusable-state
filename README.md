# reusable-state-journey
Journey through a variety of reusable state tactics for React.

Managing state is hard. When you need to make that state 100% portable and allow users to interact with easily, things get even harder.  In the following codepen and storybook examples, you'll see incremental progress made in bite-size steps towards 100% component reusability, along with a few new tools and concepts that may help you on your way. For a more thorough introduction on why this should be important to you, take a look at the readme from [Reusable Components with Redux](#which-should-i-use).

## setState, hoisting state & controlled components
- [Intro to setState](https://codepen.io/tannerlinsley/pen/rmjOaQ?editors=0010)
- [Pure setState functions](https://codepen.io/tannerlinsley/pen/rmjOaQ?editors=0010)
- [setState middleware & logging](https://codepen.io/tannerlinsley/pen/bWgVWa?editors=0011)
- [Reusable state && debugging](https://codepen.io/tannerlinsley/pen/JNEYMd?editors=0011)
- [Reusable state && debugging pt. 2](https://codepen.io/tannerlinsley/pen/xdgVbW?editors=0011)
- [Programmatically Hoisting state](https://codepen.io/tannerlinsley/pen/OmWyqZ?editors=0011)
- [Controlled component state](https://codepen.io/tannerlinsley/pen/oWBbop?editors=0011)
- [Dynamic state (isolated, hoisting or controlled)](https://codepen.io/tannerlinsley/pen/OmWMLE?editors=0011)

## setState vs React-State for shared & deep props/callbacks, performance

- [Props & Callback Hell](https://codepen.io/tannerlinsley/pen/vmgGLP?editors=0010)
- [Props & Callback Heaven](https://codepen.io/tannerlinsley/pen/zwNqbM?editors=0010)
- [React-State](https://github.com/tannerlinsley/react-state)

## Reusable Redux

- [Counter Example](https://codepen.io/tannerlinsley/pen/YVNWap?editors=0011)
- [Counter Repo](https://github.com/tannerlinsley/reusable-components-with-redux)

## Which should I use?

Building redistributable and reusable React components is extremely difficult. There are plenty of decisions to be considered:
- What kind of React Apps can use your component?
- What are the technical requirements for an app to use your component?
- How much control am you going to give the user over the internals of your component?
- How will you manage state in a way that is transparent to the user
- Does your component's state need features like undo/redo/replay/import/export
- How sophisticated or complicated is your state? Do you need powerful devtools like Redux?
- How small do yo want your components bundle to be?

Depending on all your responses above, you will likely land in one of these buckets:
- Only Redux / MobX / other-state-manager users can utilize your component.
  - If you are searching how to create reusable components for **only** the users of one of these libraries, you'll be better off following that library's specific reusable component paradigm (if it has one). But don't do that! Think of all the amazing React apps out there that might use something different from you! They would probably benefit from your library if you build it for everyone, so keep reading!
- You want to use MobX to manage the internal state of your component.
  - Though it is possible to use MobX agnostically for internal component state, we don't suggest it. MobX bundles tend to be much larger than their functional alternatives and are generally harder to debug given they embrace mutability rather than immutability. If you still prefer using it, we won't stop you. :) But if you would like to see how to write more predictable code while cutting bundle size by potentially 90%, keep reading!
- Your component should be usable by any and all React apps. Sweet!
  - Your component is extremely simple with limited or no state involved.
    - Congrats! `setState` is your first and best friend. You should be able to perform every ounce of state management, debugging and functionality you want with nothing other than React itself!
    - [See the `setState` example](#setstate-hoisting-state--controlled-components)
  - Your component has a moderate amount of state involved and its subcomponents may need to share a global state.
    - At this point, `setState` may get you most of the way there (maybe even painfully all the way), but it can begin to feel clunky or weak. Due to it's limited API, `setState` pushes you into passing large amounts of `state` and `updateState` callbacks as props to child components.  Depending on how deep your child components go or if your subcomponents need to share data or some type of "global" component state, using `setState` exclusively can significantly bloat your views, decrease performance and be a nightmare to maintain or refactor. `Context` may feel like a good fit here, because it is! But, unfortunately, the way `Context` currently ships in React, it can be very difficult to understand and correctly configure (especially for stable update cycles, change detection, and rerenders). If not properly configured, you can run into even more difficulties down the road.
    - Luckily, there is a tool called `React-State` that saves the day! This handy library is extremely lightweight and enhances the power of the traditional `setState` paradigm via `Context` and `PureComponent`.
    - [See the React-State example](#setstate-vs-react-state-for-shared--deep-propscallbacks-performance)
  - You have tons of state in your component. You need an extremely predictable and sophisticated state machine with devtools and customization that will get you out of any pickle. Your subComponents need to be highly connected, optimized and sensitive to very specific parts of your state.
    - Redux to the rescue! **Yes**, we said Redux. "But wait! Didn't you say I need to cater to **all** React users?!" Yes we did. :) Redux is an amazing flux-based state manager that has grounded itself as the de facto solution in the React ecosystem for managing application state. At this point, your component probably feels like a full-blown application, so it only makes sense to use the right tool for the right job!
    - Generally, Redux is looked at as a *top-level* application state manager, but this is not true. It's mechanics are highly reusable for just about anywhere you would use state, including reusable components.
    - Redux is small, even when you included `react-redux` bindings, you're still only looking at 8kb. To some that may feel heavy for a reusable component, but if your stat is as complex as it sounds, those will be the most valuable kilobytes in your entire component's bundle!
    - [See the Redux example](#reusable-redux)
