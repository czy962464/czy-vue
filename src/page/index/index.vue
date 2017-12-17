<template>
  <div>
    <header class="header">
      <div class="back iconfont">&#xe624;</div>
      <div class="search">
        <a href="#" class="prompt">输入城市/景点、游玩</a>
      </div>
      <div class="city">城市</div>
    </header>
    <swiper :options="swiperOption">
      <swiper-slide v-for='item in swiperInfo' :key='item.id'>
        <div class="swiper-img-con">
          <img  class="swiper-img" :src='item.imgUrl'/>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
    <swiper :options="{}">
      <swiper-slide v-for='(pageInfo, index) in pages' :key='index'>
        <div class="icon-wrapper">
          <div v-for='item in pageInfo' :key='item.id' class="icon-item">
            <div class="icon-img-con">
              <img class="icon-img" :src='item.imgUrl'/>
              <span class="icon-title">{{item.title}}</span>
            </div>
          </div>
        </div>
      </swiper-slide>
    </swiper>
    <ul class="mp-listitem">
      <li class="mp-listentrance mp-listentrance-left">
        <i class="iconfont">&#xe600;</i>
        定位失败
      </li>
      <li class="mp-listentrance">
        <a href="#">
          <i class="iconfont">&#xe62e;</i>
          五折泡温泉
        </a>
      </li>
    </ul>
    <ul class="mp-activity">
      <li class="mp-activity-item mp-activity-item-left">
        <img class="mp-activity-img" src="http://img1.qunarzz.com/piao/fusion/1710/a2/e395615b16fb1302.png">
      </li>
      <li class="mp-activity-item">
        <img class="mp-activity-img" src="http://img1.qunarzz.com/piao/fusion/1711/8a/4c62d1a89fc2d602.png">
      </li>
    </ul>
    <div class="lazy-load">
      <h2 class="mp-modtitle">热销推荐</h2>
      <div class="mp-hot-con">
        <ul class="hotList">
          <li class="mp-hot-prod" v-for="item in hotlistInfo" :key='item.id'>
            <div class="mp-hotlist-img">
              <img :src="item.imgUrl">
            </div>
            <div class="mp-hotlist-infos">
              <div class="mp-hotlist-title">{{item.title}}</div>
              <div class="mp-hotlist-desc">{{item.info}}</div>
            </div>
            <div class="mp-hotlist-pric">
              ¥
              <em class="mp-price-num">{{item.price}}</em>
              <span class="mp-price-text">起</span>
            </div>
          </li>
        </ul>
        <div class="mp-modmore">
          <a href="#">查看所有产品</a>
        </div>
      </div>
    </div>
    <div class="lazy-load">
      <h2 class="mp-modtitle">周末去哪儿</h2>
      <div>
        <div class="mp-product-item" v-for="item in productInfo" :key='item.id'>
          <a href="#">
            <div class="product-imgcontainer">
              <img :src="item.imgUrl">
            </div>
            <div class="mp-product-info">
              <p class="product-name">{{item.title}}</p>
              <p class="product-desc">{{item.info}}</p>
            </div>
          </a>
        </div>
      </div>
    </div>
    <div class="mp-price-desc">
      <i class="iconfont mp-price-desc-icon">&#xe625;</i>
      <div class="mp-price-desc-info">
        <span class="mp-price-desc-highlight">票面价</span>
        是指通过景区指定窗口售卖的纸质门票上标注的价格
      </div>
    </div>
    <div class="footer">
      <div class="main_nav_wrapper">
        <ul class="main_nav">
          <li>
            <i class="iconfont icon">&#xe601;</i>
            <span class="title">机票</span>
          </li>
          <li>
            <i class="iconfont icon">&#xe61f;</i>
            <span class="title">酒店</span>
          </li>
          <li>
            <i class="iconfont icon">&#xe63c;</i>
            <span class="title">公寓</span>
          </li>
          <li>
            <i class="iconfont icon">&#xe66c;</i>
            <span class="title">更多</span>
          </li>
        </ul>
      </div>
      <ul class="footer_nav">
        <li><a href="#">登录</a></li>
        <li><a href="#">我的订单</a></li>
        <li><a href="#">最近浏览</a></li>
        <li><a href="#">关于我们</a></li>
      </ul>
      <ul class="mobile_pc">
        <li class="active"><a href="#">触屏版</a></li>
        <li><a href="#">电脑版</a></li>
      </ul>
      <div class="copyright">
        <span>Qunar 京ICP备05021087</span>
        <a href="#">意见反馈</a>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Index',
    data () {
      return {
        swiperInfo: [],
        iconInfo: [],
        hotlistInfo: [],
        productInfo: [],
        swiperOption: {
          autoplay: 10000,
          pagination: '.swiper-pagination',
          loop: true
        }
      }
    },
    computed: {
      pages () {
        const pages = []
        this.iconInfo.forEach((value, index) => {
          let page = Math.floor(index / 8)
          if (!pages[page]) {
            pages[page] = []
          }
          pages[page].push(value)
        })
        return pages
      }
    },
    methods: {
      getIndexData () {
        this.$http.get('/static/index.json')
            .then(this.handelGetDataSucc.bind(this))
      },
      handelGetDataSucc (res) {
        const body = res.body
        if (body && body.data && body.data.swiper) {
          this.swiperInfo = body.data.swiper
          this.iconInfo = body.data.icons
          this.hotlistInfo = body.data.hotlist
          this.productInfo = body.data.product
        }
      }
    },
    created () {
      this.getIndexData()
    }
  }
</script>

<style scoped>
  .header {
    display: flex;
    background: #05bad5;
    color: #fff;
  }
  .back {
    width: .64rem;
    line-height: .86rem;
    text-align: center;
  }
  .search {
    flex: 1;
    margin: .14rem .18rem;
    background: #fff;
    border-radius: .1rem;
  }
  .prompt {
    font-size: 0.26rem;color: #ccc;line-height: 0.6rem;
  }
  .city {
    width: 1.14rem;
    line-height: .86rem;
    text-align: left;
    overflow: hidden;
    white-space: nowrap;
  }
  .city::after{
    content:"";
    display: block;
    width: 0px;
    height: 0px;
    border: 0.1rem solid #fff;
    border-color: white transparent transparent transparent;
    position: absolute;
    right: 0.3rem;
    top: 0.36rem;
  }
  .swiper-img-con {
    overflow: hidden;
    width: 100%;
    height: 0;
    padding-bottom: 31.25%;
  }
  .swiper-img {
    width: 100%;
  }
  .icon-item {
    box-sizing: border-box;
    float: left;
    width: 25%;
    padding: 0.35rem;
  }
  .icon-img-con {
    width:100%;
    height: 0;
    padding-bottom: 100%;
  }
  .icon-img {
    width: 62%;
    margin-left: 19%; 
    margin-bottom: 0.2rem;
  }
  .icon-title {
    display: block;
    text-align: center;
  }
  .mp-listitem{
    height: 1.17rem;
    font-size: 0.28rem;
    background: #f5f5f5;
  }
  .mp-listentrance {
    width: 49.9%;
    height: 0.97rem;
    text-align: center;
    line-height: 0.97rem;
    border-top: 0.01rem solid #ccc;
    float: left;
    color: #212121;
    background: #fff;
  }
  .mp-listentrance a{
    color: #212121;
  }
  .mp-listentrance-left{
    border-right: 0.01rem solid #ccc;
  }
  .mp-activity{
    height:1.4rem;
    border-top: 0.01rem solid #ccc;
    border-bottom: 0.01rem solid #ccc;
    background: #fff;
  }
  .mp-activity-item{
    width: 49.9%;
    height:1.4rem;
    float: left;
  }
  .mp-activity-item-left{
    border-right: 0.01rem solid #ccc;
  }
  .mp-activity-img{
    width: 100%;
    height: 100%;
  }
  .mp-modtitle{
    height: 0.8rem;
    padding-left: .26rem;
    line-height: 0.8rem;
    color: #212121;
    background: #f5f5f5;
  }
  .mp-hot-prod{
    position: relative;
    overflow: hidden;
    height: 1.4rem;
    padding: .24rem;
    border-bottom: 0.01rem solid #ccc;
  }
  .mp-hotlist-img{
    width: 1.4rem;
    height: 1.4rem;
    position: absolute;
  }
  .mp-hotlist-img img{
    width: 100%;
  }
  .mp-hotlist-infos{
    margin-left: 1.6rem;
  }
  .mp-hotlist-title{
    overflow: hidden;
    margin-top: .04rem;
    margin-bottom: .1rem;
    color: #212121;
    font-size: .3rem;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .mp-hotlist-desc{
    overflow: hidden;
    margin-bottom: .1rem;
    height: .4rem;
    line-height: .4rem;
    color: #9e9e9e;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .mp-hotlist-pric{
    margin-left: 1.6rem;
    color: #ff8300;
  }
  .mp-price-num{
    font-size: .36rem;
    padding: 0 .04rem;
  }
  .mp-price-text{
    color: #9e9e9e;
    font-size: .24rem;
  }
  .mp-modmore{
    height: .88rem;
    color: #00afc7;
    line-height: .88rem;
    text-align: center;
  }
  .mp-product-item{
    position: relative;
    margin-bottom: 0.1rem;
    background: #fff;
  }
  .product-imgcontainer{
    overflow: hidden;
    height: 0;
    padding-bottom: 37.4375%;
  }
  .product-imgcontainer img{
    width: 100%;
  }
  .mp-product-info{
    position: relative;
    padding: .14rem .2rem .2rem .2rem;
  }
  .product-name{
    overflow: hidden;
    padding-right: 1.4rem;
    color: #212121;
    font-size: .28rem;
    line-height: .48rem;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .product-desc{
    overflow: hidden;
    padding-right: 1.4rem;
    color: #616161;
    font-size: .24rem;
    line-height: .42rem;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .mp-price-desc{
    margin-top: .1rem;
    padding: 0.14rem 0.1rem;
    font-size: 0.24rem;
    line-height: 0.32rem;
    background: #fff;
    color: #616161;
  }
  .mp-price-desc-icon{
    width: 0.24rem;
    display: block;
    float: left;
    font-size: 0.24rem;
    line-height: 0.32rem;
  }
  .mp-price-desc-info{
    margin-left: 0.3rem;
  }
  .mp-price-desc-highlight{
    font-weight: bold;
  }
  .main_nav{
    position: relative;
    width: 6rem;
    height: 0.5rem;
    margin: 0 auto;
    padding: 0.2rem 0.2rem 0 0.2rem;
    overflow: hidden;
  }
  .main_nav li{
    margin: 0px;
    padding-left: 0.2rem;
    height: 0.62rem;
    width: 1.3rem;
    float: left;
    position: relative;
    background: none;
  }
  .main_nav li a{
    display: block;
    height: 0.44rem;
    width: 100%;
    font-size: 0.24rem;
  }
  .icon{
    float: left;
    width: 0.44rem;
    height: 0.44rem;
    background-size: 3.5rem 1rem;
  }
  .title{
    float: left;
    display: inline-block;
    color: #9e9e9e;
    margin: 0.06rem 0 0 0.1rem;
    border-bottom: 0.01rem solid #acacac;
    font-size: 0.24rem;
  }
  .copyright{
    color: #9e9e9e;
    text-align: center;
    font-size: 0.16rem;
    padding: 0.2rem;
  }
  .footer_nav{
    width: 100%;
    height: 0.78rem;
    border-bottom: 0.01rem solid #cacaca;
  }
  .footer_nav li{
    float: left;
    padding-left: 0.4rem;
  }
  .footer_nav li a{
    position: relative;
    color: #25a4bb;
    font-size: 0.28rem;
    line-height: 0.78rem;
  }
  .mobile_pc{
    padding: 0.2rem 0 0 0;
    text-align: center;
  }
  .mobile_pc li{
    display: inline-block;
    margin: 0 0.3rem;
  }
  .active a{
    color: #000;
  }
  .copyright a{
    color: #9e9e9e;
  }
</style>
