<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-wodezichan"></i>
                <div>收银3</div>
            </li>

            <li>
                <i class="icon iconfont icon-dianpu"></i>
                <div>店铺41</div>
            </li>

            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品5</div>
            </li>

            <li>
                <i class="icon iconfont icon-huiyuanqia"></i>
                <div>会员6</div>
            </li>

            <li>
                <i class="icon iconfont icon-tongji"></i>
                <div>统计</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name:'leftNav',
        data () {
            return {

            }
        }
    }
</script>

<style scoped>
.left-nav{
    color: aliceblue;
    font-size: 10px;
    height: 100%;
    background-color: antiquewhite;
    float: left;
    width: 10%;
}
.iconfont{
    width: 12px;
}
.left-nav ul{
    padding: 0px;
    margin: 0px;
}
.left-nav li{
    color: black;
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
}
</style>
