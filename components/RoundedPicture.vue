<template>
    <div class="rounded-full overflow-hidden w-72 h-72 mx-auto">
        <img :src="src" :alt="alt" class="object-cover w-full h-full" />
    </div>
</template>

<script>
export default {
    name: 'RoundedPicture',
    props: {
        src: {
            type: String,
            required: true
        },
        alt: {
            type: String,
            default: ''
        }
    }
}
</script>

<style scoped>
</style>
