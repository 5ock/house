<template>
  <div id="app">
    <header class=header>
      <h2 class="title">好房嚴選</h2>
    </header>
    <div class="arrowLeft" 
      v-show="currentTarget !== 0"
      @click="arrowEvent('left')"></div>
    <div class="visibleBlock">
      <div class="houseCardWrap" :style="{left:houseCardWrapLeft+'px'}">
        <div class="houseCard" v-for="(item, index) in houseData" :key="index">
          <div class="imgBlcok">
            <img class="img" :src="item.CoverPic" />
          </div>
          <div class="desBlcok">
            <p class="address">{{item.Address}}</p>
            <p class="caseName">{{item.CaseName}}</p>
            <div class="specification">
              <span>{{item.Patterns}}</span>
              <span>{{item.RegArea}}坪</span>
            </div>
            <span
              class="originalPrice"
              v-show="item.OriginalPrice>item.Price">{{item.OriginalPrice}}萬</span>
            <span
              class="discountPrice "
            >{{item.Price}}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="arrowRight"
      v-show="currentTarget !== lastTarget"
      @click="arrowEvent('right')"></div>
  </div>
</template>

<script>
import './assets/reset.css'
export default {
  name: 'App',
  data() {
    return {
      houseData: [],
      currentTarget: 0,
      lastTarget: 0,
      houseCardWrapLeft: 0
    }
  },
  mounted() {
    this.getData();
  },
  methods: {
    arrowEvent(direction) {
      if(direction == 'right') {
        this.currentTarget++;
        this.houseCardWrapLeft = 0 - (196 * this.currentTarget);
      } else {
        this.currentTarget--;
        this.houseCardWrapLeft = 0 - (196 * this.currentTarget);
      }
    },
    getData() {
      let me = this;
      fetch('http://www.json-generator.com/api/json/get/bTLZcsoUGG?indent=2')
      .then(res=>res.json())
      .then(jsonData=>{
        me.houseData = jsonData;
        me.lastTarget = jsonData.length - 5;
      })
      .catch(err=>console.log(err))
    }
  }
}
</script>

<style>
#app {
  width: 980px;
  margin: 20px auto 0;
}
.header {
  width: 100%;
  margin: 0 0 20px;
  text-align: center;
}
.title {
  z-index: 1;
  line-height: 16px;
  height: 16px;
  background-color: #fff;
  display: inline-block;
  padding: 0 15px;
}

.arrowRight {
    position: absolute;
    top:160px;
    right: 450px;
    border-right: 4px solid; 
    border-bottom: 4px solid;
    width: 15px; 
    height: 15px;
    transform: rotate(-45deg);
}

.arrowLeft {
    position: absolute;
    top:160px;
    left: 450px;
    border-left: 4px solid; 
    border-bottom: 4px solid;
    width: 15px; 
    height: 15px;
    transform: rotate(45deg);
}

/*  visible block house card*/
.visibleBlock {
  width: 980px;
  height: 220px;
  overflow: hidden;
  position: relative;
}

.houseCardWrap {
  position: absolute;
  white-space: nowrap;
  transition: left 0.5s ease 0s;
}
/* house card */
.houseCard {
  display: inline-block;
  text-align: left;
  margin: 4px 9px;
  background-color: #f5f5f5;
  font-size:13px;
}
.imgBlcok {
  width: 178px;
  height: 135px;
}
.img {
  width: 100%;
  height: 100%;
  object-fit:cover;
}
.desBlcok {
  padding: 10px;
}

.address {
  color: #515151;
}
.caseName {
  font-size:15px;
  color: #000;
  font-weight: 600;
}

.specification {
  margin-top: 12px;
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.originalPrice {
  text-decoration: line-through;
  float: left;
}

.discountPrice {
  color: red;
  font-weight: 600;
  font-size: 15px;
  float: right;
}

.discountPrice::after {
  content: '萬';
  color: #000;
  font-size: 12px;
}
</style>
