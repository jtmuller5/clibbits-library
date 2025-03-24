# Query Invalidation

Waiting for queries to become stale before they are fetched again doesn't always work, especially when you know for a fact that a query's data is out of date because of something the user has done. For that purpose, the QueryClient has an invalidateQueries method that lets you intelligently mark queries as stale and potentially refetch them too!

```tsx
// Invalidate every query in the cache
queryClient.invalidateQueries()
// Invalidate every query with a key that starts with `todos`
queryClient.invalidateQueries({ queryKey: ['todos'] })
```

Note: Where other libraries that use normalized caches would attempt to update local queries with the new data either imperatively or via schema inference, TanStack Query gives you the tools to avoid the manual labor that comes with maintaining normalized caches and instead prescribes targeted invalidation, background-refetching and ultimately atomic updates.

When a query is invalidated with invalidateQueries, two things happen:

1. It is marked as stale. This stale state overrides any staleTime configurations being used in useQuery or related hooks
2. If the query is currently being rendered via useQuery or related hooks, it will also be refetched in the background

## Query Matching with invalidateQueries

When using APIs like invalidateQueries and removeQueries (and others that support partial query matching), you can match multiple queries by their prefix, or get really specific and match an exact query. For information on the types of filters you can use, please see Query Filters.

In this example, we can use the todos prefix to invalidate any queries that start with todos in their query key:

```tsx
import { useQuery, useQueryClient } from '@tanstack/react-query'

// Get QueryClient from the context
const queryClient = useQueryClient()

queryClient.invalidateQueries({ queryKey: ['todos'] })

// Both queries below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
const todoListQuery = useQuery({
  queryKey: ['todos', { page: 1 }],
  queryFn: fetchTodoList,
})
```

You can even invalidate queries with specific variables by passing a more specific query key to the invalidateQueries method:

```tsx
queryClient.invalidateQueries({
  queryKey: ['todos', { type: 'done' }],
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { type: 'done' }],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
```

The invalidateQueries API is very flexible, so even if you want to only invalidate todos queries that don't have any more variables or subkeys, you can pass an exact: true option to the invalidateQueries method:

```tsx
queryClient.invalidateQueries({
  queryKey: ['todos'],
  exact: true,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { type: 'done' }],
  queryFn: fetchTodoList,
})
```

If you find yourself wanting even more granularity, you can pass a predicate function to the invalidateQueries method. This function will receive each Query instance from the query cache and allow you to return true or false for whether you want to invalidate that query:

```tsx
queryClient.invalidateQueries({
  predicate: (query) =>
    query.queryKey[0] === 'todos' && query.queryKey[1]?.version >= 10,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 20 }],
  queryFn: fetchTodoList,
})

// The query below will be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 10 }],
  queryFn: fetchTodoList,
})

// However, the following query below will NOT be invalidated
const todoListQuery = useQuery({
  queryKey: ['todos', { version: 5 }],
  queryFn: fetchTodoList,
})
```

# Query Keys

At its core, TanStack Query manages query caching for you based on query keys. Query keys have to be an Array at the top level, and can be as simple as an Array with a single string, or as complex as an array of many strings and nested objects. As long as the query key is serializable, and **unique to the query's data**, you can use it!

## Simple Query Keys

The simplest form of a key is an array with constants values. This format is useful for:
* Generic List/Index resources
* Non-hierarchical resources

```tsx
// A list of todos
useQuery({ queryKey: ['todos'], ... })

// Something else, whatever!
useQuery({ queryKey: ['something', 'special'], ... })
```

## Array Keys with variables

When a query needs more information to uniquely describe its data, you can use an array with a string and any number of serializable objects to describe it. This is useful for:
* Hierarchical or nested resources
   * It's common to pass an ID, index, or other primitive to uniquely identify the item
* Queries with additional parameters
   * It's common to pass an object of additional options

```tsx
// An individual todo
useQuery({ queryKey: ['todo', 5], ... })

// An individual todo in a "preview" format
useQuery({ queryKey: ['todo', 5, { preview: true }], ...})

// A list of todos that are "done"
useQuery({ queryKey: ['todos', { type: 'done' }], ... })
```

## Query Keys are hashed deterministically!

This means that no matter the order of keys in objects, all of the following queries are considered equal:

```tsx
useQuery({ queryKey: ['todos', { status, page }], ... })
useQuery({ queryKey: ['todos', { page, status }], ...})
useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })
```

The following query keys, however, are not equal. Array item order matters!

```tsx
useQuery({ queryKey: ['todos', status, page], ... })
useQuery({ queryKey: ['todos', page, status], ...})
useQuery({ queryKey: ['todos', undefined, page, status], ...})
```

## If your query function depends on a variable, include it in your query key

Since query keys uniquely describe the data they are fetching, they should include any variables you use in your query function that **change**. For example:

```tsx
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),
  })
}
```

Note that query keys act as dependencies for your query functions. Adding dependent variables to your query key will ensure that queries are cached independently, and that any time a variable changes, *queries will be refetched automatically* (depending on your staleTime settings). See the **exhaustive-deps** section for more information and examples.

## Further reading

For tips on organizing Query Keys in larger applications, have a look at **Effective React Query Keys** and check the **Query Key Factory Package** from the Community Resources.

# Query Functions

A query function can be literally any function that **returns a promise**. The promise that is returned should either **resolve the data** or **throw an error**.

All of the following are valid query function configurations:

```tsx
useQuery({ queryKey: ['todos'], queryFn: fetchAllTodos })
useQuery({ queryKey: ['todos', todoId], queryFn: () => fetchTodoById(todoId) })
useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    const data = await fetchTodoById(todoId)
    return data
  },
})
useQuery({
  queryKey: ['todos', todoId],
  queryFn: ({ queryKey }) => fetchTodoById(queryKey[1]),
})
```

## Handling and Throwing Errors

For TanStack Query to determine a query has errored, the query function **must throw** or return a **rejected Promise**. Any error that is thrown in the query function will be persisted on the error state of the query.

```tsx
const { error } = useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    if (somethingGoesWrong) {
      throw new Error('Oh no!')
    }
    if (somethingElseGoesWrong) {
      return Promise.reject(new Error('Oh no!'))
    }

    return data
  },
})
```

## Usage with fetch and other clients that do not throw by default

While most utilities like axios or graphql-request automatically throw errors for unsuccessful HTTP calls, some utilities like fetch do not throw errors by default. If that's the case, you'll need to throw them on your own. Here is a simple way to do that with the popular fetch API:

```tsx
useQuery({
  queryKey: ['todos', todoId],
  queryFn: async () => {
    const response = await fetch('/todos/' + todoId)
    if (!response.ok) {
      throw new Error('Network response was not ok')
    }
    return response.json()
  },
})
```

## Query Function Variables

Query keys are not just for uniquely identifying the data you are fetching, but are also conveniently passed into your query function as part of the QueryFunctionContext. While not always necessary, this makes it possible to extract your query functions if needed:

```tsx
function Todos({ status, page }) {
  const result = useQuery({
    queryKey: ['todos', { status, page }],
    queryFn: fetchTodoList,
  })
}

// Access the key, status and page variables in your query function!
function fetchTodoList({ queryKey }) {
  const [_key, { status, page }] = queryKey
  return new Promise()
}
```

## QueryFunctionContext

The QueryFunctionContext is the object passed to each query function. It consists of:

* **queryKey**: QueryKey: [**Query Keys**]
* **client**: QueryClient: [**QueryClient**]
* **signal?**: AbortSignal
   * **AbortSignal** instance provided by TanStack Query
   * Can be used for **Query Cancellation**
* **meta**: Record<string, unknown> | undefined
   * an optional field you can fill with additional information about your query

Additionally, **Infinite Queries** get the following options passed:

* **pageParam**: TPageParam
   * the page parameter used to fetch the current page
* **direction**: 'forward' | 'backward'
   * **deprecated**
   * the direction of the current page fetch
   * To get access to the direction of the current page fetch, please add a direction to pageParam from getNextPageParam and getPreviousPageParam.

   # Query Options

One of the best ways to share queryKey and queryFn between multiple places, yet keep them co-located to one another, is to use the queryOptions helper. At runtime, this helper just returns whatever you pass into it, but it has a lot of advantages when using it **with TypeScript**. You can define all possible options for a query in one place, and you'll also get type inference and type safety for all of them.

```ts
import { queryOptions } from '@tanstack/react-query'

function groupOptions(id: number) {
  return queryOptions({
    queryKey: ['groups', id],
    queryFn: () => fetchGroups(id),
    staleTime: 5 * 1000,
  })
}

// usage:

useQuery(groupOptions(1))
useSuspenseQuery(groupOptions(5))
useQueries({
  queries: [groupOptions(1), groupOptions(2)],
})
queryClient.prefetchQuery(groupOptions(23))
queryClient.setQueryData(groupOptions(42).queryKey, newGroups)
```

For Infinite Queries, a separate **infiniteQueryOptions** helper is available.

You can still override some options at the component level. A very common and useful pattern is to create per-component **select** functions:

```ts
// Type inference still works, so query.data will be the return type of select instead of queryFn

const query = useQuery({
  ...groupOptions(1),
  select: (data) => data.groupName,
})
```

# Paginated / Lagged Queries

Rendering paginated data is a very common UI pattern and in TanStack Query, it "just works" by including the page information in the query key:

```tsx
const result = useQuery({
  queryKey: ['projects', page],
  queryFn: fetchProjects,
})
```

However, if you run this simple example, you might notice something strange:
**The UI jumps in and out of the success and pending states because each new page is treated like a brand new query.**

This experience is not optimal and unfortunately is how many tools today insist on working. But not TanStack Query! As you may have guessed, TanStack Query comes with an awesome feature called placeholderData that allows us to get around this.

## Better Paginated Queries with placeholderData

Consider the following example where we would ideally want to increment a pageIndex (or cursor) for a query. If we were to use useQuery, **it would still technically work fine**, but the UI would jump in and out of the success and pending states as different queries are created and destroyed for each page or cursor. By setting placeholderData to (previousData) => previousData or keepPreviousData function exported from TanStack Query, we get a few new things:

* **The data from the last successful fetch is available while new data is being requested, even though the query key has changed**.
* When the new data arrives, the previous data is seamlessly swapped to show the new data.
* isPlaceholderData is made available to know what data the query is currently providing you

```tsx
import { keepPreviousData, useQuery } from '@tanstack/react-query'
import React from 'react'

function Todos() {
  const [page, setPage] = React.useState(0)

  const fetchProjects = (page = 0) =>
    fetch('/api/projects?page=' + page).then((res) => res.json())

  const { isPending, isError, error, data, isFetching, isPlaceholderData } =
    useQuery({
      queryKey: ['projects', page],
      queryFn: () => fetchProjects(page),
      placeholderData: keepPreviousData,
    })

  return (
    <div>
      {isPending ? (
        <div>Loading...</div>
      ) : isError ? (
        <div>Error: {error.message}</div>
      ) : (
        <div>
          {data.projects.map((project) => (
            <p key={project.id}>{project.name}</p>
          ))}
        </div>
      )}
      <span>Current Page: {page + 1}</span>
      <button
        onClick={() => setPage((old) => Math.max(old - 1, 0))}
        disabled={page === 0}
      >
        Previous Page
      </button>
      <button
        onClick={() => {
          if (!isPlaceholderData && data.hasMore) {
            setPage((old) => old + 1)
          }
        }}
        // Disable the Next Page button until we know a next page is available
        disabled={isPlaceholderData || !data?.hasMore}
      >
        Next Page
      </button>
      {isFetching ? <span> Loading...</span> : null}
    </div>
  )
}
```

## Lagging Infinite Query results with placeholderData

While not as common, the placeholderData option also works flawlessly with the useInfiniteQuery hook, so you can seamlessly allow your users to continue to see cached data while infinite query keys change over time.

# Query Retries

When a useQuery query fails (the query function throws an error), TanStack Query will automatically retry the query if that query's request has not reached the max number of consecutive retries (defaults to 3) or a function is provided to determine if a retry is allowed.

You can configure retries both on a global level and an individual query level.

* Setting retry = false will disable retries.
* Setting retry = 6 will retry failing requests 6 times before showing the final error thrown by the function.
* Setting retry = true will infinitely retry failing requests.
* Setting retry = (failureCount, error) => ... allows for custom logic based on why the request failed.

***On the server, retries default to 0 to make server rendering as fast as possible.***

```tsx
import { useQuery } from '@tanstack/react-query'

// Make a specific query retry a certain number of times
const result = useQuery({
  queryKey: ['todos', 1],
  queryFn: fetchTodoListPage,
  retry: 10, // Will retry failed requests 10 times before displaying an error
})
```

***Info: Contents of the error property will be part of failureReason response property of useQuery until the last retry attempt. So in above example any error contents will be part of failureReason property for first 9 retry attempts (Overall 10 attempts) and finally they will be part of error after last attempt if error persists after all retry attempts.***

## Retry Delay

By default, retries in TanStack Query do not happen immediately after a request fails. As is standard, a back-off delay is gradually applied to each retry attempt.

The default retryDelay is set to double (starting at 1000ms) with each attempt, but not exceed 30 seconds:

```tsx
// Configure for all queries
import {
  QueryCache,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
    },
  },
})

function App() {
  return <QueryClientProvider client={queryClient}>...</QueryClientProvider>
}
```

Though it is not recommended, you can obviously override the retryDelay function/integer in both the Provider and individual query options. If set to an integer instead of a function the delay will always be the same amount of time:

```tsx
const result = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
  retryDelay: 1000, // Will always wait 1000ms to retry, regardless of how many retries
})
```

# Infinite Queries

Rendering lists that can additively "load more" data onto an existing set of data or "infinite scroll" is also a very common UI pattern. TanStack Query supports a useful version of useQuery called useInfiniteQuery for querying these types of lists.

When using useInfiniteQuery, you'll notice a few things are different:

- **data** is now an object containing infinite query data:
  - **data.pages** array containing the fetched pages
  - **data.pageParams** array containing the page params used to fetch the pages
- The **fetchNextPage** and **fetchPreviousPage** functions are now available (fetchNextPage is required)
- The **initialPageParam** option is now available (and required) to specify the initial page param
- The **getNextPageParam** and **getPreviousPageParam** options are available for both determining if there is more data to load and the information to fetch it. This information is supplied as an additional parameter in the query function
- A **hasNextPage** boolean is now available and is true if getNextPageParam returns a value other than null or undefined
- A **hasPreviousPage** boolean is now available and is true if getPreviousPageParam returns a value other than null or undefined
- The **isFetchingNextPage** and **isFetchingPreviousPage** booleans are now available to distinguish between a background refresh state and a loading more state

Note: Options initialData or placeholderData need to conform to the same structure of an object with data.pages and data.pageParams properties.

## Example

Let's assume we have an API that returns pages of projects 3 at a time based on a cursor index along with a cursor that can be used to fetch the next group of projects:

```tsx
fetch('/api/projects?cursor=0')
// { data: [...], nextCursor: 3}
fetch('/api/projects?cursor=3')
// { data: [...], nextCursor: 6}
fetch('/api/projects?cursor=6')
// { data: [...], nextCursor: 9}
fetch('/api/projects?cursor=9')
// { data: [...] }
```

With this information, we can create a "Load More" UI by:

1. Waiting for useInfiniteQuery to request the first group of data by default
2. Returning the information for the next query in getNextPageParam
3. Calling fetchNextPage function

```tsx
import { useInfiniteQuery } from '@tanstack/react-query'

function Projects() {
  const fetchProjects = async ({ pageParam }) => {
    const res = await fetch('/api/projects?cursor=' + pageParam)
    return res.json()
  }

  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery({
    queryKey: ['projects'],
    queryFn: fetchProjects,
    initialPageParam: 0,
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  })

  return status === 'pending' ? (
    <p>Loading...</p>
  ) : status === 'error' ? (
    <p>Error: {error.message}</p>
  ) : (
    <>
      {data.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.data.map((project) => (
            <p key={project.id}>{project.name}</p>
          ))}
        </React.Fragment>
      ))}
      <div>
        <button
          onClick={() => fetchNextPage()}
          disabled={!hasNextPage || isFetchingNextPage}
        >
          {isFetchingNextPage
            ? 'Loading more...'
            : hasNextPage
              ? 'Load More'
              : 'Nothing more to load'}
        </button>
      </div>
      <div>{isFetching && !isFetchingNextPage ? 'Fetching...' : null}</div>
    </>
  )
}
```

It's essential to understand that calling fetchNextPage while an ongoing fetch is in progress runs the risk of overwriting data refreshes happening in the background. This situation becomes particularly critical when rendering a list and triggering fetchNextPage simultaneously.

Remember, there can only be a single ongoing fetch for an InfiniteQuery. A single cache entry is shared for all pages, attempting to fetch twice simultaneously might lead to data overwrites.

If you intend to enable simultaneous fetching, you can utilize the `{ cancelRefetch: false }` option (default: true) within fetchNextPage.

To ensure a seamless querying process without conflicts, it's highly recommended to verify that the query is not in an isFetching state, especially if the user won't directly control that call.

```jsx
<List onEndReached={() => !isFetchingNextPage && fetchNextPage()} />
```

## What happens when an infinite query needs to be refetched?

When an infinite query becomes stale and needs to be refetched, each group is fetched sequentially, starting from the first one. This ensures that even if the underlying data is mutated, we're not using stale cursors and potentially getting duplicates or skipping records. If an infinite query's results are ever removed from the queryCache, the pagination restarts at the initial state with only the initial group being requested.

## What if I want to implement a bi-directional infinite list?

Bi-directional lists can be implemented by using the `getPreviousPageParam`, `fetchPreviousPage`, `hasPreviousPage` and `isFetchingPreviousPage` properties and functions.

```tsx
useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  initialPageParam: 0,
  getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  getPreviousPageParam: (firstPage, pages) => firstPage.prevCursor,
})
```

## What if I want to show the pages in reversed order?

Sometimes you may want to show the pages in reversed order. If this is case, you can use the select option:

```tsx
useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  select: (data) => ({
    pages: [...data.pages].reverse(),
    pageParams: [...data.pageParams].reverse(),
  }),
})
```

## What if I want to manually update the infinite query?

### Manually removing first page:

```tsx
queryClient.setQueryData(['projects'], (data) => ({
  pages: data.pages.slice(1),
  pageParams: data.pageParams.slice(1),
}))
```

### Manually removing a single value from an individual page:

```tsx
const newPagesArray =
  oldPagesArray?.pages.map((page) =>
    page.filter((val) => val.id !== updatedId),
  ) ?? []

queryClient.setQueryData(['projects'], (data) => ({
  pages: newPagesArray,
  pageParams: data.pageParams,
}))
```

### Keep only the first page:

```tsx
queryClient.setQueryData(['projects'], (data) => ({
  pages: data.pages.slice(0, 1),
  pageParams: data.pageParams.slice(0, 1),
}))
```

Make sure to always keep the same data structure of pages and pageParams!

## What if I want to limit the number of pages?

In some use cases you may want to limit the number of pages stored in the query data to improve the performance and UX:

- when the user can load a large number of pages (memory usage)
- when you have to refetch an infinite query that contains dozens of pages (network usage: all the pages are sequentially fetched)

The solution is to use a "Limited Infinite Query". This is made possible by using the `maxPages` option in conjunction with `getNextPageParam` and `getPreviousPageParam` to allow fetching pages when needed in both directions.

In the following example only 3 pages are kept in the query data pages array. If a refetch is needed, only 3 pages will be refetched sequentially.

```tsx
useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  initialPageParam: 0,
  getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  getPreviousPageParam: (firstPage, pages) => firstPage.prevCursor,
  maxPages: 3,
})
```

## What if my API doesn't return a cursor?

If your API doesn't return a cursor, you can use the pageParam as a cursor. Because `getNextPageParam` and `getPreviousPageParam` also get the pageParam of the current page, you can use it to calculate the next / previous page param.

```tsx
return useInfiniteQuery({
  queryKey: ['projects'],
  queryFn: fetchProjects,
  initialPageParam: 0,
  getNextPageParam: (lastPage, allPages, lastPageParam) => {
    if (lastPage.length === 0) {
      return undefined
    }
    return lastPageParam + 1
  },
  getPreviousPageParam: (firstPage, allPages, firstPageParam) => {
    if (firstPageParam <= 1) {
      return undefined
    }
    return firstPageParam - 1
  },
})
```

# Mutations

Unlike queries, mutations are typically used to create/update/delete data or perform server side-effects. For this purpose, TanStack Query exports a useMutation hook.

Here's an example of a mutation that adds a new todo to the server:

```tsx
function App() {
  const mutation = useMutation({
    mutationFn: (newTodo) => {
      return axios.post('/todos', newTodo)
    },
  })

  return (
    <div>
      {mutation.isPending ? (
        'Adding todo...'
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}

          {mutation.isSuccess ? <div>Todo added!</div> : null}

          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: 'Do Laundry' })
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  )
}
```

A mutation can only be in one of the following states at any given moment:

- `isIdle` or `status === 'idle'` - The mutation is currently idle or in a fresh/reset state
- `isPending` or `status === 'pending'` - The mutation is currently running
- `isError` or `status === 'error'` - The mutation encountered an error
- `isSuccess` or `status === 'success'` - The mutation was successful and mutation data is available

Beyond those primary states, more information is available depending on the state of the mutation:

- `error` - If the mutation is in an error state, the error is available via the error property.
- `data` - If the mutation is in a success state, the data is available via the data property.

In the example above, you also saw that you can pass variables to your mutations function by calling the mutate function with a single variable or object.

Even with just variables, mutations aren't all that special, but when used with the `onSuccess` option, the Query Client's `invalidateQueries` method and the Query Client's `setQueryData` method, mutations become a very powerful tool.

> **IMPORTANT**: The mutate function is an asynchronous function, which means you cannot use it directly in an event callback in React 16 and earlier. If you need to access the event in onSubmit you need to wrap mutate in another function. This is due to React event pooling.

```tsx
// This will not work in React 16 and earlier
const CreateTodo = () => {
  const mutation = useMutation({
    mutationFn: (event) => {
      event.preventDefault()
      return fetch('/api', new FormData(event.target))
    },
  })

  return <form onSubmit={mutation.mutate}>...</form>
}

// This will work
const CreateTodo = () => {
  const mutation = useMutation({
    mutationFn: (formData) => {
      return fetch('/api', formData)
    },
  })
  const onSubmit = (event) => {
    event.preventDefault()
    mutation.mutate(new FormData(event.target))
  }

  return <form onSubmit={onSubmit}>...</form>
}
```

## Resetting Mutation State

It's sometimes the case that you need to clear the error or data of a mutation request. To do this, you can use the reset function to handle this:

```tsx
const CreateTodo = () => {
  const [title, setTitle] = useState('')
  const mutation = useMutation({ mutationFn: createTodo })

  const onCreateTodo = (e) => {
    e.preventDefault()
    mutation.mutate({ title })
  }

  return (
    <form onSubmit={onCreateTodo}>
      {mutation.error && (
        <h5 onClick={() => mutation.reset()}>{mutation.error}</h5>
      )}
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <br />
      <button type="submit">Create Todo</button>
    </form>
  )
}
```

## Mutation Side Effects

`useMutation` comes with some helper options that allow quick and easy side-effects at any stage during the mutation lifecycle. These come in handy for both invalidating and refetching queries after mutations and even optimistic updates

```tsx
useMutation({
  mutationFn: addTodo,
  onMutate: (variables) => {
    // A mutation is about to happen!

    // Optionally return a context containing data to use when for example rolling back
    return { id: 1 }
  },
  onError: (error, variables, context) => {
    // An error happened!
    console.log(`rolling back optimistic update with id ${context.id}`)
  },
  onSuccess: (data, variables, context) => {
    // Boom baby!
  },
  onSettled: (data, error, variables, context) => {
    // Error or success... doesn't matter!
  },
})
```

When returning a promise in any of the callback functions it will first be awaited before the next callback is called:

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: async () => {
    console.log("I'm first!")
  },
  onSettled: async () => {
    console.log("I'm second!")
  },
})
```

You might find that you want to trigger additional callbacks beyond the ones defined on useMutation when calling mutate. This can be used to trigger component-specific side effects. To do that, you can provide any of the same callback options to the mutate function after your mutation variable. Supported options include: `onSuccess`, `onError` and `onSettled`. Please keep in mind that those additional callbacks won't run if your component unmounts before the mutation finishes.

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, variables, context) => {
    // I will fire first
  },
  onError: (error, variables, context) => {
    // I will fire first
  },
  onSettled: (data, error, variables, context) => {
    // I will fire first
  },
})

mutate(todo, {
  onSuccess: (data, variables, context) => {
    // I will fire second!
  },
  onError: (error, variables, context) => {
    // I will fire second!
  },
  onSettled: (data, error, variables, context) => {
    // I will fire second!
  },
})
```

## Consecutive mutations

There is a slight difference in handling `onSuccess`, `onError` and `onSettled` callbacks when it comes to consecutive mutations. When passed to the mutate function, they will be fired up only once and only if the component is still mounted. This is due to the fact that mutation observer is removed and resubscribed every time when the mutate function is called. On the contrary, useMutation handlers execute for each mutate call.

Be aware that most likely, `mutationFn` passed to useMutation is asynchronous. In that case, the order in which mutations are fulfilled may differ from the order of mutate function calls.

```tsx
useMutation({
  mutationFn: addTodo,
  onSuccess: (data, variables, context) => {
    // Will be called 3 times
  },
})

const todos = ['Todo 1', 'Todo 2', 'Todo 3']
todos.forEach((todo) => {
  mutate(todo, {
    onSuccess: (data, variables, context) => {
      // Will execute only once, for the last mutation (Todo 3),
      // regardless which mutation resolves first
    },
  })
})
```

## Promises

Use `mutateAsync` instead of `mutate` to get a promise which will resolve on success or throw on an error. This can for example be used to compose side effects.

```tsx
const mutation = useMutation({ mutationFn: addTodo })

try {
  const todo = await mutation.mutateAsync(todo)
  console.log(todo)
} catch (error) {
  console.error(error)
} finally {
  console.log('done')
}
```

## Retry

By default, TanStack Query will not retry a mutation on error, but it is possible with the retry option:

```tsx
const mutation = useMutation({
  mutationFn: addTodo,
  retry: 3,
})
```

If mutations fail because the device is offline, they will be retried in the same order when the device reconnects.

## Persist mutations

Mutations can be persisted to storage if needed and resumed at a later point. This can be done with the hydration functions:

```tsx
const queryClient = new QueryClient()

// Define the "addTodo" mutation
queryClient.setMutationDefaults(['addTodo'], {
  mutationFn: addTodo,
  onMutate: async (variables) => {
    // Cancel current queries for the todos list
    await queryClient.cancelQueries({ queryKey: ['todos'] })

    // Create optimistic todo
    const optimisticTodo = { id: uuid(), title: variables.title }

    // Add optimistic todo to todos list
    queryClient.setQueryData(['todos'], (old) => [...old, optimisticTodo])

    // Return context with the optimistic todo
    return { optimisticTodo }
  },
  onSuccess: (result, variables, context) => {
    // Replace optimistic todo in the todos list with the result
    queryClient.setQueryData(['todos'], (old) =>
      old.map((todo) =>
        todo.id === context.optimisticTodo.id ? result : todo,
      ),
    )
  },
  onError: (error, variables, context) => {
    // Remove optimistic todo from the todos list
    queryClient.setQueryData(['todos'], (old) =>
      old.filter((todo) => todo.id !== context.optimisticTodo.id),
    )
  },
  retry: 3,
})

// Start mutation in some component:
const mutation = useMutation({ mutationKey: ['addTodo'] })
mutation.mutate({ title: 'title' })

// If the mutation has been paused because the device is for example offline,
// Then the paused mutation can be dehydrated when the application quits:
const state = dehydrate(queryClient)

// The mutation can then be hydrated again when the application is started:
hydrate(queryClient, state)

// Resume the paused mutations:
queryClient.resumePausedMutations()
```

## Persisting Offline mutations

If you persist offline mutations with the `persistQueryClient` plugin, mutations cannot be resumed when the page is reloaded unless you provide a default mutation function.

This is a technical limitation. When persisting to an external storage, only the state of mutations is persisted, as functions cannot be serialized. After hydration, the component that triggers the mutation might not be mounted, so calling `resumePausedMutations` might yield an error: `No mutationFn found`.

```tsx
const persister = createSyncStoragePersister({
  storage: window.localStorage,
})
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      gcTime: 1000 * 60 * 60 * 24, // 24 hours
    },
  },
})

// we need a default mutation function so that paused mutations can resume after a page reload
queryClient.setMutationDefaults(['todos'], {
  mutationFn: ({ id, data }) => {
    return api.updateTodo(id, data)
  },
})

export default function App() {
  return (
    <PersistQueryClientProvider
      client={queryClient}
      persistOptions={{ persister }}
      onSuccess={() => {
        // resume mutations after initial restore from localStorage was successful
        queryClient.resumePausedMutations()
      }}
    >
      <RestOfTheApp />
    </PersistQueryClientProvider>
  )
}
```

We also have an extensive offline example that covers both queries and mutations.

## Mutation Scopes

Per default, all mutations run in parallel - even if you invoke `.mutate()` of the same mutation multiple times. Mutations can be given a scope with an id to avoid that. All mutations with the same `scope.id` will run in serial, which means when they are triggered, they will start in `isPaused: true` state if there is already a mutation for that scope in progress. They will be put into a queue and will automatically resume once their time in the queue has come.

```tsx
const mutation = useMutation({
  mutationFn: addTodo,
  scope: {
    id: 'todo',
  },
})
```

## Further reading

For more information about mutations, have a look at #12: Mastering Mutations in React Query from the Community Resources.

# Updates from Mutation Responses

When dealing with mutations that **update** objects on the server, it's common for the new object to be automatically returned in the response of the mutation. Instead of refetching any queries for that item and wasting a network call for data we already have, we can take advantage of the object returned by the mutation function and update the existing query with the new data immediately using the **Query Client's setQueryData** method:

```tsx
const queryClient = useQueryClient()

const mutation = useMutation({
  mutationFn: editTodo,
  onSuccess: (data) => {
    queryClient.setQueryData(['todo', { id: 5 }], data)
  },
})

mutation.mutate({
  id: 5,
  name: 'Do the laundry',
})

// The query below will be updated with the response from the
// successful mutation
const { status, data, error } = useQuery({
  queryKey: ['todo', { id: 5 }],
  queryFn: fetchTodoById,
})
```

You might want to tie the onSuccess logic into a reusable mutation, for that you can create a custom hook like this:

```tsx
const useMutateTodo = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: editTodo,
    // Notice the second argument is the variables object that the `mutate` function receives
    onSuccess: (data, variables) => {
      queryClient.setQueryData(['todo', { id: variables.id }], data)
    },
  })
}
```

## Immutability

Updates via setQueryData must be performed in an *immutable* way. **DO NOT** attempt to write directly to the cache by mutating data (that you retrieved from the cache) in place. It might work at first but can lead to subtle bugs along the way.

```tsx
queryClient.setQueryData(['posts', { id }], (oldData) => {
  if (oldData) {
    // ❌ do not try this
    oldData.title = 'my new post title'
  }
  return oldData
})

queryClient.setQueryData(
  ['posts', { id }],
  // ✅ this is the way
  (oldData) =>
    oldData
      ? {
          ...oldData,
          title: 'my new post title',
        }
      : oldData,
)
```

# Filters

Some methods within TanStack Query accept a QueryFilters or MutationFilters object.

## Query Filters

A query filter is an object with certain conditions to match a query with:

```tsx
// Cancel all queries
await queryClient.cancelQueries()

// Remove all inactive queries that begin with `posts` in the key
queryClient.removeQueries({ queryKey: ['posts'], type: 'inactive' })

// Refetch all active queries
await queryClient.refetchQueries({ type: 'active' })

// Refetch all active queries that begin with `posts` in the key
await queryClient.refetchQueries({ queryKey: ['posts'], type: 'active' })
```

A query filter object supports the following properties:

* **queryKey?: QueryKey**
   * Set this property to define a query key to match on.
* **exact?: boolean**
   * If you don't want to search queries inclusively by query key, you can pass the exact: true option to return only the query with the exact query key you have passed.
* **type?: 'active' | 'inactive' | 'all'**
   * Defaults to all
   * When set to active it will match active queries.
   * When set to inactive it will match inactive queries.
* **stale?: boolean**
   * When set to true it will match stale queries.
   * When set to false it will match fresh queries.
* **fetchStatus?: FetchStatus**
   * When set to fetching it will match queries that are currently fetching.
   * When set to paused it will match queries that wanted to fetch, but have been paused.
   * When set to idle it will match queries that are not fetching.
* **predicate?: (query: Query) => boolean**
   * This predicate function will be used as a final filter on all matching queries. If no other filters are specified, this function will be evaluated against every query in the cache.

## Mutation Filters

A mutation filter is an object with certain conditions to match a mutation with:

```tsx
// Get the number of all fetching mutations
await queryClient.isMutating()

// Filter mutations by mutationKey
await queryClient.isMutating({ mutationKey: ['post'] })

// Filter mutations using a predicate function
await queryClient.isMutating({
  predicate: (mutation) => mutation.state.variables?.id === 1,
})
```

A mutation filter object supports the following properties:

* **mutationKey?: MutationKey**
   * Set this property to define a mutation key to match on.
* **exact?: boolean**
   * If you don't want to search mutations inclusively by mutation key, you can pass the exact: true option to return only the mutation with the exact mutation key you have passed.
* **status?: MutationStatus**
   * Allows for filtering mutations according to their status.
* **predicate?: (mutation: Mutation) => boolean**
   * This predicate function will be used as a final filter on all matching mutations. If no other filters are specified, this function will be evaluated against every mutation in the cache.

## Utils

### matchQuery

```tsx
const isMatching = matchQuery(filters, query)
```

Returns a boolean that indicates whether a query matches the provided set of query filters.

### matchMutation

```tsx
const isMatching = matchMutation(filters, mutation)
```

Returns a boolean that indicates whether a mutation matches the provided set of mutation filters.

# Initial Query Data

There are many ways to supply initial data for a query to the cache before you need it:

**Declaratively:**
- Provide `initialData` to a query to prepopulate its cache if empty

**Imperatively:**
- Prefetch the data using `queryClient.prefetchQuery`
- Manually place the data into the cache using `queryClient.setQueryData`

## Using initialData to prepopulate a query

There may be times when you already have the initial data for a query available in your app and can simply provide it directly to your query. If and when this is the case, you can use the `config.initialData` option to set the initial data for a query and skip the initial loading state!

> **IMPORTANT**: `initialData` is persisted to the cache, so it is not recommended to provide placeholder, partial or incomplete data to this option and instead use `placeholderData`

```tsx
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
})
```

## staleTime and initialDataUpdatedAt

By default, `initialData` is treated as totally fresh, as if it were just fetched. This also means that it will affect how it is interpreted by the `staleTime` option.

If you configure your query observer with `initialData`, and no `staleTime` (the default `staleTime: 0`), the query will immediately refetch when it mounts:

```tsx
// Will show initialTodos immediately, but also immediately refetch todos after mount
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
})
```

If you configure your query observer with `initialData` and a `staleTime` of 1000 ms, the data will be considered fresh for that same amount of time, as if it was just fetched from your query function.

```tsx
// Show initialTodos immediately, but won't refetch until another interaction event is encountered after 1000 ms
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
  staleTime: 1000,
})
```

So what if your `initialData` isn't totally fresh? That leaves us with the last configuration that is actually the most accurate and uses an option called `initialDataUpdatedAt`. This option allows you to pass a numeric JS timestamp in milliseconds of when the `initialData` itself was last updated, e.g. what `Date.now()` provides. Take note that if you have a unix timestamp, you'll need to convert it to a JS timestamp by multiplying it by 1000.

```tsx
// Show initialTodos immediately, but won't refetch until another interaction event is encountered after 1000 ms
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: initialTodos,
  staleTime: 60 * 1000, // 1 minute
  // This could be 10 seconds ago or 10 minutes ago
  initialDataUpdatedAt: initialTodosUpdatedTimestamp, // eg. 1608412420052
})
```

This option allows the `staleTime` to be used for its original purpose, determining how fresh the data needs to be, while also allowing the data to be refetched on mount if the `initialData` is older than the `staleTime`. In the example above, our data needs to be fresh within 1 minute, and we can hint to the query when the `initialData` was last updated so the query can decide for itself whether the data needs to be refetched again or not.

If you would rather treat your data as prefetched data, we recommend that you use the `prefetchQuery` or `fetchQuery` APIs to populate the cache beforehand, thus letting you configure your `staleTime` independently from your `initialData`

## Initial Data Function

If the process for accessing a query's initial data is intensive or just not something you want to perform on every render, you can pass a function as the `initialData` value. This function will be executed only once when the query is initialized, saving you precious memory and/or CPU:

```tsx
const result = useQuery({
  queryKey: ['todos'],
  queryFn: () => fetch('/todos'),
  initialData: () => getExpensiveTodos(),
})
```

## Initial Data from Cache

In some circumstances, you may be able to provide the initial data for a query from the cached result of another. A good example of this would be searching the cached data from a todos list query for an individual todo item, then using that as the initial data for your individual todo query:

```tsx
const result = useQuery({
  queryKey: ['todo', todoId],
  queryFn: () => fetch('/todos'),
  initialData: () => {
    // Use a todo from the 'todos' query as the initial data for this todo query
    return queryClient.getQueryData(['todos'])?.find((d) => d.id === todoId)
  },
})
```

## Initial Data from the cache with initialDataUpdatedAt

Getting initial data from the cache means the source query you're using to look up the initial data from is likely old. Instead of using an artificial `staleTime` to keep your query from refetching immediately, it's suggested that you pass the source query's `dataUpdatedAt` to `initialDataUpdatedAt`. This provides the query instance with all the information it needs to determine if and when the query needs to be refetched, regardless of initial data being provided.

```tsx
const result = useQuery({
  queryKey: ['todos', todoId],
  queryFn: () => fetch(`/todos/${todoId}`),
  initialData: () =>
    queryClient.getQueryData(['todos'])?.find((d) => d.id === todoId),
  initialDataUpdatedAt: () =>
    queryClient.getQueryState(['todos'])?.dataUpdatedAt,
})
```

## Conditional Initial Data from Cache

If the source query you're using to look up the initial data from is old, you may not want to use the cached data at all and just fetch from the server. To make this decision easier, you can use the `queryClient.getQueryState` method instead to get more information about the source query, including a `state.dataUpdatedAt` timestamp you can use to decide if the query is "fresh" enough for your needs:

```tsx
const result = useQuery({
  queryKey: ['todo', todoId],
  queryFn: () => fetch(`/todos/${todoId}`),
  initialData: () => {
    // Get the query state
    const state = queryClient.getQueryState(['todos'])

    // If the query exists and has data that is no older than 10 seconds...
    if (state && Date.now() - state.dataUpdatedAt <= 10 * 1000) {
      // return the individual todo
      return state.data.find((d) => d.id === todoId)
    }

    // Otherwise, return undefined and let it fetch from a hard loading state!
  },
})
```

## Further reading

For a comparison between Initial Data and Placeholder Data, have a look at the Community Resources.

# Important Defaults

Out of the box, TanStack Query is configured with **aggressive but sane** defaults. **Sometimes these defaults can catch new users off guard or make learning/debugging difficult if they are unknown by the user.** Keep them in mind as you continue to learn and use TanStack Query:

* Query instances via useQuery or useInfiniteQuery by default **consider cached data as stale**.
  ***To change this behavior, you can configure your queries both globally and per-query using the staleTime option. Specifying a longer staleTime means queries will not refetch their data as often***

* Stale queries are refetched automatically in the background when:
   * New instances of the query mount
   * The window is refocused
   * The network is reconnected
   * The query is optionally configured with a refetch interval
   ***To change this functionality, you can use options like refetchOnMount, refetchOnWindowFocus, refetchOnReconnect and refetchInterval.***

* Query results that have no more active instances of useQuery, useInfiniteQuery or query observers are labeled as "inactive" and remain in the cache in case they are used again at a later time.

* By default, "inactive" queries are garbage collected after **5 minutes**.
  ***To change this, you can alter the default gcTime for queries to something other than 1000 * 60 * 5 milliseconds.***

* Queries that fail are **silently retried 3 times, with exponential backoff delay** before capturing and displaying an error to the UI.
  ***To change this, you can alter the default retry and retryDelay options for queries to something other than 3 and the default exponential backoff function.***

* Query results by default are **structurally shared to detect if data has actually changed** and if not, **the data reference remains unchanged** to better help with value stabilization with regards to useMemo and useCallback. If this concept sounds foreign, then don't worry about it! 99.9% of the time you will not need to disable this and it makes your app more performant at zero cost to you.
  ***Structural sharing only works with JSON-compatible values, any other value types will always be considered as changed. If you are seeing performance issues because of large responses for example, you can disable this feature with the config.structuralSharing flag. If you are dealing with non-JSON compatible values in your query responses and still want to detect if data has changed or not, you can provide your own custom function as config.structuralSharing to compute a value from the old and new responses, retaining references as required.***

## Further Reading

Have a look at the following articles from our Community Resources for further explanations of the defaults:
* **Practical React Query**
* **React Query as a State Manager**

# Background Fetching Indicators

A query's status === 'pending' state is sufficient enough to show the initial hard-loading state for a query, but sometimes you may want to display an additional indicator that a query is refetching in the background. To do this, queries also supply you with an isFetching boolean that you can use to show that it's in a fetching state, regardless of the state of the status variable:

```tsx
function Todos() {
  const {
    status,
    data: todos,
    error,
    isFetching,
  } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })

  return status === 'pending' ? (
    <span>Loading...</span>
  ) : status === 'error' ? (
    <span>Error: {error.message}</span>
  ) : (
    <>
      {isFetching ? <div>Refreshing...</div> : null}

      <div>
        {todos.map((todo) => (
          <Todo todo={todo} />
        ))}
      </div>
    </>
  )
}
```

## Displaying Global Background Fetching Loading State

In addition to individual query loading states, if you would like to show a global loading indicator when **any** queries are fetching (including in the background), you can use the useIsFetching hook:

```tsx
import { useIsFetching } from '@tanstack/react-query'

function GlobalLoadingIndicator() {
  const isFetching = useIsFetching()

  return isFetching ? (
    <div>Queries are fetching in the background...</div>
  ) : null
}
```