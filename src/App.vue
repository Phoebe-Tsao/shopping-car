<template>
  <div id="app">
    <h1 class="main-title">商品列表</h1>

    <div class="table-header">
      <span class="header-name">名稱</span>
      <span class="header-price">價格</span>
    </div>

    <ul class="product-list">
      <li v-for="product in products" :key="product.id">
        <div class="product-details">
          <span class="product-name">{{ product.name }}</span>
          <span class="product-price">{{ formatCurrency(product.price) }}</span>
        </div>
        <button class="btn" @click="addToCart(product)">添加到購物車</button>
      </li>
    </ul>

    <button class="btn cart-toggle" @click="toggleCartVisibility">
      {{ isCartVisible ? '隱藏購物車' : '顯示購物車' }}
    </button>

    <div v-if="isCartVisible" class="cart-section">
      <h1 class="main-title">購物車</h1>

      <div class="table-header">
        <span class="header-name">名稱</span>
        <span class="header-price">價格</span>
      </div>

      <ul class="cart-list">
        <li v-for="(item, index) in cart" :key="index">
          <div class="cart-details">
            <span class="cart-name">{{ item.product.name }}</span>
            <span class="cart-price">{{ formatCurrency(item.product.price) }} x {{ item.quantity }}</span>
          </div>
          <div class="cart-actions">
            <button class="btn" @click="increaseQuantity(index)">+</button>
            <button class="btn" @click="decreaseQuantity(index)">-</button>
            <button class="btn" @click="removeFromCart(index)">刪除</button>
          </div>
        </li>
      </ul>

      <h2 class="total-price">總價: {{ formatCurrency(totalPrice) }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      products: [
        { id: 1, name: '衣服', price: 100 },
        { id: 2, name: '褲子', price: 200 },
        { id: 3, name: '裙子', price: 300 }
      ],
      cart: [],
      isCartVisible: false
    };
  },
  methods: {
    addToCart(product) {
      const existingItem = this.cart.find(item => item.product.id === product.id);
      if (existingItem) {
        existingItem.quantity += 1;
      } else {
        this.cart.push({ product, quantity: 1 });
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    increaseQuantity(index) {
      this.cart[index].quantity += 1;
    },
    decreaseQuantity(index) {
      if (this.cart[index].quantity > 1) {
        this.cart[index].quantity -= 1;
      }
    },
    formatCurrency(value) {
      return `$${Math.round(value)}`;
    },
    toggleCartVisibility() {
      this.isCartVisible = !this.isCartVisible;
    }
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => total + item.product.price * item.quantity, 0);
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  padding: 20px;
  color: #333;
}

.main-title {
  text-align: center;
  font-size: 24px;
  margin-bottom: 20px;
}

.table-header {
  display: flex;
  justify-content: space-between;
  background-color: white;
  color: #007bff;
  padding: 10px;
  font-weight: bold;
  border-radius: 5px;
}

.header-name {
  flex-basis: 50%;
  text-align: left;
  padding-left: 20px;
}

.header-price {
  flex-basis: 75%;
  text-align: left;
  padding-left: 10px;
}

.product-details {
  display: flex;
  justify-content: space-between;
  flex-basis: 80%; 
  align-items: center; 
}

.product-name, .product-price {
  flex: 1;
  text-align: left;
  padding-left: 20px;
}

.cart-details {
  display: flex;
  justify-content: space-between;
  flex-basis: 80%; 
  align-items: center;
}

.cart-name, .cart-price {
  flex: 1;
  text-align: left;
  padding-left: 20px;
}

.product-list {
  list-style-type: none;
  padding: 0;
  margin-bottom: 20px;
}

.product-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
  background-color: #fff;
  border-radius: 5px;
  margin-bottom: 10px;
}

.cart-section {
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

.cart-list {
  list-style-type: none;
  padding: 0;
  margin-bottom: 20px;
}

.cart-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
  background-color: #fff;
  border-radius: 5px;
  margin-bottom: 10px;
}

.btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px;
  font-size: 14px;
  border-radius: 5px;
  cursor: pointer;
}

.btn:hover {
  background-color: #0056b3;
}

.cart-actions .btn {
  margin: 0 5px;
}

.cart-toggle {
  display: block;
  width: 30%;
  margin: 20px auto;
  text-align: center;
}

.total-price {
  text-align: center;
  font-size: 20px;
  margin-top: 20px;
}
</style>
