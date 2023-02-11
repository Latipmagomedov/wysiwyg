<template>
    <component
        :is="tag"
        contenteditable
        @input="update"
        ref="element"
        class="field"
    >
    </component>
</template>

<script setup>
import {ref, onMounted, watch} from "vue"

const props = defineProps({
    tag: String,
    modelValue: String,
})

const emit = defineEmits(["update:modelValue"])

const element = ref(null)

const updateContent = (newcontent) => element.value.innerHTML = newcontent
const update = () => emit("update:modelValue", element.value.innerHTML)

watch(() => props.modelValue, (newval) => {
    if (newval !== element.value.innerHTML) updateContent(newval ?? "")
})

onMounted(() => {
    updateContent(props.modelValue ?? "")
    element.value.focus()
})
</script>

<style scoped lang="scss">
.field {
    padding-bottom: 60px;
    display: flex;
    flex-direction: column;

    &::v-deep(img) {
        max-width: 400px;
    }
}
</style>

