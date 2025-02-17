<template>
    <div class="max-w-sm rounded overflow-hidden shadow-lg bg-white">
        <Carousel :media="project.media" />
        <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{ project.title }}</div>
            <p class="text-gray-700 text-base">{{ project.summary }}</p>
        </div>
        <div class="px-6 pt-4 pb-2">
            <span v-for="(tag, index) in project.tags" :key="index" class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">
                {{ tag }}
            </span>
        </div>
        <div class="px-6 py-4">
            <button @click="showModal" class="bg-gradient-to-r from-orange-400 via-orange-500 to-rose-500 text-white px-4 py-2 rounded-full hover:opacity-90 transition-opacity duration-300">Expand</button>
        </div>
        <Modal v-if="isModalVisible" @close="closeModal">
            <template #header>{{ project.title }}</template>
            <template #body>
                <Carousel :media="project.media" />
                <p class="text-gray-700 text-base mt-4">{{ project.description }}</p>
                <div class="mt-4">
                    <div v-for="(link, index) in project.links" :key="index" class="flex items-center space-x-2">
                        <img :src="getIcon(link.type)" alt="icon" class="w-6 h-6">
                        <a :href="link.src" target="_blank" class="text-blue-500 hover:underline">{{ link.src }}</a>
                        <span v-if="link.platform" class="text-gray-500">({{ link.platform }})</span>
                    </div>
                </div>
            </template>
            <template #footer>
                <button @click="closeModal" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Close Modal</button>
            </template>
        </Modal>
    </div>
</template>

<script>
import Carousel from './Carousel.vue';
import Modal from './Modal.vue';

export default {
    name: 'Card',
    components: {
        Carousel,
        Modal
    },
    props: {
        project: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            isModalVisible: false
        };
    },
    methods: {
        showModal() {
            this.isModalVisible = true;
        },
        closeModal() {
            this.isModalVisible = false;
        },
        getIcon(type) {
            return Modal.methods.getIcon(type);
        }
    }
}
</script>
