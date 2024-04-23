<template>
  <div>
    <textarea
      v-model="title"
      @keydown.tab="creatTask"
      @keyup.enter="creatTask"
      class="focus:bg-white focus:shadow resize-none rounded w-full border-none"
      :class="{
        'h-7': !focused,
        'h-28': focused,
      }"
      style="outline: none !important"
      @focus="focused = true"
      @blur="focused = false"
      :placeholder="!focused ? '+ Add a task' : 'Enter a title for this card'"
    />
  </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid'
import type { Task } from '@/types'

const emit = defineEmits<{
  (e: 'add', payload: Task): void
}>()

const focused = ref(false)
const title = ref('')

function creatTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault()
    emit('add', {
      id: nanoid(),
      title: title.value,
      createdAt: new Date(),
    })
    title.value = ''
  }
}
</script>
