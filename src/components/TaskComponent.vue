<template>
  <v-row
      align="center"
  >
    <v-checkbox
        hide-details
        color="success"
        class="shrink mr-2 mt-0"
        v-model="model"
    ></v-checkbox>
    <v-text-field :value="text" :disabled="disabledTask" readonly></v-text-field>
    <v-btn :disabled="model" class="pa-0" color="blue" style="margin-left: 5px;" @click="editTask" text>
      <v-icon large>
        mdi-pencil
      </v-icon>
    </v-btn>
    <v-btn class="pa-0" color="red" style="margin-left: 5px;" @click="deleteSelectedTask" text>
      <v-icon large>
        mdi-delete
      </v-icon>
    </v-btn>
  </v-row>
</template>

<script>
export default {
  name: "TaskComponent",
  props: {
    value: {
      type: Boolean,
      default: false,
    },
    text: {
      type: String,
      default: '',
    },
    disabledTask: {
      type: Boolean,
      default: false,
    }
  },
  computed: {
    model: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", value);
      },
    },
  },
  methods: {
    editTask() {
      this.$emit('editTask')
    },
    deleteSelectedTask() {
      this.$emit('deleteSelectedTask')
    }
  }
}
</script>

<style scoped>

</style>
