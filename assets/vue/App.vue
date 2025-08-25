<template>
    <div>
        <h1 class="text-2xl font-bold mt-4 ml-12">Tasks depuis VueJS</h1>

        <table
            class="container mx-auto table-auto w-full border-collapse border border-gray-400 mt-4"
        >
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
                <tr v-else-if="tasks.length === 0">
                    <td colspan="4" class="border px-4 py-2">Aucune tâche</td>
                </tr>
                <tr v-else v-for="task in tasks" :key="task.id">
                    <td class="border px-4 py-2">{{ task.id }}</td>
                    <td class="border px-4 py-2">{{ task.title }}</td>
                    <td class="border px-4 py-2">{{ task.description }}</td>
                    <td class="border px-4 py-2">{{ task.status }}</td>
                    <td class="border px-4 py-2">
                        <UpdatedTasks />
                    </td>
                </tr>
            </tbody>
        </table>
        <CreateTask class="my-4" @created="addTask" />
    </div>
</template>

<script>
import CreateTask from "./components/Create.vue";
// import DeletedTasks from "./components/Deleted.vue";
// import UpdatedTasks from "./components/Updated.vue";

export default {
    name: "App",
    components: { CreateTask },

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
            // ajouter en tête de liste
            this.tasks.unshift(task);
        },

        updateTask(updatedTask) {
            const index = this.tasks.findIndex(
                (task) => task.id === updatedTask.id
            );
            if (index !== -1) {
                this.tasks.splice(index, 1, updatedTask);
            }
        },
    },
};
</script>
