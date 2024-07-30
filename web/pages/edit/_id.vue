<template>
  <div class="m-10 md:w-1/4 md:mx-auto">
    <SvgIcon name="icons/pencil" class="p-1 mx-auto w-10 h-10 bg-black text-white rounded" />
    <h1 class="font-extrabold  text-center text-3xl my-6">
      Edit a task
    </h1>
    <Form :task="task" @formUpdate="editTask" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      task: {}
    }
  },
  async fetch () {
    const [err, task] = await this.$api.tasks.findOne(this.$route.params.id)

    if (err) {
      throw new Error(err)
    }

    this.task = task
  },
  methods: {
    async editTask (body) {
      const [err] = await this.$api.tasks.updateTask(this.$route.params.id, body)
      if (err) {
        throw new Error(err)
      }
    }
  }
}
</script>
