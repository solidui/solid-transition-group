<p>
  <img width="100%" src="https://assets.solidjs.com/banner?project=Transition%20Group&type=core" alt="Solid Transition Group">
</p>

# Solid Transition Group

Animation library influenced by React Transition Group and Vue Transitions for the SolidJS library.

Known limitation: Transition and Transition Group work on detecting changes on DOM children. Only supports single DOM child. Not Text or Fragments.

Animations aren't always smooth under rapid input still working on improving.

## Installation

```bash
npm install solid-transition-group
# or
yarn add solid-transition-group
# or
pnpm add solid-transition-group
```

## Transition

Props:

| Name             | Type                     | Description                                                                                           |
| ---------------- | ------------------------ | ----------------------------------------------------------------------------------------------------- |
| `name`           | `string`                 | Used to automatically generate transition CSS class names. e.g. `name: 'fade'` will auto expand to `.fade-enter`, `.fade-enter-active`, etc. Defaults to `"s"`. |
| `appear`         | `boolean`                | Whether to apply transition on initial render. Defaults to `false`.                                    |
| `mode`           | `"inout" \| "outin" \| undefined`                 | Controls the timing sequence of leaving/entering transitions. Available modes are `"outin"` and `"inout"`; defaults to simultaneous. |
| `enterActiveClass`| `string`      | CSS class applied to the entering element for the entire duration of the enter transition.                               |
| `enterClass`     | `string`      | CSS class applied to the entering element at the start of the enter transition, and removed the frame after.                  |
| `enterToClass`   | `string`      | CSS class applied to the entering element after the enter transition starts.                             |
| `exitActiveClass`| `string`      | CSS class applied to the exiting element for the entire duration of the exit transition.    |
| `exitClass`      | `string`      | CSS class applied to the exiting element at the start of the exit transition, and removed the frame after.                                                  |
| `exitToClass`    | `string`      | CSS class applied to the exiting element after the exit transition starts.                              |


Events:

| Name             | Type                                            | Description                                               |
| ---------------- | ----------------------------------------------- | --------------------------------------------------------- |
| `onBeforeEnter`  | `(el: Element) => void`                         | Function called before the entering transition starts. (before enter classes are applied) |
| `onEnter`        | `(el: Element, done: () => void) => void`       | Function called when the entering transition starts. (after `enterToClass` class is applied) |
| `onAfterEnter`   | `(el: Element) => void`                         | Function called after the entering transition ends. (after all enter classes are removed) |
| `onBeforeExit`   | `(el: Element) => void`                         | Function called before the leaving transition starts. (before exit classes are applied) |
| `onExit`         | `(el: Element, done: () => void) => void`       | Function called when the leaving transition starts. (after `exitToClass` class is applied) |
| `onAfterExit`    | `(el: Element) => void`                         | Function called after the leaving transition ends. (after all enter classes are removed) |


Usage:
`<Transition>` serve as transition effects for single element/component. The `<Transition>` only applies the transition behavior to the wrapped content inside; it doesn't render an extra DOM element, or show up in the inspected component hierarchy.

```jsx
// simple CSS animation
<Transition name="slide-fade">{show() && <div>Hello</div>}</Transition>

// JS Animation
<Transition
  onEnter={(el, done) => {
    const a = el.animate([{ opacity: 0 }, { opacity: 1 }], {
      duration: 600
    });
    a.finished.then(done);
  }}
  onExit={(el, done) => {
    const a = el.animate([{ opacity: 1 }, { opacity: 0 }], {
      duration: 600
    });
    a.finished.then(done);
  }}
>
  {show() && <div>Hello</div>}
</Transition>
```

# TransitionGroup

Props:

- moveClass - overwrite CSS class applied during moving transition.
- exposes the same props as `<Transition>` except `mode`.

Events:

- exposes the same events as `<Transition>`.

Usage:
`<TransitionGroup>` serve as transition effects for multiple elements/components.

`<TransitionGroup>` supports moving transitions via CSS transform. When a child's position on screen has changed after an update, it will get applied a moving CSS class (auto generated from the name attribute or configured with the move-class attribute). If the CSS transform property is "transition-able" when the moving class is applied, the element will be smoothly animated to its destination using the FLIP technique.

```jsx
<ul>
  <TransitionGroup name="slide">
    <For each={state.items}>{item => <li>{item.text}</li>}</For>
  </TransitionGroup>
</ul>
```
