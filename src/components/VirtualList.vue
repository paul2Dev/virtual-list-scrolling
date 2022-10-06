<script setup>
import { ref } from 'vue'
import { useVirtualList, useInfiniteScroll } from '@vueuse/core'

const data = ref(Array.from(Array(50).keys(), () => 'Dummy data'))
const { list, containerProps, wrapperProps } = useVirtualList(
  data,
  {
    // Keep `itemHeight` in sync with the item's row.
    itemHeight: 96,
  },
)

useInfiniteScroll(
  containerProps.ref,
  () => {
    //load more dummy
    data.value.push(...Array.from(Array(10).keys(), () => 'More dummy data'))
  },
  { distance: 10 } //distance in pixels from the bottom of container where the method will be triggered
)

</script>

<template>
  <div v-bind="containerProps" class="h-screen p-2 rounded text-white"> 
    <div v-bind="wrapperProps" class="max-w-sm mx-auto">
      <div 
      v-for="{ index, data } in list" 
      :key="index"
      class="rounded-lg h-[80px] flex flex-col px-4 justify-center bg-slate-800 mb-4 border-slate-600"
      >
        <h2 class="mb-2 text-2xl">Item #{{ index }}</h2>
        <p class="text-sm">{{ data }}</p>
      </div>
    </div>
  </div>
</template>
