<script setup lang="ts">
import {onMounted, onUnmounted, ref, VNodeRef} from "vue";
import styles from './Autocomplete.module.scss';
import {Spell} from "../MagicSearch/MagicSearch.types";

const props = defineProps<{
  items: Spell[]
}>()

const emit = defineEmits<{
  (e: 'spell', toSet: Spell): void
}>();

const search = ref<string>("");
const results = ref<Spell[]>([]);
const isOpen = ref<boolean>(false);
const root = ref<VNodeRef | undefined>(undefined)
const arrowCounter = ref<number>(-1);
const filterResults = () => {
  results.value = props.items.filter(item => item.name.toLowerCase().indexOf(search.value.toLowerCase()) > -1);
}

const onChange = () => {
  filterResults();
  isOpen.value = true;
}

const setResult = (result: Spell) => {
  search.value = result.name;
  emit('spell', result);
  isOpen.value = false;
}

const onArrowDown = () => {
  if (arrowCounter.value < results.value.length - 1) {
    arrowCounter.value = arrowCounter.value + 1;
  }
}

const onArrowUp = () => {
  if (arrowCounter.value > 0) {
    arrowCounter.value = arrowCounter.value - 1;
  }
}

const onEnter = () => {
  const selected = results.value[arrowCounter.value];
  emit('spell', selected);
  search.value = selected.name;
  arrowCounter.value = -1;
  isOpen.value = false;
}

const handleClickOutside = (event: MouseEvent) => {
  if (!root.value?.contains(event.target as Node)) {
    isOpen.value = false;
    arrowCounter.value = -1;
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside);
});

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside);
})

</script>

<template>
  <div :ref="root" :class="styles.search">
    <input
        type="text"
        v-model="search"
        @input="onChange"
        @keydown.down="onArrowDown"
        @keydown.up="onArrowUp"
        @keydown.enter="onEnter"
    />
    <ul v-show="isOpen" :class="styles.results">
      <li :class="i === arrowCounter ? [styles.result, styles.isActive] : styles.result" v-for="(result, i) in results" :key="`${result.name}_${i}`" @click="setResult(result)">
        <span :class="styles.level" v-if="result.level !== 'Cantrip'">LV {{ result.level.charAt(0) }}</span>
        <span :class="styles.level" v-else>C</span>
        <span>{{ result.name }}</span>
      </li>
    </ul>
  </div>
</template>
