<template>
    <div class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-50">
        <div class="bg-gray-900 text-white rounded-lg overflow-hidden shadow-xl transform transition-all 
                    w-full h-full sm:w-full sm:h-full 
                    md:w-[60%] md:h-[85%] md:max-h-[85vh] 
                    lg:w-[60%] lg:h-[85%] lg:max-h-[85vh]">
            <header class="bg-gray-800 px-4 py-3 border-b border-gray-700 flex justify-between items-center">
                <h2 class="text-xl font-bold text-white">
                    <slot name="header"> This is the default title! </slot>
                </h2>
                <button type="button" class="text-gray-500 hover:text-gray-300" @click="close">
                    &times;
                </button>
            </header>

            <section class="px-4 py-5 overflow-y-auto flex-1 h-full">
                <slot name="body"> This is the default body! </slot>
            </section>
            
            <footer class="bg-gray-800 px-4 py-3 border-t border-gray-700 flex justify-end">
                <slot name="footer">
                    <button type="button" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600" @click="close">Close</button>
                </slot>
            </footer>
        </div>
    </div>
</template>

<script>
let icons = {
    github: "images/icons/github.svg",
    apple: "images/icons/apple.svg",
    windows: "images/icons/windows.svg",
    android: "images/icons/android.svg",
    steam: "images/icons/steam.svg",
    epic: "images/icons/epic.svg",
    meta: "images/icons/meta.svg",
    other: "images/icons/world.svg",
};
export default {
    name: "Modal",
    methods: {
        close() {
            this.$emit("close");
        },
        getIcon(type) {
            return icons[type] || icons.other;
        }
    }
};
</script>
