<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-btn color="primary" dark class="mb-2" @click="addItem">New Item</v-btn>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card>
            <v-card-text>
              <v-container >
                <v-form-base :model="editedItem" :schema="schema"></v-form-base>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="okay">Ok</v-btn>
              <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>
<script>
  import VFormBase from './vFormBase'  // 
  // import VFormBase from 'vuetify-form-base'
  
  export default {
    components:{
      VFormBase
    },
    data: () => ({
      switch1: true,
      schema:{
        name:{type:'text', label:'Dessert name'},
        calories:{type:'text',label:'Calories'},
        fat:{type:'text',label:'Fat (g)'},
        carbs:{type:'text',label:'Carbs (g)'},
        protein:{type:'text',label:'Protein (g)'}
      },
      dialog: false,
      headers: [
        {
          text: 'Dessert (100g serving)',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Calories', value: 'calories' },
        { text: 'Fat (g)', value: 'fat' },
        { text: 'Carbs (g)', value: 'carbs' },
        { text: 'Protein (g)', value: 'protein' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {},
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      bufferItemForCanceling:{}
    }),

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.desserts = [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
          }
        ]
      },
      addItem(){
        this.desserts.push({...this.defaultItem})
        this.editItem(this.desserts[this.desserts.length-1])
      },
      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        
        this.editedItem= item
        this.bufferItemForCanceling = {...item }
        this.dialog = true
      },
      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        this.desserts.splice(index, 1)
      },
      okay () {
        this.dialog = false
      },
      cancel () {
        this.editedItem = Object.assign(this.editedItem, this.bufferItemForCanceling )       
        this.dialog = false
      },
    },
  }
</script>
