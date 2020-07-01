<template>
  <p v-if="!shoppingList.length">No items in shopping list!</p>
  <ol v-else>
    <li
    v-for="item in shoppingList"
    :key="item.id"
    @click="markAsBought(item)">
      <p>
        {{ item.count }}{{ item.unit }} {{ item | pluralize }} - {{ item.bought ? 'bought!' : 'need to buy...' }}
      </p>
      <button @click.stop="$emit('removeItem', item.id)">Delete</button>
    </li>
  </ol>
</template>

<script>
export default {
  name: 'ShoppingList',
  props: {
    shoppingList: {
      type: Array,
      required: true
    }
  },
  methods: {
    markAsBought(item) {
      item.bought = !item.bought
      return true
    }
  },
  filters: {
    pluralize(item) {
      let itemName = null;
      if (item.unit || item.name[item.name.length - 1] == 's')
        itemName = item.name
      else
        itemName = item.name.concat('s')
      return itemName
    }
  }
}
</script>

<style lang="css" scoped>
p,
ol {
  padding-left: 10vh;
}

ol > li > p {
  padding: inherit;
}

@media only screen and (max-width: 600px) {
  p,
  ol {
    padding-left: 6vh;
  }
}
</style>
