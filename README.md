# Project Information

GitHub Link: https://github.com/Pha1ae/week12_starter_project

Framework: React + Vite

Introduction: This project introduces the usage of `useEffect` and `useRef` through four labs: an automatic timer, timer controls, input focus control, and timer management using Ref.

# Installation

Node.js 20.19+ or 22.12+ is recommended. Run the following commands in the project directory:

```bash
npm install
npm run dev
```

Open the Local URL shown in the terminal and switch between the LAB tabs. Run `npm run build` before submission.

# Source Code Analysis

## What is the role of App.jsx, and what are the key LAB components?

`App.jsx` uses `useState` to store the currently selected LAB and renders the corresponding component based on the value of `active`. When switching between LABs, the previous component is unmounted, triggering its Cleanup.

- LAB 1 uses `useEffect` to create an automatic timer.
- LAB 2 uses `useState` and `useEffect` to implement Start, Stop, and Reset controls.
- LAB 3 uses `useRef` to access a DOM element and implement input focus and clearing.
- LAB 4 uses `useRef` to store the Timer ID and control the timer through event handlers.

## How do Effect, Dependency Array, and Cleanup work?

An Effect is used to handle side effects, such as timers and event listeners. The Dependency Array controls when the Effect runs again. Cleanup removes resources created by the previous Effect when the component unmounts or before the Effect runs again due to dependency changes.

## What is the difference between the `.current` property and `useState`?

Updating a value managed by `useState` usually triggers a re-render, making it suitable for managing data displayed in the UI.

Modifying `useRef.current` does not trigger a re-render, making it suitable for storing Timer IDs, DOM references, and similar values.

## Key Code Analysis

### Key `useEffect` Code (LAB 2)

```js
useEffect(() => {
  if (!running) return;

  const timer = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, [running]);
```

When `running` is `true`, `useEffect` creates a timer that increases `count` by 1 every second. When `running` changes to `false`, Cleanup clears the existing timer.

The Dependency Array `[running]` ensures that the Effect runs according to changes in the running state.

### Key `useRef` Code (LAB 4)

```js
const timerRef = useRef(null);

const start = () => {
  if (timerRef.current !== null) return;

  timerRef.current = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);

  setRunning(true);
};
```

`useRef` stores the Timer ID in `timerRef.current`. Before starting the timer, the function checks whether a timer already exists to prevent duplicate intervals.

The timer updates `count` every second, while changes to the Timer ID do not trigger a re-render.

When stopping the timer, `clearInterval(timerRef.current)` is used to clear the interval.

## Comparison of the Four LABs

### Why does useEffect need a Dependency Array?

The Dependency Array helps prevent unnecessary repeated execution.

For example, LAB 2 uses `[running]`, so the timer is created or cleared only when the running state changes, instead of being recreated every time `count` updates.

### What problems can occur without Cleanup?

Without Cleanup, problems such as duplicate timers, unnecessary memory and resource usage, repeated event listeners, and asynchronous operations continuing after a component is unmounted may occur.

### What is the most important difference between useState and useRef?

Updating `useState` usually triggers a component re-render, while modifying `useRef.current` does not trigger a re-render.

### How do LAB 2 and LAB 4 manage timers differently?

LAB 2 uses the `running` state to control `useEffect`. The Effect creates the timer, and Cleanup clears it.

LAB 4 uses `useRef` to store the Timer ID and directly manages the timer in event handlers such as Start and Stop. `useEffect` is mainly responsible for Cleanup when the component unmounts.

### Customizing

Modified file: `src/components/Lab2_TimerControl.jsx`

Modified code:

```js
const minutes = String(Math.floor(count / 60)).padStart(2, '0');
const seconds = String(count % 60).padStart(2, '0');

return (
  <div className="count">{minutes}:{seconds}</div>
);
```

Changed the timer display to the `mm:ss` format to show both minutes and seconds.

## Deployment

Vercel: https://week12-starter-project-blush.vercel.app/

## Weekly Review

### Three Things I Learned

1. Functional state updates are very useful because they can prevent problems such as reading stale state values.
2. How to use `useState` to manage component state and `useEffect` to manage side effects.
3. How `useRef` works and how it can store mutable values without triggering unnecessary re-renders.

### Difficulties and Solutions

**Difficulty:** My incomplete understanding of Hooks often caused problems in my code.

**Solution:** I reviewed the working principles of each Hook and learned how they are used in a program, which helped me write correct code.

### AI Usage

I used AI to help me understand concepts, working principles, and the original source code when I encountered difficulties. AI was also helpful during debugging.