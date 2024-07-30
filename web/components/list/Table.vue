<template>
  <div class="m-10 container mx-auto bg-white rounded-md p-2">
    <div class="divide-y">
      <div v-for="(task, index) in tasks" :key="index" class="p-4">
        <div class="flex justify-between">
          <div>
            <div class="flex mb-2">
              <div :class="['text-xs font-medium px-2.5 py-0.5 rounded-full mr-2 bg-red-100 text-red-800', { 'bg-green-100 text-green-800' : isDone(task)}]">
                {{ task.status.label }}
              </div>
              <div class="text-indigo-600 text-sm">
                {{ task.title }}
              </div>
            </div>
            <div class="text-gray-500 text-sm mb-2">
              {{ task.description }}
            </div>
            <div class="flex flex-col md:flex-row gap-2 text-xs text-gray-500">
              <div>
                Created at {{ getDate(task.createdAt) }}
              </div>
              <div class="font-bold">
                Updated at {{ getDate(task.updatedAt) }}
              </div>
            </div>
          </div>
          <div class="grid justify-items-end">
            <div class="flex gap-2 cursor-pointer">
              <NuxtLink :to="`/edit/${task.id}`">
                <SvgIcon name="icons/pencil" class="p-1 w-5 h-5 bg-indigo-600 text-white rounded" />
              </NuxtLink>
              <SvgIcon name="icons/trash" class="p-1 w-5 h-5 bg-red-500 text-white rounded" @click="deleteTask(task.id)" />
            </div>
            <div v-if="task.date" class="flex">
              <SvgIcon name="icons/calendar" class="p-1 w-5 h-5 bg-gray-400 text-white rounded" />
              <p class="text-sm ml-2 text-gray-500">
                {{ getDate(task.date) }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  props: {
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
    getDate (date) {
      return moment(date, 'YYYY-MM-DD').format('MMMM D, YYYY - h:mm:ss')
    },
    deleteTask (id) {
      this.$emit('delete', id)
    },
    isDone (task) {
      return task.status.value === 'DONE'
    }
  }
}
</script>
