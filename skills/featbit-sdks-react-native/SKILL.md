---
name: featbit-sdks-react-native
description: >
  Expert guidance for integrating FeatBit React Native SDK in mobile React Native
  applications. Use when user asks about "React Native SDK", "React Native feature flags",
  "FeatBit mobile", or mentions React Native with FeatBit. Do not use for React web apps,
  Node.js, Python, Java, Go, or .NET questions.
license: MIT
metadata:
  author: FeatBit
  version: 1.0.0
  category: sdk-integration
---

# FeatBit React Native SDK

## Source

https://github.com/featbit/featbit-react-native-sdk

## Setup Workflow

Copy and track progress:
- [ ] Step 1: Install the package
- [ ] Step 2: Build config first
- [ ] Step 3: Wrap the app with the provider
- [ ] Step 4: Read feature flag values in a screen or component
- [ ] Step 5: Read advanced README sections only when needed

**Step 1: Install the package**

Run:

```bash
npm install @featbit/react-native-sdk
```

**Step 2: Build config first**

React Native requires `buildConfig()` before the provider is used.

```tsx
const config = buildConfig({
  options: {
    sdkKey: '<your-env-secret>',
    streamingUri: 'ws://localhost:5100',
    eventsUrl: 'http://localhost:5100',
    user: { keyId: '<user-key>', name: 'Jane', customizedProperties: [] }
  }
});
```

**Step 3: Wrap the app and read feature flag values**

Use the official pattern:

```tsx
export default withFbProvider(config)(App);
const { 'my-flag': myFlag } = useFlags();
```

If the provider never becomes ready or the first value is the fallback unexpectedly, verify `sdkKey`, `streamingUri`, `eventsUrl`, and the initial user, then retry.

## Feature Flag Evaluation

After the provider is ready, read feature flag values from `useFlags()`:

```tsx
const { 'my-flag': myFlag } = useFlags();
```

Use `useFbClient()` only when the user needs the underlying client directly.

## Read Next Only When Needed

- Read the official README section for [initializing the SDK](https://github.com/featbit/featbit-react-native-sdk#initializing-the-sdk) when the user asks how the provider and feature flag access work together.
- Read the official README section for the `asyncWithFbProvider` pattern only when that topic is requested.
