<style lang="less" scoped>
    .child-view {
        position: absolute;
        width:100%;
        transition: all .2s cubic-bezier(.55,0,.1,1);
    }
    .slide-left-enter, .slide-right-leave-active {
        opacity: 0;
        -webkit-transform: translate(50px, 0);
        transform: translate(50px, 0);
    }
    .slide-left-leave-active, .slide-right-enter {
        opacity: 0;
        -webkit-transform: translate(-50px, 0);
        transform: translate(-50px, 0);
    }
    html,body{
        font-family: "Microsoft Yahei";
    }
</style>
<template>
    <div>
            <transition :name="transitionName">
                <router-view class="child-view"></router-view>
            </transition>


            <v-footer id="foot" v-if="showfooter"></v-footer>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                showfooter:true
            }
        },
        created(){
            this.showFoot();
            this.getAppparam();
        },
        methods:{
            showFoot(){
                var oHeight = $(document).height(); //屏幕当前的高度
                $(window).resize(function(){
                    if($(document).height() < oHeight){
                        $("#foot").css("display","none");
                    }else{
                        $("#foot").css("display","block");
                    }
                });
            },
            getAppparam(){//获取app传的参数
                this.footParams = this.$route.query.isApp;
                if(this.footParams==1 || this.footParams==2){
                    this.showfooter=false;
                }
            },
        },
        beforeRouteUpdate (to, from, next) {
            // 如果isBack为true时，证明是用户点击了回退，执行slide-right动画
            let isBack = this.$router.isBack;
            if (isBack) {
                this.transitionName = 'slide-right'
            } else {
                this.transitionName = 'slide-left'
            }
            // 做完回退动画后，要设置成前进动画，否则下次打开页面动画将还是回退
            this.$router.isBack = false;
            next()
        },



    }
</script>