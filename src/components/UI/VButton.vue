<template>
    <button class="v-button">
        {{ label }}
    </button>
</template>

<script setup>
defineProps({
    label: String
});
</script>

<style scoped lang="scss">
.v-button {
    height: 48px;
    padding: 0 25px;
    border-radius: 12px;
    background-color: $blue-2;
    color: #fff;
    font-size: 16px;
    font-weight: 400;
    cursor: pointer;
    transition: 0.3s;

    &:hover {
        background-color: $blue-3;
    }

    &:active {
        background-color: $blue-2;
    }
}
</style>
