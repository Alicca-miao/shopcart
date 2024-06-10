<template>
  <div id="app">
    <h3>Vue3 购物车 demo</h3>

    <!-- 产品 -->
    <div class="product">
      <h4>商品信息</h4>
      <table>
        <thead>
          <tr>
            <th>id</th>
            <th>名称</th>
            <th>价格</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(shop,index) in shop_list" :key="index">
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
      <table>
        <thead>
          <tr>
            <th>id</th>
            <th>名称</th>
            <th>价格</th>
            <th>数量</th>
            <th>操作</th>
          </tr>

        </thead>
        <tbody>
          <tr v-for="(shop,index) in cartProducts" :key="index">
            <td>{{ shop.id }}</td>
            <td>{{ shop.name }}</td>
            <td>{{ shop.price }}</td>
            <td>{{ shop.num }}</td>
            <td>
              <div @click="delProduct(shop)">删除</div>
            </td>
          </tr>

        </tbody>
      </table>

    </div>

    <!-- 计价器 -->






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

    const addtocart=function(shop:ParamsProps){
      //参数是shop
      //逻辑是，看number有无，没有则push+新增num属性
      let {id}=shop
      let record = data.cartProducts.find((n)=>{
        return n.id===id
        //返回的是等于id的那个shop整个元素
      })
      if(record && record.num){
        record.num++
      }
      else{
        data.cartProducts.push({
          ...shop,
          num:1
        })
      }

    }

    const clearAllCart = () => {
      data.cartProducts = [];
    };

    // const delProduct = ({ id }) => {
    //   data.cartProducts.forEach(({ id: sid }, i) => {
    //     if (id === sid) {
    //       data.cartProducts.splice(i, 1);
    //     }
    //   });
    // };

    const delProduct=(shop)=>{
      let {id}=shop
      data.cartProducts.forEach((item,i)=>{
        let {id:sid}=item
        if(sid===id)
        data.cartProducts.splice(i,1)
      })
    }

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

</style>
