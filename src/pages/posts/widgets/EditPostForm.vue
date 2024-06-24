<script setup lang="ts">
import { computed, ref, watch, onMounted, onBeforeUnmount } from 'vue'
import { EmptyProject, Project } from '../types'
import { SelectOption } from 'vuestic-ui'
import { Splitpanes, Pane } from 'splitpanes'
import VaMediumEditor from '../../../components/va-medium-editor/VaMediumEditor.vue'
import 'splitpanes/dist/splitpanes.css'
import PostEditor from '../components/PostEditor.vue'

const props = defineProps<{
  project: Project | null
  saveButtonLabel: string
}>()

defineEmits<{
  (event: 'save', project: Project): void
  (event: 'close'): void
}>()

const defaultNewProject: EmptyProject = {
  project_name: '',
  project_owner: undefined,
  team: [],
  status: undefined,
}

const newProject = ref({ ...defaultNewProject })

const isFormHasUnsavedChanges = computed(() => {
  return Object.keys(newProject.value).some((key) => {
    if (key === 'team') {
      return false
    }

    return (
      newProject.value[key as keyof EmptyProject] !== (props.project ?? defaultNewProject)?.[key as keyof EmptyProject]
    )
  })
})

defineExpose({
  isFormHasUnsavedChanges,
})

watch(
  () => props.project,
  () => {
    if (!props.project) {
      return
    }

    newProject.value = {
      ...props.project,
      project_owner: props.project.project_owner,
    }
  },
  { immediate: true },
)

const required = (v: string | SelectOption) => !!v || 'This field is required'
</script>

<template>
  <splitpanes class="default-theme">
    <pane>
      <VaForm v-slot="{ validate }" class="flex flex-col gap-2">
        <VaInput v-model="newProject.project_name" label="Project name" :rules="[required]" />
        <PostEditor />
        <div class="flex justify-end flex-col-reverse sm:flex-row mt-4 gap-2">
          <VaButton preset="secondary" color="secondary" @click="$emit('close')">Cancel</VaButton>
          <VaButton @click="validate() && $emit('save', newProject as Project)">{{ saveButtonLabel }}</VaButton>
        </div>
      </VaForm>
    </pane>
    <pane>
      <div class="item">test</div>
    </pane>
  </splitpanes>
</template>

<style lang="scss" scoped>
.va-select-content__autocomplete {
  flex: 1;
}

.va-input-wrapper__text {
  gap: 0.2rem;
}
</style>
