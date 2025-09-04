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
- [ ] Haptics and Notications

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
    if (item1.completedAtTimestamp && item2.completedAtTimestamp) {
      return item2.completedAtTimestamp - item1.completedAtTimestamp;
    }

    if (item1.completedAtTimestamp && !item2.completedAtTimestamp) {
      return 1;
    }

    if (!item1.completedAtTimestamp && item2.completedAtTimestamp) {
      return -1;
    }

    if (!item1.completedAtTimestamp && !item2.completedAtTimestamp) {
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
- Copy the [Usage code](https://docs.expo.dev/versions/latest/sdk/haptics/#installation) and put it in our `idea.tsx` to try it out all the haptics vibes

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

# Push Notifications

---
transition: slide-left
---

# Scheduling Notifications

---
transition: slide-left
---

# Homework

- Think about your Mobile App Assignment due Oct. 5
- Think about Capstone Planning Project
   - can submit before due date if you wish