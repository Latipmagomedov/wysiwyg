<template>
    <div class="editor" ref="editorField">
        <ControlButtons
            @undo="undo"
            @redo="redo"
            @formatText="formatText"
            @addImage="addImage"
            @copy="copy"/>

        <ContentField
            v-model="editor"
            @update:modelValue="changeEditor"
            tag="div"
            class="editor__field"/>
    </div>
</template>

<script setup>
import {onMounted, ref} from "vue"
import ControlButtons from './ControlButtons'
import ContentField from './ContentField'

const editor = ref('')

const editorField = ref(null)
const editorFieldHTML = ref(null)

// объект истории действий
const history = {
    back: [],
    forward: []
}

// Сохранение действий
const performAction = () => {
    if (!history.back.length || history.back[history.back.length - 1] !== editorFieldHTML.value.innerHTML) {
        history.back.push(editorFieldHTML.value.innerHTML)
    }
}

//Возврат действий
const undo = () => {
    if (!history.back.length) return
    history.forward.push(editorFieldHTML.value.innerHTML)
    editor.value = history.back.pop()
}

// Повтор действий
const redo = () => {
    if (!history.forward.length) return
    history.back.push(editorFieldHTML.value.innerHTML)
    editor.value = history.forward.pop()
}

// Вызывается при каждом изменении в редакторе и сохраняет действия
const changeEditor = () => {
    history.forward = []
    history.back.push(editorFieldHTML.value.innerHTML)
}

// Для обертывания выделенного текста в тег
const wrapTag = (tagName, style) => {
    const tag = document.createElement(tagName)
    if (style) tag.style.cssText = style.join('')
    const userSelection = window.getSelection();
    const selectedTextRange = userSelection.getRangeAt(0)
    selectedTextRange.surroundContents(tag)
}

// Форматирование текста
const formatText = (type) => {
    performAction()
    if (type === 'title') wrapTag('h3', ['font-size: 40px;'])
    if (type === 'paragraph') wrapTag('p')
}

// Добавление изображения
const addImage = () => {
    const link = prompt('Вставьте ссылку на изображение');
    if (!link) return

    const selection = document.getSelection()
    if (document.execCommand && document.queryCommandSupported('insertImage') && selection.anchorNode) {
        document.execCommand('insertImage', false, link)
    } else {
        editor.value += `<img src="${link}" alt="image" style="max-width: 400px">`
    }
}

// Копирование HTML
const copy = () => navigator.clipboard.writeText(editorFieldHTML.value.innerHTML)

onMounted(() => {
    editorFieldHTML.value = editorField.value.querySelector('.editor__field')
})
</script>

<style scoped lang="scss">
.editor {
    &__field {
        min-height: 600px;
        margin-top: 31px;
        font-size: 15px;
        outline: none;
    }
}
</style>

