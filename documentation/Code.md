# Code Overview

This documentation describes how DeckMateAI services work and provides insights into key parts of the code.

---

### Menu
For quick access to specific sections, use the navigation below:

1. [State Manager](#state-manager) - Global state management powered by **Next Auth**, utilizing secure Next.js JWT tokens and cookies.
2. [Main Client Side](#main-client-side) - The **Hero Page**, serving as the entry point of the application, lazy-loads components and initializes services.
3. [Server Side](#server-side) - An **Express.js server** that manages routing, applies authentication middleware, evaluates requests, runs algorithms, and returns processed data.
4. [Next.js API Server](#nextjs-api-server) - Handles lightweight API tasks, including dynamic UI updates, user interactions, and **Stripe webhook payments**.
5. [Node.js API Server](#nodejs-api-server) - The core backend that manages AI-powered tools, Clash Royale and Brawl Stars API integrations, and advanced algorithms.

---

DeckMateAI is designed using a **two-server architecture** that separates front-end rendering, lightweight tasks, and payment processing from the heavy lifting done by backend AI tools and gaming API integrations. This modular structure ensures scalability and maintainability.

---

## State Manager

The **State Manager** is implemented using **Next Auth**, a flexible authentication solution for Next.js. It securely manages global state with JWT tokens and cookies to ensure a seamless and secure user experience.

- **Key Features**:
  - Global state management for user sessions.
  - Secure storage of authentication tokens using cookies.
  - Supports role-based access for authenticated users.

This is how authentication is structured:
```ts
export const authOptions: NextAuthOptions = {
    secret: process.env.NEXTAUTH_SECRET,
    adapter: PrismaAdapter(db),
    session: { strategy: "jwt" },
    pages: {
        signIn: "/login",
    },
    providers: [
        GoogleProvider({
            clientId: process.env.GOOGLE_CLIENT_ID!,
            clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
        }),
    ],
    callbacks: { ... }
}
```

---

## Main Client Side

The **Main Client Side** consists of the **Hero Page**, which acts as the core entry point of the application.

- **Key Responsibilities**:
  - Initializes and lazy-loads critical UI components.
  - Prepares services like user session management, data fetching, and event handling.
  - Optimizes performance by splitting code into manageable chunks for faster load times.

- **Technologies Used**:
  - **React**: For building reusable and interactive UI components.
  - **Next.js**: For server-side rendering and client-side hydration.
  - **Tailwind CSS**: For responsive and scalable styling.

This is we call components to be lazy loaded:
```ts
import dynamic from "next/dynamic";

// Lazy load component
const HeroBanner = dynamic(
    () => import("@/components/hero-section/heroBanner")
);
```

---

## Server Side

The **Server Side** is an Express.js server responsible for handling client requests and returning processed data.

- **Key Responsibilities**:
  - Applies **authentication middleware** to validate requests.
  - Routes incoming requests to appropriate services or algorithms.
  - Executes AI algorithms and game data processing for Clash Royale and Brawl Stars.
  - Ensures security by enforcing rate limits and request validation.

In Next.js API requests, a folder-based architecture is utilized, promoting a highly organized structure. This design helps maintain clean and modular code, minimizing the likelihood of disorganization or confusion.

An example of how HTTP methods are implemented in Next.js API routes:
```ts
export async function GET() {
  // Handle GET requests
}

export async function PUT() {
  // Handle PUT requests
}

export async function POST() {
  // Handle POST requests
}

export async function DELETE() {
  // Handle DELETE requests
}

export async function PATCH() {
  // Handle PATCH requests
}

export async function HEAD() {
  // Handle HEAD requests
}

// Additional HTTP methods can be added as needed
```
---

## Next.js API Server

The **Next.js API Server** handles smaller tasks related to the user interface, payments, and lightweight API calls. 

- **Key Responsibilities**:
  - Dynamically serve UI content and process user interactions.
  - Handle **Stripe webhook payments** for one-time purchases or donations.
  - Manage quick user updates like profile changes or preference settings.

- **Notable Features**:
  - Integrated with the **Next.js rendering engine** for seamless front-end and back-end communication.
  - Lightweight and optimized for low-latency tasks.

---

## Node.js API Server

The **Node.js API Server** serves as the powerhouse of DeckMateAI, handling the core functionality and computationally intensive tasks.

- **Key Responsibilities**:
  - Interacts with official **Clash Royale** and **Brawl Stars APIs** to fetch and process game data.
  - Runs AI-powered algorithms to generate optimized decks and provide actionable insights for users.
  - Manages the backend logic for all AI tools, ensuring scalability and accuracy.
  - Handles API proxying and rate-limiting to ensure compliance with third-party API usage policies.

- **Technologies Used**:
  - **Express.js**: For API routing and middleware.
  - **Node.js**: For efficient server-side computation.
  - **OpenAI API**: For advanced natural language processing and AI-generated recommendations.
  - **TensorFlow** (Tested): Initially explored for combining decks from different games and analyzing their effectiveness in various scenarios. However, this approach required users to have extensive gameplay data for meaningful recommendations. As a result, some algorithms were adopted to ensure accessibility and reliability for all users. This approach required users to have played at least 3-4 games to provide sufficient data.

- **Integration Highlights**:
  - **Clash Royale API**: Fetches player and clan data, providing detailed insights into gameplay and deck performance.
  - **Brawl Stars API**: Retrieves hero statistics and performance metrics across various game modes.
  - Proxy server integration ensures efficient and secure communication with third-party APIs.

Example of Brawl Stars service:
```ts
/**
 * Fetch rankings for a brawler in a given country or globally.
 * @param {string} countryCode - The country code (e.g., "global" or "US").
 * @param {number} brawlerId - The ID of the selected brawler.
 * @param {number} page - The current page for pagination.
 * @returns {Promise<Ranker[]>} A promise resolving to the rankings data or an error.
 */
async function fetchBrawlerRankings(countryCode: string, brawlerId: number, page: number = 1): Promise<Ranker[]>  {
  // Handle service
}
```