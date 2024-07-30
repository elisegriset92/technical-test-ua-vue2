<template>
  <div class="m-10 container mx-auto">
    <p v-if="$fetchState.pending">
      Fetching tasks...
    </p>
    <p v-else-if="$fetchState.error" class="text-red-700 text-2xl py-5">
      Unable to fetch tasks.
    </p>
    <div v-else>
      <div class="flex justify-between">
        <h1 class="text-lg">
          My Tasks
        </h1>
        <FormSelect
          class="w-1/5"
          label=""
          name="filterStatus"
          :value="statusOptions[0].value"
          :options="statusOptions"
          @input="filter"
        />
      </div>
      <ListTable :tasks="tasks" @delete="deleteTask" />
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      tasks: [],
      statusOptions: [{
        label: 'All',
        value: 'ALL'
      }, {
        label: 'To Do',
        value: 'TODO'
      }, {
        label: 'Done',
        value: 'DONE'
      }]
    }
  },
  async fetch () {
    const [err, tasks] = await this.$api.tasks.findAll()

    if (err) {
      throw new Error(err)
    }

    this.tasks = tasks
  },
  methods: {
    async getTodos (args) {
      const [err, tasks] = await this.$api.tasks.findAll({ status: args })
      if (err) {
        throw new Error(err)
      }
      this.tasks = tasks
    },
    filter (status) {
      this.getTodos(status === 'ALL' ? undefined : status)
    },
    deleteTask (id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
      this.$api.tasks.deleteTask(id)
    }
  }
}
</script>
