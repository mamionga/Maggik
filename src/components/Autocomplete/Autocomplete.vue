<script setup lang="ts">
import {onMounted, onUnmounted, ref} from "vue";
import styles from './Autocomplete.module.scss';

const props = defineProps<{
  items: string[]
}>()

const search = ref<string>("");
const results = ref<string[]>([]);
const isOpen = ref<boolean>(false);
const root = ref<HTMLElement | null>(null)
const arrowCounter = ref<number>(-1);
const filterResults = () => {
  results.value = props.items.filter(item => item.toLowerCase().indexOf(search.value.toLowerCase()) > -1);
}

const onChange = () => {
  filterResults();
  isOpen.value = true;
}

const setResult = (result: string) => {
  search.value = result;
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
  search.value = results.value[arrowCounter.value];
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
      <li :class="i === arrowCounter ? [styles.result, styles.isActive] : styles.result" v-for="(result, i) in results" :key="result + '_' + i" @click="setResult(result)">
        {{ result }}
      </li>
    </ul>
  </div>
</template>
