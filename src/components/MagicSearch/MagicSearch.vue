<script setup lang="ts">
import {ref} from "vue";
import styles from './MagicSearch.module.scss';
import Autocomplete from "../Autocomplete/Autocomplete.vue";
import spellsJson from '../../data/spells.json';
import {Spell} from "./MagicSearch.types";
import SpellContainer from "../Spell/SpellContainer.vue";

const items: Spell[] = spellsJson.sort((a, b) => {
  if (a.name < b.name) {
    return -1;
  }
  if (a.name > b.name) {
    return 1;
  }
  return 0;
});

const spell = ref<Spell | null>(null);

const setSpell = (toSet: Spell) => {
  spell.value = toSet;
}
</script>

<template>
  <div :class="styles.container">
    <h1>Magic Search</h1>
    <Autocomplete :items="items" @spell="setSpell" />
    <SpellContainer v-if="spell" :spell="spell" />
  </div>
</template>