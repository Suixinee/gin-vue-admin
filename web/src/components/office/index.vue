<template>
  <div class="office border border-solid border-gray-100 h-full">
    <el-row>
      <div v-if="ext==='docx'">
        <Docx v-model="fullFileURL" />
      </div>
      <div v-else-if="ext==='pdf'">
        <Pdf v-model="fullFileURL" />
      </div>
      <div v-else-if="ext==='xlsx'">
        <Excel v-model="fullFileURL" />
      </div>
      <div v-else-if="ext==='image'">
        <el-image :src="fullFileURL" lazy/>
      </div>
    </el-row>

  </div>
</template>
<script>
export default {
  name: "Office"
}
</script>
<script setup>
import {ref, watch, computed} from "vue"
import Docx from "@/components/office/docx.vue";
import Pdf from "@/components/office/pdf.vue";
import Excel from "@/components/office/excel.vue";

const path = ref(import.meta.env.VITE_BASE_API)

const props = defineProps({
  modelValue: {
    type: String,
    default: () => ""
  }
})
const fileUrl = ref("")
const ext = ref("")
watch(
    () => props.modelValue,
    val => {
      fileUrl.value = val
      const fileExt = val.split(".")[1] || ''
      const image = ['png', 'jpg', 'jpge', 'gif']
      ext.value = image.includes(fileExt) ? 'image' : fileExt
    },
    {immediate: true}
)
const fullFileURL = computed(() => {
  return path.value + '/' + fileUrl.value
})
</script>
<style lang="scss" scoped>
.office {
  height: 400px;
  width: 100%;
}
</style>