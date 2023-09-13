<script setup></script>
<template>
    <div class="fixed top-0 left-0 h-full w-full">
        <div class="fixed top-0 left-0 h-full w-full bg-gray-300 opacity-30"></div>
        <div
            class="modal-container grid rounded mt-[25%] mx-auto w-[80%] h-1/2 p-8"
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
