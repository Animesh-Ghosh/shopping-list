<template>
  <form @submit.prevent="onSubmit">
    <label for="item-name">Item Name</label>
    <input id="item-name"
    placeholder="Add item to shopping list..."
    v-model.lazy="itemName"/>
    <label for="countable">Countable?</label>
    <input id="countable"
    type="checkbox"
    v-model="isCountable"/>
    <label for="item-count">Quantity</label>
    <input id="item-count"
    type="number"
    min="1"
    v-model.number.lazy="itemCount"/>
    <select v-model="selectedUnit"
    :disabled="isCountable">
      <option disabled="" value="" selected="">
        Pick a Unit
      </option>
      <option v-for="option of unitOptions"
      :key="option.value"
      :value="option.value">
        {{ option.text }}
      </option>
    </select>
    <button type="submit"
    @click.prevent="onSubmit">
      Add Item
    </button>
    <ul v-if="formErrors.length">
      <li v-for="error of formErrors"
      :key="error.id">
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

<style scoped="">
form {
  padding: 1rem;
  display: grid;
  grid-template-columns: 1fr repeat(2, 2fr);
  grid-template-areas:
    'name name-input name-input'
    'countable countable-check .'
    'quantity quantity-amount quantity-unit'
    '. add-button add-button'
    'form-errors form-errors form-errors';
}

@media only screen and (max-width: 600px) {
  form {
    padding: 1rem 0;
    grid-template-columns: repeat(2, 1fr);
    grid-template-areas:
      'name .'
      'name-input name-input'
      'countable countable-check'
      'quantity .'
      'quantity-amount quantity-unit'
      'add-button add-button'
      'form-errors form-errors';
  }
}

form > input,
form > select,
form > button {
  font-family: 'Corbert Wide', Verdana, Geneva, sans-serif;
}

form > label[for='item-name'] {
  grid-area: name;
}

#item-name {
  grid-area: name-input;
}

form > label[for='countable'] {
  grid-area: countable;
}

#countable {
  grid-area: countable-check;
}

form > label[for='item-count'] {
  grid-area: quantity;
}

#item-count {
  grid-area: quantity-amount;
}

form > select {
  grid-area: quantity-unit;
}

form > button[type='submit'] {
  grid-area: add-button;
}

form > ul {
  grid-area: form-errors;
  padding-left: 1em;
}
</style>
