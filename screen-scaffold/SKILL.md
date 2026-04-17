---
name: screen-scaffold
description: Scaffold a new screen for the sidekick-app. Use when the user wants to add a new screen, page, or view. Generates the typed component, wires navigation, and follows the project's glassmorphic style conventions.
---

The user wants to scaffold a new React Native screen for the sidekick-app project.

## What to Ask (if not provided)
1. **Screen name** (e.g. "ProfileScreen", "NotificationsScreen")
2. **Screen type**: stack screen (pushed via navigation) or tab screen (added to bottom tab bar)?
3. **Tab icon** (if tab screen): which Lucide icon to use?
4. **Any params** the screen receives (e.g. `{ userId: string }`)?

## Project Conventions to Follow

**File location**: `c:/Users/liorm/sidekick-app/screens/<ScreenName>.tsx`

**Standard imports**:
```typescript
import React, { useState } from 'react';
import { View, Text, StyleSheet, ScrollView, TouchableOpacity } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';
import { LinearGradient } from 'expo-linear-gradient';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';
// Add Lucide icons as needed: import { IconName } from 'lucide-react-native';
// Add context hooks as needed: import { useAuth } from '../context/AuthContext';
```

**Component signature**:
```typescript
type Props = NativeStackScreenProps<RootStackParamList, 'ScreenName'>;

export default function ScreenName({ navigation, route }: Props) {
```

**Glassmorphic card pattern**:
```typescript
<View style={styles.card}>
  <LinearGradient
    colors={['rgba(255,255,255,0.12)', 'rgba(255,255,255,0.04)']}
    start={{ x: 0, y: 0 }}
    end={{ x: 0, y: 1 }}
    style={[StyleSheet.absoluteFill, { borderRadius: 16 }]}
  />
  {/* card content */}
</View>
```

**Background gradient** (dark theme used across all screens):
```typescript
<LinearGradient colors={['#1a0533', '#0d0d1a']} style={styles.container}>
```

**Accent colors**: `#E8612A` (primary orange), `#22C55E` (green/success), `rgba(255,255,255,0.5)` (borders)

**StyleSheet pattern**:
```typescript
const styles = StyleSheet.create({
  container: { flex: 1 },
  safeArea: { flex: 1, paddingHorizontal: 20 },
  card: {
    borderRadius: 16,
    borderWidth: 1,
    borderColor: 'rgba(255,255,255,0.15)',
    overflow: 'hidden',
    padding: 16,
    marginBottom: 12,
  },
});
```

## Steps to Follow

1. Generate the screen file at `screens/<ScreenName>.tsx` using the conventions above.
2. **Add to `RootStackParamList`** in `App.tsx`:
   - Open `App.tsx`, find `RootStackParamList`, add the new screen with its params (or `undefined` if none).
3. **Wire navigation**:
   - For a **stack screen**: Add `<Stack.Screen name="ScreenName" component={ScreenName} />` inside the Stack.Navigator in App.tsx. Import the component at the top.
   - For a **tab screen**: Add `<Tab.Screen name="ScreenName" component={ScreenName} options={{ tabBarIcon: ... }} />` inside the Tab.Navigator. Use a Lucide icon with fill when focused, stroke when not.
4. Report: file created, App.tsx changes made, how to navigate to it (e.g. `navigation.navigate('ScreenName', { param })`).
