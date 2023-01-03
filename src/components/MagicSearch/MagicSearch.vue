<script setup lang="ts">
import styles from './MagicSearch.module.scss';
import Autocomplete from "../Autocomplete/Autocomplete.vue";
const items = [
  'Apple',
  'Banana',
  'Orange',
  'Mango',
  'Pear',
  'Peach',
  'Grape',
  'Tangerine',
  'Pineapple'
];
</script>

<template>
  <div :class="styles.container">
    <h1>Magic Search</h1>
    <Autocomplete :items="items" />
  </div>
</template>