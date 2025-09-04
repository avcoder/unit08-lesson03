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
- in `./app/index.tsx` create onDelete
  ```tsx
  const handleDelete = (id: string) => {
    const newShoppingList = shoppingList.filter(item => item.id !== id)
    setShoppingList(newShoppingList);
  }
  ...
  renderItem={ ({item}) => <ShoppingListITem ... onDelete={() => handleDelete(item.id)} />}
  ```
---
transition: slide-left
---

# S

---
transition: slide-left
---

# S

---
transition: slide-left
---

# E

---
transition: slide-left
---

# L

---
transition: slide-left
---

# C

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