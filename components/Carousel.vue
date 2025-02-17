<template>
    <div :class="['carousel', { 'carousel-modal': isModal }]">
        <div v-for="(item, index) in media" :key="index" class="carousel-item">
            <img v-if="item.type === 'image'" :src="item.src" :alt="item.alt" :class="['h-full, w-full']" />
            <iframe v-if="item.type === 'youtube'" :src="item.src" :title="item.alt" :class="['h-full']" frameborder="0" allowfullscreen></iframe>
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
        },
        isModal: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        carouselHeight() {
            return this.isModal ? 'h-60' : 'h-48';
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
.carousel-modal .carousel-item img,
.carousel-modal .carousel-item iframe {
    height: 24rem; /* h-96 */
}
</style>
