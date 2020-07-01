<template>
  <div id="app">
    <h1>Shopping List</h1>
    <ShoppingListItemForm @addItem="addItem"/>
    <ShoppingList :shoppingList="shoppingList"/>
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
      })
      console.log('Item added!')
      return true
    },
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
</style>
