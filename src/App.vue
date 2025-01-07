<template>
  <el-form
    :label-position="labelPosition"
    label-width="auto"
    :model="formLabelAlign"
    style="max-width: 600px"
  >
    <el-form-item label="Name" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.name" />
    </el-form-item>
    <el-form-item label="Password" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.password" type="password" />
    </el-form-item>
    <el-form-item label="Code" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.code" />
      <img :src="codeUrl" alt="" @click="resetCodeClick">
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submit">Submit</el-button>
    </el-form-item>
  </el-form>

  <button @click="downloadImage">download</button>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { FormItemProps, FormProps } from 'element-plus'

const labelPosition = ref<FormProps['labelPosition']>('right')
const itemLabelPosition = ref<FormItemProps['labelPosition']>('')

const codeUrl = ref<string>('/api/user/code')
const formLabelAlign = reactive({
  name: '',
  password: '',
  code: '',
})

const resetCodeClick = () =>  codeUrl.value = codeUrl.value + '?' + Math.random()

const submit = () =>{
  fetch('/api/user/create',{
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formLabelAlign)
  }).then(res=>res.json()).then(res=>{
    console.log('res',res);
    resetCodeClick()
  })
}


const useFeatch = async (urls:string) =>{
 const res = await fetch(urls).then(res=>res.arrayBuffer()) 
 const blod = new Blob([res])
 const url = URL.createObjectURL(blod)
 const a = document.createElement('a')
 a.href = url
 a.download = 'image.png'
 a.click()
 
 }
const downloadImage = () =>{
  // useFeatch('http://localhost:3000/upload/stream')
  window.open('http://localhost:3000/upload/export')
}

</script>
