# Full-Stack Modern Software Architecture Notes
**Target Stack**: React 18 / Next.js, Node.js / Express / Spring Boot, PostgreSQL, Redis, Docker.

---

## 1. Frontend Architecture: React 18 & State Management

### Key Concepts
* **Virtual DOM vs Real DOM**: React creates an in-memory representation of UI elements. When state changes, the reconciliation algorithm (React Fiber) diffs the new virtual tree with the previous one, batching minimal DOM mutations.
* **Component Lifecycle & Hooks**:
  - `useEffect`: Handles side effects (data fetching, subscriptions). Always specify dependency array to avoid infinite re-render loops.
  - `useMemo`: Memoizes expensive computation results between renders.
  - `useCallback`: Memoizes function references to prevent unnecessary child component re-renders.
  - `useRef`: Retains mutable values across renders without triggering a re-render; also used for direct DOM access.
* **Global State Management**:
  - **Zustand / Redux Toolkit**: Centralized store with predictable unidirectional data flow (`Action -> Dispatch -> Reducer -> Store -> View`).
  - **React Query (TanStack Query)**: The modern industry standard for *Server State* (caching, background refetching, deduplication, optimistic UI updates).

```typescript
// Example: TanStack Query hook with automatic caching & retry
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

interface UserProfile {
  id: string;
  name: string;
  email: string;
}

export function useUserProfile(userId: string) {
  return useQuery<UserProfile>({
    queryKey: ['user', userId],
    queryFn: async () => {
      const { data } = await axios.get(`/api/v1/users/${userId}`);
      return data;
    },
    staleTime: 1000 * 60 * 5, // Cache valid for 5 minutes
    retry: 2,
  });
}
```

---

## 2. Backend Architecture: RESTful APIs & Microservices

### REST vs GraphQL vs gRPC
| Feature | REST | GraphQL | gRPC |
| :--- | :--- | :--- | :--- |
| **Data Format** | JSON / XML | JSON | Protocol Buffers (Binary) |
| **Over/Under-fetching** | Common drawback | Completely eliminated | None |
| **Transport** | HTTP/1.1 or HTTP/2 | HTTP/1.1 or HTTP/2 | HTTP/2 exclusively |
| **Best Use Case** | Public web APIs, CRUD services | Complex relational frontend apps | Low-latency internal microservice communication |

### Production Backend Design Principles
1. **Layered Controller-Service-Repository Pattern**:
   - `Controller`: Validates request input, extracts headers/cookies, calls service layer, returns HTTP status codes.
   - `Service`: Contains business logic, transactions, third-party integrations.
   - `Repository / Data Access`: Executes SQL queries / ORM interactions.
2. **Idempotency**:
   - `GET`, `PUT`, `DELETE` are idempotent (repeated identical calls have the exact same end state).
   - `POST` is not idempotent by default. Implement **Idempotency-Key** headers in payment/order workflows to prevent duplicate billing.
3. **Database Connection Pooling**:
   - Avoid creating a fresh database connection per HTTP request (connection handshake is expensive). Use connection pools (e.g., `HikariCP` in Java, `pg.Pool` in Node.js) with configured minimum/maximum active connections.

---

## 3. Relational (PostgreSQL) vs NoSQL (MongoDB)

| Criteria | PostgreSQL (Relational) | MongoDB (Document NoSQL) |
| :--- | :--- | :--- |
| **Schema** | Strict, structured schema | Flexible, dynamic JSON/BSON schema |
| **Transactions** | Strong ACID compliance natively | Multi-document ACID supported since v4.0, but costlier |
| **Joins** | High-performance joins on indexed foreign keys | Lookup aggregations (slower at massive scale) |
| **Scaling** | Vertical scaling + read replicas; sharding requires Citus | Horizontal sharding built-in |
| **Ideal Use Case** | Banking, billing, ERP, structured relational workflows | Catalogs, event logging, content management, real-time telemetry |
