<script setup lang="ts">
import { Pane, Splitpanes } from 'splitpanes'

const props = defineProps<{
  /**
   * The key to use for storing the pane sizes in localStorage.
   */
  storageKey?: string
  leftSize?: number
  minSize?: number
}>()

const DEFAULT = 30

const state = useDevToolsPanelsState()
const key = props.storageKey
const size = key
  ? computed({
    get: () => state.value[key] || props.leftSize || DEFAULT,
    set: (v) => { state.value[key] = v },
  })
  : ref(props.leftSize || DEFAULT)
</script>

<template>
  <Splitpanes h-full of-hidden @resize="size = $event[0].size">
    <Pane border="r base" h-full class="of-auto!" :size="size" :min-size="$slots.right ? (minSize || 10) : 100">
      <slot name="left" />
    </Pane>
    <Pane v-if="$slots.right" relative h-full class="of-auto!" :min-size="minSize || 10">
      <slot name="right" />
    </Pane>
  </Splitpanes>
</template>
