<template>
  <draggable
    :list="columns"
    group="columns"
    :animation="150"
    handle=".drag-handle"
    item-key="id"
    class="flex gap-4 overflow-x-auto items-start"
  >
    <template #item="{ element: column }: { element: Column }">
      <div
        class="column bg-gray-200 p-5 rounded min-w-[250px]"
      >
        <header class="font-bold mb-4">
          <DragHandle /> {{ column.title }}
        </header>
        <draggable
          :list="column.tasks"
          :group="{
            name: 'tasks',
            pull: alt ? 'clone' : true,
          }"
          :animation="150"
          handle=".drag-handle"
          item-key="id"
        >
        <template #item="{ element: task }: { element: Task }">
          <TrelloBoardTask
            :task="task"
          />
          </template>
        </draggable>
        <footer>
          <button class="text-gray-500">+ Add a Card</button>
        </footer>
      </div>
    </template>
  </draggable>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid'
import draggable from 'vuedraggable'

import type { Column, Task } from '@/types'

const alt = useKeyModifier('Alt')

const columns = reactive<Column[]>([
  {
    id: nanoid(),
    title: 'To do',
    tasks: [
      { id: nanoid(), title: 'Task 1', createdAt: new Date() },
      { id: nanoid(), title: 'Task 2', createdAt: new Date() },
      { id: nanoid(), title: 'Lorem ipsum asd asd asdasd', createdAt: new Date()}
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
