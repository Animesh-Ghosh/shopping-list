<template>
  <form @submit.prevent="onSubmit">
    <fieldset>
      <legend>Add Item to Shopping List</legend>
      <label for="item-name">Item Name</label>
      <input
      id="item-name"
      placeholder="Add item to shopping list..."
      v-model.lazy="itemName"/>
      <label for="countable">Countable?</label>
      <div id="item-countable">
        <input
        id="countable"
        type="checkbox"
        v-model="isCountable"/>
      </div>
      <label for="item-count">Quantity</label>
      <div id="item-qty">
        <input
        id="item-count"
        type="number"
        min="1"
        v-model.number.lazy="itemCount"/>
        <select
        v-if="!isCountable"
        v-model="selectedUnit">
          <option disabled="" value="" selected="">Pick a Unit</option>
          <option
          v-for="option of unitOptions"
          :key="option.value"
          :value="option.value"
          >
            {{ option.text }}
          </option>
        </select>
      </div>
      <button
      type="submit"
      @click.prevent="onSubmit">
        Add Item
      </button>
    </fieldset>
    <ul>
      <li
      v-for="error of formErrors"
      :key="error.id"
      >
        {{ error.message }}
      </li>
    </ul>
  </form>
</template>

<script>
export default {
  name: 'ShoppingListItemForm',
  data () {
    return {
      itemName: '',
      isCountable: false,
      selectedUnit: '',
      unitOptions: [
        { text: 'Kilogram', value: 'kg' },
        { text: 'Gram', value: 'gm' },
        { text: 'Litre', value: 'ltr' },
        { text: 'Millilitre', value: 'ml' },
      ],
      itemCount: 1,
      formErrors: [],
    }
  },
  methods: {
    checkFormErrors() {
      let errorsExist = false
      this.formErrors.splice(0, this.formErrors.length)
      if (!this.itemName) {
        this.formErrors.push({
          id: 1,
          message: 'Item should not be empty!'
        })
        errorsExist = true
      }
      if (this.isCountable == false && !this.selectedUnit) {
        this.formErrors.push({
          id: 2,
          message: 'Unit cannot be unspecified!'
        })
        errorsExist = true
      }
      return errorsExist
    },
    resetForm() {
      this.itemName = ''
      this.isCountable = false
      this.selectedUnit = ''
      this.itemCount = 1
      return true
    },
    onSubmit() {
      const formHasErrors = this.checkFormErrors()
      if (formHasErrors) {
        console.error('Form invalid!')
      }
      else {
        const item = {
          name: this.itemName,
          unit: this.selectedUnit,
          count: this.itemCount
        }
        this.$emit('addItem', item)
        this.resetForm()
      }
      return formHasErrors
    }
  }
}
</script>

<style lang="css" scoped>
form {
  padding: 1rem;
  display: flex;
  flex-flow: column;
}

form > fieldset {
  display: grid;
  grid-template-columns: max-content 1fr;
}

form > fieldset > div {
  display: flex;
  align-content: flex-start;
}

#item-qty > input,
#item-qty > select {
  flex-grow: 1;
}

button[type="submit"] {
  grid-area: 5/-2/5/-1;
}

form > ul {
  padding-left: 1em;
}
</style>
