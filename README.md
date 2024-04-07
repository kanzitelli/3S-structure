# ➰➰➰ `3S-structure`: Streamline Feature Delivery and Code Maintenance

[![3S structure](https://img.shields.io/badge/3S-structure-blue)](https://gihub.com/kanzitelli/3S-structure)

The `3S-structure` is an opinionated project organization strategy designed specifically for React Native mobile apps and React web apps, such as admin portals and dashboards. This structure streamlines the development workflow by accelerating feature rollout and simplifying code maintenance tasks. Central to the `3S-structure` is its division into three layers: Stores for managing persisted data, Services for handling business logic, and Screens for the user interface. This approach not only enhances project neatness and efficiency but also supports the seamless reuse of code across different React-based projects, offering developers a highly efficient and organized way to build their apps.

### Content

1. [Backstory](#backstory)
2. [Structure](#structure)
3. [Usage in real-world applications](#usage-in-real-world-applications)

   3.1. [Decomposition of layers in apps](#decomposition-of-layers-in-apps)

4. [Conclusion](#conclusion)

## Backstory

Long story short: After years of developing apps and facing the ongoing challenge of maintenance, I devised a project structure that significantly aids in code sharing and saves developers' time.

<details>
<summary>Full text</summary>

My journey as an indie developer-entrepreneur began with a focus on iOS app development using Objective-C in 2013. By 2017, I expanded into creating native Android apps with Java. It was around this period that I discovered [React Native](https://reactnative.dev/), though at the time, it felt too young and not quite ready for production use. Developing and maintaining apps for both platforms proved time-consuming due to the need to rewrite logic, adopt different UI approaches, and other platform-specific requirements. Despite these challenges, I sought to find common ground and shared logic between the platforms. My solution was a Singleton approach for app data storage. Although this method received criticism and even mockery during job interviews, it worked well for me, simplifying app maintenance and feature additions.

As [React Native](https://reactnative.dev/) matured, I began transitioning some of my projects to this framework, especially those not reliant on complex audio/music functionalities or other deeply native features. I adapted the Singleton approach, which had evolved over time, to manage data within my [React Native](https://reactnative.dev/) apps. This experience led to the creation and ongoing maintenance of [various starter projects](https://github.com/kanzitelli) for [React Native](https://reactnative.dev/), incorporating different navigation libraries like [React Navigation](https://reactnavigation.org/) and [React Native Navigation](https://github.com/wix/react-native-navigation). This process naturally resulted in a consistent project structure that was effective across different navigation libraries and easier to maintain.

When working on [Buddify](https://buddify.app/), our task was to develop a [Shopify app](https://buddify.app/get/shopify-app) — a primarily React application with a custom authentication setup. This project, like an admin portal, required no server-side rendering (SSR) and had a single entry point. Implementing the same structured approach proved to be efficient, enabling rapid progress.

After implementing the main features in the [Buddify Shopify app](https://buddify.app/get/shopify-app), our team was looking for new ways to assist our merchants in boosting sales. This led to the creation of the [BUDDY Marketplace: Discount Shopping Hub](https://buddify.app/get/buddy-ios). Utilizing the same project structure facilitated the reuse of a substantial codebase for the mobile app, allowing us to develop a fully functional MVP within 2-3 months.

Given the success of this approach in both scenarios and its effectiveness in code sharing between apps, I was motivated to establish a dedicated repository. This repository aims to provide a detailed explanation and real-world examples of the project structure, showcasing its benefits and applicability.

</details>

## Structure

The `3S-structure` adopts a three-layered approach to app development:

### `Stores` layer

Think of this layer as the central hub for all your app's data storage. It ensures that data is accessible across the entire app.

Examples:

- Auth Store: Stores user authentication status, access tokens, and other relevant information.

- UI Store: Keeps track of UI preferences such as dark mode status, cart item count, etc.

### `Services` layer

This layer acts as the heart of your app's business logic, where you implement core functionalities. It's where you manipulate data stored in Stores, handle API calls, and carry out other critical business operations.

Examples:

Examples:

- Auth Service: Manages login and signup processes, retrieves authenticated data from the server, etc.

- Cart Service: Contains methods for adding or removing products from the cart, processing checkout operations, and calculating totals.

### `Screens`

This is where the user interface and user interactions come to life. It's the visual and interactive layer of the app.

Examples:

- Login Screen: Where users sign in to the app.

- Feed Screen: Displays content or products to the user.

Navigation plays a crucial role in any app and varies depending on the type of app. The `3S-structure` excels in scenarios where the app's layout is defined in a single file, as opposed to a file-based navigation system. For a detailed breakdown of each layer and insights on navigation strategies, see the section on [decomposition of layers in apps](#decomposition-of-layers-in-apps).

## Usage in real-world applications

I've applied this approach effectively in both mobile and web applications:

#### Mobile apps (React Native)

The `3S-structure` has been fully adopted and implemented in the [expo-starter](https://github.com/kanzitelli/expo-starter), and it plays a crucial role in the development of the [BUDDY Marketplace app](https://buddify.app/get/buddy-ios).

#### Web apps (React)

Similarly, this structure has proven beneficial in the creation of the [Buddify Shopify app](https://buddify.app/get/shopify-app) and various internal business tools, including admin portals and dashboards.

### Decomposition of layers in apps

Typically, frontend applications have a `src/` directory containing all the code. In React (Native) projects, there's often an entry file (e.g., `index.jsx` or `App.tsx`) that exports the main App component.

To adhere to the `3S-structure`, your `src/` should include the following folders: `stores/`, `services/`, and `screens/`. For larger projects, I also incorporate additional folders like `components/` for UI elements and `utils/` for various utilities (providers, hooks, types, constants, etc.).

#### Stores

Stores are crafted using [MobX](https://mobx.js.org/), which is ideal for its class-based store management and easy data persistence via [mobx-persist-store](https://github.com/quarrant/mobx-persist-store). MobX's reactivity feature facilitates the construction of complex logic with ease.

For [setting up the stores layer](https://github.com/kanzitelli/expo-starter/blob/master/src/stores/index.tsx) in React (Native), you'll have a `stores` object containing all app stores, a `useStores()` hook for UI access, and a `hydrateStores()` method for pre-UI hydration.

#### Services

Services are pure JavaScript classes that include an `async init()` method for initialization tasks like fetching device info or loading remote data.

The [basic setup](https://github.com/kanzitelli/expo-starter/blob/master/src/services/index.tsx) involves a `services` object with all app services, a `useServices()` hook for UI access, and an `initServices()` method that initializes all services (calling each service's `init()` method) before the UI loads.

⚠️ Ensure services initialization doesn't involve heavy data loads or computations to avoid delaying the UI display.

#### Screens

Screens are [standard React Components](https://github.com/kanzitelli/expo-starter/blob/master/src/screens/_screen-sample.tsx) (functional or class-based) representing app pages and are used as components in navigation routes. They render after stores and services initialization, allowing access through `useStores()` and `useServices()` hooks.

#### Navigation

Choosing the right navigation solution is critical. For React Native, I developed [Navio](https://github.com/kanzitelli/rn-navio), a library built on [React Navigation](https://reactnavigation.org/) that minimizes boilerplate and enhances developer experience. For web apps like the Shopify app, I utilized [react-router-dom](https://reactrouter.com/en/main) and the [<BrowserRouter />](https://reactrouter.com/en/main/router-components/browser-router) component, ensuring seamless integration without extra setup.

## Conclusion

This project structure comes from years of making different kinds of apps and wanting to write as little extra code as possible but still get great results.

It's straightforward but leads to using the same code many times, adding new features easily, better upkeep, and keeping the big picture in a simple structure.

If you like this way of doing things, feel free to show your support with a ⭐️.
