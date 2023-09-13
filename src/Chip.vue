<script setup>
defineProps({
    label: {
        type: String,
        required: true,
    },
    color: {
        type: String,
        required: false,
        default: 'purple',
    },
});
</script>
<template>
    <div :style="{ background: color }" class="rounded-full p-1 text-white font-medium">
        {{ label }}
    </div>
</template>
