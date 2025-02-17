<template>
    <header class="bg-white py-6 shadow-md">
        <div class="container mx-auto flex justify-between items-center px-4">
            <div>
                <h1 class="text-2xl font-bold text-orange-600">
                    David Acosta Laverde
                </h1>
            </div>
            <nav class="hidden md:flex space-x-4">
                <a href="#about" class="text-gray-900 hover:text-orange-600">About</a>
                <a href="#projects" class="text-gray-900 hover:text-orange-600">Projects</a>
                <a href="#contact" class="text-gray-900 hover:text-orange-600">Contact</a>
            </nav>
            <div class="relative">
                <select @change="changeLanguage" class="text-gray-900 hover:text-orange-600">
                    <option value="English">English</option>
                    <option value="Español">Español</option>
                </select>
            </div>
            <button @click="toggleMenu" class="md:hidden text-gray-900 hover:text-orange-600">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
                </svg>
            </button>
        </div>
        <div v-if="isMenuOpen" class="md:hidden">
            <nav class="flex flex-col space-y-2 px-4 py-2">
                <a href="#about" class="text-gray-900 hover:text-orange-600">About</a>
                <a href="#projects" class="text-gray-900 hover:text-orange-600">Projects</a>
                <a href="#contact" class="text-gray-900 hover:text-orange-600">Contact</a>
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
