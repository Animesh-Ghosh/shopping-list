<template>
  <p v-if="!shoppingList.length">No items in shopping list!</p>
  <ol v-else>
    <li v-for="item in shoppingList"
    :key="item.id"
    @click="markAsBought(item)">
      <div class="shopping-item">
        <p>
          {{ item.count }}{{ item.unit }} {{ item | pluralize }} - {{ item.bought ? 'bought!' : 'need to buy...' }}
        </p>
        <button @click.stop="$emit('removeItem', item.id)">Delete</button>
      </div>
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
      const message = item.bought
        ? `${item.name} marked as bought!`
        : `Need to buy ${item.name}...`
      console.log(message)
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

<style scoped="">
p,
ol {
  padding-left: 10vh;
}

.shopping-item > p {
  padding: inherit;
}

@media only screen and (max-width: 600px) {
  p,
  ol {
    padding-left: 5vh;
  }
}
</style>
