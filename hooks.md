# hooks

## [Only Call Hooks at the Top Level](https://reactjs.org/docs/hooks-rules.html#only-call-hooks-at-the-top-level)

Order should be deterministic. The mechanism is somewhat like `autorun`

```jsx
let state = []
let setters = []
let firstRun = true
let cursor = 0

const createSetter = (cursor) =>
  (newState) => states[cursor] = newState

let isRegistered = false
let cursor = 0
const states = []
const setters = []

const setState = (initialState) => {
  if (isRegistered) {
    states.push(initialState)
    setters.push(createSetter(cursor))
  }

  const state = states[cursor]
  const setter = setters[cursor]
  cursor += 1

  return [state, setter]
}

const App = () => {
  const [count, setCount] = useState(0)

  return (
    <button onClick={() => {setCount(count + 1)}}>
      {count}
    </button>
  )
}

const render = (Comp) => {
  isRegistered = false
  cursor = 0
  reactInternalRender(Comp)
}

const update = (Comp) => {
  isRegistered = true
  cursor = 0
  reactInternalUpdate(Comp)
}
```

## Life cycle methods

```jsx
const useMount = fn => {
  useEffect(() => {fn()}, [])
}

const useUnmount = fn => {
  useEffect(() => fn, [])
}

const useUpdate = fn => {
  const mounting = useRef(true)

  useEffect(() => {
    if (mounting.current) {
      mounting.current = false
    } else {
      fn()
    }
  })
}
```
