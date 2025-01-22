<script setup>
import ProductListNavbarComponent from '@/components/InsuranceProductComponent/ProductListNavbarComponent.vue';
import axios from 'axios';

import { onMounted } from 'vue';
import { ref } from 'vue';
import { reactive } from 'vue';

const categories = reactive([{ id: 0, name: '全部' }])
const products = reactive({})
const datas = ref({ "categoryid": 1})//, "is_feature": false


const loadProducts = async () => {
    const BASE_URL = import.meta.env.VITE_APIURL
    
    let API_URL = `${BASE_URL}/productlist/GetAllProduct`;

    // 根据分类ID决定 API 路径
    if (datas.value.categoryid !== 0) {
        Object.keys(products).forEach(key => delete products[key]);
        
        API_URL = `${BASE_URL}/productlist/GetAllProduct/${datas.value.categoryid}`;
    }   
    const response = await axios.get(API_URL)
    Object.assign(products, response.data)
}

const CategoryHandler = async (id) => {
    datas.value.categoryid = id;  // 更新分类ID
    await loadProducts();
};


onMounted(async () => {
    loadProducts();
})



</script>

<template>
    
    <div>
        <h2>產品櫥窗</h2>
        <ProductListNavbarComponent @categoryClick="CategoryHandler"></ProductListNavbarComponent>
    </div>
    <div class="row row-cols-1 row-cols-md-3 g-4 mt-3">
        <div class="col" v-for="product in products" :key="product.productid">
            <div class="card h-100">
                <!-- <RouterLink :to="{ name: `category${datas.categoryid}`, params: { categoryid: datas.categoryid} }"> -->
                    <a :href="'/' + `category${datas.categoryid}`">
                        <img :src="product.productPicture" class="card-img-top" :alt="product.productname">

                    </a>
                <!-- </RouterLink> -->

                <div class="card-body">
                    <h5 class="card-title">{{ product.productname }}</h5>
                    <p class="card-text">    {{ product.productDescription && product.productDescription.length <= 100 ? product.productDescription : (product.productDescription ? product.productDescription.substring(0, 100) : '')}}</p>
                        <!-- <p>{{ datas.categoryid }}</p> -->
                </div>
            </div>
        </div>
    </div>


</template>

<style lang="css" scoped></style>