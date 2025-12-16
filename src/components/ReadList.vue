<script setup lang="ts">
import type { Book } from '../types'
const { timeline } = defineProps<{ timeline: any[] }>()

const yearGroups = Object.groupBy(timeline.sort((a,b) => a.timestamp - b.timestamp), ({timestamp}) => new Date(timestamp).getFullYear())

function formatReadTimestamp(timestamp: number) {
  const date = new Date(timestamp);

  return `${date.getMonth() + 1} / ${date.getFullYear()}`
}
</script>

<template>
  <div v-for="([year, books], ind) in Object.entries(yearGroups)" :key="year">
    <h2> {{year}} | {{books.length}}</h2>

      <p v-for="(book, index) in books" :key="index">
        <a
          :href="`${book.title.url}`"
          target="_blank"
          >{{ book.title.text }} | {{ book.author }} | {{ formatReadTimestamp(book.timestamp) }}</a
        >
      </p>

  </div>
</template>

<style>
.read {
  list-style-image: url('../assets/check-mark.svg');
}
</style>
