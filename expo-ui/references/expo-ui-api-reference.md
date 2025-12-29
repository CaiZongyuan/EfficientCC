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
