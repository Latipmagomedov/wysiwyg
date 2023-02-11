<template>
    <div class="v-input">
        <input
            v-model="inputVal"
            :type="type"
            class="v-input__input"
            :placeholder="placeholder"
        >
    </div>
</template>

<script setup>
import {computed} from "vue"

const props = defineProps({
    modelValue: String,
    placeholder: String,
    type: String
});

const emit = defineEmits(["update:modelValue"])

const inputVal = computed({
    get: () => props.modelValue,
    set: (value) => emit("update:modelValue", value)
});
</script>

<style scoped lang="scss">
.v-input {
    height: 48px;
    border-radius: 12px;
    overflow: hidden;
    display: flex;
    align-items: center;
    background-color: $dark-2;

    &__input {
        width: 100%;
        height: 100%;
        padding-left: 16px;
        background-color: transparent;
        color: #fff;
        font-size: 14px;
        font-weight: 400;

        &::placeholder {
            color: rgba(255, 255, 255, 0.4);
            font-size: 14px;
            font-weight: 400;
        }
    }
}
</style>
