# Getting started with Speed Insights

This guide will help you get started with using Vercel Speed Insights on your project, showing you how to enable it, add the package to your project, deploy your app to Vercel, and view your data in the dashboard.

## Prerequisites

- A Vercel account. If you don't have one, you can [sign up for free](https://vercel.com/signup).
- A Vercel project. If you don't have one, you can [create a new project](https://vercel.com/new).
- The Vercel CLI installed. If you don't have it, you can install it using the following command:

### Install Vercel CLI

Choose your package manager:

```bash
# Using pnpm
pnpm i vercel

# Using yarn
yarn add vercel

# Using npm
npm install vercel

# Using bun
bun add vercel
```

## Enable Speed Insights in Vercel

On the [Vercel dashboard](https://vercel.com/dashboard), select your Project followed by the **Speed Insights** tab. You can also select the button below to be taken there. Then, select **Enable** from the dialog.

> **💡 Note:** Enabling Speed Insights will add new routes (scoped at `/_vercel/speed-insights/*`) after your next deployment.

## Add `@vercel/speed-insights` to your project

Using the package manager of your choice, add the `@vercel/speed-insights` package to your project:

```bash
# Using pnpm
pnpm add @vercel/speed-insights

# Using yarn
yarn add @vercel/speed-insights

# Using npm
npm install @vercel/speed-insights

# Using bun
bun add @vercel/speed-insights
```

> **Note:** When using the HTML implementation, there is no need to install the `@vercel/speed-insights` package.

## Implementation by Framework

### Next.js

The `SpeedInsights` component is a wrapper around the tracking script, offering more seamless integration with Next.js.

#### For Next.js 13.5+

Add the following component to your main app file (`pages/_app.tsx` or `pages/_app.jsx`):

**TypeScript:**
```tsx
import type { AppProps } from 'next/app';
import { SpeedInsights } from '@vercel/speed-insights/next';

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <>
      <Component {...pageProps} />
      <SpeedInsights />
    </>
  );
}

export default MyApp;
```

**JavaScript:**
```jsx
import { SpeedInsights } from "@vercel/speed-insights/next";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <SpeedInsights />
    </>
  );
}

export default MyApp;
```

#### For Next.js App Router (13.5+)

Add the following component to the root layout (`app/layout.tsx` or `app/layout.jsx`):

**TypeScript:**
```tsx
import { SpeedInsights } from "@vercel/speed-insights/next";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <SpeedInsights />
      </body>
    </html>
  );
}
```

**JavaScript:**
```jsx
import { SpeedInsights } from "@vercel/speed-insights/next";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <SpeedInsights />
      </body>
    </html>
  );
}
```

#### For Next.js < 13.5

Create a dedicated component to avoid opting out from SSR on the layout and pass the pathname of the route to the `SpeedInsights` component:

**app/insights.tsx:**
```tsx
"use client";

import { SpeedInsights } from "@vercel/speed-insights/react";
import { usePathname } from "next/navigation";

export function Insights() {
  const pathname = usePathname();

  return <SpeedInsights route={pathname} />;
}
```

**app/insights.jsx:**
```jsx
"use client";

import { SpeedInsights } from "@vercel/speed-insights/react";
import { usePathname } from "next/navigation";

export function Insights() {
  const pathname = usePathname();

  return <SpeedInsights route={pathname} />;
}
```

Then, import the `Insights` component in your layout:

**app/layout.tsx:**
```tsx
import type { ReactNode } from "react";
import { Insights } from "./insights";

export default function RootLayout({ children }: { children: ReactNode }) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <Insights />
      </body>
    </html>
  );
}
```

**app/layout.jsx:**
```jsx
import { Insights } from "./insights";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <Insights />
      </body>
    </html>
  );
}
```

### React (Create React App)

The `SpeedInsights` component is a wrapper around the tracking script, offering more seamless integration with React.

Add the following component to the main app file (`App.tsx` or `App.jsx`):

**TypeScript:**
```tsx
import { SpeedInsights } from '@vercel/speed-insights/react';

export default function App() {
  return (
    <div>
      {/* ... */}
      <SpeedInsights />
    </div>
  );
}
```

**JavaScript:**
```jsx
import { SpeedInsights } from "@vercel/speed-insights/react";

export default function App() {
  return (
    <div>
      {/* ... */}
      <SpeedInsights />
    </div>
  );
}
```

### Remix

The `SpeedInsights` component is a wrapper around the tracking script, offering a seamless integration with Remix.

Add the following component to your root file (`app/root.tsx` or `app/root.jsx`):

**TypeScript:**
```tsx
import { SpeedInsights } from '@vercel/speed-insights/remix';

export default function App() {
  return (
    <html lang="en">
      <body>
        {/* ... */}
        <SpeedInsights />
      </body>
    </html>
  );
}
```

**JavaScript:**
```jsx
import { SpeedInsights } from "@vercel/speed-insights/remix";

export default function App() {
  return (
    <html lang="en">
      <body>
        {/* ... */}
        <SpeedInsights />
      </body>
    </html>
  );
}
```

### SvelteKit

Import the `injectSpeedInsights` function from the package and call it in your root layout file:

**src/routes/+layout.ts:**
```ts
import { injectSpeedInsights } from "@vercel/speed-insights/sveltekit";

injectSpeedInsights();
```

**src/routes/+layout.js:**
```js
import { injectSpeedInsights } from "@vercel/speed-insights/sveltekit";

injectSpeedInsights();
```

### Vue

The `SpeedInsights` component is a wrapper around the tracking script, offering more seamless integration with Vue.

Add the following component to the main app template (`src/App.vue`):

**TypeScript:**
```vue
<script setup lang="ts">
import { SpeedInsights } from '@vercel/speed-insights/vue';
</script>

<template>
  <SpeedInsights />
</template>
```

**JavaScript:**
```vue
<script setup>
import { SpeedInsights } from '@vercel/speed-insights/vue';
</script>

<template>
  <SpeedInsights />
</template>
```

### Nuxt

The `SpeedInsights` component is a wrapper around the tracking script, offering more seamless integration with Nuxt.

Add the following component to the default layout (`layouts/default.vue`):

**TypeScript:**
```vue
<script setup lang="ts">
import { SpeedInsights } from '@vercel/speed-insights/vue';
</script>

<template>
  <SpeedInsights />
</template>
```

**JavaScript:**
```vue
<script setup>
import { SpeedInsights } from '@vercel/speed-insights/vue';
</script>

<template>
  <SpeedInsights />
</template>
```

### Astro

Speed Insights is available for both static and SSR Astro apps.

To enable this feature, declare the `<SpeedInsights />` component from `@vercel/speed-insights/astro` near the bottom of one of your layout components, such as `BaseHead.astro`:

```astro
---
import SpeedInsights from '@vercel/speed-insights/astro';
const { title, description } = Astro.props;
---
<title>{title}</title>
<meta name="title" content={title} />
<meta name="description" content={description} />

<SpeedInsights />
```

#### Optional: Remove sensitive information from URLs

Optionally, you can remove sensitive information from the URL by adding a `speedInsightsBeforeSend` function to the global `window` object. The `<SpeedInsights />` component will call this method before sending any data to Vercel:

```astro
---
import SpeedInsights from '@vercel/speed-insights/astro';
const { title, description } = Astro.props;
---
<title>{title}</title>
<meta name="title" content={title} />
<meta name="description" content={description} />

<script is:inline>
  function speedInsightsBeforeSend(data){
    console.log("Speed Insights before send", data)
    return data;
  }
</script>
<SpeedInsights />
```

### Other Frameworks

Import the `injectSpeedInsights` function from the package, which will add the tracking script to your app. **This should only be called once in your app, and must run in the client**.

Add the following code to your main app file (`main.ts` or `main.js`):

**TypeScript:**
```ts
import { injectSpeedInsights } from "@vercel/speed-insights";

injectSpeedInsights();
```

**JavaScript:**
```js
import { injectSpeedInsights } from "@vercel/speed-insights";

injectSpeedInsights();
```

### HTML

When using the HTML implementation, there is no need to install the `@vercel/speed-insights` package.

Add the following scripts before the closing tag of the `<body>`:

```html
<script>
  window.si = window.si || function () { (window.siq = window.siq || []).push(arguments); };
</script>
<script defer src="/_vercel/speed-insights/script.js"></script>
```

## Deploy your app to Vercel

You can deploy your app to Vercel's global [CDN](/docs/cdn) by running the following command from your terminal:

```bash
vercel deploy
```

Alternatively, you can [connect your project's git repository](/docs/git#deploying-a-git-repository), which will enable Vercel to deploy your latest pushes and merges to main.

Once your app is deployed, it's ready to begin tracking performance metrics.

> **💡 Note:** If everything is set up correctly, you should be able to find the `/_vercel/speed-insights/script.js` script inside the body tag of your page.

## View your data in the dashboard

Once your app is deployed, and users have visited your site, you can view the data in the dashboard.

To do so, go to your [dashboard](https://vercel.com/dashboard), select your project, and click the **Speed Insights** tab.

After a few days of visitors, you'll be able to start exploring your metrics. For more information on how to use Speed Insights, see [Using Speed Insights](/docs/speed-insights/using-speed-insights).

Learn more about how Vercel supports [privacy and data compliance standards](/docs/speed-insights/privacy-policy) with Vercel Speed Insights.

## Next steps

Now that you have Vercel Speed Insights set up, you can explore the following topics to learn more:

- [Learn how to use the `@vercel/speed-insights` package](/docs/speed-insights/package)
- [Learn about metrics](/docs/speed-insights/metrics)
- [Read about privacy and compliance](/docs/speed-insights/privacy-policy)
- [Explore pricing](/docs/speed-insights/limits-and-pricing)
- [Troubleshooting](/docs/speed-insights/troubleshooting)
