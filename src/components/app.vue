<style scoped>
    @import "../styles/common.css";
    .icon-loading{
        position: fixed;
        top: 10px;
        right: 10px;
        color: #0099e5;
        animation: ani-app-loading 1s linear infinite;
    }
    @keyframes ani-app-loading {
        from { transform: rotate(0deg);}
        50%  { transform: rotate(180deg);}
        to   { transform: rotate(360deg);}
    }
</style>
<template>
    <div>
        <router-view></router-view>
        <Back-top></Back-top>
        <transition name="fade">
            <div class="loading" v-show="loading">
                <Icon type="load-c" size="18" class="icon-loading"></Icon>
            </div>
        </transition>
        <!--<Modal v-if="lang === 'zh-CN'" v-model="fee" title="第一期前端探索交流会" width="750" :closable="false" :mask-closable="false">-->
            <!--<div class="ivu-article">-->
                <!--<p>活动介绍：前端探索交流会 FEE(Front End Explore) 是由 TalkingData 可视化团队发起的非盈利性前端开发者交流会，旨在分享、探索和传播具有创新的、有价值的思想、灵感或经验。</p>-->
                <!--<p>直播时间：2017-12-09 14:30</p>-->
                <!--<p style="font-size: 16px;font-weight: bold;">-->
                    <!--直播地址：<a href="https://live.bilibili.com/1353202" target="_blank">https://live.bilibili.com/1353202</a>-->
                <!--</p>-->
                <!--<p>-->
                    <!--关注 FEE 公众号，获取最新动态：-->
                <!--</p>-->
                <!--<row>-->
                    <!--<i-col span="12">-->
                        <!--<img src="../images/fee-code.jpg" width="200px">-->
                    <!--</i-col>-->
                <!--</row>-->
            <!--</div>-->
            <!--<div slot="footer">-->
                <!--<Button type="text" size="large" @click="feeclose">关闭</Button>-->
                <!--<Button type="primary" size="large" style="width: 100px" @click="gotofee">查看直播</Button>-->
            <!--</div>-->
        <!--</Modal>-->
    </div>
</template>
<script>
    import bus from './bus';

    export default {
        data () {
            return {
                liveVisible: false,
                iViewVisible: false,
                lang: this.$lang,
                fee: true
            }
        },
        computed: {
            loading () {
                return bus.loading;
            }
        },
        mounted () {
            this.lang = this.$lang;
            if (window.localStorage.getItem('liveModalTime')) {
                const time = parseInt(window.localStorage.getItem('liveModalTime'));
                const today = this.getTodayUnix();
                if ((today - time) > 86400000 * 5) this.liveVisible = true;
            } else {
                this.liveVisible = true;
            }

            if (this.liveVisible && this.$lang === 'zh-CN') {
                this.$Notice.config({
                    top: 85
                });
                this.$Notice.info({
                    title: '开源项目限时推广',
                    desc: '<p><a href="https://github.com/TalkingData/inmap" target="_blank">inMap</a> 是一款基于百度地图的大数据可视化库，专注于大数据方向的散点、热力图、网格、聚合等方式展示，致力于让大数据可视化变得简单易用。</p><p>点击 Star 支持作者 → <a style="vertical-align: middle;display: inline-block" href="https://github.com/TalkingData/inmap" target="_blank"><img src="https://img.shields.io/github/stars/TalkingData/inmap.svg?style=social"></a></p>',
                    duration: 0,
                    onClose: () => {
                        const today = this.getTodayUnix();
                        window.localStorage.setItem('liveModalTime', today);
                        this.$Message.success('关闭成功，近期不再提示', 4);
                    }
                });
                this.$Notice.config({
                    top: 24
                });
            } else if (this.liveVisible && this.$lang === 'en-US') {
                this.$Notice.config({
                    top: 85
                });
                this.$Notice.info({
                    title: 'Open source project promotion',
                    desc: '<p><a href="https://github.com/TalkingData/inmap" target="_blank">inMap</a> is a big data visualization library based on Baidu map. It focuses on the scatter, heat map, grid and aggregation algorithm. Make it easy to use.</p><p>Click Star to support the author: <br><a style="vertical-align: middle;display: inline-block" href="https://github.com/TalkingData/inmap" target="_blank"><img src="https://img.shields.io/github/stars/TalkingData/inmap.svg?style=social"></a></p>',
                    duration: 0,
                    onClose: () => {
                        const today = this.getTodayUnix();
                        window.localStorage.setItem('liveModalTime', today);
                        this.$Message.success('Closed successfully, no longer prompt', 4);
                    }
                });
                this.$Notice.config({
                    top: 24
                });
            }

            // 广告统计
            window.clickAdBuy = function () {
                _hmt.push(['_trackEvent', 'index-ad-buy', 'click'])
            }

            window.clickAdVideo = function () {
                _hmt.push(['_trackEvent', 'index-ad-video', 'click'])
            }
        },
        methods: {
            getTodayUnix () {
                const date = new Date();
                date.setHours(0);
                date.setMinutes(0);
                date.setSeconds(0);
                date.setMilliseconds(0);
                return date.getTime();
            },
            handleLive () {
                const today = this.getTodayUnix();
                window.localStorage.setItem('liveModalTime', today);
            },
            feeclose () {
                this.fee = false;
            },
            gotofee () {
                _hmt.push(['_trackEvent', 'gotofee', 'click']);
                window.open('https://live.bilibili.com/1353202');
            }
        }
    }
</script>
