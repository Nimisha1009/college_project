<template>
     <q-table :rows="rows" :columns="columns">
    <template v-slot:body-cell-actionControl="props">
      <q-td>
        <q-btn icon="edit" :to="'./edit/' + props.row.id" unelevated color="amber" dense></q-btn>
        <q-btn icon="delete" @click="deleteData(props.row.id)" unelevated color="red" dense></q-btn>
      </q-td>

    </template>
  </q-table>
    </template>
    <script>
    export default {
        name:'ViewCategories',
        data(){
            return {
                rows: [],
                columns: [
        { label: 'ID', field: 'id', name: 'id', align: 'left' },
        { label: 'Category name', field: 'name', name: 'name', align: 'left' },
        { label: 'category Description', field: '', name: '', align: 'left' },
        { label: 'Image', field: '', name: 'actionControl' },
      ]
            }
        },
        methods: {
            async fetchproducts_categories(){
             let httpClient = await this.$api.get('http://localhost:8055/items/products_categories')
             this.rows = httpClient.data.data
            },
            async deleteData (id) {
      this.$q.dialog({
        title: 'Deleting Data',
        message: 'Are you sure?',
        cancel: true,
        persistent: true
      }).onOk(async () => {
        let httpClient = await this.$api.delete('/items/products_categories/' + id)
        this.fetchData()
      })
    }
        },
        created(){
            this.$mitt.on('module-data-changed:products_categories', this.fetchData)
            this.fetchproducts_categories()
        }
    }
    
    </script>