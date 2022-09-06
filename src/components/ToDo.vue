<template>
  <v-container>
    <v-card
        class="mx-auto"
        max-width="1000"
    >
      <v-img
          class="white--text"
          height="200px"
          style="background: linear-gradient(180deg, rgba(0,104,255,1) 0%, rgba(255,255,255,1) 100%);"
      >
        <v-card-title>
          <h1>TODO-LIST</h1>
          <v-img
              class="align-start"
              :src="require('../assets/logo.svg')"
              contain
              position="right"
              height="35"
          />
        </v-card-title>
        <v-card-title class="justify-center">
          <h2>{{ date }}</h2>
        </v-card-title>
      </v-img>

      <v-form @submit.prevent="submitForm" class="d-flex pa-4" style="height: 90px">
        <v-text-field
            class="mr-2"
            v-model="text"
            label="Enter a new task"
            outlined
        >
        </v-text-field>
        <v-btn
            dark
            x-large
            color="blue"
            style="height: 55px"
            type="submit"
        >
          <v-icon>
            {{ btnIcon }}
          </v-icon>
          {{ btnText }}
        </v-btn>
      </v-form>

      <v-card-title>
        <h4>ToDo:</h4>
      </v-card-title>
      <v-card-subtitle>
        You have <b>{{ list.length }}</b> tasks
      </v-card-subtitle>

      <v-card-text class="text--primary">
        <div>
          <div v-if="!list.length">
            <h1 class="text-center blue--text text-uppercase">
              You don't have tasks
              <svg style="height:34px; vertical-align: text-top;" viewBox="0 0 24 24">
                <path fill="currentColor"
                      d="M20,12A8,8 0 0,0 12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12M22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2A10,10 0 0,1 22,12M15.5,8C16.3,8 17,8.7 17,9.5C17,10.3 16.3,11 15.5,11C14.7,11 14,10.3 14,9.5C14,8.7 14.7,8 15.5,8M10,9.5C10,10.3 9.3,11 8.5,11C7.7,11 7,10.3 7,9.5C7,8.7 7.7,8 8.5,8C9.3,8 10,8.7 10,9.5M12,14C13.75,14 15.29,14.72 16.19,15.81L14.77,17.23C14.32,16.5 13.25,16 12,16C10.75,16 9.68,16.5 9.23,17.23L7.81,15.81C8.71,14.72 10.25,14 12,14Z"/>
              </svg>
            </h1>
          </div>
          <v-col>
            <div
                v-for="(value, index) in list"
                :key="value.message"
            >
              <TaskComponent
                  v-model="value.status"
                  :text="value.text"
                  :disabledTask="value.status"
                  @editTask="startEditing(index)"
                  @deleteSelectedTask="deleteSelectedTask(index)"
              />
            </div>
          </v-col>
        </div>
      </v-card-text>
      <v-card-actions class="justify-space-between pa-4">
        <v-btn
            color="blue"
            text
            @click="deleteAllTask">
          Clear all
          <v-icon left>
            mdi-delete
          </v-icon>
        </v-btn>
        <div class="d-flex justify-space-around" style="gap: 20px">
          <div class="blue pa-2">
            <span class="white--text">DONE: {{ completedTasksCount }}</span>
          </div>
          <div class="red darken-1 pa-2">
            <span class="white--text">PENDING: {{ pendingTasksCount }}</span>
          </div>
        </div>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
import TaskComponent from "@/components/TaskComponent";

export default {
  name: 'ToDo',
  components: {
    TaskComponent
  },

  data: () => ({
    text: '',
    editedTask: null,
    date: new Date().toLocaleDateString(),
    list: [],
  }),

  methods: {
    submitForm() {
      if (!this.text || !this.text.trim()) {
        return;
      }

      if (this.editedTask === null) {
        this.addNewTask(this.text)
      } else {
        this.editTask(this.editedTask, this.text)
      }

      this.text = '';
    },

    addNewTask(text) {
      this.list.push({
        text,
        status: false
      })
    },

    editTask(id, text) {
      this.list[id].text = text
      this.editedTask = null
    },

    deleteSelectedTask(index) {
      this.$delete(this.list, index)
    },

    startEditing(index) {
      this.text = this.list[index].text
      this.editedTask = index
    },

    deleteAllTask() {
      this.list = [];
    },
  },

  computed: {
    completedTasksCount() {
      return this.list.filter(item => item.status).length;
    },

    pendingTasksCount() {
      return this.list.filter(item => !item.status).length;
    },

    btnIcon() {
      return this.editedTask !== null ? 'mdi-pencil' : 'mdi-plus'
    },

    btnText() {
      return this.editedTask !== null ? 'Change' : 'Add'
    }
  },

  mounted() {
    if (localStorage.list) {
      this.list = JSON.parse(localStorage.list);
    }
  },

  watch: {
    list: {
      handler(newNotes) {
        localStorage.list = JSON.stringify(newNotes);
      },
      deep: true
    }
  }
}
</script>

<style>
input[disabled] {
  text-decoration: line-through;
}
</style>
