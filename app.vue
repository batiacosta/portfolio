<template>
    <header>
        <Header />
    </header>
    <div id="app" class="font-sans sm:mr-10 sm:ml-10 md:mr-20 md:ml-20">
        <!-- <button type="button" class="btn" @click="showModal">
            Open Modal!
        </button>

        <Modal v-show="isModalVisible" @close="closeModal" /> -->

        <div id="introduction" class="text-center">
            <h1
                class="text-3xl font-bold bg-gradient-to-r from-yellow-400 via-orange-500 to-rose-700 bg-clip-text text-transparent m-8"
            >
                XR Game Developer
            </h1>
            <h1 class="text-gray-900 text-xl m-8">
                Hello, I am David, an XR Game Developer, and it is an honor to
                meet you. I specialize in creating immersive and interactive
                experiences for mobile, PC, and XR platforms. My expertise
                includes working with Unity Engine, C#, Unreal Engine, C++, and
                Blueprints. Here, you can explore my projects, find my contact
                information, and view my resume.
            </h1>
        </div>
        <div id="porfolio">
            <h1 class="text-xl font-bold text-gray-900 text-center">
                Projects
            </h1>
        </div>
    </div>
</template>

<script>
import Modal from "./components/Modal.vue";
import Header from "./components/Header.vue";

export default {
    name: "App",
    components: {
        Modal,
        Header,
    },
    data() {
        return {
            isModalVisible: false,
        };
    },
    methods: {
        showModal() {
            this.isModalVisible = true;
        },
        closeModal() {
            this.isModalVisible = false;
        },
    },
};
</script>
