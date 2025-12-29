# Building SwiftUI apps with Expo UI

Learn how to use Expo UI to integrate SwiftUI into your Expo apps.

**Platform:** iOS, macOS, tvOS

> Available in SDK 54 and above.

Expo UI brings SwiftUI to React Native. You can use modern SwiftUI primitives to build your apps.

## Features

- **SwiftUI primitives**: Expo UI is not another UI library. It brings SwiftUI primitives to Expo.
- **1-to-1 mapping**: The components in Expo UI have a 1-to-1 mapping to SwiftUI views. You can easily explore available views in the SwiftUI ecosystem, such as Explore SwiftUI or the Libraried app, and find the corresponding Expo UI component.
- **Full-app support**: Expo UI is designed to be used throughout the entire app. You can write your app entirely in Expo UI, while maintaining flexibility at the same time. The integration works at the component level. You can also mix React Native components, Expo UI components, DOM components, or custom 2D components using `react-native-skia`.

## Installation

You'll need to install the `@expo/ui` package in your Expo project. Run the following command to install it:

```bash
npx expo install @expo/ui
```

## Usage

Expo UI has several SwiftUI components available. You can use them in your app by importing them from `@expo/ui/swift-ui`. However, to cross the boundary from React Native (UIKit) to SwiftUI, you need to use the `<Host>` component. The `<Host>` is the container for SwiftUI views. You can think of it like `<svg>` in the DOM or `<Canvas>` in `react-native-skia`. Under the hood, it uses `UIHostingController` to render SwiftUI views in UIKit.

### Basic usage with `Host`

```jsx
import { CircularProgress, Host } from '@expo/ui/swift-ui';
import { View, Text } from 'react-native';

export default function LoadingView() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Host matchContents>
        <CircularProgress />
      </Host>
      <Text>Loading...</Text>
    </View>
  );
}
```

### Using `HStack` and `VStack`

You can also use the `HStack` and `VStack` components to build the entire layout in SwiftUI.

```jsx
import { CircularProgress, Host, HStack, LinearProgress, VStack } from '@expo/ui/swift-ui';

export default function LoadingView() {
  return (
    <Host style={{ flex: 1, margin: 32 }}>
      <VStack spacing={32}>
        <HStack spacing={32}>
          <CircularProgress />
          <CircularProgress color="orange" />
        </HStack>
        <LinearProgress progress={0.5} />
        <LinearProgress color="orange" progress={0.7} />
      </VStack>
    </Host>
  );
}
```

### Modifiers

SwiftUI modifier is a powerful way to customize the appearance and behavior of SwiftUI components. Expo UI also provides modifiers for SwiftUI components. You can import modifiers from `@expo/ui/swift-ui/modifiers` and pass them as an array to the `modifiers` prop. In the following example, the `expo-mesh-gradient` and `glassEffect` modifier are combined to create Liquid Glass text.

> Note: `glassEffect` modifier requires Xcode 26+ and iOS 26+.

```jsx
import { Host, Text } from '@expo/ui/swift-ui';
import { glassEffect, padding } from '@expo/ui/swift-ui/modifiers';
import { MeshGradientView } from 'expo-mesh-gradient';
import { View } from 'react-native';

export default function Page() {
  return (
    <View style={{ flex: 1 }}>
      <MeshGradientView
        style={{ flex: 1 }}
        columns={3}
        rows={3}
        colors={['red', 'purple', 'indigo', 'orange', 'white', 'blue', 'yellow', 'green', 'cyan']}
        points={[
          [0.0, 0.0],
          [0.5, 0.0],
          [1.0, 0.0],
          [0.0, 0.5],
          [0.5, 0.5],
          [1.0, 0.5],
          [0.0, 1.0],
          [0.5, 1.0],
          [1.0, 1.0],
        ]}
      />
      <Host style={{ position: 'absolute', top: 0, right: 0, left: 0, bottom: 0 }}>
        <Text
          size={32}
          modifiers={[
            padding({
              all: 16,
            }),
            glassEffect({
              glass: {
                variant: 'clear',
              },
            }),
          ]}>
          Glass effect text
        </Text>
      </Host>
    </View>
  );
}
```

### iOS Settings app example

Combining the Expo UI components and modifiers, you can build a UI like iOS Settings app.

```jsx
import {
  Button,
  Form,
  Host,
  HStack,
  Image,
  Section,
  Spacer,
  Switch,
  Text,
} from '@expo/ui/swift-ui';
import { background, clipShape, frame } from '@expo/ui/swift-ui/modifiers';
import { Link } from 'expo-router';
import { useState } from 'react';

export default function SettingsView() {
  const [isAirplaneMode, setIsAirplaneMode] = useState(true);

  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section>
          <HStack spacing={8}>
            <Image
              systemName="airplane"
              color="white"
              size={18}
              modifiers={[
                frame({ width: 28, height: 28 }),
                background('#ffa500'),
                clipShape('roundedRectangle'),
              ]}
            />
            <Text>Airplane Mode</Text>
            <Spacer />
            <Switch value={isAirplaneMode} onValueChange={setIsAirplaneMode} />
          </HStack>

          <Link href="/wifi" asChild>
            <Button>
              <HStack spacing={8}>
                <Image
                  systemName="wifi"
                  color="white"
                  size={18}
                  modifiers={[
                    frame({ width: 28, height: 28 }),
                    background('#007aff'),
                    clipShape('roundedRectangle'),
                  ]}
                />
                <Text color="primary">Wi-Fi</Text>
                <Spacer />
                <Image systemName="chevron.right" size={14} color="secondary" />
              </HStack>
            </Button>
          </Link>
        </Section>
      </Form>
    </Host>
  );
}
```

## Common questions

### Can I use flexbox or other styles in SwiftUI components?

Flexbox styles can be applied to the `<Host>` component itself. Once you're inside the SwiftUI context, however, `Yoga` is not available — layouts should be defined using `<HStack>` and `<VStack>` instead.

### What's the `Host` component?

`<Host>` is the container for SwiftUI views. You can think of it like `<svg>` in the DOM or `<Canvas>` in `react-native-skia`. Under the hood, it uses `UIHostingController` to render SwiftUI views in UIKit.

### How is Expo UI different from libraries like `react-native-paper` or `react-native-elements`?

Expo UI is not "yet another" UI library and not an opinionated design kit. Instead, it's a primitives library. It exposes native SwiftUI and Jetpack Compose components directly to JavaScript, rather than re-implementing or simulating UI in JavaScript.

### Can I use `@expo/ui/swift-ui` on Android or web?

The first milestone for Expo UI is achieving a 1-to-1 mapping from SwiftUI to Expo UI. Universal support will come in the next stage of the roadmap. Our priority is to establish strong SwiftUI support first, and then expand to Jetpack Compose on Android and DOM support on the Web.

### Can I use React Native components inside SwiftUI components?

Yes, you can place React Native components as JSX children of Expo UI components. Expo UI automatically creates a `UIViewRepresentable` wrapper for you.

However, keep in mind that the SwiftUI layout system works differently from UIKit and has some limitations. According to Apple's documentation:

> SwiftUI fully controls the layout of the UIKit view's `center`, `bounds`, `frame`, and `transform` properties. Don't directly set these layout-related properties on the view managed by a `UIViewRepresentable` instance from your own code because that conflicts with SwiftUI and results in undefined behavior.

Also note that once you render React Native components, you're leaving the SwiftUI context. If you want to add Expo UI components again, you'll need to reintroduce a `<Host>` wrapper.

We recommend keeping SwiftUI layouts self-contained. Interop is possible, but it works best when boundaries are clearly defined.

### I'm a SwiftUI developer. Why should I learn Expo UI?

Because React's promise of "learn once, write anywhere", it now extends to SwiftUI and Jetpack Compose. With Expo UI, you can apply your SwiftUI knowledge to build apps that run in the React Native ecosystem, extend to the Web through DOM components, and even integrate 2D and 3D rendering. The system is flexible enough that different parts of your app can use different approaches — giving you seamless integration at the component level.
