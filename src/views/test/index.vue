<template>
  <div id="aaa" style="width: 300px; height: 200px"></div>
  <button @click="test">测试</button>
</template>

<script setup lang="ts">
import { getCurrentInstance, ref, h, render } from 'vue'
import { ElInput } from 'element-plus'

const ins = getCurrentInstance()

const modelValue = ref('2342')
const test = () => {
  const vnode = h('div', [
    h('div', modelValue.value),
    h(ElInput, {
      modelValue: modelValue.value,
      'onUpdate:modelValue'(value) {
        modelValue.value = value
        console.log(234, value)
      }
    }),
    h('input', {
      type: 'text',
      value: modelValue.value,
      onInput(e) {
        modelValue.value = e.target.value
        console.log(234, e.target.value)
      }
    })
  ])
  console.log(vnode, ins)
  vnode.appContext = ins?.appContext
  const container = document.getElementById('aaa')
  const div = document.createElement('div')
  render(vnode, div)
  container?.appendChild(div)
}
</script>

<style lang="scss" scoped></style>
