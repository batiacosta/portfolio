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
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        project: {
            type: Object,
            required: true
        }
    },
    computed: {
        firstImage() {
            return this.project.media.find(item => item.type === 'image');
        }
    },
    methods: {
        showModal() {
            this.$emit('show-modal', this.project);
        }
    }
}
</script>

<style scoped>
.filter-orange {
    filter: invert(48%) sepia(95%) saturate(748%) hue-rotate(1deg) brightness(100%) contrast(101%);
}
</style>
