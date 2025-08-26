<template>
    <div>
        <button @click="open = true" class="px-3 py-1 text-white rounded">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="25"
                height="25"
                viewBox="0 0 24 24"
                fill="none"
            >
                <path
                    d="M4 12C4 12 5.6 7 12 7M12 7C18.4 7 20 12 20 12M12 7V4M18 5L16 7.5M6 5L8 7.5M15 13C15 14.6569 13.6569 16 12 16C10.3431 16 9 14.6569 9 13C9 11.3431 10.3431 10 12 10C13.6569 10 15 11.3431 15 13Z"
                    stroke="#464455"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                />
            </svg>
        </button>

        <Teleport to="body">
            <div v-if="open">
                <!-- Overlay -->
                <div class="overlay" @click="open = false"></div>

                <!-- Modal -->
                <div class="modal">
                    <h3 class="text-2xl font-bold mb-4 text-black">Détails de la tâche</h3>
                    <div class="border border-gray-300 my-2 w-full"></div>
                    <div class="text-black">
                        <p><strong>ID :</strong> {{ task.id }}</p>
                        <p><strong>Titre :</strong> {{ task.title }}</p>
                        <p><strong>Description :</strong> {{ task.description }}</p>
                        <p><strong>Statut :</strong> {{ task.status }}</p>
                    </div>

                    <div class="flex justify-end mt-4">
                        <button
                            @click="open = false"
                            class="bg-red-500 text-white px-4 py-2 rounded"
                        >
                            Retour
                        </button>
                    </div>
                </div>
            </div>
        </Teleport>
    </div>
</template>

<script>
export default {
    name: "ShowTask",
    props: {
        task: { type: Object, required: true },
    },
    data() {
        return {
            open: false,
        };
    },
};
</script>

<style scoped>
.modal {
    position: fixed;
    top: 20%;
    left: 50%;
    width: 350px;
    transform: translateX(-50%);
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
    background: rgba(0, 0, 0, 0.7);
    z-index: 3000;
}
</style>
