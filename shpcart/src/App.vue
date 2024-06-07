<template>
  <div id="app">
    <h3>Vue3 购物车 demo</h3>

    <!-- 产品 -->
    <div class="product">
      <h4>商品信息</h4>
      <table class="table table-hover table-bordered">
        <thead>
           <tr>
            <th>id</th>
            <th>名称</th>
             <th>价格</th>
             <th>操作</th>
           </tr>
        </thead>
        <tbody>
          <tr v-for="(shop,index) in shoplist" :key="index" >
            <td>{{ shop.id }}</td>

            <td>{{ shop.name }}</td>
            <td>{{ shop.price }}</td>
            <td>

              <div @click="addtocart(shop)">购物车</div>
            </td>
          </tr>

        </tbody>
      </table>
    </div>





    <!-- 购物车 -->
    <div>
      <h4>已选商品</h4>
      <div class="grid-table">
        <div class="header">id</div>
        <div class="header">名称</div>
        <div class="header">价格</div>
        <div class="header">数量</div>
        <div class="header">操作</div>
        <div v-for="(shop, index) in cartProducts" :key="index">
          <div class="cell">{{shop.id}}</div>
          <div class="cell">{{shop.name}}</div>
          <div class="cell">{{shop.price}}</div>
          <div class="cell">{{shop.num}}</div>
          <div class="cell">
            <div @click="delProduct(shop)" class="btn btn-danger btn-sm">删除</div>
          </div>
        </div>
        <div v-if="cartProducts.length === 0">
          <div class="cell" colspan="5">您的购物车空空如也。。。</div>
        </div>
      </div>
    </div>

    <!-- 计价器 -->
    <div class="item-wrapper">
      <div class="item">总数：<strong>{{totalNum}}</strong></div>
      <div class="item">总价：<strong>{{totalPrice}}</strong></div>
      <div class="item btn btn-danger" @click="clearAllCart">清空购物车</div>
    </div>




    
  </div>
</template>

<script lang="ts" setup>
import { defineComponent, reactive, onMounted, toRefs, computed } from "vue";
import { StateDataProps, ParamsProps } from "@/useProps.ts";

const state: StateDataProps = {
  shop_list: [
    { id: 11, name: "鱼香肉丝", price: 12 },
    { id: 22, name: "宫保鸡丁", price: 14 },
    { id: 34, name: "土豆丝", price: 10 },
    { id: 47, name: "米饭", price: 2 },
  ],
  added: [],
};

 const { shop_list, added: cartProducts } = state;



   
// 从 state 对象中提取 added 属性，并将其赋值给一个新变量 cartProducts。
    
let data = reactive({
      shop_list,
      cartProducts,
    });

    const addToCart = (shop: ParamsProps) => {
      const { id } = shop;
      let record = data.cartProducts.find((n) => n.id == id);
      // Array.prototype.find() 是 JavaScript 中数组的一个内置方法，用于找到并返回数组中满足提供的测试函数的第一个元素
      if (record && record.num) {
        record.num++;
      } else {
        data.cartProducts.push({
          ...shop,
          //为了和后面的num并列啊解构了对象（类型见useprops）
          num: 1,
        });
      }
    };

    const clearAllCart = () => {
      data.cartProducts = [];
    };

    const delProduct = ({ id }) => {
      data.cartProducts.forEach(({ id: sid }, i) => {
        if (id == sid) {
          data.cartProducts.splice(i, 1);
        }
      });
    };

    const totalPrice = computed(() => {
      let total = 0;
      data.cartProducts.forEach(({ price, num }) => {
        total += price * num;
      });
      return total;
    });

    const totalNum = computed(() => {
      let total = 0;
      data.cartProducts.forEach(({ num }) => {
        total += num;
      });
      return total;
    });

    // onMounted(() => {
    //   console.info("onMounted");
    // });

    
  
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.grid-table {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px;
  margin: 0 auto;
  max-width: 800px;
  border: 1px solid #ddd;
}

.grid-table .header {
  font-weight: bold;
  background-color: #f2f2f2;
  text-align: center;
  padding: 10px;
  border: 1px solid #ddd;
}

.grid-table .cell {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}

.btn {
  background-color: #17a2b8;
  color: white;
  padding: 5px 10px;
  text-align: center;
  cursor: pointer;
}

.btn:hover {
  background-color: #138496;
}

.btn-danger {
  background-color: #dc3545;
}

.btn-danger:hover {
  background-color: #c82333;
}

.item-wrapper {
  display: flex;
  background-color: #dfdfdf;
  align-items: center;
  justify-content: center;
}

.item {
  flex: 1;
}
</style>
