<template>
    <div>
        <button @click="open = true" class="px-3 py-1 text-white rounded">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="20"
                height="25"
                viewBox="0 0 192 192"
                xml:space="preserve"
                fill="none"
            >
                <path
                    d="m104.175 90.97-4.252 38.384 38.383-4.252L247.923 15.427V2.497L226.78-18.646h-12.93zm98.164-96.96 31.671 31.67"
                    class="cls-1"
                    style="
                        fill: none;
                        fill-opacity: 1;
                        fill-rule: nonzero;
                        stroke: #000000;
                        stroke-width: 12;
                        stroke-linecap: round;
                        stroke-linejoin: round;
                        stroke-dasharray: none;
                        stroke-opacity: 1;
                    "
                    transform="translate(-77.923 40.646)"
                />
                <path
                    d="m195.656 33.271-52.882 52.882"
                    style="
                        fill: none;
                        fill-opacity: 1;
                        fill-rule: nonzero;
                        stroke: #000000;
                        stroke-width: 12;
                        stroke-linecap: round;
                        stroke-linejoin: round;
                        stroke-miterlimit: 5;
                        stroke-dasharray: none;
                        stroke-opacity: 1;
                    "
                    transform="translate(-77.923 40.646)"
                />
            </svg>
        </button>

        <Teleport to="body">
            <div v-if="open">
                <div class="overlay" @click="open = false"></div>

                <div class="modal">
                    <form @submit.prevent="updateTask">
                        <h3 class="text-2xl text-[#222222] font-bold mb-4">
                            Modifier une tâche
                        </h3>
                        <div class="border border-gray-300 my-5 w-full"></div>

                        <input
                            type="text"
                            id="title"
                            v-model="form.title"
                            required
                            class="text-[#222222] w-full text-[11px] border border-gray-400 my-2 rounded p-2"
                            placeholder="Entrer un titre..."
                        />

                        <textarea
                            id="description"
                            v-model="form.description"
                            required
                            class="text-[#222222] w-full text-[11px] border border-gray-400 my-1 rounded p-2"
                            placeholder="Description de la tâche..."
                        ></textarea>

                        <select
                            id="status"
                            v-model="form.status"
                            required
                            class="w-full text-[#222222] text-[11px] border border-gray-400 my-1 rounded p-2"
                        >
                            <option value="A faire">A faire</option>
                            <option value="En cours">En cours</option>
                            <option value="Terminé">Terminé</option>
                        </select>

                        <div class="flex justify-between">
                            <button
                                type="submit"
                                :disabled="loading"
                                class="bg-yellow-500 text-white px-4 py-2 rounded"
                            >
                                {{ loading ? "..." : "Sauvegarder" }}
                            </button>
                            <button
                                type="button"
                                @click="open = false"
                                class="bg-gray-400 text-white px-4 py-2 rounded"
                            >
                                Annuler
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </Teleport>
    </div>
</template>

<script>
    export default {
    name: "UpdateTask",
    props: {
        task: { type: Object, required: true },
    },
    emits: ["updated"],
    data() {
        return {
            open: false,
            loading: false,
            form: { ...this.task },
        };
    },
    methods: {
        async updateTask() {
            this.loading = true;
            try {
                const res = await fetch(`/api/tasks/${this.task.id}`, {
                    method: "PUT",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify(this.form),
                });

                if (!res.ok) throw new Error("Erreur réseau");
                const updatedTask = await res.json();
                this.$emit("updated", updatedTask);
                this.open = false;
            } catch (e) {
                console.error(e);
                alert("Erreur lors de la modification");
            } finally {
                this.loading = false;
            }
        },
    },
    };
</script>
