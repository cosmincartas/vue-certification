<script setup></script>
<template>
    <div class="fixed top-0 left-0 h-full w-full">
        <div class="fixed top-0 left-0 h-full w-full bg-gray-300 opacity-30"></div>
        <div class="grid rounded bg-yellow-500 mt-[25%] mx-auto w-[80%] h-1/2">
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

<style scoped></style>
