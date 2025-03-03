<script setup lang="ts">
import { breakpointsTailwind } from '@vueuse/core'

const props = defineProps<{
  open: boolean,
  drawerClass: string,
}>()
const emit = defineEmits<{(e: 'close'): void }>()

const mounted = ref(false)

onMounted(() => {
  setTimeout(() => {
    mounted.value = true
  }, 300)
})

const { lg } = useBreakpoints(breakpointsTailwind)

const classContainer = computed(() => {
  if (lg.value || !mounted.value) {
    return 'lt-lg:opacity-0'
  }
  return [
    'z-100',
    props.open
      ? 'opacity-100'
      : 'opacity-0 pointer-events-none'
  ].join(' ')
})

const classDrawer = computed(() => {
  if (lg.value || !mounted.value) {
    return 'lt-lg:-translate-x-full'
  }
  return [
    props.drawerClass || '',
    props.open
      ? 'translate-x-0'
      : '-translate-x-full'
  ]
})
</script>

<template>
  <div
    :class="[classContainer, { transition: mounted }]"
    class="lt-lg:fixed lt-lg:inset-0 lt-lg:z-100 duration-200 relative"
  >
    <div class="absolute inset-0 flex-auto bg-black/60 -z-1 lg:hidden" @click="emit('close')" />
    <div
      :class="[classDrawer, { transition: mounted }]"
      class="transform duration-200"
    >
      <slot />
    </div>
  </div>
</template>
