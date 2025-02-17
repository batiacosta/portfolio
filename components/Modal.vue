<template>
    <div class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all sm:max-w-2xl sm:w-full">
            <header class="bg-gray-100 px-4 py-3 border-b border-gray-200 flex justify-between items-center">
                <slot name="header"> This is the default title! </slot>
                <button type="button" class="text-gray-500 hover:text-gray-700" @click="close">
                    &times;
                </button>
            </header>

            <section class="px-4 py-5">
                <slot name="body"> This is the default body! </slot>
            </section>

            
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
