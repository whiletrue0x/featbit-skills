---
name: featbit-sdks-react
description: >
  Expert guidance for integrating FeatBit React Client SDK in React web applications.
  Use when user asks about "React SDK", "React feature flags", "FeatBit React",
  "useFlags hook", or mentions React components with FeatBit. Do not use for React Native,
  Node.js server-side, Python, Java, Go, or .NET questions.
license: MIT
metadata:
  author: FeatBit
  version: 1.0.0
  category: sdk-integration
---

# FeatBit React Client SDK

## Source

https://github.com/featbit/featbit-react-client-sdk

## Setup Workflow

Copy and track progress:
- [ ] Step 1: Install the package
- [ ] Step 2: Wrap the app with the provider
- [ ] Step 3: Read feature flag values in a component
- [ ] Step 4: If initialization or user switching needs more detail, read the matching reference file

**Step 1: Install the package**

Run:

```bash
npm install @featbit/react-client-sdk
```

**Step 2: Wrap the app with the provider**

Use this minimal setup:

```tsx
const FbProvider = await asyncWithFbProvider({
    options: {
        sdkKey: '<your-env-secret>',
        streamingUrl: 'ws://localhost:5100',
        eventsUrl: 'http://localhost:5100',
        user: { keyId: '<user-key>', name: 'Jane', customizedProperties: [] }
    }
});
```

**Step 3: Read feature flag values**

Use the React hook pattern:

```tsx
const { 'my-flag': myFlag } = useFlags();
```

If the provider never becomes ready or the first value is the fallback unexpectedly, verify `sdkKey`, `streamingUrl`, `eventsUrl`, and the initial user, then retry.

## Feature Flag Evaluation

After the provider is ready, read feature flag values from `useFlags()`:

```tsx
const { 'my-flag': myFlag } = useFlags();
```

Use `useFbClient()` only when the user needs the underlying JavaScript client directly.

## Read Next Only When Needed

- Read `references/consuming-flags.md` only when the user asks how to use feature flags inside components or needs `useFlags`, `useFbClient`, or `withFbConsumer` examples.
- Read `references/initialization.md` only when the user asks about `asyncWithFbProvider` vs `withFbProvider` or provider bootstrapping.
- Read `references/user-management.md` only when the user asks about switching users after initialization.
- Read `references/configuration-and-advanced.md` only when the user asks about bootstrap values, offline mode, or flag key casing.
- Read the official README sections for [initializing the SDK](https://github.com/featbit/featbit-react-client-sdk#initializing-the-sdk), [consuming flags](https://github.com/featbit/featbit-react-client-sdk#consuming-flags), [switch user after initialization](https://github.com/featbit/featbit-react-client-sdk#switch-user-after-initialization), [fallback flag values](https://github.com/featbit/featbit-react-client-sdk#populating-the-sdk-with-fallback-flag-values), and [flag keys](https://github.com/featbit/featbit-react-client-sdk#flag-keys) only when those topics are requested.
