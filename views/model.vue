<template>
    <div>
        <div class="mask" v-if="show">
            <div class="mod">
                <h4 class="flex-item">清空购物车</h4>
                <p  class="flex-item">您确定要清除购物车??</p>
                <div class="btn flex-item">
                    <button class="btn-cancel" @click="shutdown">取消</button>
                    <button class="btn-certain"  @click="handleCartEmpty">确定</button>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props: ["show"],

        created() {
        },
        methods: {
            shutdown() {
                this.$emit("close")
            },
            handleCartEmpty(){
                this.$store.commit('emptyCart');
                this.$emit("close")
            },

            },
        computed: {},
        components: {}
    }
</script>
<style scoped>
    .mask {
        width: 100%;
        height: 100%;
        background-color: rgba(1, 1, 1, 0.5);
        position: fixed;
        left: 0;
        top: 0;
    }

    .mod {
        width: 300px;
        height: 150px;
        position: fixed;
        left: 50%;
        top: 50%;
        transform: translate3d(-50%, -50%, 0);
        background-color: white;
        /*border: 2px solid red;*/
        box-shadow: 0 0 5px 3px #2f2f2f;
        display: flex;
        flex-direction: column;

    }

    .mod  .flex-item {
        /*background-color: cornflowerblue;*/
        width: 200px;
        height: 100px;
        margin: 10px;
    }
    .flex-item button{
        outline: none;
        width:50px;
        height:30px;
        line-height: 30px;
        cursor: pointer;
        border: 1px solid transparent;



    }
    .flex-item button:hover{
        color: red;
        border: 1px solid orangered;
    }
    .flex-item .btn-certain{
        background-color: #3399ff;
    }
</style>
