# Expo UI v10 Examples

This document contains example code from the local expo-ui-v10 directory showcasing v10-specific features.

> **⚠️ WARNING: v10 Breaking Changes**
> Examples in this file use Expo UI v10 features. The official documentation is based on v9 (stable). v10 may contain breaking changes and API differences. When using v10 features, be aware that:
> - Some components or modifiers may have different APIs than documented
> - New features in v10 may not be covered in official documentation yet
> - Always check TypeScript types for current API signature

## Glass Effect Example

Demonstrates the `GlassEffectContainer` and `glassEffect` modifier available in v10:

```tsx
import {
  Host,
  HStack,
  GlassEffectContainer,
  Image,
  Namespace,
  VStack,
  Button,
  Text,
} from '@expo/ui/swift-ui';
import {
  padding,
  glassEffect,
  animation,
  Animation,
  glassEffectId,
  background,
  cornerRadius,
  frame,
} from '@expo/ui/swift-ui/modifiers';

export default function GlassEffect() {
  const [isGlassExpanded, setIsGlassExpanded] = useState(false);
  const namespaceId = useId();

  return (
    <View style={{ flex: 1, experimental_backgroundImage: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)` }}>
      <Host style={{ flex: 1 }}>
        <VStack spacing={60} modifiers={[animation(Animation.spring({ duration: 0.8 }), isGlassExpanded)]}>
          <Namespace id={namespaceId}>
            <GlassEffectContainer
              spacing={30}
              modifiers={[
                animation(Animation.spring({ duration: 0.8 }), isGlassExpanded),
                padding({ all: 30 }),
                cornerRadius(20),
              ]}>
              <VStack spacing={25}>
                <HStack spacing={25}>
                  <Image
                    systemName="paintbrush.fill"
                    size={42}
                    modifiers={[
                      frame({ width: 50, height: 50 }),
                      padding({ all: 15 }),
                      glassEffect({ glass: { variant: 'clear' } }),
                      glassEffectId('paintbrush', namespaceId),
                      cornerRadius(15),
                    ]}
                  />
                </HStack>
              </VStack>
            </GlassEffectContainer>
          </Namespace>
        </VStack>
      </Host>
    </View>
  );
}
```

### Key v10 Features:
- `GlassEffectContainer` - Container for glass morphism effects
- `glassEffect()` modifier - Apply glass effect with variant options
- `glassEffectId()` - Link glass effects with namespace
- `Namespace` - Create namespace for shared effect IDs
- `animation()` modifier with `Animation.spring()`

## Button Styles Example (v10)

Shows new button style variants available in v10:

```tsx
import { Button, Host, List, Section, Label, Image, VStack, Text, Progress } from '@expo/ui/swift-ui';
import {
  background,
  buttonStyle,
  controlSize,
  disabled,
  fixedSize,
  foregroundStyle,
  labelStyle,
  padding,
  shapes,
  tint,
} from '@expo/ui/swift-ui/modifiers';

export default function ButtonScreen() {
  return (
    <Host style={{ flex: 1 }}>
      <List>
        <Section title="System Styles">
          <Button label="Default" />
          <Button label="Glass button" modifiers={[buttonStyle('glass')]} />
          <Button label="Glass Prominent" modifiers={[buttonStyle('glassProminent')]} />
          <Button label="Bordered" modifiers={[buttonStyle('bordered')]} />
          <Button label="Borderless" modifiers={[buttonStyle('borderless')]} />
          <Button label="Bordered Prominent" modifiers={[buttonStyle('borderedProminent')]} />
          <Button label="Plain" modifiers={[buttonStyle('plain')]} />
        </Section>

        <Section title="Control Size">
          <Button label="Mini" modifiers={[controlSize('mini'), buttonStyle('glassProminent'), fixedSize()]} />
          <Button label="Small" modifiers={[controlSize('small'), buttonStyle('bordered')]} />
          <Button label="Regular" modifiers={[controlSize('regular'), buttonStyle('glass')]} />
          <Button label="Large" modifiers={[controlSize('large'), buttonStyle('glassProminent')]} />
          <Button label="Extra Large" modifiers={[controlSize('extraLarge'), buttonStyle('glassProminent')]} />
        </Section>

        <Section title="Button Roles">
          <Button label="Default" role="default" />
          <Button label="Cancel" role="cancel" />
          <Button label="Destructive" role="destructive" />
        </Section>

        <Section title="Custom label">
          <Button>
            <VStack spacing={4}>
              <Image systemName="folder" />
              <Text>Folder</Text>
            </VStack>
          </Button>
          <Button>
            <Progress color="blue" variant="circular" />
          </Button>
        </Section>
      </List>
    </Host>
  );
}
```

### Key v10 Features:
- `buttonStyle('glass')` - Glass morphism button style
- `buttonStyle('glassProminent')` - Prominent glass variant
- `controlSize()` - Control size modifier (mini, small, regular, large, extraLarge)
- `fixedSize()` - Prevent button from growing
- `disabled()` - Disable button
- `labelStyle('iconOnly')` - Show only icon
- Button `role` prop (default, cancel, destructive)
- Custom labels with any SwiftUI content

## Form Components Example (v10)

Demonstrates new form components in v10:

```tsx
import {
  Button,
  ColorPicker,
  Host,
  Picker,
  Form,
  Section,
  Slider,
  Switch,
  Text,
  DisclosureGroup,
  ContentUnavailableView,
  LabeledContent,
} from '@expo/ui/swift-ui';
import { buttonStyle, font, pickerStyle, tag } from '@expo/ui/swift-ui/modifiers';

export default function FormScreen() {
  const [color, setColor] = useState<string | null>('blue');
  const [selectedIndex, setSelectedIndex] = useState<number | null>(null);
  const [sliderValue, setSliderValue] = useState<number>(0.5);
  const [switchValue, setSwitchValue] = useState<boolean>(true);
  const [disclosureGroupExpanded, setDisclosureGroupExpanded] = useState<boolean>(false);

  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section title="My form Section">
          <Text modifiers={[font({ size: 17 })]}>Some text!</Text>

          <LabeledContent label="Name">
            <Text>Beto</Text>
          </LabeledContent>

          <LabeledContent
            label={
              <>
                <Text>Single Subtitle</Text>
                <Text>Subtitle</Text>
              </>
            }>
            <Text>Single Subtitle Value</Text>
          </LabeledContent>

          <LabeledContent label="Labeled Slider">
            <Slider value={sliderValue} onValueChange={setSliderValue} />
          </LabeledContent>

          <Switch value={switchValue} label="This is a switch" onValueChange={setSwitchValue} />

          <ColorPicker
            label="Select a color"
            selection={color}
            supportsOpacity
            onValueChanged={setColor}
          />

          <Picker
            label="Menu picker"
            modifiers={[pickerStyle('menu')]}
            selection={selectedIndex}
            onSelectionChange={(selection) => setSelectedIndex(selection)}>
            {options.map((option, index) => (
              <Text key={index} modifiers={[tag(index)]}>
                {option}
              </Text>
            ))}
          </Picker>

          <DisclosureGroup
            onStateChange={setDisclosureGroupExpanded}
            isExpanded={disclosureGroupExpanded}
            label="Show Details">
            <Text>Name: John Doe</Text>
            <Text>Email: john.doe@example.com</Text>
          </DisclosureGroup>

          <ContentUnavailableView
            title="Card expired"
            systemImage="creditcard.trianglebadge.exclamationmark"
            description="Please update your payment information."
          />
        </Section>
      </Form>
    </Host>
  );
}
```

### Key v10 Features:
- `LabeledContent` - Wrapper for labeled form elements
- `DisclosureGroup` - Expandable/collapsible content sections
- `ContentUnavailableView` - Empty state/placeholder view
- `pickerStyle('menu')` modifier - Menu picker style
- `tag()` modifier - Tag picker options
- `font({ size: 17 })` modifier - Font customization
- `supportsOpacity` prop on ColorPicker
- Multi-part labels with JSX arrays

## Summary of v10 Additions

Based on the local examples, v10 includes:

### New Components:
- `GlassEffectContainer`
- `LabeledContent`
- `DisclosureGroup`
- `ContentUnavailableView`

### New Modifiers:
- `glassEffect()` - Apply glass morphism effect
- `glassEffectId()` - ID for glass effect coordination
- `buttonStyle()` - Extended button styles (glass, glassProminent)
- `controlSize()` - Size control (mini to extraLarge)
- `pickerStyle()` - Picker style variants
- `tag()` - Picker option tagging
- `font()` - Font styling
- `fixedSize()` - Prevent automatic sizing
- `disabled()` - Disable controls
- `labelStyle()` - Label display options

### New Utilities:
- `Namespace` - Create namespaced effect IDs
- `Animation` with spring() - Animation configurations

> **Note**: Always refer to TypeScript definitions for the most accurate API signatures, as v10 may have differences from v9 documentation.
