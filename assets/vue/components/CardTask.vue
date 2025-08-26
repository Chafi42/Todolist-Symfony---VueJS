<template>
    <div class="mt-6 w-full absolute top-50">
        <h1 v-if="tasks.length < 2" class="text-2xl font-bold mt-4 ml-12">
            Task via Vue.js
        </h1>
        <h1 v-else class="text-2xl font-bold mt-4 ml-12">Tasks via Vue.js</h1>

        <table class="mt-4 container mx-auto table-auto w-full">
            <thead>
                <tr>
                    <th class="border px-4 py-2">ID</th>
                    <th class="border px-4 py-2">Titre</th>
                    <th class="border px-4 py-2">Description</th>
                    <th class="border px-4 py-2">Statut</th>
                    <th class="border px-4 py-2">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-if="loading">
                    <td colspan="4" class="border px-4 py-2">Chargement…</td>
                </tr>

                <tr v-else v-for="task in tasks" :key="task.id">
                    <td class="border px-4 py-2">{{ task.id }}</td>
                    <td class="border px-4 py-2">{{ task.title }}</td>
                    <td class="border px-4 py-2">{{ task.description }}</td>
                    <td class="border px-4 py-2">{{ task.status }}</td>
                    <td class="border px-4 py-2">
                        <ShowTask :task="task" />
                        <UpdatedTask :task="task" />
                        <DeletedTask :task="task" @deleted="removeTask" />
                    </td>
                </tr>
            </tbody>
        </table>
        <CreateTask class="my-4" @created="addTask" />
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
        updateTaskInList(updatedTask) {
            const index = this.tasks.findIndex((t) => t.id === updatedTask.id);
            if (index !== -1) this.tasks[index] = updatedTask;
        },
        removeTask(id) {
            this.tasks = this.tasks.filter((t) => t.id !== id);
        },
    },
};
</script>
