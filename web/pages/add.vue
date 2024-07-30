<template>
  <div class="m-10 md:w-1/4 md:mx-auto">
    <SvgIcon name="icons/plus-circle" class="p-1 mx-auto w-10 h-10 bg-black text-white rounded" />
    <h1 class="font-extrabold text-center text-3xl my-6">
      Add a task
    </h1>
    <Form @formUpdate="postTask($event)" />
  </div>
</template>

<script>

export default {
  methods: {
    postTask (event) {
      const [err] = this.$api.tasks.createTask(event)
      if (err) {
        throw new Error(err)
      }
    }
  }
}
</script>
