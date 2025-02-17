<template>
    <header class="bg-gray-800 text-white p-4 shadow-md">
        <Header @onLanguageChange="setLanguage" />
    </header>
    <div id="app" class="font-sans sm:mr-10 sm:ml-10 md:mr-20 md:ml-20">
        <div id="introduction" class="text-center py-10">
            <h1 class="text-4xl font-extrabold bg-gradient-to-r from-yellow-400 via-orange-500 to-rose-700 bg-clip-text text-transparent mb-6">
                <TypingEffect :strings="titles" :typeSpeed="50" :backSpeed="50" :loop="true" />
            </h1>
            <RoundedPicture src="/images/profile.jpg" alt="David Acosta Laverde" />
            <div class="text-gray-700 text-lg mx-auto p-2 leading-relaxed max-w-2xl mt-6 text-justify sm:text-justify">
                <p v-for="(paragraph, index) in descriptionParagraphs" :key="index">
                    {{ paragraph }}
                </p>
            </div>
            <div class="mt-6 flex justify-center space-x-4">
                <a v-for="(social, index) in socials" :key="index" :href="social.src" target="_blank" class="text-gray-900 hover:text-orange-600">
                    <img :src="getSocialIconSrc(social.type)" :alt="social.type" class="w-6 h-6">
                </a>
            </div>
        </div>
        <div id="work" class="py-10">
            <h2 class="text-2xl font-bold text-gray-900 text-center mb-8">
                {{ workSubtitle }}
            </h2>
            <p class="text-gray-700 text-lg mx-auto leading-relaxed max-w-2xl mb-8 text-justify sm:text-justify">
                {{ workDescription }}
            </p>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
                <Card v-for="(project, index) in workProjects" :key="index" :project="project" />
            </div>
        </div>
        <div id="projects" class="py-10">
            <h2 class="text-2xl font-bold text-gray-900 text-center mb-8">
                {{ projectsSubtitle }}
            </h2>
            <p class="text-gray-700 text-lg mx-auto leading-relaxed max-w-2xl mb-8 text-justify sm:text-justify">
                {{ projectsDescription }}
            </p>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
                <Card v-for="(project, index) in projects" :key="index" :project="project" />
            </div>
        </div>
    </div>
</template>

<script>
import Header from "./components/Header.vue";
import RoundedPicture from "./components/RoundedPicture.vue";
import Card from "./components/Card.vue";
import TypingEffect from './components/TypingEffect.vue';

export default {
    name: "App",
    components: {
        Header,
        RoundedPicture,
        Card,
        TypingEffect
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
            projectsDescription: ''
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

                    const projectsSection = data.subtitles.find(sub => sub.subtitle.some(s => s.content.includes('Projects')));
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
        }
    }
};
</script>
