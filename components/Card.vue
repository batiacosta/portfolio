<template>
    <div class="max-w-sm w-full sm:w-auto rounded overflow-hidden shadow-lg bg-gray-800 transform hover:scale-105 transition-transform duration-300">
        <div v-if="firstImage" class="h-48 overflow-hidden">
            <img :src="firstImage.src" :alt="firstImage.alt" class="w-full object-cover">
        </div>
        <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2 text-white">{{ project.title }}</div>
            <p class="text-gray-300 text-base">{{ project.summary }}</p>
        </div>
        <div class="px-6 pt-4 pb-2">
            <span v-for="(tag, index) in project.tags" :key="index" class="inline-block bg-gray-700 rounded-full px-3 py-1 text-sm font-semibold text-gray-300 mr-2 mb-2">
                {{ tag }}
            </span>
        </div>
        <div class="px-6 py-4">
            <button @click="showModal" class="bg-gradient-to-r from-orange-400 via-orange-500 to-rose-500 text-white px-4 py-2 rounded-full hover:opacity-90 transition-opacity duration-300">Expand</button>
        </div>
        <Modal v-if="isModalVisible" @close="closeModal">
            <template #header>{{ project.title }}</template>
            <template #body>
                <div class="text-gray-300 text-base mt-4 text-justify">
                    <p v-for="(paragraph, index) in project.description.split('\n\n')" :key="index" class="mb-4">{{ paragraph }}</p>
                    <div class="mt-4">
                        <div v-for="(link, index) in project.links" :key="index" class="flex items-center space-x-2 mb-2">
                            <img :src="getIcon(link.type)" alt="icon" class="w-6 h-6 filter-orange">
                            <span v-if="link.platform" class="text-gray-300 font-bold">{{ link.platform }}</span>
                            <a :href="link.src" target="_blank" class="text-orange-400 hover:underline font-bold">Link</a>
                        </div>
                    </div>
                    <div class="mt-4">
                        <div v-for="(tag, index) in project.tags" :key="index" class="inline-block bg-gray-700 rounded-full px-3 py-1 text-sm font-semibold text-gray-300 mr-2 mb-2">
                            {{ tag }}
                        </div>
                    </div>
                    <div class="mt-4">
                        <div v-for="(item, index) in project.media" :key="index" class="mb-4">
                            <img v-if="item.type === 'image'" :src="item.src" :alt="item.alt" class="w-full object-cover mb-4" />
                            <iframe v-if="item.type === 'youtube'" :src="item.src" :title="item.alt" class="w-full lg:h-80" frameborder="0" allowfullscreen></iframe>
                        </div>
                    </div>
                </div>
            </template>
            <template #footer>
                <button @click="closeModal" class="bg-gradient-to-r from-orange-400 via-orange-500 to-rose-500 text-white px-4 py-2 rounded hover:bg-green-600">Close</button>
            </template>
        </Modal>
    </div>
</template>

<script>
import Modal from './Modal.vue';

export default {
    name: 'Card',
    components: {
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
    computed: {
        firstImage() {
            return this.project.media.find(item => item.type === 'image');
        }
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

<style scoped>
.filter-orange {
    filter: invert(48%) sepia(95%) saturate(748%) hue-rotate(1deg) brightness(100%) contrast(101%);
}
</style>
