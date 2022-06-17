<template>
  <v-container style="max-width: 650px; margin-top: 200px; ">
    <v-alert outlined color="purple">
      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          TodoList
        </h1>
        <!-- Input -->
        <v-text-field
          color="#1B5E20"
          v-model="taskName"
          label="Entrer la tâche"
          name="task-entry"
          append-icon="mdi-plus"
          @click:append="submitTask"
        ></v-text-field>
        <!-- Todo table -->
        <table class="table table-hover mt-5">
          <thead>
            <tr>
              <th scope="col">Tâche</th>
              <th scope="col">Statut</th>
              <th scope="col" class="text-center">Modifier</th>
              <th
                v-if="taskNameToUpdate !== null"
                scope="col"
                class="text-center"
              >
                Enregistrer
              </th>
              <th scope="col" class="text-center">Supprimer</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
              <td v-if="editedTaskIndex !== null && editedTaskIndex === index">
                <input
                  v-model="taskNameToUpdate"
                  type="text"
                  placeholder="Entrer la nouvelle tâche à modifier"
                  name="task-item"
                  class="form-control"
                />
              </td>
              <td v-else>{{ task.name }}</td>
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
                  <span class="fa fa-pen" color="#7986CB"></span>
                </div>
              </td>
              <td v-if="taskNameToUpdate !== null">
                <div class="text-center" @click="submitTask">
                  <span class="fa fa-save" color="#7986CB"></span>
                </div>
              </td>
              <td>
                <div class="text-center" @click="deleteTask(index)">
                  <span class="fa fa-trash" color="#8D6E63"></span>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </v-col>
    </v-alert>
  </v-container>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      taskName: "",
      taskNameToUpdate: null,
      editedTaskIndex: null,
      availableStatues: ["À faire", "En cours", "Fait"],
      tasks: [],
    };
  },
  methods: {
    submitTask() {
      if (this.editedTaskIndex == null) {
        if (this.taskName === "") {
          alert("Le nom de la tâche est vide! Entrez svp un nom valide.");
          return;
        }
        this.tasks.push({
          name: this.taskName,
          status: "À faire",
        });
      } else {
        this.tasks[this.editedTaskIndex].name = this.taskNameToUpdate;
        this.taskNameToUpdate = null;
        this.editedTaskIndex = null;
      }
      this.taskName = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.taskNameToUpdate = this.tasks[index].name;
      this.editedTaskIndex = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatues.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatues[newIndex];
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
</style>
