<template>
    <header class="bg-gradient-to-r from-gray-900 via-gray-800 to-gray-700 py-6 shadow-md">
        <div class="container mx-auto flex justify-between items-center px-4">
            <div>
                <h1 class="text-3xl font-extrabold text-white">
                    David Acosta Laverde
                </h1>
            </div>
            <nav class="hidden md:flex space-x-4">
                <a href="#about" class="text-white hover:text-yellow-400">About</a>
                <a href="#projects" class="text-white hover:text-yellow-400">Projects</a>
                <a href="#contact" class="text-white hover:text-yellow-400">Contact</a>
            </nav>
            <div class="relative">
                <select @change="changeLanguage" class="text-white bg-transparent border border-white rounded px-2 py-1">
                    <option value="English">English</option>
                    <option value="Español">Español</option>
                </select>
            </div>
            <button @click="toggleMenu" class="md:hidden text-white hover:text-yellow-400">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
                </svg>
            </button>
        </div>
        <div v-if="isMenuOpen" class="md:hidden">
            <nav class="flex flex-col space-y-2 px-4 py-2">
                <a href="#about" class="text-white hover:text-yellow-400">About</a>
                <a href="#projects" class="text-white hover:text-yellow-400">Projects</a>
                <a href="#contact" class="text-white hover:text-yellow-400">Contact</a>
            </nav>
        </div>
    </header>
</template>

<script>
export default {
    name: "Header",
    data() {
        return {
            isMenuOpen: false
        };
    },
    methods: {
        changeLanguage(event) {
            this.$emit('onLanguageChange', event.target.value);
        },
        toggleMenu() {
            this.isMenuOpen = !this.isMenuOpen;
        }
    }
};
</script>

<style scoped>
header {
    background-image: url('/images/header-bg.jpg');
    background-size: cover;
    background-position: center;
}
</style>
