<script setup lang="ts">
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import { inject, provide, reactive } from "vue";
import type User from "./types/User";
import type Entry from "./types/Entry";
import { userInjectionKey } from "./injectionKey";

// const sum = (x: number, y: number) => x + y;
// sum(1,2)



const user: User = reactive({
  id: 1,
  username: "aaa bbb",
  settings: [],
});

provide(userInjectionKey, user);

//in child component
const injectedUser = inject(userInjectionKey);

console.log(user.id);

const entryArray: Array<Entry> = reactive([]);
// const entries: Entry[]= reactive([])

const handleCreateEntry = (entry: Entry) => {
  console.log(entry);
  // entryArray.push(entry);
  entryArray.unshift(entry);
};
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in entryArray" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
