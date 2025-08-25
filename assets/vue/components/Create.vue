<template>
    <button
        @click="open = true"
        class="px-4 py-2 bg-[#020C69] hover:skew-y-3 text-white rounded shadow"
    >
        Ajouter
    </button>

    <Teleport to="body">
        <div v-if="open">
            <!-- Overlay -->
            <div class="overlay" @click="open = false"></div>

            <!-- Modal -->
            <div class="modal">
                <form
                    class="border-[#222222] my-2 text-gray-700"
                    @submit.prevent="createTask"
                >
                    <h3 class="text-2xl font-bold mb-4 top-3 absolute left-4">
                        Ajouter une tâche
                    </h3>
                    <div class="border border-gray-300 my-5 w-full"></div>

                    <div class="w-full">
                        <label
                            for="title"
                            class="block text-sm font-medium text-[#222222] mt-2"
                        >
                            Titre
                        </label>
                        <input
                            type="text"
                            id="title"
                            v-model="task.title"
                            required
                            class="w-full text-[11px] border border-gray-400 my-2 rounded p-2"
                            placeholder="Entrer un titre..."
                        />
                    </div>

                    <div>
                        <label
                            for="description"
                            class="block text-xs font-medium text-[#222222] mt-2"
                        >
                            Description
                        </label>
                        <textarea
                            id="description"
                            v-model="task.description"
                            required
                            class="w-full text-[11px] border border-gray-400 my-1 rounded p-2"
                            placeholder="Description de la tâche..."
                        ></textarea>
                    </div>

                    <div>
                        <label
                            for="status"
                            class="block text-sm font-medium text-[#222222] mt-2"
                        >
                            Status
                        </label>
                        <select
                            class="w-full text-[11px] border border-gray-400 my-1 rounded p-2"
                            id="status"
                            v-model="task.status"
                            required
                        >
                            <option value="A faire">A faire</option>
                            <option value="En cours">En cours</option>
                            <option value="Terminé">Terminé</option>
                        </select>
                    </div>

                    <button
                        type="submit"
                        :disabled="loading"
                        class="bg-[#020C69] text-white px-4 py-2 rounded mt-3 hover:animate-bounce hover:h-12 disabled:opacity-50"
                    >
                        {{ loading ? "Envoi..." : "Créer la tâche" }}
                    </button>
                </form>

                <!-- Close Button -->
                <button
                    class="absolute top-3 right-8 text-[#222222]"
                    @click="open = false"
                >
                    ✕
                </button>
            </div>
        </div>
    </Teleport>
</template>

<script>
export default {
    name: "CreateTask",
    emits: ["created"],
    data() {
        return {
            task: {
                title: "",
                description: "",
                status: "A faire",
            },
            loading: false,
            open: false,
        };
    },
    methods: {
        async createTask() {
            this.loading = true;
            try {
                const res = await fetch("/api/tasks/new", {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify(this.task),
                });
                if (!res.ok) throw new Error("Erreur réseau");
                const newTask = await res.json();
                this.$emit("created", newTask);
                this.resetForm();
                this.open = false;
            } catch (e) {
                console.error(e);
                alert("Erreur lors de la création");
            } finally {
                this.loading = false;
            }
        },
        resetForm() {
            this.task = { title: "", description: "", status: "A faire" };
        },
    },
};
</script>

<style>
.modal {
    position: fixed;
    top: 20%;
    left: 50%;
    width: 350px;
    transform: translateX(-50%);
    color: white;
    background: #f5f5f5;
    z-index: 3001;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
}

.overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.7); /* fond noir semi-transparent */
    z-index: 3000; /* en dessous de la modal */
}
</style>
