<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <h2 class="title">Hola, {{ name }}.</h2>
        <div class="box">
          <div class="field">
            <label class="label" for="task">Nueva tarea:</label>
            <div class="control">
              <input type="text" name="task" class="input" v-model="newTask.title">
            </div>
          </div>
          <div class="field">
            <label class="label" for="hours">Horas:</label>
            <div class="control">
              <input type="number" name="hours" class="input" v-model="newTask.time">
            </div>
          </div>
          <a class="button is-info is-medium" @click="addTask">Guardar</a>
          <a class="button is-danger is-medium" @click="cancel">Cancelar</a>
        </div>
        <div class="box">
          <h3 class="subtitle">Lista de tareas registradas</h3>
          <div v-if="tasks.length">
            <article v-for="(t, i) in tasks" class="message is-success">
              <div class="message-header">
                {{ t.title }} | Tiempo: {{ t.time }} horas.
                <button class="delete" @click="deleteTask(i)"></button>
              </div>
              <div class="message-body">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque risus mi, tempus quis placerat ut, porta nec nulla. Vestibulum rhoncus ac ex sit amet fringilla. Nullam gravida purus diam, et dictum felis venenatis efficitur. Aenean ac eleifend lacus, in mollis lectus. Donec sodales, arcu et sollicitudin porttitor, tortor urna tempor ligula, id porttitor mi magna a neque. Donec dui urna, vehicula et sem eget, facilisis sodales sem.
              </div>
            </article>
            <div class="notification is-warning">
              Tiempo total: {{ totalTime }}
            </div>
          </div>
          <p v-else class="notification is-danger">
            No hay aún.
          </p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',

  data () {
    return {
      name: 'Alberto Fragoso',
      tasks: [],
      newTask: {
        title: '',
        time: 0
      }
    }
  },

  created () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },

  methods: {
    addTask () {
      if (!this.newTask.title || !this.newTask.time) { return }

      this.tasks.push({
        title: this.newTask.title,
        time: this.newTask.time
      })

      localStorage.setItem("tasks", JSON.stringify(this.tasks))

      this.newTask.title = ''
      this.newTask.time = 0
    },
    deleteTask (index) {
      this.tasks.splice(index, 1)
      localStorage.setItem("tasks", JSON.stringify(this.tasks))
    },
    cancel () {
      this.newTask.title = ''
      this.newTask.time = 0
    }
  },

  computed: {
    totalTime () {
      if (!this.tasks.length) { return 0 }

      let total = 0
      let horas = ''

      this.tasks.forEach(t => {
        total += parseInt(t.time)
      })
      horas = (total > 1) ? 'horas' : 'hora'

      return `${total} ${horas}`
    }
  }
}
</script>

<style lang="scss">
@import './scss/main.scss';
.results {
  margin-top: 20px;
}
</style>
