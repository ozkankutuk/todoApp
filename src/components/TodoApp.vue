<template>
  <div class="container">
    <h2 class="mt-5 text-center">Todo App</h2>
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter text" class="form-control" style="border-radius: 5px 0 0 5px">
      <button @click="storeTask" class="btn btn-success" style="border-radius: 0 5px 5px 0px">Submit</button>
    </div>

    <table class="table table-bordered mt-5">
      <thead>
      <tr>
        <th>Task</th>
        <th width="100px">Status</th>
        <th colspan="2" class="text-center">&nbsp;</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td>
          <span :class="{
                'finished': task.status === 'finished',
                'text-danger':task.status === 'to-do',
                'text-warning':task.status === 'in-progress',
                'text-success':task.status === 'finished'
                }"
          >
            {{ task.name }}
          </span>
        </td>
        <td>
          <span class="pointer" @click="changeStatus(index)"
                :class="{
                'text-danger':task.status === 'to-do',
                'text-warning':task.status === 'in-progress',
                'text-success':task.status === 'finished'
                }"
          >
          {{ firstCharUpper(task.status) }}
          </span>
        </td>
        <td>
          <div class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td>
          <div class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
export default {
  name: 'TodoApp',

  data () {
    return {
      task: '',
      editedTask: null,
      avaibleStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Bütün işleri bitir sonra dinlen',
          status: 'to-do'
        },
        {
          name: 'Öğlen zamanında yemeğini yemeyi unutma',
          status: 'to-do'
        },
        {
          name: 'Vue çalış',
          status: 'in-progress'
        },
      ]
    }
  },
  methods: {
    storeTask () {
      if (this.task.trim().length === 0) return

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }

      this.task = ''
    },
    deleteTask (index) {
      this.tasks.splice(index, 1)
    },
    editTask (index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },
    changeStatus (index) {
      console.log(index)
      let newIndex = this.avaibleStatuses.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.avaibleStatuses[newIndex]
    },
    firstCharUpper (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  }
}


</script>

<style scoped>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>