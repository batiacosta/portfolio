<template>
    <div class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-50">
        <div class="bg-gray-900 text-white rounded-lg overflow-hidden shadow-xl transform transition-all sm:max-w-11/12 sm:w-full lg:max-w-11/12 lg:max-h-[90vh] lg:max-w-[90vw]">
            <section class="px-4 py-5 overflow-y-auto max-h-[65vh]">
                <h2 class="text-xl font-bold text-white">
                    <slot name="header"> This is the default title! </slot>
                </h2>
                <slot name="body"> This is the default body! </slot>
                <slot name="footer">
                    <button type="button" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600" @click="close">Close</button>
                </slot>
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
