<script setup>
defineProps({
    isVisible: Boolean,
})
</script>
<template>
    <div v-if="isVisible" class="fixed top-0 left-0 h-full w-full">
        <div class="fixed top-0 left-0 h-full w-full bg-gray-300 opacity-30 overflow-hidden"></div>
        <div
            class="modal-container relative grid rounded mt-[15%] mx-auto w-[50%] h-1/2 p-8"
            style="background-color: slategray">
            <div>
                <slot name="title"></slot>
            </div>
            <div>
                <slot></slot>
            </div>
            <div>
                <slot name="actions"></slot>
            </div>
        </div>
    </div>
</template>

<style scoped>
.modal-container {
    grid-template-rows: auto 1fr auto;
}
</style>
