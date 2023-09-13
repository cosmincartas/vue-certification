<script setup>
/* const props = defineProps({
    movie: {
        tupe: Object,
        required: true,
    },
}); */
</script>

<template>
    <div class="movie-card grid bg-white">
        <div>
            <slot name="banner"></slot>
        </div>
        <div class="movie-details grid p-4 gap-2">
            <div class="font-bold text-xl">
                <slot></slot>
            </div>
            <div class="flex gap-2">
                <slot name="genre"></slot>
            </div>
            <div>
                <slot name="description"></slot>
            </div>
            <div class="w-full h-full flex items-center">
                <slot name="rating"></slot>
            </div>
        </div>
    </div>
</template>
<style scoped>
.movie-card {
    flex: 1;
    grid-template-rows: 60% auto;
}

.movie-details {
    grid-template-rows: auto 32px 1fr 20px;
}
</style>
