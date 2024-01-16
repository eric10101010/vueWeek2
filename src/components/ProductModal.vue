<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6">
                <h2 class="mb-3">單一產品細節</h2>
                <template v-if="tempProduct.title">
                    <div class="card mb-3">
                        <img :src="tempProduct.imageUrl" class="card-img-top mb-3" :alt="主圖" />
                        <div class="card-body">
                            <h5 class="card-title">
                            {{  tempProduct.title }}
                            <span class="badge bg-primary ms-2">{{ tempProduct.category }}</span>
                            </h5>
                            <p class="card-text">商品描述：{{ tempProduct.description }}</p>
                            <p class="card-text">商品內容：{{ tempProduct.content  }}</p>
                            <div class="d-flex">
                                <p class="card-text me-2">{{ tempProduct.price }}</p>
                                <p class="card-text text-secondary"><del>{{ tempProduct.origin_price }}</del></p>
                                元 / {{ tempProduct.unit }}
                            </div>
                        </div>
                    </div>
                    <!-- 顯示多張圖片 -->
                    <div class="row">
                        <div class="col-4 g-3" v-for="image in tempProduct.imagesUrl" :key="image">
                            <img :src="image" :alt="image" class="img-fluid object-fit-cover">
                        </div>
                    </div>
                </template>
                <p v-else class="text-secondary">請選擇一個商品查看</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['tempProduct']
    }
</script>

