<template>
  <div>
    <div class="header">
      <h1>Poster Shop</h1>
      <form class="searchbar">
        <input type="submit" value="Search" class="btn">
      </form>
    </div>
    <div class="main">
      <div class="products">
        <div class="product" v-for="(item, index) in items" v-bind:key="item.id">
          <h4 class="product-title">{{item.title}}</h4>
          <button class="add-to-cart btn" v-on:click="addItem(index)">Add to cart</button>
        </div>
      </div>
      <div class="cart">
        <h2>Shopping Cart</h2>
        <ul>
          <li class="cart-item" v-for="(item, index) in cart" v-bind:key="item.id">
            <div class="item-title">{{item.title}}</div>
            <span class="item-qty">{{item.price | currency}} x {{item.quantity}}</span>
            <button class="btn" v-on:click="addQuantity(index)">+</button>
            <button class="btn" v-on:click="removeQuantity(index)">-</button>
          </li>
        </ul>
        <div v-if="cart.length > 0">
          <div>Total: {{total | currency}}</div>
        </div>
        <div v-else class="empty-cart">
          <div>There are currently no items in the cart</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const PRICE = 9.99
export default {
  name: 'home',
  data: () => ({
    total: 0,
    items: [
      { id: 1, title: 'Item 1' },
      { id: 2, title: 'Item 2' },
      { id: 3, title: 'Item 3' },
      { id: 4, title: 'Item 4' }
    ],
    cart: []
  }),
  methods: {
    addItem(index) {
      let cartItem = this.cart.filter(
        item => item.id === this.items[index]['id']
      )

      if (cartItem.length === 1) {
        const cartItemIndex = this.cart.indexOf(cartItem[0])
        this.addQuantity(cartItemIndex)
      } else {
        this.cart.push({
          ...this.items[index],
          quantity: 1,
          price: PRICE
        })
        this.total += PRICE
      }
    },
    addQuantity(index) {
      if (this.cart[index].quantity <= 0) {
        this.deleteCartItem(index)
      } else {
        this.cart[index].quantity++
        this.cart[index].price += PRICE
        this.total += PRICE
      }
    },
    removeQuantity(index) {
      if (this.cart[index].quantity <= 1) {
        this.deleteCartItem(index)
      } else {
        this.cart[index].quantity--
        this.cart[index].price -= PRICE
        this.total -= PRICE
      }
    },
    deleteCartItem(index) {
      this.cart.splice(index, 1)
    }
  },
  filters: {
    currency(price) {
      return '$'.concat(price.toFixed(2))
    }
  }
}
</script>