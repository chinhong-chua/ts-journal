<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";

import type Emoji from "@/types/Emoji";
import { ref, type Ref, computed, onMounted } from "vue";
import type Entry from "@/types/Entry";
import { inject } from "vue";
import { userInjectionKey } from "@/injectionKey";

const user = inject(userInjectionKey)

const text = ref("");

// const emoji : Ref<Emoji | null> = ref(null);
const emoji = ref<Emoji | null>(null);

const charCount = computed<number>(() => {
  return text.value.length;
});

const maxChars = 280;

const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;

  // console.log(textarea.value)
  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    text.value = textarea.value = textarea.value.substring(0, maxChars);
  }
};

//events
// defineEmits(["@create"]);

// defineEmits<{
//   (e: "@create", entry: { text: string; emoji: Emoji | null }): void;
// }>();
// defineEmits<{
//   (e: "@create", entry: Entry): void;
// }>();
const emit = defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

const handleSubmit = () => {
  emit("@create", {
    body: text.value,
    emoji: emoji.value,
    createdAt: new Date(),
    userId: 1,
    id: Math.random(),
  });
  text.value = "";
  emoji.value = null;
};

//template refs
const textarea = ref<HTMLTextAreaElement | null>(null);

onMounted(() => textarea.value?.focus());
</script>
<template>
  <!-- <form
    class="entry-form"
    @submit.prevent="
      $emit('@create', {
        body: text,
        emoji,
        createdAt: new Date(),
        userId: 1,
        id: Math.random(),
      })
    "
  > -->
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea
      ref="textarea"
      :value="text"
      @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${user?.username || 'anonymous'}`"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
