# Conventions for using TanStack Query in Skill.College project

## Custom Hook

We will always create a custom hook when we want to fetch data using `useQuery` or any other hook from TanStack query. Directly fetching data inside a component is not recommended.

## Naming Related

### 1. Always name the data variable appropriately

❌ NOT TO DO

```TSX
const { data } = useData()

return data.map(d => <p key={d.code}>{d.name}</p>)
```

✅ TO DO

```TSX
const { data: hubs } = useHubs()

return hubs.map(hub => <p key={hub.code}>{hub.name}</p>)
```

### 2. If creating custom hook the name it appropriately

❌ NOT TO DO

```TSX
export function useData() {
  return useQuery(...)
}
```

✅ TO DO

```TSX
export function useCourses() {
  return useQuery(...)
}
```

## Query Key

Alwas create a `const` for the query key and export it from that file so that we can use it in other components.

❌ NOT TO DO

```TSX
export function useEmployees() {
  return useQuery({
    queryKey: ['EMPLOYEES_LIST']
  })
}
```

✅ TO DO

```TSX
export const EMPLOYEES_QUERY_KEY = 'EMPLOYEES_LIST'

export function useEmployees() {
  return useQuery({
    queryKey: [EMPLOYEES_QUERY_KEY],
    ...
  })
}
```

When using filters in API, we can still export the static part of the query key.

For example,

```TSX
export const EMPLOYEES_QUERY_KEY = 'EMPLOYEES_LIST'

export function useEmployees() {
  const searchParams = useSearchParams()

  return useQuery({
    queryKey: [EMPLOYEES_QUERY_KEY, searchParams],
    ...
  })
}
```

Then when we want to invalidate the query of perform other actions to the query we use the exported constant. And we can get other values of query key from global state like search parameters or state inside context (if any).

## Handling loading and error states

Always handle `isPending` and `isError` states which are returned from TanStack Query hooks. If the same custom hook for data fetching, is rendered multiple times at once (i.e. it is used multiple times in the same page), then at least one of them should handle the loading and error states, if all of them doesn't handle those states.
