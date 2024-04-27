<script setup>
import { ref } from 'vue'
import router from '../router'

const columns = ref([
  {
    name: 'id',
    align: 'left',
    label: 'No.',
    field: 'id',
    sortable: true,
  },
  {
    name: 'fname',
    align: 'left',
    label: 'Firstname',
    field: 'fname',
    sortable: true,
  },
  {
    name: 'lname',
    align: 'left',
    label: 'Lastname',
    field: 'lname',
    sortable: true,
  },
  {
    name: 'username',
    align: 'left',
    label: 'Username',
    field: 'username',
    sortable: true,
  },
  {
    name: 'avatar',
    align: 'center',
    label: 'Avatar',
    field: 'avatar',
    sortable: true,
  },
  {
    name: 'actions',
    align: 'center',
    label: '',
    field: 'avatar',
    sortable: true,
  },
])

const rows = ref([])

const fetchData = () => {
  fetch('https://www.melivecode.com/api/users')
    .then((response) => response.json())
    .then((result) => {
      rows.value = result
    })
}
fetchData()

const onEdit = (id) => {
  router.push('/update/' + id)
}

const onDelete = (id) => {
  const myHeaders = new Headers()
  myHeaders.append('Content-Type', 'application/json')

  const raw = JSON.stringify({
    id: id,
  })

  const requestOptions = {
    method: 'DELETE',
    headers: myHeaders,
    body: raw,
    redirect: 'follow',
  }

  fetch('https://www.melivecode.com/api/users/delete', requestOptions)
    .then((response) => response.json())
    .then((result) => {
      alert(result.message)
      fetchData()
    })
    .catch((error) => console.error(error))
}

const onCreate = () => {
  router.push('/create')
}
</script>

<template>
  <div class="q-pa-md">
    <div class="q-pa-md">
      <q-btn icon="add" @click="onCreate()" />
    </div>

    <q-table title="Records" :rows="rows" :columns="columns" row-key="name">
      <template v-slot:body-cell-avatar="props">
        <q-td :props="props">
          <q-img :src="props.row.avatar" />
        </q-td>
      </template>
      <template v-slot:body-cell-actions="props">
        <q-td :props="props">
          <q-btn
            icon="mode_edit"
            @click="onEdit(props.row.id)"
            flat
            dense
            color="primary"
          />
          <q-btn
            icon="delete"
            @click="onDelete(props.row.id)"
            flat
            dense
            color="negative"
          />
        </q-td>
      </template>
    </q-table>
  </div>
</template>
