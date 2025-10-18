<template>
  <ListTable :options="option" :records="records" ref="tableRef">
    <ListColumn
      v-for="column in columns"
      :key="`column-${column.field}`"
      :field="`${column.field}`"
      :title="column.title"
      :width="column.width"
      :columns="column.columns as never"
      editor="dynamic-render-editor"
    >
      <template #edit="{ value }">
        <div
          style="
            width: 100%;
            height: 100px;
            border: 1px solid red;
            background-color: white;
          "
        >
          {{ value }}
        </div>
      </template>
    </ListColumn>
  </ListTable>
</template>

<script lang="ts" setup>

import { ListTable, ListColumn } from '@visactor/vue-vtable'

const generateRandomString = (length: number) => {
  let result = ''
  const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
  for (let i = 0; i < length; i++) {
    result += characters.charAt(Math.floor(Math.random() * characters.length))
  }
  return result
}

const generateRandomHobbies = () => {
  const hobbies = [
    'Reading books',
    'Playing video games',
    'Watching movies',
    'Cooking',
    'Hiking',
    'Traveling',
    'Photography',
    'Playing musical instruments',
    'Gardening',
    'Painting',
    'Writing',
    'Swimming'
  ]

  const numHobbies = Math.floor(Math.random() * 3) + 1
  const selectedHobbies = []

  for (let i = 0; i < numHobbies; i++) {
    const randomIndex = Math.floor(Math.random() * hobbies.length)
    selectedHobbies.push(hobbies[randomIndex])
    hobbies.splice(randomIndex, 1)
  }

  return selectedHobbies.join(', ')
}

const generateRandomBirthday = () => {
  const start = new Date('1970-01-01')
  const end = new Date('2000-12-31')
  const randomDate = new Date(
    start.getTime() + Math.random() * (end.getTime() - start.getTime())
  )
  const year = randomDate.getFullYear()
  const month = randomDate.getMonth() + 1
  const day = randomDate.getDate()
  return `${year}-${month < 10 ? '0' + month : month}-${
    day < 10 ? '0' + day : day
  }`
}

const generateRandomPhoneNumber = () => {
  const areaCode = [
    '130',
    '131',
    '132',
    '133',
    '134',
    '135',
    '136',
    '137',
    '138',
    '139'
  ]
  const prefix = areaCode[Math.floor(Math.random() * areaCode.length)]
  const suffix = String(Math.random()).substr(2, 8)
  return prefix + suffix
}

const generatePersons = (count: number) => {
  return Array.from(new Array(count)).map((_, i) => {
    const first = generateRandomString(10)
    const last = generateRandomString(4)
    return {
      id: i + 1,
      email1: `${first}_${last}@xxx.com`,
      name: first,
      lastName: last,
      hobbies: generateRandomHobbies(),
      birthday: generateRandomBirthday(),
      tel: generateRandomPhoneNumber(),
      address: `No.${i + 100} ${generateRandomString(
        10
      )} ${generateRandomString(5)}`,
      sex: i % 2 === 0 ? 'boy' : 'girl',
      work: i % 2 === 0 ? 'back-end engineer' : 'front-end engineer',
      city: 'beijing'
    }
  })
}

const records = generatePersons(10)
const columns = [
  { field: 'id', title: 'ID', width: 80, sort: true },
  {
    field: 'full name',
    title: 'Full name',
    columns: [
      {
        field: 'name',
        title: 'First Name\n(input editor)',
        width: 120,
        editor: 'input-editor'
      },
      {
        field: 'lastName',
        title: 'Last Name\n(input editor)',
        width: 100,
        editor: 'dynamic-render-editor'
      }
    ]
  },
  { field: 'birthday', title: 'birthday\n(date editor)', width: 120 },
  { field: 'sex', title: 'sex\n(list editor)', width: 100 },
  { field: 'address', title: 'address\n(textArea editor)', width: 300 },
  { field: 'tel', title: 'telephone', width: 150 },
  { field: 'work', title: 'job', width: 200 },
  { field: 'city', title: 'city', width: 150 }
]

const option = {
  limitMaxAutoWidth: 600,
  heightMode: 'autoHeight',
  editCellTrigger: 'click'
}
</script>
