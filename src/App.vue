<template>
  <div id="app">
    <h1>Shopping List</h1>
    <ShoppingListItemForm @addItem="addItem"/>
    <ShoppingList
    :shoppingList="shoppingList"
    @removeItem="removeItem"/>
  </div>
</template>

<script>
import ShoppingListItemForm from '@/components/ShoppingListItemForm.vue'
import ShoppingList from '@/components/ShoppingList.vue'

export default {
  name: 'App',
  components: {
    ShoppingListItemForm,
    ShoppingList
  },
  data() {
    return {
      shoppingList: [],
    }
  },
  mounted() {
    console.log('App mounted!')
  },
  methods: {
    addItem(item) {
      this.shoppingList.push({
        ...item,
        id: this.shoppingList.length + 1,
        bought: false
      })
      console.log('Item added!')
      return true
    },
    removeItem(id) {
      const response = window.confirm('Delete item?')
      if(response) {
        const idx = this.shoppingList.findIndex(item => item.id == id)
        const itemName = this.shoppingList[idx].name
        console.log(`${itemName} removed!`)
        this.shoppingList.splice(idx, 1)
      }
      return response
    }
  },
}
</script>

<style>
*, ::before, ::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

@font-face {
  font-family: 'Corbert Wide';
  src: url('./assets/CorbertWide-Regular.ttf');
}

:root {
  font-size: 16px;
  font-family: 'Corbert Wide', Verdana, Geneva, sans-serif;
}

#app {
  background-image: linear-gradient(to top right, lightgreen, lightyellow);
  min-height: 100vh;
  padding: 1rem;
}

@media only screen and (max-width: 600px) {
  #app {
    padding: 0;
  }
}
</style>
