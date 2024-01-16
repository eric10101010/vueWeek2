<template>
    <div class="container mt-3">
        <div class="row py-3">
            <div class="col-md-10">
                <h2 class="py-3">產品列表</h2>
            <table class="table table-hover mt-4">
                <thead>
                    <tr>
                        <th width="150">產品名稱</th>
                        <th width="120">原價</th>
                        <th width="120">售價</th>
                        <th width="150">是否啟用</th>
                        <th width="120">查看細節</th>
                        <th width="80">刪除產品</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in productsList" :key="item.id">
                        <td width="150">{{ item.title }}</td>
                        <td width="120">{{ item.origin_price }}</td>
                        <td width="120">{{ item.price }}</td>
                        <td width="150">
                            <span v-if="item.is_enabled"  class="text-success" >啟用</span>
                            <span v-else>未啟用</span>
                        </td>
                        <td width="120">
                            <button type="button" 
                                    class="btn btn-primary" 
                                    @click="checkDetail(item)"
                            >
                            查看細節
                            </button>
                        </td>
                        <td width="80">
                            <button type="button" class="btn btn-danger" @click="delProduct(item.id)">
                            刪除
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
            <p>目前有 <span class="text-success">{{ productsList.length }}</span> 項產品</p>
            <ProductModal :tempProduct="tempProduct"></ProductModal>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import ProductModal from '../components/ProductModal.vue'

const { VITE_API, VITE_PATH } = import.meta.env

export default {
    data() {
        return {
            productsList: [],
            tempProduct: {}
        }
    },
    methods: {
        //確認登入
        checkUser() {
            const token = document.cookie
                .split("; ")
                .find((row) => row.startsWith("hexToken="))
                ?.split("=")[1];
            axios.defaults.headers.common['Authorization'] = token;
            
            axios.post(`${VITE_API}/api/user/check`, null)
                .then(() => {
                    this.getProducts();
                })
                .catch(err => {
                    console.dir(err);
                })
        },
        //取得產品列表
        getProducts() {
            axios.get(`${VITE_API}/api/${VITE_PATH}/admin/products`)
            .then(res => {
                this.productsList = res.data.products;
                // console.log(res.data);
            })
            .catch(err => {
                console.dir(err.response.data);
            })
        },
        //查看產品細節
        checkDetail(detail){
            this.tempProduct = {...detail};
        },
        //刪除產品
        delProduct(id) {
            axios.delete(`${VITE_API}/api/${VITE_PATH}/admin/product/${id}`)
            .then(res => {
                this.productsList = res.data.products;
                alert('產品已成功被刪除');
                this.getProducts();
            })
            .catch(err => {
                alert('刪除產品失敗');
                console.dir(err);
            })
        }
    },
    mounted() {
        this.checkUser();
    },
    components: {
        ProductModal
    }
}

</script>