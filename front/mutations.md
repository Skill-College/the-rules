# Mutations

## What are mutations?

Mutations means changes made to the Database through APIs. This includes all POST, PUT, DELETE and even GET APIs that make changes to the database.

Examples:

1. Submitting a form.
2. Activating / Deactivating something.

and many more...

## What we did till now

Till now we created React Server Actions and used them with forms and in client components for performing mutations.

## What we are changing?

From now onwards, we will only use `useMutation` (and other related) hooks from TanStack Query library for all mutations. We will not create any React Server Actions from now on.

_If there is a hard requirement to create React Server Action, then it should be well discussed before implementing._

## Conventions to follow with `useMutation` hook

- `useMutation` hook must be defined in it's own custom hook in a separate file.
- `useMutation` hook must use `clientApiFetch` function only for making the API calls.
- Types of request body or response must **NOT** be provided to the `clientApiFetch` function explicitly. The function has all the types defined by default.
- The mutation function of the `useMutation` hook should not get `FormData` as its parameters. It should only get the data required by the API as plain object or any other type. Extracting data from `FormData` object should be done explicitly and `useMutation` hook must now know how the data is getting transformed. We have created generic helper types like `ExtractRequestBodyType` which we can use to get the request body type of the API as parameter of the mutation function.

## Why we are making this decision?

There are a few reasons. We are listing them in order of importance.

1. When using React Server Action, it makes unwanted requests to the Frontend hosting server whenever there is some mutation request that we have to send to API server. With `useMutation` hook, we can send mutation requests directly from client side to the API server without including the Frontend server in the roundtrip.
2. Making requests directly from client, reduces the resource consumption of Frontend server. Which will help reduce the server costs.
3. We are already using TanStack Query for data fetching, so we can easily use its features with `useMutation` hook.

## Drawbacks

- When using `useMutation` with forms, we have to set the value of `disabled` prop of submit button to `disabled={isPending}` manually. We don't have this state handled automatically inside the `<AdminBtn>` component. We will update this in future so that we don't have to handle this manually.
