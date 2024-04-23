<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      :list="columns"
      group="columns"
      :animation="150"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            <input
              type="text"
              @keyup.enter="($event) => ($event.target as HTMLInputElement).blur()"
              v-model="column.title"
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
            />
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
              <div>
                <TrelloBoardTask
                  :task="task"
                  @delete="(taskId: ID) => deleteTask(column, taskId)"
                />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="($event) => column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <button @click="createColumn" class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50">
      + Add Another Column
    </button>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid'
import draggable from 'vuedraggable'

import type { Column, Task, ID } from '@/types'

const alt = useKeyModifier('Alt')

const columns = reactive<Column[]>([
  {
    id: nanoid(),
    title: 'To do',
    tasks: [
      { id: nanoid(), title: 'Task 1', createdAt: new Date() },
      { id: nanoid(), title: 'Task 2', createdAt: new Date() },
      {
        id: nanoid(),
        title: 'Lorem ipsum asd asd asdasd',
        createdAt: new Date(),
      },
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

// function addCard (columnId: string) {
//   const column = columns.find((column) => column.id === columnId)
//   if (!column) return

//   column.tasks.push({
//     id: nanoid(),
//     title: 'New Task',
//     createdAt: new Date(),
//   })
// }

function deleteTask(column: Column, taskId: ID) {
  column.tasks = column.tasks.filter((task) => task.id !== taskId)
  // for (const column of columns) {
  //   const index = column.tasks.findIndex((task) => task.id === taskId)
  //   if (index !== -1) {
  //     column.tasks.splice(index, 1)
  //     return
  //   }
  // }
}

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: [],
  }

  columns.push(column);
  nextTick(() => {
    const titleInput = document.querySelector('.column:last-child .title-input') as HTMLInputElement
    titleInput.focus()
  })
}
</script>
