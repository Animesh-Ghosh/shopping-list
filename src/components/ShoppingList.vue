<template>
  <ol>
    <li v-for="item in shoppingList" :key="item.id">
      {{ item.count }}{{ item.unit }} {{ pluralize(item) }}
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
ol {
  padding-left: 10vh;
}
</style>
