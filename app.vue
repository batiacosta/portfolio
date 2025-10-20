<template>
    <header class="bg-gradient-to-r from-gray-900 via-gray-700 to-gray-900 py-6 shadow-md">
        <Header @onLanguageChange="setLanguage" />
    </header>
    <div id="app" class="font-sans bg-dark-pattern text-white min-h-screen w-full">
        <div class="container mx-auto px-4 lg:px-16 xl:px-32">
            <div id="introduction" class="text-center py-10">
                <h1 class="text-4xl font-extrabold bg-gradient-to-r from-yellow-400 via-orange-500 to-rose-700 bg-clip-text text-transparent mb-6">
                    <TypingEffect :strings="titles" :typeSpeed="50" :backSpeed="50" :loop="true" />
                </h1>
                <RoundedPicture src="/images/profile.jpg" alt="David Acosta Laverde" />
                <div class="text-gray-300 text-lg mx-auto p-2 leading-relaxed max-w-2xl mt-6 text-justify sm:text-justify">
                    <p v-for="(paragraph, index) in descriptionParagraphs" :key="index">
                        {{ paragraph }}
                    </p>
                </div>
                <div class="mt-6 flex justify-center space-x-4">
                    <a v-for="(social, index) in socials" :key="index" :href="social.src" target="_blank" class="text-gray-300 hover:text-yellow-400">
                        <img :src="getSocialIconSrc(social.type)" :alt="social.type" class="w-6 h-6 filter invert transition-transform duration-200 transform hover:scale-125">
                    </a>
                </div>
            </div>
            <div id="work" class="py-10">
                <h2 class="text-2xl font-bold text-gray-300 text-center mb-8">
                {{ workSubtitle }}
                </h2>
                <p class="text-gray-300 text-lg mx-auto p-2 leading-relaxed max-w-2xl mt-6 text-justify sm:text-justify">
                {{ workDescription }}
                </p>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8 justify-items-center">
                <Card v-for="(project, index) in workProjects" :key="index" :project="project" @show-modal="openModal" />
                </div>
            </div>
            <div id="projects" class="py-10">
                <h2 class="text-2xl font-bold text-gray-300 text-center mb-8">
                    {{ projectsSubtitle }}
                </h2>
                <p class="text-gray-300 text-lg mx-auto p-2 leading-relaxed max-w-2xl mt-6 text-justify sm:text-justify">
                    {{ projectsDescription }}
                </p>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8 justify-items-center">
                    <Card v-for="(project, index) in projects" :key="index" :project="project" @show-modal="openModal" />
                </div>
            </div>
        </div>
    </div>
        
    <!-- Modal -->
    <teleport to="body">
        <Modal v-if="isModalVisible" @close="closeModal">
            <template #header>{{ selectedProject?.title }}</template>
            <template #body>
                <div class="text-gray-300 text-base mt-4 text-justify">
                    <p v-for="(paragraph, index) in selectedProject?.description?.split('\n\n')" :key="index" class="mb-4">{{ paragraph }}</p>
                    <div class="mt-4">
                        <h3 class="text-lg font-bold text-white mb-2">Links:</h3>
                        <div v-for="(link, index) in selectedProject?.links" :key="index" class="flex items-center space-x-2 mb-2">
                            <img :src="getIcon(link.type)" alt="icon" class="w-6 h-6 filter-orange">
                            <span v-if="link.platform" class="text-gray-300 font-bold">{{ link.platform }}</span>
                            <a :href="link.src" target="_blank" class="text-orange-400 hover:underline font-bold">Link</a>
                        </div>
                    </div>
                    
                    <div class="mt-4">
                        <h3 class="text-lg font-bold text-white mb-2">Tags:</h3>
                        <div v-for="(tag, index) in selectedProject?.tags" :key="index" class="inline-block bg-gray-700 rounded-full px-3 py-1 text-sm font-semibold text-gray-300 mr-2 mb-2">
                            {{ tag }}
                        </div>
                    </div>
                    
                    <div class="mt-4">
                        <h3 class="text-lg font-bold text-white mb-2">Media:</h3>
                        <div v-for="(item, index) in selectedProject?.media" :key="index" class="mb-6 lg:mb-8">
                            <img v-if="item.type === 'image'" :src="item.src" :alt="item.alt" class="w-full object-cover mb-4 rounded mx-auto px-2 sm:px-4 lg:px-8 xl:px-16" />
                            <iframe v-if="item.type === 'youtube'" :src="item.src" :title="item.alt" class="w-full h-64 lg:h-80 rounded mx-auto px-2 sm:px-4 lg:px-8 xl:px-16" frameborder="0" allowfullscreen></iframe>
                        </div>
                    </div>
                </div>
            </template>
            <template #footer>
                <button @click="closeModal" class="bg-gradient-to-r from-orange-400 via-orange-500 to-rose-500 text-white px-4 py-2 rounded hover:opacity-90">Close</button>
            </template>
        </Modal>
    </teleport>
</template>

<script>
import Header from "./components/Header.vue";
import RoundedPicture from "./components/RoundedPicture.vue";
import Card from "./components/Card.vue";
import TypingEffect from './components/TypingEffect.vue';
import Modal from './components/Modal.vue';

export default {
    name: "App",
    components: {
        Header,
        RoundedPicture,
        Card,
        TypingEffect,
        Modal
    },
    data() {
        return {
            language: 'English',
            titles: [],
            description: '',
            projects: [],
            workProjects: [],
            socials: [],
            workSubtitle: '',
            workDescription: '',
            projectsSubtitle: '',
            projectsDescription: '',
            isModalVisible: false,
            selectedProject: null
        };
    },
    computed: {
        descriptionParagraphs() {
            return this.description.split('\n\n');
        }
    },
    created() {
        this.detectLanguage();
        this.loadContent();
    },
    methods: {
        detectLanguage() {
            const userLang = navigator.language || navigator.userLanguage;
            if (userLang.startsWith('es')) {
                this.language = 'Español';
            } else {
                this.language = 'English';
            }
        },
        setLanguage(language) {
            console.log(`Selected language is ${language}`);
            this.language = language;
            this.loadContent();
        },
        loadContent() {
            fetch('/json/home.json')
                .then(response => response.json())
                .then(data => {
                    this.titles = data.title.filter(item => item.language === this.language).map(item => item.content);
                    this.description = data.description.find(item => item.language === this.language).content;
                    this.socials = data.socials;

                    const workSection = data.subtitles.find(sub => sub.subtitle.some(s => s.content.includes('Work')));
                    this.workSubtitle = workSection.subtitle.find(s => s.language === this.language).content;
                    this.workDescription = workSection.description.find(d => d.language === this.language).content;

                    const projectsSection = data.subtitles.find(sub => sub.subtitle.some(s => s.content.includes('Ventures')));
                    this.projectsSubtitle = projectsSection.subtitle.find(s => s.language === this.language).content;
                    this.projectsDescription = projectsSection.description.find(d => d.language === this.language).content;
                });

            fetch('/json/work.json')
                .then(response => response.json())
                .then(data => {
                    this.workProjects = data.map(project => ({
                        ...project,
                        title: project.title[this.language],
                        summary: project.summary[this.language],
                        description: project.description[this.language],
                        tags: project.tags[this.language]
                    }));
                });

            fetch('/json/projects.json')
                .then(response => response.json())
                .then(data => {
                    this.projects = data.map(project => ({
                        ...project,
                        title: project.title[this.language],
                        summary: project.summary[this.language],
                        description: project.description[this.language],
                        tags: project.tags[this.language]
                    }));
                });
        },
        getSocialIconSrc(type) {
            switch (type) {
                case 'LinkedIn':
                    return '/images/icons/linkedin.svg';
                case 'GitHub':
                    return '/images/icons/github.svg';
                case 'YouTube':
                    return '/images/icons/youtube.svg';
                default:
                    return '';
            }
        },
        openModal(project) {
            this.selectedProject = project;
            this.isModalVisible = true;
        },
        closeModal() {
            this.isModalVisible = false;
            this.selectedProject = null;
        },
        getIcon(type) {
            let icons = {
                github: "/images/icons/github.svg",
                apple: "/images/icons/apple.svg",
                windows: "/images/icons/windows.svg",
                android: "/images/icons/android.svg",
                steam: "/images/icons/steam.svg",
                epic: "/images/icons/epic.svg",
                meta: "/images/icons/meta.svg",
                other: "/images/icons/world.svg",
            };
            return icons[type] || icons.other;
        }
    }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

body {
    font-family: 'Press Start 2P', cursive;
}

.bg-dark-pattern {
    background-color: #1a1a1a;
    background-image: url('/images/game-pattern.png');
    background-size: cover;
    background-position: center;
    min-height: 100vh;
}

.filter-orange {
    filter: invert(48%) sepia(95%) saturate(748%) hue-rotate(1deg) brightness(100%) contrast(101%);
}
</style>
