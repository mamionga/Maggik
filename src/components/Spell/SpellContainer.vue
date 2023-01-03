<script setup lang="ts">
import {Spell} from "../MagicSearch/MagicSearch.types";
import styles from "./SpellContainer.module.scss";
import {computed} from "vue";

const props = defineProps<{
  spell: Spell
}>()

const spell = computed(() => props.spell)
</script>

<template>
  <div :class="styles.spell">
    <div :class="styles.level">{{ spell.level }} - {{ spell.school }}</div>
    <ul>
      <li><strong>Casting Time: </strong>
        <span v-if="spell.ritual === 'yes'"> Ritual.</span>
        {{ spell.casting_time }}
      </li>
      <li><strong>Range: </strong>{{ spell.range }}</li>
      <li><strong>Components: </strong>{{ spell.components }}</li>
      <li><strong>Duration: </strong>
        <span v-if="spell.concentration === 'yes'"> Concentration.</span>
        {{ spell.duration }}</li>
      <li><strong>Classes: </strong>{{ spell.class }}</li>
    </ul>
    <div :class="styles.description" v-html="spell.desc"></div>
    <div :class="styles.page">{{spell.page}}</div>
  </div>
</template>