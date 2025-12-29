# SwiftUI API Reference

SwiftUI components for building native iOS interfaces with @expo/ui.

**Platform:** iOS, tvOS
**Bundled version:** ~0.2.0-beta.7

> This library is currently in beta and subject to breaking changes. It is not available in the Expo Go app — use development builds to try it out.

The SwiftUI components in `@expo/ui/swift-ui` allow you to build fully native iOS interfaces using SwiftUI from React Native.

## Installation

```bash
npx expo install @expo/ui
```

If you are installing this in an existing React Native app, make sure to install `expo` in your project.

## Components

### BottomSheet

**Platform:** iOS

```jsx
import { BottomSheet, Host, Text } from '@expo/ui/swift-ui';
import { useWindowDimensions } from 'react-native';

const { width } = useWindowDimensions();

<Host style={{ position: 'absolute', width }}>
  <BottomSheet isOpened={isOpened} onIsOpenedChange={e => setIsOpened(e)}>
    <Text>Hello, world!</Text>
  </BottomSheet>
</Host>
```

### Button

**Platform:** iOS

> The borderless variant is not available on Apple TV.

```jsx
import { Button, Host } from '@expo/ui/swift-ui';

<Host style={{ flex: 1 }}>
  <Button
    variant="default"
    onPress={() => {
      setEditingProfile(true);
    }}>
    Edit profile
  </Button>
</Host>
```

### CircularProgress

**Platform:** iOS

```jsx
import { CircularProgress, Host } from '@expo/ui/swift-ui';

<Host style={{ width: 300 }}>
  <CircularProgress progress={0.5} color="blue" />
</Host>
```

### ColorPicker

**Platform:** iOS (not available on Apple TV)

```jsx
import { ColorPicker, Host } from '@expo/ui/swift-ui';

<Host style={{ width: 400, height: 200 }}>
  <ColorPicker
    label="Select a color"
    selection={color}
    onValueChanged={setColor}
  />
</Host>
```

### ContextMenu

**Platform:** iOS

> Note: Also known as DropdownMenu.

```jsx
import { ContextMenu, Host } from '@expo/ui/swift-ui';

<Host style={{ width: 150, height: 50 }}>
  <ContextMenu>
    <ContextMenu.Items>
      <Button
        systemImage="person.crop.circle.badge.xmark"
        onPress={() => console.log('Pressed1')}>
        Hello
      </Button>
      <Button
        variant="bordered"
        systemImage="heart"
        onPress={() => console.log('Pressed2')}>
        Love it
      </Button>
      <Picker
        label="Doggos"
        options={['very', 'veery', 'veeery', 'much']}
        variant="menu"
        selectedIndex={selectedIndex}
        onOptionSelected={({ nativeEvent: { index } }) => setSelectedIndex(index)}
      />
    </ContextMenu.Items>
    <ContextMenu.Trigger>
      <Button variant="bordered">
        Show Menu
      </Button>
    </ContextMenu.Trigger>
  </ContextMenu>
</Host>
```

### DateTimePicker (date)

**Platform:** iOS (not available on Apple TV)

```jsx
import { DateTimePicker, Host } from '@expo/ui/swift-ui';

<Host matchContents>
  <DateTimePicker
    onDateSelected={date => {
      setSelectedDate(date);
    }}
    displayedComponents='date'
    initialDate={selectedDate.toISOString()}
    variant='wheel'
  />
</Host>
```

### DateTimePicker (time)

**Platform:** iOS (not available on Apple TV)

```jsx
import { DateTimePicker, Host } from '@expo/ui/swift-ui';

<Host matchContents>
  <DateTimePicker
    onDateSelected={date => {
      setSelectedDate(date);
    }}
    displayedComponents='hourAndMinute'
    initialDate={selectedDate.toISOString()}
    variant='wheel'
  />
</Host>
```

### Gauge

**Platform:** iOS (not available on Apple TV)

```jsx
import { Gauge, Host } from "@expo/ui/swift-ui";

<Host matchContents>
  <Gauge
    max={{ value: 1, label: '1' }}
    min={{ value: 0, label: '0' }}
    current={{ value: 0.5 }}
    color={[
      PlatformColor('systemRed'),
      PlatformColor('systemOrange'),
      PlatformColor('systemYellow'),
      PlatformColor('systemGreen'),
    ]}
    type="circularCapacity"
  />
</Host>
```

### Host

A component that allows you to put the other `@expo/ui/swift-ui` components in React Native. It acts like `<svg>` for DOM, `<Canvas>` for `react-native-skia`, which underlying uses `UIHostingController` to render the SwiftUI views in UIKit.

Since the `Host` component is a React Native `View`, you can pass the `style` prop to it or `matchContents` prop to make the `Host` component match the contents' size.

**Wrapping Button in Host:**

```jsx
import { Button, Host } from '@expo/ui/swift-ui';

function Example() {
  return (
    <Host matchContents>
      <Button
        onPress={() => {
          console.log('Pressed');
        }}>
        Click
      </Button>
    </Host>
  );
}
```

**Host with flexbox and VStack:**

```jsx
import { Button, Host, VStack, Text } from '@expo/ui/swift-ui';

function Example() {
  return (
    <Host style={{ flex: 1 }}>
      <VStack spacing={8}>
        <Text>Hello, world!</Text>
        <Button
          onPress={() => {
            console.log('Pressed');
          }}>
          Click
        </Button>
      </VStack>
    </Host>
  );
}
```

### LinearProgress

**Platform:** iOS

```jsx
import { LinearProgress, Host } from '@expo/ui/swift-ui';

<Host style={{ width: 300 }}>
  <LinearProgress progress={0.5} color="red" />
</Host>
```

### List

**Platform:** iOS

```jsx
import { Host, List } from '@expo/ui/swift-ui';

<Host style={{ flex: 1 }}>
  <List
    scrollEnabled={false}
    editModeEnabled={editModeEnabled}
    onSelectionChange={(items) => alert(`indexes of selected items: ${items.join(', ')}`)}
    moveEnabled={moveEnabled}
    onMoveItem={(from, to) => alert(`moved item at index ${from} to index ${to}`)}
    onDeleteItem={(item) => alert(`deleted item at index: ${item}`)}
    listStyle='automatic'
    deleteEnabled={deleteEnabled}
    selectEnabled={selectEnabled}>
    {data.map((item, index) => (
      <LabelPrimitive key={index} title={item.text} systemImage={item.systemImage} color={color} />
    ))}
  </List>
</Host>
```

### Picker (segmented)

**Platform:** iOS

```jsx
import { Host, Picker } from '@expo/ui/swift-ui';

<Host matchContents>
  <Picker
    options={['$', '$$', '$$$', '$$$$']}
    selectedIndex={selectedIndex}
    onOptionSelected={({ nativeEvent: { index } }) => {
      setSelectedIndex(index);
    }}
    variant="segmented"
  />
</Host>
```

### Picker (wheel)

**Platform:** iOS (not available on Apple TV)

```jsx
import { Host, Picker } from '@expo/ui/swift-ui';

<Host style={{ height: 100 }}>
  <Picker
    options={['$', '$$', '$$$', '$$$$']}
    selectedIndex={selectedIndex}
    onOptionSelected={({ nativeEvent: { index } }) => {
      setSelectedIndex(index);
    }}
    variant="wheel"
  />
</Host>
```

### Slider

**Platform:** iOS (not available on Apple TV)

```jsx
import { Host, Slider } from '@expo/ui/swift-ui';

<Host style={{ minHeight: 60 }}>
  <Slider
    value={value}
    onValueChange={(value) => {
      setValue(value);
    }}
  />
</Host>
```

### Switch (toggle)

**Platform:** iOS

> Note: Also known as Toggle.

```jsx
import { Host, Switch } from '@expo/ui/swift-ui';

<Host matchContents>
  <Switch
    checked={checked}
    onValueChange={checked => {
      setChecked(checked);
    }}
    color="#ff0000"
    label="Play music"
    variant="switch"
  />
</Host>
```

### Switch (checkbox)

**Platform:** iOS

```jsx
import { Host, Switch } from '@expo/ui/swift-ui';

<Host matchContents>
  <Switch
    checked={checked}
    onValueChange={checked => {
      setChecked(checked);
    }}
    label="Play music"
    variant="checkbox"
  />
</Host>
```

### TextField

**Platform:** iOS

```jsx
import { Host, TextField } from '@expo/ui/swift-ui';

<Host matchContents>
  <TextField autocorrection={false} defaultValue="A single line text input" onChangeText={setValue} />
</Host>
```

## API

Full documentation is not yet available. Use TypeScript types to explore the API.

```javascript
// Import from the SwiftUI package
import { BottomSheet } from '@expo/ui/swift-ui';
```

---

# GlassEffect API Reference

React components that render a liquid glass effect using iOS's native UIVisualEffectView.

**Platform:** iOS 26+
**Package:** `expo-glass-effect`

> `GlassView` is only available on iOS 26 and above. It will fallback to regular `View` on unsupported platforms.

React components that render native iOS liquid glass effect using `UIVisualEffectView`. Supports customizable glass styles and tint color.

## Installation

```bash
npx expo install expo-glass-effect
```

If you are installing this in an existing React Native app, make sure to install `expo` in your project.

### Known issues

The `isInteractive` prop can only be set once on mount and cannot be changed dynamically after the component has been rendered. If you need to toggle interactive behavior, you must remount the component with a different `key`.

## Components

### GlassView

The `GlassView` component renders the native iOS glass effect. It supports different glass effect styles and can be customized with tint colors for various aesthetic needs.

**Platform:** iOS 26+

```jsx
import { StyleSheet, View, Image } from 'react-native';
import { GlassView } from 'expo-glass-effect';

export default function App() {
  return (
    <View style={styles.container}>
      <Image
        style={styles.backgroundImage}
        source={{
          uri: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=400&h=400&fit=crop',
        }}
      />

      {/* Basic Glass View */}
      <GlassView style={styles.glassView} />

      {/* Glass View with clear style */}
      <GlassView style={styles.tintedGlassView} glassEffectStyle="clear" />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  backgroundImage: {
    ...StyleSheet.absoluteFill,
    width: '100%',
    height: '100%',
  },
  glassView: {
    position: 'absolute',
    top: 100,
    left: 50,
    width: 200,
    height: 100,
    borderRadius: 12,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  tintedGlassView: {
    position: 'absolute',
    top: 250,
    left: 50,
    width: 200,
    height: 100,
    borderRadius: 12,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
});
```

### GlassContainer

The `GlassContainer` component allows you to combine multiple glass views into a combined effect.

**Platform:** iOS 26+

```jsx
import { StyleSheet, View, Image } from 'react-native';
import { GlassView, GlassContainer } from 'expo-glass-effect';

export default function GlassContainerDemo() {
  return (
    <View style={styles.container}>
      <Image
        style={styles.backgroundImage}
        source={{
          uri: 'https://images.unsplash.com/photo-1547036967-23d11aacaee0?w=400&h=600&fit=crop',
        }}
      />
      <GlassContainer spacing={10} style={styles.containerStyle}>
        <GlassView style={styles.glass1} isInteractive />
        <GlassView style={styles.glass2} />
        <GlassView style={styles.glass3} />
      </GlassContainer>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  backgroundImage: {
    ...StyleSheet.absoluteFillObject,
    width: '100%',
    height: '100%',
  },
  containerStyle: {
    position: 'absolute',
    top: 200,
    left: 50,
    width: 250,
    height: 100,
    flexDirection: 'row',
    alignItems: 'center',
    gap: 5,
  },
  glass1: {
    width: 60,
    height: 60,
    borderRadius: 30,
  },
  glass2: {
    width: 50,
    height: 50,
    borderRadius: 25,
  },
  glass3: {
    width: 40,
    height: 40,
    borderRadius: 20,
  },
});
```

## API

```javascript
import { GlassView, GlassContainer, isLiquidGlassAvailable } from 'expo-glass-effect';
```

### GlassContainer

**Type:** `React.Element<GlassContainerProps>`

#### Props

##### spacing

Optional • Type: `number` • Default: `undefined`

The distance at which glass elements start affecting each other. Controls when glass elements begin to merge together.

### GlassView

**Type:** `React.Element<GlassViewProps>`

#### Props

##### glassEffectStyle

Optional • Type: `GlassStyle` • Default: `'regular'`

Glass effect style to apply to the view.

Acceptable values: `'clear'` | `'regular'`

##### isInteractive

Optional • Type: `boolean` • Default: `false`

Whether the glass effect should be interactive.

##### tintColor

Optional • Type: `string`

Tint color to apply to the glass effect.

### Methods

#### isLiquidGlassAvailable()

Indicates whether the app is using the Liquid Glass design. The value will be `true` when the Liquid Glass components are available in the app.

This only checks for component availability. The value may also be `true` if the user has enabled accessibility settings that limit the Liquid Glass effect.

To check if the user has disabled the Liquid Glass effect via accessibility settings, use `AccessibilityInfo.isReduceTransparencyEnabled()`.

```jsx
import { isLiquidGlassAvailable } from 'expo-glass-effect';

export default function CheckLiquidGlass() {
  return (
    <Text>
      {isLiquidGlassAvailable()
        ? 'Liquid Glass effect is available'
        : 'Liquid Glass effect is not available'}
    </Text>
  );
}
```
