<template>
  <v-container style="max-width: 700px; margin-top: 200px; ">
    <h1 class="d-flex justify-space-around" style="color:#4A148C;">
      To-do List
    </h1>
    <v-alert outlined color="purple">
      <v-col class="mb-4">
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
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Tâche</th>
                <th class="text-left">Statut</th>
                <th class="text-left">Modifier</th>
                <th v-if="taskNameToUpdate !== null" class="text-left">
                  Enregistrer
                </th>
                <th class="text-left">Supprimer</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(task, index) in tasks" :key="index">
                <td
                  v-if="editedTaskIndex !== null && editedTaskIndex === index"
                >
                  <v-text-field
                    v-model="taskNameToUpdate"
                    type="text"
                    placeholder="Entrer la nouvelle tâche à modifier"
                    name="task-item"
                  />
                </td>
                <td v-else>{{ task.name }}</td>
                <td>
                  <span
                    class="pointer noselect"
                    @click="changeStatus(index)"
                    :class="{
                      'error--text': task.status === 'À faire',
                      'warning--text': task.status === 'En cours',
                      'success--text': task.status === 'Fait',
                    }"
                    >{{ task.status }}</span
                  >
                </td>
                <td>
                  <div @click="editTask(index)">
                    <span class="fa fa-pen" color="#7986CB"></span>
                  </div>
                </td>
                <td v-if="taskNameToUpdate !== null">
                  <div @click="submitTask">
                    <span class="fa fa-save" color="#AFB42B"></span>
                  </div>
                </td>
                <td>
                  <div @click="deleteTask(index)">
                    <span class="fa fa-trash" color="#8D6E63"></span>
                  </div>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-alert>
  </v-container>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      // Variables
      taskName: "",
      taskNameToUpdate: null,
      editedTaskIndex: null,
      availableStatues: ["À faire", "En cours", "Fait"],
      tasks: [],
    };
  },
  methods: {
    // Add new task
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
    // Delete Task
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    // Edit task
    editTask(index) {
      this.taskNameToUpdate = this.tasks[index].name;
      this.editedTaskIndex = index;
    },
    // Change task's status
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
