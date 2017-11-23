<template>
    <div class="home">
      <div class="homenav">
        <!-- 地址栏 -->
         <div class="address" >
              <div class="location">
                <span class="fa fa-map-marker fa-1g"></span>
                <span>{{locationaddress | maplocation}}</span>
                <span class="fa fa-sort-desc"></span>
              </div>
              <div class="tem">
                 <h5 class="temfont">{{temnum.temperature+"°"}}</h5>  
                 <img class="temtubiao" src="//fuss10.elemecdn.com/9/b9/c8e482821be2080edcffbb3a8d376png.png?imageMogr/thumbnail/!69x69r/gravity/Center/crop/69x69 ">
                 <p class="temfont">{{temnum.description}}</p>
              </div>           
         </div>
  
         <!-- 搜索框 -->
         <div class="search-wrapper">
             <span class=" fa fa-search">搜索商家、商品名称</span>       
         </div>
  
         <!-- 热搜 -->
         <div id="hot">
             <div class="hotbox">
            <div class="hotsearch" v-for="item in hotword">
                 <a class="hotfont">{{item.word}}</a>
            </div>
            </div>
         </div>
      </div>

         <!-- 轮播图 -->
        <div class="box">
            <div class="lunboshow" v-for="item in lunbo">                      
                <img :src="'http://fuss10.elemecdn.com/' + item.image_hash  + '.jpeg'" alt="" class="lunbotu">  
                <p>{{item.name}} </p>
              </div>
            <swiper>
                <swiper-item>
                    <div class="fadeInUp animated">
                      <div class="lunboshow" v-for="item in lunbo">                      
                        <img :src="'http://fuss10.elemecdn.com/' + item.image_hash  + '.jpeg'" alt="" class="lunbotu">  
                        <p>{{item.name}} </p>
                      </div>
                     </div>
                </swiper-item>
              <swiper-item>
                <div class="fadeInUp animated">
                  <div class="lunboshow" v-for="item in lunbo">
                    <img :src="'http://fuss10.elemecdn.com/' + item.image_hash  + '.jpeg'" alt="" class="lunbotu">  
                    <p>{{item.name}} </p> 
                  </div>
                </div>
              </swiper-item>
            </swiper>
        </div>
         
    </div>
  </template>
  
  <script>
 import Vue from 'vue'
 import { Swiper, GroupTitle, SwiperItem} from 'vux'

 Vue.filter('maplocation',value=>{
     if (value.length >= 8) {
        return value.slice(0,7)+"..."
     } else {
        return value;
     }
 });
export default {
  name: 'hello',
  components:{
    Swiper, GroupTitle, SwiperItem
  },
  data () {
    return {
          locationaddress:'',
          temnum:{},
          hotword:[],
          lunbo:[]
    }
  },
  mounted () {
    // 地址
    this.http.get('/elmapi/v2/pois/ww8p3nhuhtsh').then(res=>{
        console.log(res.data);
        if (res.data) {
            this.status = 'success';
            this.locationaddress = res.data.name;
            // console.log(JSON.stringify(this.roomlist));
        } else {
            this.status = 'fail';
        }
    },err=>{
        console.log(err);
        this.status = 'fail'
    });

    // 天气
    this.http.get('/elmapi/bgs/weather/current?latitude=37.87059&longitude=112.550667').then(res=>{
        console.log(res.data);
        if (res.data) {
            this.status = 'success';
            this.temnum = res.data;
            // console.log(JSON.stringify(this.roomlist));
        } else {
            this.status = 'fail';
        }
    },err=>{
        console.log(err);
        this.status = 'fail'
    });

    // 热搜
    this.http.get('/elmapi/shopping/v3/hot_search_words?latitude=37.87059&longitude=112.550667').then(res=>{
        
        if (res.data) {
            this.status = 'success';
            this.hotword = res.data;console.log(res.data);
            // console.log(JSON.stringify(this.roomlist));
        } else {
            this.status = 'fail';
        }
    },err=>{
        console.log(err);
        this.status = 'fail'
    });

    // 轮播图
    this.http.get('/elmapi/shopping/v2/entries?latitude=37.87059&longitude=112.550667&templates[]=main_template').then(res=>{       
          if (res.data) {
              this.status = 'success';
              this.lunbo = res.data[0].entries;
              console.log(res.data);
              console.log(res.data[0].entries.image_hash)
              // console.log(JSON.stringify(this.roomlist));
          } else {
              this.status = 'fail';
          }      
      },err=>{
          console.log(err);
          this.status = 'fail'
      });

}
}
</script>

  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  body,html{
      overflow: hidden;
    }
  h1, h2 {
    font-weight: normal;
  }  
  ul {
    list-style-type: none;
    padding: 0;
  } 
  li {
    display: inline-block;
    margin: 0 10px;
  }

  
  .homenav{
    width: 100vw;
    float: left;
    height: 145px;
    background-image: linear-gradient(90deg,#0af,#0085ff); 
  }
  /* 地址栏 */
  .address{
    width: 333px;
    height: 33px;
    position: relative;
    padding:8px 15px;
  }
  .location{
    width: 60%;
    font-weight: 700;
    color: whitesmoke;
    float:left;
  }
  /* 天气 */
  .tem{
    font-size:3vw;
    width: 100px;
    height: 40px;
    color: whitesmoke;
    float:left;
    position:relative;
    margin-left:80vw;
    margin-top:-25px
    }
  .temfont{
     font-size: 0.8rem;
    }
  .temtubiao{
    width: 7vw;
    height: 7vw;
    position:absolute;
    left:6vw;
    top:4px;
  }
  /* 搜索框 */
  .search-wrapper{
    width: 95vw;
    height: 34.5px;
    font-size: 12px;
    border-radius: .026667rem;
    background-color: whitesmoke;
    margin-top: 5vw;
    margin-left: 2.5vw;
    text-align: center;
    line-height: 34.5px;
  }
  
  /* 热搜 */
  #hot{
    width: 95%; 
    overflow-x:auto;
    overflow-y: hidden;
    margin: 0 auto;
    margin-top: 8px;
  }
  .hotbox{
    width: 145%;
    height:3rem; 
    font-size: 1rem; 
    overflow-x:auto;
    overflow-y: hidden;
  }
  .hotsearch{
     margin-top: 13px;
     display: inline-block;
     font-size: 14px;
     margin-left: 10px;         
  }
  .hotfont{
    color: whitesmoke;
  }

  /* 轮播图 */
  .box{
    /* width: 100vw;
    height: 90vw; */
  }
  .lunboshow{
    float: left;
    width: 24vw;
    height: 24vw;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    font-size: 0.78rem;
    color: gray;
  }
  .lunbotu{
    width: 3.3rem;
    height: 3.3rem;
  }
  </style>