<template>
  <div>
    <h1>Редактор устройств</h1>
    <div>
      <input v-model="newDeviceName" placeholder="Название нового устройства" />
      <button @click="addDevice">Добавить устройство</button>
    </div>

    <ul>
      <li v-for="(device, index) in devices" :key="index">
        <div v-if="editingDeviceIndex === index">
          <input v-model="device.name" />
          <button @click="saveDevice(index)">Сохранить</button>
          <button @click="cancelEdit">Отменить</button>
        </div>
        <div v-else>
          <span>{{ device.name }}</span>
          <button @click="startEdit(index)">Редактировать</button>
          <button @click="removeDevice(index)">Удалить</button>
        </div>

        <DeviceNodes :nodes="device.nodes" @update-nodes="updateNodes(index)" />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import DeviceNodes from './components/DeviceNodes.vue'

const devices = ref([])
const newDeviceName = ref('')
const editingDeviceIndex = ref(null)

const addDevice = () => {
  if (newDeviceName.value.trim()) {
    devices.value.push({ name: newDeviceName.value, nodes: [] })
    console.log(`newDeviceName.value=${newDeviceName.value}`)
    newDeviceName.value = ''
  }
}

const removeDevice = (index) => {
  devices.value.splice(index, 1)
}

const startEdit = (index) => {
  editingDeviceIndex.value = index
}

const saveDevice = (index) => {
  editingDeviceIndex.value = null
}

const cancelEdit = () => {
  editingDeviceIndex.value = null
}

const updateNodes = (index) => (newNodes) => {
  devices.value[index].nodes = newNodes
}
</script>

<style scoped>
ul {
  list-style-type: none;
}
</style>
