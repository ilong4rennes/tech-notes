# React Query

`useQuery`
- React Query Hook for fetching, caching, syncing, and updating server data

```
const {
  data,
  dataUpdatedAt,
  error,
  errorUpdatedAt,
  failureCount,
  failureReason,
  fetchStatus,
  isError,
  isFetched,
  isFetchedAfterMount,
  isFetching,
  isInitialLoading,
  isLoading,
  isLoadingError,
  isPaused,
  isPending,
  isPlaceholderData,
  isRefetchError,
  isRefetching,
  isStale,
  isSuccess,
  promise,
  refetch,
  status,
} = useQuery(
  {
    queryKey,
    queryFn,
    gcTime,
    enabled,
    networkMode,
    initialData,
    initialDataUpdatedAt,
    meta,
    notifyOnChangeProps,
    placeholderData,
    queryKeyHashFn,
    refetchInterval,
    refetchIntervalInBackground,
    refetchOnMount,
    refetchOnReconnect,
    refetchOnWindowFocus,
    retry,
    retryOnMount,
    retryDelay,
    select,
    staleTime,
    structuralSharing,
    subscribed,
    throwOnError,
  },
  queryClient,
)
```

**Parameter1 (Options)**

- queryKey: unknown[]
    - **Required**
    - The query key to use for this query.
    - The query key will be hashed into a stable hash. See [Query Keys](https://tanstack.com/query/latest/docs/framework/react/guides/query-keys) for more information.
    - The query will automatically update when this key changes (as long as enabled is not set to false).
- queryFn: (context: QueryFunctionContext) => `Promise<TData>`
    - **Required, but only if no default query function has been defined** See [Default Query Function](https://tanstack.com/query/latest/docs/framework/react/guides/default-query-function) for more information.
    - The function that the query will use to request data.
    - Receives a [QueryFunctionContext](https://tanstack.com/query/latest/docs/framework/react/guides/query-functions#queryfunctioncontext)
    - Must return a promise that will either resolve data or throw an error. The data cannot be undefined.
- enabled: boolean | (query: Query) => boolean
    - Set this to false to disable this query from automatically running.
    - Can be used for [Dependent Queries](https://tanstack.com/query/latest/docs/framework/react/guides/dependent-queries).

**Parameter2 (QueryClient)**

- queryClient?: QueryClient,
    - Use this to use a custom QueryClient. Otherwise, the one from the nearest context will be used.

**Returns**

- status: QueryStatus
    - Will be:
        - pending if there's no cached data and no query attempt was finished yet.
        - error if the query attempt resulted in an error. The corresponding error property has the error received from the attempted fetch
        - success if the query has received a response with no errors and is ready to display its data. The corresponding data property on the query is the data received from the successful fetch or if the query's enabled property is set to false and has not been fetched yet data is the first initialData supplied to the query on initialization.
- isPending: boolean
    - A derived boolean from the status variable above, provided for convenience.
- isSuccess: boolean
    - A derived boolean from the status variable above, provided for convenience.
- isError: boolean
    - A derived boolean from the status variable above, provided for convenience.
- isLoadingError: boolean
    - Will be true if the query failed while fetching for the first time.
- isRefetchError: boolean
    - Will be true if the query failed while refetching.
- data: TData
    - Defaults to undefined.
    - The last successfully resolved data for the query.
- dataUpdatedAt: number
    - The timestamp for when the query most recently returned the status as "success".
- error: null | TError
    - Defaults to null
    - The error object for the query, if an error was thrown.

# Batching Requests

- Docs: https://tanstack.com/query/v4/docs/framework/react/community/batching-requests-using-bathshit 
## The `create` Function API

The `create` function generates a batched fetcher with three main components:
```typescript
const batcher = create({
  fetcher: (keys) => Promise<Results>,
  resolver: Function,
  scheduler: Function,
  name?: string // optional for debugging
});
```

### 1. `fetcher` Parameter

The fetcher resolves a list of queries (in this case syllabusNumbers) into a single API call. It's a function that:

`fetcher: async (keys: KeyType[]) => Promise<ResultType[]>`

- Takes an array of keys (syllabusNumbers in your case)
- Makes a single network request that fetches data for all keys at once
- Returns a promise that resolves to an array of results

In your implementation, you're:

1. Building a URL with query parameters for all syllabus numbers
2. Making a single GET request with axios
3. Returning the response data

### 2. `resolver` Parameter

The resolver determines how to match returned results back to the original keys. The library provides helper functions:

`resolver: keyResolver("propertyName")`

The `keyResolver("number")` function will resolve the correct syllabus using the field "number" from each returned item.

The library provides these resolver functions:

- `keyResolver(propertyName)`: Matches based on a property of each result item
- `indexedResolver()`: Handles responses that are objects/records with keys matching the requested IDs

### 3. `scheduler` Parameter

The scheduler controls how and when batches are created:

`scheduler: windowScheduler(timeInMs)`

The `windowScheduler(10)` will batch all calls to the fetcher that are made within 10 milliseconds. This means any syllabus requests happening within that time window will be grouped into a single API call.

The library also provides:

- `maxBatchSizeScheduler({ maxBatchSize: N })`: Limits batch size to a maximum number of items

## Motivation for Batching Requests

1. **Performance**: Making multiple small API requests creates network overhead. Each HTTP request has headers, connection setup time, and latency. Batching reduces these costs by sending one larger request instead of many small ones.
2. **Reduced Server Load**: The server processes one request instead of dozens, reducing database queries and server CPU usage.