<template>
  <div class="container" style="max-width: 600px">
    <v-container>
      <v-row class="text-center">
        <v-col class="mb-4">
          <h1 class="display-2 font-weight-bold mb-3">
            TodoList
          </h1>
          <!-- Input -->
          <div class="d-flex">
            <input
              v-model="taskName"
              type="text"
              placeholder="Entrer la tâche"
              name="task-entry"
              class="form-control"
            />
            <button @click="submitTask" class=" btn rounded-0 blue">
              Ajouter
            </button>
          </div>
          <!-- Todo table -->
          <table class="table table-hover mt-5">
            <thead>
              <tr>
                <th scope="col">Tâche</th>
                <th scope="col">Statut</th>
                <th scope="col" class="text-center">#</th>
                <th scope="col" class="text-center">#</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(task, index) in tasks" :key="index">
                <td>{{ task.name }}</td>
                <td>
                  <input
                    v-model="taskName"
                    type="text"
                    placeholder="Entrer la tâche"
                    name="task-item"
                    class="form-control"
                  />
                </td>
                <td>
                  <span
                    class="pointer noselect"
                    @click="changeStatus(index)"
                    :class="{
                      'text-danger': task.status === 'À faire',
                      'text-warning': task.status === 'En cours',
                      'text-success': task.status === 'Fait',
                    }"
                    >{{ task.status }}</span
                  >
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
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "TodoList",

  data() {
    return {
      taskName: "",
      editedTask: null,
      availableStatues: ["À faire", "En cours", "Fait"],
      tasks: [],
    };
  },
  methods: {
    submitTask() {
      if (this.taskName === "") return;

      if (this.editedTask == null) {
        this.tasks.push({
          name: this.taskName,
          status: "À faire",
        });
      } else {
        this.tasks[this.editedTask].name = this.taskName;
        this.editedTask = null;
      }

      this.taskName = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.taskName = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatues.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatues[newIndex];
    },
  },
};
</script>

<style scped>
.pointer {
  cursor: pointer;
}
</style>
