<script setup lang="ts">
 import type { Column,Task } from "~~/types";
 import draggable from "vuedraggable";
 import { nanoid } from "nanoid";

  const columns = ref<Column[]>([
    {
     id : nanoid(),
     title : "BackLog",
     task : [
        {id : nanoid(),title: "email Task",createdAt :  new Date()},
        {id : nanoid(),title: "CRUD Task",createdAt :  new Date()},
        {id : nanoid(),title: "SMS Task",createdAt :  new Date()}
    ],
    },
    { id : nanoid(),title : "ToDo",task : [] },
    { id : nanoid(),title : "In Progress",task : [] },
    { id : nanoid(),title : "QA",task : [] },
    { id : nanoid(),title : "Complete",task : [] },
  ]);
  const alt = useKeyModifier("Alt");

  function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    task: [],
  };
  columns.value.push(column);
  nextTick(() => {
    (
      document.querySelector(
        ".column:last-of-type .title-input"
      ) as HTMLInputElement
    ).focus();
  });
}
</script>
<template>
 <div class="flex items-start overflow-x-auto gap-4">
   <draggable
   v-model="columns"
   group="columns"
   item-key="id"
   :animation="150"
   handle=".drag-handle"
   class="flex gap-4  items-start"
   >
   <template #item="{element: column} : {element: Column}">
    <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
     <header class="mb-4 text-bold">
       <DragHandle />
        <input
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="
                column.title === ''
                  ? (columns = columns.filter((c) => c.id !== column.id))
                  : null
              "
              type="text"
              v-model="column.title"
            />
     </header>
     <draggable
      v-model="column.task"
      :group="{name: 'tasks', pull: alt ? 'clone' : true}"
      item-key="id"
      handle=".drag-handle"
      :animation="150"
      >
       <template #item="{element: task} : {element: Task}">
        <div >
          <TrelloBoardTask :task="task" />
        </div>
       </template>
       </draggable>  
     <NewTasks @add = "column.task.push($event)" />
    </div>
   </template>
   </draggable>
   <button
      @click="createColumn"
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
    >
      + Add Another Column
    </button>
 </div>
</template>