<template>
  <div class="border-solid border-2 border-gray-100 rounded-xl bg-white py-4 md:w-[448px] md:h-[487px] flex flex-col justify-evenly px-8">
    <FormInput
      label="Title"
      name="title"
      :value="taskValue.title ? taskValue.title : ''"
      @input="update('title', $event)"
    />
    <span v-if="errors.title" class="text-xs text-red-600">{{ errors.title }}</span>

    <FormInputTextArea
      label="Describe it"
      name="description"
      :value="taskValue.description ? taskValue.description : ''"
      @input="update('description', $event)"
    />
    <span v-if="errors.description" class="text-xs text-red-600">{{ errors.description }}</span>
    <div class="flex gap-2">
      <FormInput type="date" label="Date" name="date" :value="dateFormatted" @input="update('date', $event)" />
      <FormSelect
        label="Status"
        name="status"
        :value="taskValue.status ? taskValue.status.value : 'TODO'"
        :options="statusOptions"
        @input="update('status', $event)"
      />
    </div>
    <div>
      <button class="bg-indigo-500 w-full text-sm text-white py-2 px-4 rounded" @click="submitTask">
        Submit
      </button>
      <button class="bg-gray-50 mt-4 w-full text-sm py-2 px-4 rounded" @click="submitTask">
        Cancel
      </button>
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
      status: 'TODO',
      errors: {}
    }
  },
  computed: {
    taskValue () {
      return this.task
    },
    dateFormatted () {
      return this.taskValue.date ? moment(this.taskValue.date).format('YYYY-MM-DD') : ''
    }
  },
  methods: {
    validateForm () {
      this.errors = {}
      if (!this.title) {
        this.errors.title = 'Le titre est requis.'
      }
      if (!this.description) {
        this.errors.description = 'La description est requise.'
      }
      return Object.keys(this.errors).length === 0
    },
    update (field, event) {
      this.errors = {}
      this[field] = event
    },
    submitTask () {
      if (!this.validateForm()) {
        return
      }
      const taskUpdated = {
        date: this.date ? moment(this.date).format() : undefined,
        title: this.title,
        description: this.description,
        status: this.status
      }
      this.$emit('formUpdate', taskUpdated)
      this.$router.push('/')
    },
    goBack () {
      this.$router.push('/')
    }
  }
}
</script>
