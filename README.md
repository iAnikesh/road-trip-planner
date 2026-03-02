# road-trip-planner

Plan your perfect journey with intelligent route optimization and seamless destination management.

![npm version](https://img.shields.io/npm/v/road-trip-planner?style=flat-square)
![npm downloads](https://img.shields.io/npm/dm/road-trip-planner?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)
![Build Status](https://img.shields.io/github/actions/workflow/status/your-org/road-trip-planner/main.yml?branch=main&style=flat-square)
![Bundle size](https://img.shields.io/bundlephobia/minzip/road-trip-planner?style=flat-square)
![Last commit](https://img.shields.io/github/last-commit/your-org/road-trip-planner?style=flat-square)

---

**road-trip-planner** is a robust and intuitive library designed to simplify the complexities of planning multi-stop road trips. Whether you're a developer building a travel application or an individual looking to streamline your next adventure, this tool provides powerful features for route optimization, destination management, and trip visualization, ensuring a smooth and efficient journey from start to finish.

## ✨ Key Features

*   🗺️ **Intelligent Route Optimization**: Generate the most efficient routes considering multiple stops and preferences.
*   📍 **Seamless Point-of-Interest Discovery**: Easily integrate and discover attractions, accommodations, and amenities along your route.
*   💰 **Integrated Budget Management**: Track expenses and manage budgets for each leg of your journey.
*   🤝 **Collaborative Planning Tools**: Share and plan trips with friends and family in real-time.
*   💾 **Offline Access & Sync**: Plan on the go, even without an internet connection, with automatic synchronization.
*   📊 **Trip Analytics & Summaries**: Gain insights into your trip's duration, distance, and cost.

## 🚀 Installation

Install **road-trip-planner** in your project using your preferred package manager:

**npm**

```bash
npm install road-trip-planner

**yarn**

```bash
yarn add road-trip-planner
```

**pnpm**

```bash
pnpm add road-trip-planner
```

## ⚡ Quick Start

Get your first trip planned in just a few lines of code:

```typescript
import { createTrip, addDestination, optimizeRoute, getTripDetails } from 'road-trip-planner';

async function planMyFirstTrip() {
  // 1. Create a new trip
  const myTrip = await createTrip('Cross-Country Adventure', new Date('2024-07-01'), new Date('2024-07-15'));
  console.log(`Trip "${myTrip.name}" created with ID: ${myTrip.id}`);

  // 2. Add some destinations
  await addDestination(myTrip.id, { name: 'Denver, CO', lat: 39.7392, lng: -104.9903, order: 1 });
  await addDestination(myTrip.id, { name: 'Salt Lake City, UT', lat: 40.7608, lng: -111.8910, order: 2 });
  await addDestination(myTrip.id, { name: 'Las Vegas, NV', lat: 36.1699, lng: -115.1398, order: 3 });

  // 3. Optimize the route
  const optimizedTrip = await optimizeRoute(myTrip.id);
  console.log('Optimized Route Order:', optimizedTrip.destinations.map(d => d.name).join(' -> '));

  // 4. Get full trip details
  const details = await getTripDetails(myTrip.id);
  console.log('Full Trip Details:', details);
}

planMyFirstTrip();
```

## 🛠️ Usage

**road-trip-planner** provides a set of functions to manage your trips comprehensively.

### Creating a New Trip

Start by defining your trip's basic information.

```typescript
import { createTrip } from 'road-trip-planner';

const startDate = new Date('2024-08-01');
const endDate = new Date('2024-08-10');
const newTrip = await createTrip('Coastal Drive', startDate, endDate, { budget: 1500 });

console.log(`New trip "${newTrip.name}" created with ID: ${newTrip.id}`);
```

### Adding Destinations

Add multiple stops to your trip. You can specify a desired `order` or let the optimizer determine it.

```typescript
import { addDestination } from 'road-trip-planner';

const tripId = 'your-trip-id'; // Use the ID from createTrip
await addDestination(tripId, {
  name: 'San Francisco, CA',
  lat: 37.7749,
  lng: -122.4194,
  notes: 'Explore Golden Gate Bridge'
});

await addDestination(tripId, {
  name: 'Los Angeles, CA',
  lat: 34.0522,
  lng: -118.2437,
  arrivalDate: new Date('2024-08-05')
});
```

### Optimizing the Route

Reorder destinations to find the most efficient path.

```typescript
import { optimizeRoute, OptimizationStrategy } from 'road-trip-planner';

const tripId = 'your-trip-id';
const optimizedTrip = await optimizeRoute(tripId, { strategy: OptimizationStrategy.SHORTEST_DISTANCE });

console.log('Optimized destination order:');
optimizedTrip.destinations.forEach((dest, index) => {
  console.log(`${index + 1}. ${dest.name}`);
});
```

### Managing Trip Budgets

Keep track of your expenses.

```typescript
import { updateTripBudget, addExpense, getTripBudgetSummary } from 'road-trip-planner';

const tripId = 'your-trip-id';
await updateTripBudget(tripId, 2000); // Set total budget

await addExpense(tripId, {
  description: 'Hotel in San Francisco',
  amount: 250,
  category: 'Accommodation',
  date: new Date('2024-08-02')
});

const budgetSummary = await getTripBudgetSummary(tripId);
console.log('Remaining Budget:', budgetSummary.remaining);
```

## 🌐 SSR & TypeScript Support

**road-trip-planner** is built with **Server-Side Rendering (SSR)** compatibility in mind, ensuring seamless integration into modern web frameworks like Next.js, Nuxt.js, and SvelteKit. All core functionalities are designed to work efficiently on both client and server environments.

The library is fully written in **TypeScript**, providing robust type definitions for all functions, parameters, and return values. This enhances developer experience with autocompletion, compile-time error checking, and clearer API usage.

Here's a glimpse of some exported types:

```typescript
// src/types.ts

export interface Coordinates {
  lat: number;
  lng: number;
}

export interface Destination {
  id: string;
  name: string;
  coordinates: Coordinates;
  arrivalDate?: Date;
  departureDate?: Date;
  notes?: string;
  order?: number; // Desired order for optimization
}

export enum OptimizationStrategy {
  SHORTEST_DISTANCE = 'shortest_distance',
  FASTEST_TIME = 'fastest_time',
  CUSTOM_PRIORITY = 'custom_priority',
}

export interface Trip {
  id: string;
  name: string;
  startDate: Date;
  endDate: Date;
  destinations: Destination[];
  budget?: number;
  currency?: string;
  ownerId: string;
  collaborators?: string[];
  createdAt: Date;
  updatedAt: Date;
}

export interface Expense {
  id: string;
  tripId: string;
  description: string;
  amount: number;
  currency: string;
  category: string; // e.g., 'Food', 'Fuel', 'Accommodation'
  date: Date;
}

export interface TripBudgetSummary {
  totalBudget: number;
  totalExpenses: number;
  remaining: number;
  expensesByCategory: { [category: string]: number };
}
```

## 🤝 Contributing

We welcome contributions! If you'd like to improve **road-trip-planner**, please read our [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to submit pull requests, report issues, and suggest new features.

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.