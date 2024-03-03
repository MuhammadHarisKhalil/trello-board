<script setup lang="ts">
 import type { Column } from "~~/types";
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
</script>
<template>
 <div class="flex gap-4 overflow-x-auto item-start">
    <div v-for="column in columns" :key="column.id"
    class="column bg-gray-200 p-5 rounded min-w-[250px]"
    >
     <header class="mb-4 text-bold">
        {{ column.title }}
     </header>
     <TrelloBoardTask v-for="task in column.task" :key="task.id" :task="task"/>  
     <button class="text-gray-500"> + Add  a card</button>
    </div>
 </div>
</template>