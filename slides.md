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
- [ ] Data Persistence
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
- Q: Can I submit my mobile assignment after submitting my capstone assignment?
  - A: Yes.  Revisit [assignment weights](https://unit01-lesson01.netlify.app/4)

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

# Data Persistence

---
transition: slide-left
---

# I

---
transition: slide-left
---

# M

---
transition: slide-left
---

# S

---
layout: image-right
transition: slide-left
backgroundSize: 444px 380px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 👩‍💻 [React Native on FCC](https://www.freecodecamp.org/news/build-a-meditation-app-with-react-native-expo-router/)
- 📄 [UI: React Native Paper](https://reactnativepaper.com/)
- ⚛️ [UI: React Native Elements](https://reactnativeelements.com/)
- 🧭 [React Navigation](https://reactnavigation.org/)


<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)


---
transition: slide-left
---

# Homework

- Think about your Capstone Planning Project
   - can submit before due date if you wish