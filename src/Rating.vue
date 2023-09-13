<script setup>
defineProps({
    icon: {
        type: Function,
        required: true,
    },
    rating: {
        type: Number,
        required: true,
    },
});
const emit = defineEmits(['changed']);

function changeRating(value) {
    emit('changed', value);
}
</script>
<template>
    <button
        v-for="value in 5"
        class="disabled:cursor-not-allowed"
        :class="value <= rating ? 'text-amber-300' : 'text-gray-300'"
        :disabled="value === rating"
        @click="changeRating(value)">
        <component :is="icon" class="w-[24px]" />
    </button>
</template>
<style scoped></style>
