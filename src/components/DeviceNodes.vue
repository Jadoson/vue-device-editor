<template>
  <div>
    <h3>Узлы устройства</h3>

    <div>
      <input v-model="newNodeName" placeholder="Название нового узла" />
      <button @click="addNode">Добавить узел</button>
    </div>

    <draggable v-model="nodes" @end="onDragEnd" item-key="name">
      <template #item="{ element, index }">
        <div class="node-item">
          <div v-if="editingNodeIndex === index">
            <input v-model="editedNodeName" />
            <button @click="saveNode(index)">Сохранить</button>
            <button @click="cancelEdit">Отменить</button>
          </div>
          <div v-else>
            <span>{{ element.name }}</span>
            <button @click="startEdit(index, element.name)">
              Редактировать
            </button>
            <button @click="removeNode(index)">Удалить</button>
          </div>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import draggable from 'vuedraggable'

const props = defineProps({
  nodes: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['update-nodes'])

const newNodeName = ref('')
const editedNodeName = ref('')
const editingNodeIndex = ref(null)
const nodes = ref([...props.nodes])

watch(
  () => props.nodes,
  (newNodes) => {
    nodes.value = [...newNodes]
  }
)

const updateNodes = () => {
  emit('update-nodes', nodes.value)
}

const addNode = () => {
  if (newNodeName.value.trim()) {
    nodes.value.push({ name: newNodeName.value })
    newNodeName.value = ''
    updateNodes()
  }
}

const removeNode = (index) => {
  nodes.value.splice(index, 1) / updateNodes()
}

const startEdit = (index, name) => {
  editingNodeIndex.value = index
  editedNodeName.value = name
}

const saveNode = (index) => {
  nodes.value[index].name = editedNodeName.value
  editingNodeIndex.value = null
  updateNodes()
}

const cancelEdit = () => {
  editingNodeIndex.value = null
  editedNodeName.value = ''
}

const onDragEnd = () => {
  updateNodes()
}
</script>
