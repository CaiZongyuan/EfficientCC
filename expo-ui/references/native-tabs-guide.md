# Native Tabs

Learn how to use the native tabs layout in Expo Router.

> **Native tabs is an experimental feature available in SDK 54 and later, and its API is subject to change.**

## Overview

Native tabs use the native system tab bar, providing platform-native navigation for iOS, Android, tvOS, and Web. Unlike custom or JavaScript tabs, native tabs follow platform conventions and behaviors.

**Key differences:**
- **Native tabs**: Use platform's system tab bar (iOS UITabBarController, Android Material Tabs)
- **JavaScript tabs**: Use React Navigation's tab navigator with full customization
- **Custom tabs**: Fully custom design using JavaScript

## When to Use Native Tabs

Choose native tabs when:
- Building platform-native interfaces that follow system conventions
- Wanting native performance and system integration
- Platform-specific features (iOS 26+ liquid glass effects, minimize behavior)
- Don't require extensive customization beyond system defaults

Choose JavaScript/custom tabs when:
- Need complete visual control over tab bar
- Require complex animations or custom layouts
- Need consistent cross-platform appearance

## Installation

Native tabs are part of `expo-router`. Install if not already present:

```bash
npx expo install expo-router
```

Ensure `expo-router` plugin is in app config:

```json
{
  "expo": {
    "plugins": ["expo-router"]
  }
}
```

## Basic Usage

### File-Based Routing

Create a file structure in `app/`:

```
app/
├── _layout.tsx
├── index.tsx
└── settings.tsx
```

### Define Tab Layout

**app/_layout.tsx:**

```tsx
import { NativeTabs, Icon, Label } from 'expo-router/unstable-native-tabs';

export default function TabLayout() {
  return (
    <NativeTabs>
      <NativeTabs.Trigger name="index">
        <Label>Home</Label>
        <Icon sf="house.fill" drawable="custom_android_drawable" />
      </NativeTabs.Trigger>
      <NativeTabs.Trigger name="settings">
        <Icon sf="gear" drawable="custom_settings_drawable" />
        <Label>Settings</Label>
      </NativeTabs.Trigger>
    </NativeTabs>
  );
}
```

**app/index.tsx and app/settings.tsx:**

```tsx
import { View, Text, StyleSheet } from 'react-native';

export default function Tab() {
  return (
    <View style={styles.container}>
      <Text>Tab [Home|Settings]</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

> Tabs are not automatically added. Explicitly define them using `NativeTabs.Trigger`.

## Customizing Tab Bar Items

### Icon

Customize tab icons using `Icon` component:

**SF Symbols (iOS):**

```tsx
<Icon sf={{ default: 'house', selected: 'house.fill' }} />
```

**Android Drawables:**

```tsx
<Icon drawable="ic_home" />
```

**Custom Images:**

```tsx
<Icon src={require('../../../assets/setting_icon.png')} />
<Icon src={{
  default: require('../../../assets/setting_icon.png'),
  selected: require('../../../assets/selected_setting_icon.png')
}} />
```

**Dynamic Colors (Liquid Glass):**

```tsx
import { DynamicColorIOS } from 'react-native';

<NativeTabs
  labelStyle={{
    color: DynamicColorIOS({
      dark: 'white',
      light: 'black',
    }),
  }}
  tintColor={DynamicColorIOS({
    dark: 'white',
    light: 'black',
  })}>
  <NativeTabs.Trigger name="index">
    <Icon sf={{ default: 'house', selected: 'house.fill' }} />
  </NativeTabs.Trigger>
</NativeTabs>
```

### Label

Customize tab labels:

```tsx
// Show label
<Label>Home</Label>

// Hide label
<Label hidden />

// Default: uses route name if no Label provided
```

### Badge

Add notification badges:

```tsx
<Badge>9+</Badge>
<Badge />  {/* Shows dot without text */}
```

## Advanced Features

### Hide Tabs Conditionally

```tsx
const shouldHideMessagesTab = true;

<NativeTabs.Trigger name="messages" hidden={shouldHideMessagesTab} />
```

> Hidden tabs cannot be navigated to in any way.

### Dismiss Behavior (iOS only)

By default, tapping an active tab closes all screens and returns to root. Disable with:

```tsx
<NativeTabs.Trigger name="index" disablePopToTop>
  <Label>Home</Label>
</NativeTabs.Trigger>
```

### Scroll to Top (iOS only)

By default, tapping an active tab scrolls content to top. Disable with:

```tsx
<NativeTabs.Trigger name="index" disableScrollToTop>
  <Label>Home</Label>
</NativeTabs.Trigger>
```

### iOS 26+ Features

> Requires Xcode 26 or higher.

**Separate Search Tab:**

```tsx
<NativeTabs.Trigger name="search" role="search">
  <Label>Search</Label>
</NativeTabs.Trigger>
```

**Tab Bar Search Input:**

```tsx
// app/search/_layout.tsx
import { Stack } from 'expo-router';

export default function SearchLayout() {
  return (
    <Stack>
      <Stack.Screen
        name="index"
        options={{
          title: 'Search',
          headerSearchBarOptions: {
            placement: 'automatic',
            placeholder: 'Search',
            onChangeText: () => {},
          },
        }}
      />
    </Stack>
  );
}
```

**Minimize Behavior:**

```tsx
<NativeTabs minimizeBehavior="onScrollDown">
  <NativeTabs.Trigger name="index">
    <Label>Home</Label>
  </NativeTabs.Trigger>
</NativeTabs>
```

Values: `'automatic'` | `'never'` | `'onScrollDown'` | `'onScrollUp'`

### Vector Icons Integration

```tsx
import MaterialIcons from '@expo/vector-icons/MaterialIcons';
import { VectorIcon } from 'expo-router/unstable-native-tabs';
import { Platform } from 'react-native';

<NativeTabs.Trigger name="index">
  <Label>Home</Label>
  {Platform.select({
    ios: <Icon sf="house.fill" />,
    android: <Icon src={<VectorIcon family={MaterialIcons} name="home" />} />,
  })}
</NativeTabs.Trigger>
```

## NativeTabs Component Props

### Tab Bar Styling

```tsx
<NativeTabs
  backgroundColor="white"
  iconColor="blue"
  tintColor="purple"
  blurEffect="systemMaterial"
  labelStyle={{ fontSize: 12, fontWeight: '600' }}
  labelVisibilityMode="auto"
  minimizeBehavior="automatic"
>
```

**Key Props:**
- `backgroundColor`: Tab bar background color
- `iconColor`: Default icon color
- `tintColor`: Selected icon color
- `blurEffect`: iOS blur effect (e.g., `'systemMaterial'`, `'extraLight'`, `'dark'`)
- `labelStyle`: Typography settings for labels
- `labelVisibilityMode`: Android label visibility (`'auto'` | `'selected'` | `'labeled'` | `'unlabeled'`)
- `minimizeBehavior`: iOS 26+ minimize behavior
- `backBehavior`: Android back button behavior (`'history'` | `'none'` | `'initialRoute'`)

### Per-Trigger Customization

Customize tab bar for specific triggers:

```tsx
<NativeTabs.Trigger name="page">
  <NativeTabs.Trigger.TabBar
    backgroundColor="white"
    iconColor="red"
  />
  <Label>Page</Label>
</NativeTabs.Trigger>
```

## Platform-Specific Considerations

### Android
- **Limit of 5 tabs** due to Material Design constraints
- Uses Material Tabs component
- Supports ripple effects and indicators

### iOS
- **No tab limit** (but 5 recommended for UX)
- Uses UITabBarController
- Supports blur effects, liquid glass (iOS 26+)
- Separate search tab support (iOS 26+)

### tvOS
- Follows tvOS platform conventions
- Different tab bar positioning

### Known Limitations

**Android:**
- Maximum 5 tabs
- Limited FlatList integration

**iOS:**
- Cannot measure tab bar height (position varies by device)
- FlatList scroll detection issues (use `disableTransparentOnScrollEdge`)

**General:**
- No nested native tabs (can nest JavaScript tabs inside native tabs)
- Limited FlatList support for scroll-to-top and minimize-on-scroll

## Migration from JavaScript Tabs

### Key Differences

**Use `Trigger` instead of `Screen`:**

```tsx
// Old (JavaScript tabs)
<Tabs.Screen
  name="home"
  options={{
    tabBarIcon: ({ focused, color, size }) => (
      <Icon name="home" color={color} size={size} />
    ),
  }}
/>

// New (Native tabs)
<NativeTabs.Trigger name="home">
  <Icon sf="house" />
  <Label>Home</Label>
</NativeTabs.Trigger>
```

**React Components instead of Props:**

Native tabs use React-first API with components instead of configuration objects.

**Use Stacks Inside Tabs:**

Native tabs don't have mock headers. Nest Stack layouts:

```tsx
// app/home/_layout.tsx
import { Stack } from 'expo-router';

export default function HomeLayout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ title: 'Home' }} />
      <Stack.Screen name="details" options={{ title: 'Details' }} />
    </Stack>
  );
}
```

## Common Patterns

### Conditional Tabs Based on Auth

```tsx
const { user } = useAuth();

return (
  <NativeTabs>
    <NativeTabs.Trigger name="index">
      <Label>Home</Label>
    </NativeTabs.Trigger>
    {user && (
      <NativeTabs.Trigger name="profile">
        <Label>Profile</Label>
      </NativeTabs.Trigger>
    )}
  </NativeTabs>
);
```

### Deep Linking to Specific Tabs

```tsx
// Navigate programmatically
router.push('/settings');
```

### Badge Updates

```tsx
const [unreadCount, setUnreadCount] = useState(0);

<NativeTabs.Trigger name="messages">
  {unreadCount > 0 && <Badge>{unreadCount > 99 ? '99+' : unreadCount}</Badge>}
  <Label>Messages</Label>
</NativeTabs.Trigger>
```

## Resources

- **Expo Router Native Tabs Guide**: https://docs.expo.dev/router/advanced/native-tabs/
- **API Reference**: https://docs.expo.dev/versions/latest/sdk/router-native-tabs/
- **Liquid Glass Tutorial**: Video guide for iOS 26+ glass effects
