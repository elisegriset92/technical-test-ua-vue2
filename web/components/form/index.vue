<template>
  <div class="max-w-sm">
    <div class="bg-white rounded-md px-8 pt-6 pb-8 mb-4 border-gray-200 border-solid border-1">
      <div class="mb-4 flex flex-col gap-2">
        <FormInput label="Title" name="title" :value="title" @input="update('title', $event)" />
        <FormInputTextArea
          label="Describe it"
          name="description"
          :value="description"
          @input="update('description', $event)"
        />
        <div class="flex gap-2">
          <FormInput type="date" label="Date" name="date" :value="date" @input="update('date', $event)" />
          <FormSelect
            label="Status"
            name="status"
            :value="status"
            :options="statusOptions"
            @input="update('status', $event)"
          />
        </div>
      </div>
      <div>
        <button class="bg-indigo-500 w-full text-sm text-white py-2 px-4 rounded" @click="submitTask">
          Submit
        </button>
        <button class="bg-gray-50 mt-4 w-full text-sm py-2 px-4 rounded">
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  props: {
    task: {
      type: Object,
      default: () => ({})
    }
  },
  data () {
    return {
      statusOptions: [{
        label: 'To Do',
        value: 'TODO'
      }, {
        label: 'Done',
        value: 'DONE'
      }],
      date: '',
      title: '',
      description: '',
      status: 'TODO'
    }
  },
  mounted () {
    this.retrieveTask()
  },
  methods: {
    update (field, event) {
      this[field] = event
    },
    submitTask () {
      const taskUpdated = {
        date: this.date ? moment(this.date).format() : undefined,
        title: this.title,
        description: this.description,
        status: this.status
      }
      this.$emit('formUpdate', taskUpdated)
      this.$router.push('/')
    },
    retrieveTask () {
      if (this.task && this.task.date) {
        this.date = moment(this.task.date).format('YYYY-MM-DD')
      }
      if (this.task && this.task.title) {
        this.title = this.task.title
      }
      if (this.task && this.task.description) {
        this.description = this.task.description
      }
      if (this.task && this.task.status) {
        this.status = this.task.status.value
      }
    }
  }
}
</script>
