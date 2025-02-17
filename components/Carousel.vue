<template>
    <div class="carousel">
        <div v-for="(item, index) in media" :key="index" class="carousel-item">
            <img v-if="item.type === 'image'" :src="item.src" :alt="item.alt" class="w-full h-48 object-cover" />
            <iframe v-if="item.type === 'youtube'" :src="item.src" :title="item.alt" class="w-full h-48" frameborder="0" allowfullscreen></iframe>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Carousel',
    props: {
        media: {
            type: Array,
            required: true
        }
    }
}
</script>

<style scoped>
.carousel {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
}
.carousel-item {
    flex: none;
    scroll-snap-align: start;
    width: 100%;
}
</style>
