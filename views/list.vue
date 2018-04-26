<template>
    <div v-show="list.length">
        <div class="list-control">
            <div class="list-control-filter">
                <span>品牌：</span>
                <span
                        class="list-control-filter-item"
                        :class="{on: item === filterBrand}"
                        v-for="item in brands"
                        @click="handleFilterBrand(item)">{{ item }}</span>
            </div>
            <div class="list-control-filter">
                <span>颜色：</span>
                <span
                        class="list-control-filter-item"
                        :class="{on: item === filterColor}"
                        v-for="item in colors"
                        @click="handleFilterColor(item)">{{ item }}</span>
            </div>
            <div class="list-control-order">
                <span>排序：</span>
                <span
                        class="list-control-order-item"
                        :class="{on: order === ''}"
                        @click="handleOrderDefault">默认</span>
                <span
                        class="list-control-order-item"
                        :class="{on: order.indexOf('sales') > -1}"
                        @click="handleOrderSales">
                    销量
                    <template v-if="order === 'sales-asc'">↑</template>
                    <template v-if="order === 'sales-desc'">↓</template>

                </span>
                <span
                        class="list-control-order-item"
                        :class="{on: order.indexOf('cost') > -1}"
                        @click="handleOrderCost">
                    价格
                    <template v-if="order === 'cost-asc'">↑</template>
                    <template v-if="order === 'cost-desc'">↓</template>
                </span>
                <span ref="reset" class="list-control-order-item reset" @click="handleReset" @mousedown="mousedown"
                      @mouseup="mouseup">重置</span>
            </div>
        </div>
        <!--filteredAndOrderedList：就是list列表-->
        <Product v-for="item in filteredAndOrderedList" :info="item" :key="item.id"></Product>
        <div class="product-not-found" v-show="!filteredAndOrderedList.length">暂无相关商品</div>
    </div>
</template>
<script>
    import Product from '../components/product.vue';
    import product_data from '../product';

    export default {
        components: {Product},
        computed: {
            //获取商品列表
            list() {
                return this.$store.state.productList;
            },
            //获取所有的品牌名称
            brands() {
                return this.$store.getters.brands;
            },
            //获取所有的商品的颜色
            colors() {
                return this.$store.getters.colors;
            },

            //通过不同的过滤状态，获取不同的数据
            // 获取商品列表/某个品牌的列表/获取某个颜色的商品/
            filteredAndOrderedList() {//获取的是数据的所有信息
                //将list列表扩展出来
                let list = [...this.list];
                // 按品牌过滤
                if (this.filterBrand !== '') {
                    list = list.filter(item => item.brand === this.filterBrand);
                }
                // 按颜色过滤
                if (this.filterColor !== '') {
                    list = list.filter(item => item.color === this.filterColor);
                }
                // 排序 销量/价格排序
                if (this.order !== '') {
                    if (this.order === 'sales-asc') {
                        list = list.sort((a, b) => a.sales - b.sales);
                    }else if(this.order === 'sales-desc'){
                        list = list.sort((a, b) => b.sales - a.sales);
                    }
                    else if (this.order === 'cost-desc') {
                        list = list.sort((a, b) => b.cost - a.cost);
                    } else if (this.order === 'cost-asc') {
                        list = list.sort((a, b) => a.cost - b.cost);
                    }
                }
                return list;
            }
        },
        data() {
            return {
                //这里的每一个状态都只保留一个
                filterBrand: '',//通过品牌过滤后的数据  一个品牌
                filterColor: '',//通过颜色过滤后的数据  一个颜色
                order: ''//排序后的数据 一个排序条件   默认/销量/价格（升续/降序）
            }
        },
        methods: {
            //获取品牌状态
            handleFilterBrand(brand) {
                if (this.filterBrand === brand) {
                    this.filterBrand = '';
                } else {
                    this.filterBrand = brand;
                }
            },
            //获得颜色状态
            handleFilterColor(color) {
                if (this.filterColor === color) {
                    this.filterColor = '';
                } else {
                    this.filterColor = color;
                }
            },

            //获得默认排序状态
            handleOrderDefault() {
                this.order = '';
            },
            //获得销量排序状态
            handleOrderSales() {
//                this.order = 'sales';
                if (this.order === 'sales-desc') {
                    this.order = 'sales-asc';
                } else {
                    this.order = 'sales-desc';
                }
            },
            //获得价格排序状态
            handleOrderCost() {
                if (this.order === 'cost-desc') {
                    this.order = 'cost-asc';
                } else {
                    this.order = 'cost-desc';
                }
            },
            //重置
            handleReset() {
                this.filterBrand = '';
                this.filterColor = '';
                this.order = '';
                console.log(this.$refs.reset);
            },
            mousedown() {
                this.$refs.reset.style.background = "red"
            },
            mouseup() {
                this.$refs.reset.style.background = ""
            }

        },
        mounted() {
//            this.$store.dispatch('getProductList');
            setTimeout(() => {
                this.$store.commit('setProductList', product_data);
            }, 500);
        }
    }
</script>
<style scoped>
    .list-control {
        background: #fff;
        border-radius: 6px;
        margin: 16px;
        padding: 16px;
        box-shadow: 0 1px 1px rgba(0, 0, 0, .2);
    }

    .list-control-filter {
        margin-bottom: 16px;
    }

    .list-control-filter-item,
    .list-control-order-item {
        cursor: pointer;
        display: inline-block;
        border: 1px solid #e9eaec;
        border-radius: 4px;
        margin-right: 6px;
        padding: 2px 6px;
    }

    .list-control-filter-item.on,
    .list-control-order-item.on {
        background: #f2352e;
        border: 1px solid #f2352e;
        color: #fff;
    }

    .reset:hover {
        border: 1px solid red;
    }

    .product-not-found {
        text-align: center;
        padding: 32px;
    }
</style>