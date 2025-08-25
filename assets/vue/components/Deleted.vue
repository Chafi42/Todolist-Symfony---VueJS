<template>
    <div>
        <h2 class="text-xl font-bold mb-4">Tâches supprimées</h2>
        <ul>
            <li v-for="task in deletedTasks" :key="task.id">
                {{ task.title }}
                <button @click="restoreTask(task)">Restaurer</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "DeletedTasks",
    data() {
        return {
            deletedTasks: [],
        };
    },
    methods: {
        restoreTask(task) {
            this.deletedTasks = this.deletedTasks.filter(t => t.id !== task.id);
            this.$emit('task-restored', task);
            console.log(`Tâche restaurée: ${task.title}`);
        },
    },
};
</script>
