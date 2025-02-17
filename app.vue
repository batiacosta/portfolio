<template>
    <header class=" text-white p-4 shadow-md">
        <Header />
    </header>
    <div id="app" class="font-sans sm:mr-10 sm:ml-10 md:mr-20 md:ml-20">
        <div id="introduction" class="text-center py-10">
            <h1 class="text-4xl font-extrabold bg-gradient-to-r from-yellow-400 via-orange-500 to-rose-700 bg-clip-text text-transparent mb-6">
                XR Game Developer
            </h1>
            <RoundedPicture src="/images/profile.jpg" alt="David Acosta Laverde" />
            <p class="text-gray-700 text-xl mx-auto leading-relaxed">
                Hello, I am David, an XR Game Developer, and it is an honor to meet you. I specialize in creating immersive and interactive experiences for mobile, PC, and XR platforms. My expertise includes working with Unity Engine, C#, Unreal Engine, C++, and Blueprints. Here, you can explore my projects, find my contact information, and view my resume.
            </p>
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

export default {
    name: "App",
    components: {
        Header,
        RoundedPicture,
        Card,
    },
    data() {
        return {
            projects: []
        };
    },
    created() {
        fetch('/json/projects.json')
            .then(response => response.json())
            .then(data => {
                this.projects = data;
            });
    }
};
</script>
