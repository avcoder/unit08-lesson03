---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# React Native
Mobile Development: Unit 08 - Lesson 03

- [ ] Deleting, Sorting
- [ ] Async Storage, Layout Animations
- [ ] Haptics and Local Push Notications

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Recap 

- see https://github.com/avcoder/mobile-react-native-01
- We've built the UI to add items to our note-taking/to-do app
- We've included routing using Expo Router and experimented with different ways of navigating (Stacked, Modal, programmatically, Tabbed)
- We've experimented with different ways to list our items: ScrollView vs FlatList

## Capstone and Mobile Assignment Due Dates
- Q: Can I submit my mobile assignment after submitting my capstone assignment?
  - A: Yes.  Revisit [assignment weights](https://unit01-lesson01.netlify.app/4)
- So for due dates, as long as I get both Mobile App, and Capstone before Oct 23, then we're all good.

---
transition: slide-left
---

# Deleting Items

- in `components/ShoppingListItem.tsx` in our `handleDelete` function, modify our TS and `onPress`
   ```tsx  
   type ShoppingListItemProps = {
    ...
    onDelete: () => void;
   }

   const ShoppingListItem = ({ name, isCompleted, onDelete }
    ...
    onPress: onDelete,
   ```
- in `./app/index.tsx` create handleDelete
  ```tsx
  const handleDelete = (id: string) => {
    const newShoppingList = shoppingList.filter(item => item.id !== id)
    setShoppingList(newShoppingList);
  }
  ...
  renderItem={ ({item}) => <ShoppingListItem ... onDelete={() => handleDelete(item.id)} />}
  ```
  - try deleting items now

---
transition: slide-left
---

# Completing an item (pg.1)

- in ShoppingListITem.tsx let's begin the infrastructure to place an checkmark icon beside our text
- Change our `<View>` to `<Pressable>`
  ```tsx
  import { ... Pressable } from 'react-native';
  ...
  type ShoppingListItemProps = {
    ...
    onToggleComplete: () => void;
  ...
  const ShoppingListItem = ({ name, isCompleted, onDelete, onToggleComplete }: ShoppingListItemProps) => {
  ...
  <Pressable
    ...
    onPress={onToggleComplete}
  ```

---
transition: slide-left
---

# Completing an item (pg.2)

- in `/app/index.tsx` add another property:
  ```tsx
  type ShoppingListItemType = {
    ...
    completedAt?: number;
  ...
  const handleToggleComplete = (id: string) => {
    const newShoppingList = shoppingList.map((item) => {
      if (item.id === id) {
        return {
          ...item,
          completedAt: item.completedAt ? undefined : Date.now(),
        }
      }
      return item;
    });
    setShoppingList(newShoppingList);
  }
  ...
  <ShoppingListItem
    ...
    onToggleComplete={() => handleToggleComplete(item.id)}
    isCompleted={!!item.completedAt}
  ```

---
transition: slide-left
---

# Add circle vs checkmark, if completed (pg.1)

- import any checkmark icon from [expo/vector-icons](https://icons.expo.fyi/Index)
- in `ShoppingListItem.tsx`, place checkmark icon inside our `<Pressable>`, but above our `<Text>`
- may have to wrap both icon and `<Text>` inside a `<View style={styles.row}>` 
  - use `flexDirection: "row", gap: 16`

## Exercise
- Q: What happens if you enter really long text?  Does it break the design?  Add the following:
  ```tsx
  <Text
    numberOfLines={1}
  ...
  itemText: {
    ...
    flex: 1,
  ...
  row: {
    ...
    flex: 1
  ```

---
transition: slide-left
---

# Exercise: Add circle vs checkmark (pg.2)

- Make it so that the checkmark icon appears if the item is completed; otherwise a circle icon appears
- Tip: Perhaps use ternary operator within the icon component itself for the `name` prop
  `<WhateverIconLibrary name={ someCondition ? "check" : "circle" }`
- Optional: adjust colors for completed vs incomplete using our theme.ts



---
transition: slide-left
---

# Ordering & Sorting (pg.1)

- Q: How might you accomplish the following:
  - if an item is completed, it moves to the bottom of the list and vice versa
- in `app/index.tsx`, add a new property:
  ```tsx
  type ShoppingListItemType = {
    ...
    lastUpdated: number;
  ...
  const handleSubmit = () => {
    ...
    { id: ..., lastUpdated: Date.now()}
  ...
  const handleToggleComplete = (id: string) => {
    ...
    return {
      ...
      lastUpdated: Date.now(),
  ```

---
transition: slide-left
---

# Exercise: Ordering & Sorting (pg.2)

- Paste following in our `app/index.tsx` -- Where would you call it?
```tsx
function orderShoppingList(shoppingList: ShoppingListItemType[]) {
  return shoppingList.sort((item1, item2) => {
    if (item1.completedAt && item2.completedAt) {
      return item2.completedAt - item1.completedAt;
    }

    if (item1.completedAt && !item2.completedAt) {
      return 1;
    }

    if (!item1.completedAt && item2.completedAt) {
      return -1;
    }

    if (!item1.completedAt && !item2.completedAt) {
      return item2.lastUpdatedTimestamp - item1.lastUpdatedTimestamp;
    }

    return 0;
  });
}
```

<!--
call it within FlatList data prop {orderShoppingList(shoppingList)}
-->

---
transition: slide-left
---

# Async Storage (pg.1)

- Q: What browser feature allows you to persist data?  (Hint: 2 possibilities)
- In React Native we have Async Storage
- `npx expo install @react-native-async-storage/async-storage`
- create a new folder/file:  `/utils/storage.ts`
  ```tsx
  import AsyncStorage from "@react-native-async-storage/async-storage";

  export async function getFromStorage(key: string) {
    try {
      const data = await AsyncStorage.getItem(key);
      return data ? JSON.parse(data) : null;
    } catch {
      return null;
    }
  }

  export async function saveToStorage(key: string, data: object) {
    try {
      await AsyncStorage.setItem(key, JSON.stringify(data));
    } catch {}
  }
  ```

<!--
localStorage, indexedDB
-->

---
transition: slide-left
---

# Async Storage (pg.2)

- in `app/index.tsx`:
  ```tsx
  import { getFromStorage, saveToStorage } from "../utils/storage";
  ...
  const STORAGE_KEY = "shopping-list";
  ...
  useEffect(() => {
    const fetchInitial = async () => {
      const data = await getFromStorage(STORAGE_KEY)
      if (data) {
        setShoppingList(data);
      }
    };
    fetchInitial();
  }, [])
  ...
  // in handleSubmit, handleDelete AND handleToggleComplete:
    saveToStorage(STORAGE_KEY, shoppingList)
  ```
- Test: input a few items, reload app -- do items persist now?

---
transition: slide-left
---

# Layout Animations

- A simple, but limited way to get full-page animations
- import `LayoutAnimation`:
  ```tsx
  import { ... LayoutAnimation } from 'react-native';
  ```
- Must call Layout Animation before you do a state change
  ```tsx
  LayoutAnimation.configureNext(LayoutAnimation.Presets.easeInEaseOut)
  ```

## Exercise
- Implement all 4 places where you would call `LayoutAnimation`
  - Hint: must call it before we do a state change for shoppingList
- Reload your app, and try toggling an item to complete it -- does item animate?
- Try the other animation presets as listed in the [Docs](https://reactnative.dev/docs/layoutanimation#presets)

---
transition: slide-left
---

# Haptics
Vibrations your phone generates to improve UX.  

- expo-haptics provides haptic (touch) feedback for:
  - Android devices using Vibrator system service.
  - iOS 10+ devices using the Taptic Engine.
  - Web platforms using the Web Vibration API.
- On iOS, the Taptic engine will do nothing if any of the following conditions are true on a user's device:
  - Low Power Mode is enabled. This can be detected with expo-battery.
  - User disabled the Taptic Engine in settings.
  - iOS Camera is active (to prevent destabilization).
  - iOS dictation is active (to not disturb the microphone input).
- On web, the library uses the Web Vibration API. Note the following:
  - The API must be supported by the browser (check browser compatibility)
  - The device must have vibration hardware
  - The user must grant permission to use vibration (usually automatic)


---
transition: slide-left
---

# Exercise: Haptics Installation and Usage
View the [docs](https://docs.expo.dev/versions/latest/sdk/haptics/#installation)

- `npx expo install expo-haptics`
- Copy the [Usage code](https://docs.expo.dev/versions/latest/sdk/haptics/#installation) and put it in our `idea.tsx` to try out all the haptics.  Feel the vibes! 〰️

```tsx
<Button
  title="Success"
  onPress={
    () =>
      Haptics.notificationAsync(
        Haptics.NotificationFeedbackType.Success
      )
  }
/>
```


---
transition: slide-left
---

# Differences between Haptics
`Haptics.impactAsync` vs `Haptics.notificationAsync` vs `Haptics.selectionAsync`?

| Method                    | Purpose                      | Feedback Style                    | Use Cases                     |
| ------------------------- | ---------------------------- | --------------------------------- | ----------------------------- |
| `selectionAsync()`        | Selection changes            | Light, subtle tick                | Picker, tabs, dropdowns       |
| `impactAsync(style)`      | UI interactions (press/drag) | Light / Medium / Heavy bump       | Buttons, drag & drop, toggles |
| `notificationAsync(type)` | Status or event feedback     | Success / Warning / Error pattern | Form success/error, alerts    |



---
transition: slide-left
---

# Exercise: Haptics
Now that you know how to use Haptics, let's place it in our app

1. Implement a haptic (Ex: medium impact) when the user deletes an item

2. Implement a haptic when the user completes an item

3. Implement a haptic when the user marks a previously completed item, incomplete.

```tsx
import * as Haptics from "expo-haptics";
...
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
...
if (item.completedAt) {
  Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
} else {
   Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
}
```

---
layout: image-right
transition: slide-left
image: /assets/context.png
backgroundSize: 444px 380px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🏇 [React Native Re-Animated](https://docs.swmansion.com/react-native-reanimated/)
- 💫 [Rising Stars 2024](https://risingstars.js.org/2024/en#section-all) 
- ⚖️ [Problem with React useContext](https://x.com/aidenybai/status/1876689003661136094)
- 🧮 [Comp Sci Papers every Dev should read](https://newsletter.techworld-with-milan.com/p/computer-science-papers-every-developer)
- 🤖 [AI and Illeterate Programmers](https://nmn.gl/blog/ai-illiterate-programmers)
- 📚 [React Libraries](https://www.robinwieruch.de/react-libraries/)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

---
transition: slide-left
---

# Local Push Notifications

- Push Notifications are alerts that are generated by an application while it's closed.
  - [Remote notifications](https://docs.expo.dev/push-notifications/sending-notifications/) are sent from server to device using the device push token
  - Local notifications are scheduled from within the app are sent to local device only
- `npx expo install expo-notifications expo-device`
  ```tsx
  import * as Notifications from 'expo-notifications'; // official Expo package for handling notifications
  Notifications.scheduleNotificationAsync({
    content: {
      title: "Hello",
      body: "This is a test notification",
    },
    trigger: { seconds: 5 },
  });
  ```
    ```tsx
    import * as Device from 'expo-device';  // provides device info: model name, OS, device type, manufacturer etc.
    console.log(Device.modelName);          // e.g. "iPhone 14 Pro"
    console.log(Device.osName);             // "iOS" or "Android"
    console.log(Device.isDevice);           // true (on real device), false (simulator)
    ```
- But before we can send any notifs, must ask permission first.  Let's add a utility to do this for us...

---
transition: slide-left
---

# Local Push Notifications: Permissions
```tsx
import { Platform } from "react-native"; // create this file in utils/registerForPushNotificationsAsync.ts
import * as Device from "expo-device";
import * as Notifications from "expo-notifications";
export async function registerForPushNotificationsAsync() { // devs can only ask users once; else user > settings
  if (Platform.OS === "android") { // Android requires all notifs be assigned to a channel
    await Notifications.setNotificationChannelAsync("default", {
      name: "default",
      importance: Notifications.AndroidImportance.DEFAULT,
      vibrationPattern: [0, 250, 250, 250],
      showBadge: false,
    });
  }
  
  if (Device.isDevice) { // checks if running on a real physical device. Notifs may not work on emulators
    const { status: existingStatus } = await Notifications.getPermissionsAsync(); 
    if (existingStatus !== "granted") { // hover status to see other options besides 'granted'
      const { status } = await Notifications.requestPermissionsAsync(); // Makes permission popup appear
      return status;
    } else {
      return existingStatus;
    }
  } else {
    return null;
  }
}
```

---
transition: slide-left
---

# Local Push Notifications: Usage (pg.1)

- in `/counter/index.tsx`:
  ```tsx
  import { Text, View, StyleSheet, TouchableOpacity } from "react-native";
  import { theme } from "../../theme";
  import { registerForPushNotificationsAsync } from "../../utils/registerForPushNotificationsAsync";

  export default function CounterScreen() {
    const handleRequestPermission = async () => {
      const result = await registerForPushNotificationsAsync();
      console.log("Permission: ", result);
    };

    return (
      <View style={styles.container}>
        <TouchableOpacity
           onPress={handleRequestPermission}
           style={styles.button}
           activeOpacity={0.8}
        >
          <Text style={styles.buttonText}>Request permission</Text>
        </TouchableOpacity>
     </View>
   );
  }
  ```

---
transition: slide-left
---

# Local Push Notifications: Usage (pg.2)

- add some styles
```tsx
  button: {
    backgroundColor: theme.colorBlack,
    padding: 12,
    borderRadius: 6,
  },
  buttonText: {
    color: "#fff",
    fontWeight: "bold",
    textTransform: "uppercase",
    letterSpacing: 1,
```

---
transition: slide-left
---

# Scheduled Notifications

- to actually setup a notification:
```tsx
import { ... Alert } from "react-native";
import * as Notifications from "expo-notifications";
```

- remove handleRequestPermission function and replace with:
```tsx
const scheduleNotification = async () => {
  const result = await registerForPushNotificationsAsync();
  if (result === "granted") {
      await Notifications.scheduleNotificationAsync({
        content: { title: "I'm a notification from your app! 📨" },
        trigger: { type: Notifications.SchedulableTriggerInputTypes.TIME_INTERVAL, seconds: 5 },
      });
    } else {
      Alert.alert("Unable to schedule notification", "Enable the notifications permission for Expo Go in settings");
    }
   };
   ...
   onPress={scheduleNotification}
   ...
   <Text style={styles.buttonText}>Schedule notification</Text>
```

---
transition: slide-left
---

# Challenge Exercise

- Now that you know how to set up notifications after 5 seconds, try the following challenge:
- Create a Timer, where the user can input the number of seconds and a notification with Haptics will appear after that time has elapsed

---
transition: slide-left
---

# Homework

- Think about your Mobile App Assignment due Oct. 5
- Think about Capstone Planning Project
   - can submit before due date if you wish