<template>
  <div class="mt-6 w-full relative top-50">
    <h1
      v-if="tasks.length < 2"
      class="text-xl sm:text-2xl font-bold mt-4 ml-4 sm:ml-12"
    >
      Task via Vue.js
    </h1>
    <h1 v-else class="text-xl sm:text-2xl font-bold mt-4 ml-4 sm:ml-12">
      Tasks via Vue.js
    </h1>

    <!-- Conteneur scrollable pour petits écrans -->
    <div class="mt-4 container mx-auto overflow-x-auto">
      <table
        class="table-auto w-full border-collapse text-xs sm:text-sm md:text-base"
      >
        <thead>
          <tr>
            <th class="border-1 px-2 sm:px-4 py-2">ID</th>
            <th class="border-1 px-2 sm:px-4 py-2">Auteur</th>
            <th class="border-1 px-2 sm:px-4 py-2">Description</th>
            <th class="border-1 px-2 sm:px-4 py-2">Statut</th>
            <th class="border-1 px-2 sm:px-4 py-2">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="loading">
            <td colspan="5" class="border px-4 py-2 text-center text-gray-500">
              Chargement…
            </td>
          </tr>

          <tr
            v-else
            v-for="task in tasks"
            :key="task.id"
            class="hover:bg-gray-50"
          >
            <td class="border-1 px-2 sm:px-4 py-2">{{ task.id }}</td>
            <td class="border-1 px-2 sm:px-4 py-2 break-words">
              {{ task.title }}
            </td>
            <td class="border-1 flex justify-center px-2 sm:px-4 py-2 break-words">
              {{ task.description }}
            </td>
            <td class="border-1 px-2 sm:px-4 py-2">
              {{ task.status }}
            </td>
            <td
              class="border-1 py-6 flex break-words justify-center items-center"
            >
              <ShowTask :task="task" />
              <UpdatedTask :task="task" @updated="updateTaskInList" />
              <DeletedTask :task="task" @deleted="removeTask" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- bouton création -->
    <div class="my-4 px-4 sm:px-0">
      <CreateTask @created="addTask" />
    </div>
  </div>
</template>

<script>
import CreateTask from "./Created.vue";
import ShowTask from "./Show.vue";
import UpdatedTask from "./Updated.vue";
import DeletedTask from "./Deleted.vue";

export default {
  name: "GetAll",
  components: { CreateTask, ShowTask, UpdatedTask, DeletedTask },

  data() {
    return {
      tasks: [],
      loading: true,
    };
  },

  async mounted() {
    try {
      const res = await fetch("/api/tasks");
      if (!res.ok) throw new Error("Erreur réseau");
      this.tasks = await res.json();
    } catch (e) {
      console.error(e);
    } finally {
      this.loading = false;
    }
  },

  methods: {
    addTask(task) {
      this.tasks.unshift(task);
    },
    removeTask(id) {
      this.tasks = this.tasks.filter((t) => t.id !== id);
    },
  },
};
</script>
