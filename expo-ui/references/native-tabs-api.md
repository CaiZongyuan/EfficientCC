# Native Tabs API Reference

Expo Router submodule that provides native tabs layout.

> **Native tabs is an experimental feature available in SDK 54 and later, and its API is subject to change.**

## Installation

Ensure `expo-router` is installed and configured in app config:

```bash
npx expo install expo-router
```

**app.json:**
```json
{
  "expo": {
    "plugins": ["expo-router"]
  }
}
```

## API

```javascript
import { NativeTabs, Icon, Label, Badge, VectorIcon } from 'expo-router/unstable-native-tabs';
```

## Components

### NativeTabs

Main container for native tab layout.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `backgroundColor` | All | `ColorValue` | Tab bar background color |
| `iconColor` | All | `ColorValue` | Default icon color |
| `tintColor` | All | `ColorValue` | Selected icon color |
| `blurEffect` | iOS | `string` | Blur effect (`'systemMaterial'`, `'extraLight'`, `'dark'`, etc.) |
| `labelStyle` | All | `NativeTabsLabelStyle` | Typography settings for labels |
| `labelVisibilityMode` | Android | `'auto' \| 'selected' \| 'labeled' \| 'unlabeled'` | Label visibility |
| `minimizeBehavior` | iOS 26+ | `'automatic' \| 'never' \| 'onScrollDown' \| 'onScrollUp'` | Minimize behavior |
| `backBehavior` | Android | `'history' \| 'none' \| 'initialRoute'` | Back button behavior |
| `disableIndicator` | Android | `boolean` | Disable active indicator |
| `disableTransparentOnScrollEdge` | iOS | `boolean` | Prevent transparency on scroll edge |
| `badgeBackgroundColor` | All | `ColorValue` | Badge background color |
| `badgeTextColor` | Android/Web | `ColorValue` | Badge text color |
| `indicatorColor` | Android/Web | `ColorValue` | Tab indicator color |
| `rippleColor` | Android | `ColorValue` | Ripple effect color |
| `shadowColor` | iOS | `ColorValue` | Shadow color |
| `titlePositionAdjustment` | iOS | `{horizontal: number, vertical: number}` | Label position adjustment |

**Example:**
```tsx
<NativeTabs
  backgroundColor="white"
  iconColor="blue"
  tintColor="purple"
  blurEffect="systemMaterial"
  labelStyle={{ fontSize: 12, fontWeight: '600' }}
>
  <NativeTabs.Trigger name="index">
    <Label>Home</Label>
  </NativeTabs.Trigger>
</NativeTabs>
```

### NativeTabTrigger (NativeTabs.Trigger)

Defines a tab in the tab bar. Use `NativeTabs.Trigger` alias.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `name` | All | `string` | **Required** in _layout files. Route name |
| `children` | All | `ReactNode` | Icon, Label, Badge components |
| `hidden` | All | `boolean` | Hide tab from tab bar |
| `disablePopToTop` | iOS | `boolean` | Disable pop to root on re-tap |
| `disableScrollToTop` | iOS | `boolean` | Disable scroll to top on re-tap |
| `role` | iOS | `'search' \| 'history' \| 'bookmarks' \| ...` | System-provided tab item |
| `options` | All | `NativeTabOptions` | Legacy options object (use components instead) |

**System Roles (iOS):**
- `'search'` - Separate search tab
- `'history'`, `'bookmarks'`, `'contacts'`, `'downloads'`, `'favorites'`, `'featured'`, `'more'`, `'mostRecent'`, `'mostViewed'`, `'recents'`, `'topRated'`

**Example:**
```tsx
<NativeTabs.Trigger name="home" disableScrollToTop>
  <Icon sf="house.fill" />
  <Label>Home</Label>
  <Badge>3</Badge>
</NativeTabs.Trigger>
```

### Icon

Tab icon component.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `sf` | iOS | `string \| {default: string, selected: string}` | SF Symbol name |
| `drawable` | Android | `string` | Android drawable resource name |
| `src` | All | `ImageSource \| {default, selected}` | Custom image source |
| `selectedColor` | iOS | `ColorValue` | Selected state color |

**Examples:**

SF Symbols:
```tsx
<Icon sf={{ default: 'house', selected: 'house.fill' }} />
```

Android Drawables:
```tsx
<Icon drawable="ic_home" />
```

Custom Images:
```tsx
<Icon src={require('./assets/home.png')} />
<Icon src={{
  default: require('./assets/home.png'),
  selected: require('./assets/home-active.png')
}} />
```

### Label

Tab label component.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `children` | All | `string` | Label text |
| `hidden` | All | `boolean` | Hide label |
| `selectedStyle` | All | `NativeTabsLabelStyle` | Selected state style |

**Example:**
```tsx
<Label>Home</Label>
<Label hidden />  {/* Hide label */}
```

### Badge

Notification badge for tabs.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `children` | All | `string` | Badge text (e.g., "9+") |
| `hidden` | All | `boolean` | Hide badge |
| `selectedBackgroundColor` | All | `ColorValue` | Background color |

**Example:**
```tsx
<Badge>9+</Badge>
<Badge />  {/* Shows dot without text */}
```

### NativeTabsTriggerTabBar (NativeTabs.Trigger.TabBar)

Customize tab bar style when a specific trigger is selected.

**Props:**
- Same as `NativeTabs` but applies only when the tab is selected

**Example:**
```tsx
<NativeTabs.Trigger name="page">
  <NativeTabs.Trigger.TabBar backgroundColor="white" />
  <Label>Page</Label>
</NativeTabs.Trigger>
```

### VectorIcon

Helper for using `@expo/vector-icons` with native tabs.

**Props:**

| Prop | Platform | Type | Description |
|------|----------|------|-------------|
| `family` | All | `VectorIconFamily` | Icon family (e.g., `MaterialIcons`) |
| `name` | All | `string` | Icon name |

**Example:**
```tsx
import MaterialIcons from '@expo/vector-icons/MaterialIcons';
import { VectorIcon } from 'expo-router/unstable-native-tabs';

<Icon src={<VectorIcon family={MaterialIcons} name="home" />} />
```

## Interfaces

### NativeTabsLabelStyle

Typography style for tab labels.

```typescript
{
  color?: ColorValue;
  fontFamily?: string;
  fontSize?: number;
  fontStyle?: 'italic' | 'normal';
  fontWeight?: NumericFontWeight | '100' | '200' | ... | '900';
}
```

### NativeTabOptions

Legacy options object (prefer using child components).

```typescript
{
  title?: string;
  icon?: SymbolOrImageSource;
  selectedIcon?: SymbolOrImageSource;
  badgeValue?: string;
  iconColor?: ColorValue;
  labelStyle?: NativeTabsLabelStyle;
  backgroundColor?: ColorValue;
  blurEffect?: string;
  role?: 'search' | 'history' | ...;
  // ... and more
}
```

### SymbolOrImageSource

Icon source type.

```typescript
// SF Symbol (iOS)
type SymbolOrImageSource = {
  sf?: string;
  drawable?: string;  // Android
}

// OR

// Custom image
type SymbolOrImageSource = {
  src?: ImageSourcePropType | {default: ..., selected: ...};
}
```

## Platform-Specific Notes

### Android
- **Limit**: Maximum 5 tabs (Material Design constraint)
- Uses Material Tabs component
- Supports ripple effects and indicators

### iOS
- No tab limit (5 recommended for UX)
- Uses UITabBarController
- Supports blur effects and liquid glass (iOS 26+)
- Separate search tab support (iOS 26+)
- Role-based system tabs

### tvOS
- Follows tvOS platform conventions
- Different tab bar positioning

### Web
- Basic tab support
- Limited native feel

## Known Limitations

- **No nested native tabs**: Cannot nest native tabs inside native tabs (can nest JavaScript tabs inside)
- **Limited FlatList support**: Scroll-to-top and minimize-on-scroll may not work properly
- **Cannot measure tab bar height**: Height varies by device (iPad, Vision Pro, etc.)

## Migration from JavaScript Tabs

Key differences:

| JavaScript Tabs | Native Tabs |
|-----------------|-------------|
| `Tabs.Screen` | `NativeTabs.Trigger` |
| Options object | Child components (`<Icon>`, `<Label>`) |
| Automatic tabs | Explicit trigger definition |
| Mock headers | Use nested `<Stack />` |

**Before:**
```tsx
<Tabs.Screen
  name="home"
  options={{
    tabBarIcon: ({ color }) => <Icon name="home" color={color} />,
  }}
/>
```

**After:**
```tsx
<NativeTabs.Trigger name="home">
  <Icon sf="house.fill" />
  <Label>Home</Label>
</NativeTabs.Trigger>
```

## Resources

- **Guide**: https://docs.expo.dev/router/advanced/native-tabs/
- **API Reference**: https://docs.expo.dev/versions/latest/sdk/router-native-tabs/
- **Expo Router**: https://docs.expo.dev/router/
