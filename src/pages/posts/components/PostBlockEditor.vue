<template>
    <div>
        <ul>
            <li @click="setBold()">
                B
            </li>
            <li>
                Image
            </li>
        </ul>
    </div>
</template>

<script lang="ts" setup>
import { PropType } from 'vue'
import { Editor } from '@tiptap/vue-3'

const props = defineProps({
  editor: Object as PropType<Editor>,
})

const setBold = () => {
    props.editor?.chain().focus().toggleBold().run()
}

const setItalic = () => {
    props.editor?.chain().focus().toggleItalic().run()
}

</script>

<style scoped>
/* Add your component styles here */
</style>