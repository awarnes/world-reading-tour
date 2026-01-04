<script setup lang="ts">
import countries from './assets/country-book-data.json'
import {timeline} from './assets/timeline-01-03-2026.json'
import ReadList from './components/ReadList.vue'
import BookList from './components/BookList.vue'
import FullMap from './components/FullMap.vue'
import { ref } from 'vue'

const menu = ref()
const map = ref()
const countryBooks = ref([])
const readListDialog = ref()

function showModal() {
  menu.value.showModal()
}

function closeModal() {
  menu.value.close()
  countryBooks.value = []
}

function showReadList() {
  readListDialog.value.showModal()
}

function closeReadList() {
  readListDialog.value.close()
}

function zoomIn() {
  map.value.zoomIn()
}

function zoomOut() {
  map.value.zoomOut()
}
</script>

<template>
  <div class="floating">
    <img class="highlight" src="./assets/menu.svg" @click.stop="showModal()" />
    <img class="highlight" src="./assets/plus.svg" @click="zoomIn" />
    <img class="highlight" src="./assets/minus.svg" @click="zoomOut" />
  </div>

  <FullMap ref="map" />

  <dialog ref="readListDialog" @close="closeReadList" :style="{width: '100vw', textAlign: 'center'}">
    <button @click="closeReadList">X</button>

    <ReadList :timeline="timeline" />
  </dialog>

  <dialog ref="menu" @close="closeModal">
    <button @click="showReadList">Read Books</button>
    <br />
    <br />
    <label for="country-select">Select a country</label>
    <br />
    <select id="country-select" v-model="countryBooks">
      <option :value="[]">--Select a country--</option>
      <option v-for="country in countries" :key="country.id" :value="country.books">
        {{ country.title }}
      </option>
    </select>
    <BookList :books="countryBooks" />
  </dialog>
</template>

<style>
.floating {
  position: absolute;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  background-color: whitesmoke;
}

.highlight:hover {
  background-color: aliceblue;
}
</style>
