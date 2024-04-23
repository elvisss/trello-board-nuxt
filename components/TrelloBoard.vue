<template>
  <div class="flex gap-4 overflow-x-auto items-start">
    <div
      v-for="column in columns"
      :key="column.id"
      class="column bg-gray-200 p-5 rounded min-w-[250px]"
    >
      <header class="font-bold mb-4">
        {{ column.title }}
      </header>
      <TrelloBoardTask
        v-for="task in column.tasks"
        :key="task.id"
        :task="task"
      />
      <footer>
        <button class="text-gray-500">+ Add a Card</button>
      </footer>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Column } from '@/types'
import { nanoid } from 'nanoid'

const columns = reactive<Column[]>([
  {
    id: nanoid(),
    title: 'To do',
    tasks: [
      { id: nanoid(), title: 'Task 1', createdAt: new Date() },
      { id: nanoid(), title: 'Task 2', createdAt: new Date() },
    ],
  },
  {
    id: nanoid(),
    title: 'In progress',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Review',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Ready for QA',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'QA Finished',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Done',
    tasks: [],
  },
])
</script>
