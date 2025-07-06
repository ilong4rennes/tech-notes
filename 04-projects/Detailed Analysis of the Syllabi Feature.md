## Backend Implementation

### Route Registration
In `apps/backend/src/app.ts`, two routes are registered for syllabi:
```typescript
app.route("/syllabi").get(getSyllabi)
app.route("/syllabi/all").get(getAllSyllabi);
```

### Database Interaction
The `syllabi.ts` controller interacts with what appears to be a Prisma ORM client (`db`) to query the database.

### Endpoint 1: GET /syllabi

#### Request Processing
1. **Parameter Extraction**:
   ```typescript
   const numbers = singleToArray(req.query.number).map((n) => n);
   ```
   - Converts single course number or array of numbers into a consistent array format

2. **Logging**:
   ```typescript
   console.log("Fetching syllabi for numbers:", numbers);
   ```
   - Logs the requested course numbers for debugging

3. **Database Query**:
   ```typescript
   const syllabi = await db.syllabi.findMany({
     where: { number: { in: numbers } },
     select: {
       id: true,
       season: true,
       year: true,
       number: true,
       section: true,
       url: true,
     }
   });
   ```
   - Uses Prisma's `findMany` to retrieve matching syllabi
   - Filters by course numbers using an `in` clause
   - Selects only the necessary fields

4. **Error Handling**:
   ```typescript
   try {
     // Query logic
   } catch (e) {
     console.error("Error fetching syllabi:", e);
     res.json([]);
   }
   ```
   - If an error occurs, it's logged
   - Returns an empty array instead of an error response
   - This provides graceful degradation for the frontend

5. **Response**:
   ```typescript
   console.log(`Found ${syllabi.length} syllabi`);
   res.json(syllabi);
   ```
   - Logs the number of syllabi found
   - Returns the syllabi array as JSON

### Endpoint 2: GET /syllabi/all

#### Caching Mechanism
```typescript
const allSyllabiEntry = {
  allSyllabi: [] as Syllabus[],
  lastCached: new Date(1970),
};
```
- Simple in-memory cache with timestamp

#### Cache Invalidation Logic
```typescript
if (new Date().valueOf() - allSyllabiEntry.lastCached.valueOf() > 1000 * 60 * 60 * 24) {
  // Cache is expired (older than 24 hours)
}
```

#### Data Retrieval
```typescript
const syllabiFromDB = await db.syllabi.findMany({
  where: filter,
  select: getAllSyllabiDbQuery.select,
});

const syllabi: Syllabus[] = syllabiFromDB.map((s) => ({
  id: s.id,
  season: s.season ?? "",
  year: s.year ?? 0,
  number: s.number ?? "",
  url: s.url ?? null,
}));
```
- Fetches all syllabi from database
- Maps database objects to the Syllabus type
- Handles potential null values with nullish coalescing

## Frontend Implementation

### API Client (`apps/frontend/src/app/api/syllabi.ts`)

#### Core Data Fetching Function
```typescript
const fetchSyllabi = async (numbers: string[]): Promise<Syllabus[]> => {
  try {
    const url = `${process.env.NEXT_PUBLIC_BACKEND_URL || ""}/syllabi`;
    
    console.log("Original numbers (with types):", numbers.map(n => `${n} (${typeof n})`));
    
    // Ensure numbers are always strings and padded to 5 digits
    const paddedNumbers = numbers.map(num => String(num).padStart(5, '0'));
    
    console.log("Padded numbers:", paddedNumbers);
    
    const params = new URLSearchParams(
      paddedNumbers.map((number) => ["number", number])
    );

    const response = await axios.get(url, {
      headers: {"Content-Type": "application/json"},
      params,
    });

    return response.data;
  } catch (error) {
    console.error("Error fetching syllabi:", error);
    return [];
  }
}
```
- Ensures course numbers are consistently formatted (padded to 5 digits)
- Uses axios for HTTP requests
- Extensive logging for debugging
- Returns empty array on error (matching backend behavior)

### React Query Integration

#### Single Course Hook
```typescript
export const useFetchSyllabus = (number: string) => {
  return useQuery({
    queryKey: ["syllabus", { number }],
    queryFn: () => fetchSyllabusBatcher.fetch(number),
    staleTime: STALE_TIME,
    enabled: !!number,
  });
};
```
- Uses React Query for data fetching, caching, and state management
- Only enabled when a valid course number is provided
- Uses the batching mechanism for efficient API calls

#### Multiple Courses Hook
```typescript
export const useFetchMultipleSyllabi = (numbers: string[]) => {
  return useQueries({
    queries: numbers.map((number) => ({
      queryKey: ["syllabus", { number }],
      queryFn: () => fetchSyllabusBatcher.fetch(number),
      staleTime: STALE_TIME,
      enabled: !!number,
    })),
    combine: (result) => {
      return result.reduce((acc, { data }) => {
        if (data) acc.push(data);
        return acc;
      }, [] as Syllabus[]);
    },
  });
};
```
- Handles multiple course syllabi in parallel
- Combines individual query results into a single array
- Still benefits from the batching mechanism

### Batching Mechanism
```typescript
const fetchSyllabusBatcher = create({
  fetcher: async (syllabusNumbers: string[]): Promise<Syllabus[]> => {
    try {
      const url = `${process.env.NEXT_PUBLIC_BACKEND_URL || ""}/syllabi`;
      const params = new URLSearchParams(
        syllabusNumbers.map((number) => ["number", number])
      );

      const response = await axios.get(url, {
        headers: {
          "Content-Type": "application/json",
        },
        params,
      });

      return response.data;
    } catch (error) {
      console.error("Error in syllabus batcher:", error);
      return []; 
    }
  },
  resolver: keyResolver("number"),
  scheduler: windowScheduler(10),
});
```
- Uses the batshit library to efficiently batch requests
- Groups multiple requests within a 10ms window
- Reduces network overhead for multiple syllabus requests
- Uses a resolver to match returned syllabi to their course numbers

## Complete Data Flow (End-to-End)

1. **User Interaction**:
   - A user views a course that has an associated syllabus
   - Or a user searches for specific course syllabi

2. **Component Rendering**:
   - A React component calls `useFetchSyllabus("15112")` or similar

3. **React Query Processing**:
   - React Query checks its cache for ["syllabus", {number: "15112"}]
   - If found and not stale (< 24 hours old), returns cached data
   - If not found or stale, proceeds with the request

4. **Request Batching**:
   - The request enters the batching queue
   - If other requests arrive within 10ms window, they're batched together
   - This creates a single request for multiple course numbers

5. **HTTP Request**:
   - Axios sends a GET request to `/syllabi?number=15112` 
   - Or for batched requests: `/syllabi?number=15112&number=15122&number=15150`

6. **Backend Processing**:
   - Express server receives the request
   - Routes it to the `getSyllabi` handler
   - Extracts course numbers from query params

7. **Database Query**:
   - Backend queries the database for matching syllabi
   - Selects only necessary fields (id, season, year, number, section, url)

8. **Response Generation**:
   - Backend formats the database results
   - Returns JSON array of matching syllabi

9. **Frontend Processing**:
   - Axios receives the HTTP response
   - React Query stores the data in cache (valid for 24 hours)
   - Component receives the data and renders accordingly

10. **User Experience**:
    - User sees syllabus information for the course(s)
    - Potentially clicks on the syllabus URL to view the full document

This comprehensive flow demonstrates how the syllabi feature efficiently retrieves and displays syllabi information through a well-designed data pipeline.
