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
        <div id="portfolio" class="py-10">
            <h2 class="text-2xl font-bold text-gray-900 text-center mb-8">
                Projects
            </h2>
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
import TypingEffect from './components/TypingEffect.vue'

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
            socials: []
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
