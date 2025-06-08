# ⚡️ Next.js 15+ Cheat Sheet

<details>
<summary>🚀 Getting Started</summary>

- Initialize a Next.js project (still same):

  ```bash
  npx create-next-app@latest
  ```

- **New Default**: Still **Server Components**, but **client-first layouts** are now easier with updated `use client` scoping rules.

- To make a **Client Component**:

  ```jsx
  'use client'
  ```

- ✅ **`next.config.js` enhancements**:

  - `appDir` is **enabled by default**
  - `turbo` option is available for testing TurboPack

</details>

---

<details>
<summary>🧹 Route Creation (App Router)</summary>

- Folder-based routing (same):
  ```
  app/
    about/             → /about
    contact/page.jsx   → /contact
  ```

### 🧠 Dynamic Routes

- Same structure, **but now you can co-locate loading and error UI directly**.

- **Single Dynamic**:

  ```
  app/[slug]/page.jsx → /blog-post
  ```

- **Catch-All**:

  ```
  app/[...params]/page.jsx → /a/b/c
  ```

- **Optional Catch-All**:
  ```
  app/[[...params]]/page.jsx → /
  ```
- **Route Groups**

  ```
  app/(marketing)/home/page.jsx → /home
  app/(dashboard)/settings/page.jsx → /settings
  ```

### 🆕 Layout Enhancements

- You can now use `generateStaticParams()` with metadata exports (experimental for SEO).

</details>

---

<details>
<summary>🧭 Navigation</summary>

- **`Link` from `next/link`** remains unchanged:

  ```jsx
  import Link from 'next/link'

  export default function Component() {
    return <Link href='/about'>About</Link>
  }
  ```

- **`useRouter()` hook** is now **`useRouter()` from `next/navigation` for App Router only**:

  ```tsx
  import { useRouter } from 'next/navigation'

  export default function Component() {
    const router = useRouter()

    return (
      <button onClick={() => router.push('/dashboard')}>Go to Dashboard</button>
    )
  }
  ```

</details>

---

<details>
<summary>🌐 Data Fetching</summary>

### 💥 Server-Side Fetching (still recommended)

- **Server Components use fetch directly** (Next.js optimizes caching, streaming):

  ```tsx
  export default async function Page() {
    const res = await fetch('https://api.example.com/posts', {
      cache: 'no-store'
    })
    const data = await res.json()

    return <PostList posts={data} />
  }
  ```

- ✅ **New in Next 15**:
  - You can use `Segment Configs` (`segment.config.js`) to configure caching per route.
  - Improved support for **streaming UI** with React 19+.

### 🧠 Client-Side Fetching

> Requires `'use client'`

**1. Using `useEffect`**

```tsx
'use client'
import { useEffect, useState } from 'react'

export default function ClientPage() {
  const [data, setData] = useState(null)
  const [isLoading, setIsLoading] = useState(true)

  useEffect(() => {
    const fetchData = async () => {
      const res = await fetch('/api/data')
      const json = await res.json()
      setData(json)
      setIsLoading(false)
    }
    fetchData()
  }, [])

  return <div>{isLoading ? 'Loading...' : JSON.stringify(data)}</div>
}
```

**2. Using SWR**

```tsx
'use client'
import useSWR from 'swr'

const fetcher = url => fetch(url).then(res => res.json())

export default function ClientPage() {
  const { data, error, isLoading } = useSWR('/api/posts', fetcher)

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

**3. Using React Queries - tanstack queries**

```tsx
'use client'
import { useQuery } from '@tanstack/react-query'

const fetchPosts = async () => {
  const response = await fetch('/api/posts')
  if (!response.ok) throw new Error('Network response was not ok')
  return response.json()
}

export default function ClientPage() {
  const { data, error, isLoading } = useQuery({
    queryKey: ['posts'],
    queryFn: fetchPosts
  })

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

</details>

---

<details>
<summary>📦 Middleware & Streaming</summary>

### 🔁 Middleware

Middleware remains in `middleware.ts` at root:

```ts
import { NextResponse } from 'next/server'

export function middleware(req) {
  return NextResponse.next()
}
```

- Can be configured via `matcher` in `middleware.config.js` (Next 15+ feature)

### 🌀 Streaming & Suspense

Next.js 15 improves streaming support:

```tsx
export default async function Page() {
  const dataPromise = fetchData()

  return (
    <Suspense fallback={<Skeleton />}>
      <DataComponent dataPromise={dataPromise} />
    </Suspense>
  )
}
```

</details>

---


<details>
<summary>📂 Predefined File Names in `app` Directory</summary>


| Filename        | Purpose                                                                                  |
|-----------------|------------------------------------------------------------------------------------------|
| `page.tsx`      | Required for every route. Defines the UI for the page.                                  |
| `layout.tsx`    | Wraps pages in a shared layout. Supports nested layouts for route segments.             |
| `loading.tsx`   | Shows a loading state during server-side rendering or route transitions.                |
| `error.tsx`     | Custom error UI for route-level error handling.                                         |
| `template.tsx`  | Provides a persistent UI template that resets state on navigation within a segment.     |
| `head.tsx`      | Customizes the HTML `<head>` metadata for SEO and scripts/styles.                       |
| `not-found.tsx` | Renders a custom 404 page for that route segment.                                       |
| `route.ts`      | Defines API route handlers inside the app directory (replaces `api/` in pages directory)|

</details>

---

<details>
<summary>✅ Summary Table</summary>

| Feature                   | Server | Client |
| ------------------------- | ------ | ------ |
| `'use client'`            | ❌     | ✅     |
| `fetch()`                 | ✅     | ✅     |
| `useEffect`               | ❌     | ✅     |
| SWR                       | ❌     | ✅     |
| TanStack Query (optional) | ❌     | ✅     |
| Streaming UI              | ✅     | ❌     |
| Layout Segments           | ✅     | ❌     |

</details>

---