<template>
  <div id="documengtary_wrap">
    <!-- title:彩帝跟单 -->
    <div id="documengtary_title">
      <h3>彩帝跟单</h3>
    </div>
    <!-- join:申请加入 -->
    <div id="documengtary_join">
      <router-link to="/join">
      <img src="../../assets/tth-documentary/join.png">
      </router-link>
    </div>
    <new-documentaryrank></new-documentaryrank>
    <!-- ranking:彩帝排行 -->
    <!--<div id="documengtary_ranking_wrap">-->
      <!--&lt;!&ndash; title &ndash;&gt;-->
      <!--<router-link to="/rank">-->
      <!--<h6>-->
        <!--<span>彩帝排行</span>-->
        <!--<i class="iconfont icon-arrow-right"></i>-->
      <!--</h6>-->
      <!--</router-link>-->
      <!-- body  -->
      <!--<div id="documengtary_ranking_body-wrap">-->
        <!--<div class="documengtary_ranking_body" v-for="(god,index,key) in halfgods" @click="clicked(index)">-->
          <!--<div class="documengtary_ranking_body_avatar"><img :src="god.avatar"></div>-->
          <!--<div class="documengtary_ranking_body_username">{{(god.nick).split('',5).join('')}}</div>-->
          <!--<div class="documengtary_ranking_body_grade">近{{(god.hitState).split('').length}}中{{parseInt((god.hitState.split('').length) * parseInt(god.hitRate) / 100)}}</div>-->
          <!--<div class="now" v-if="god.recommend > 0">-->
            <!--{{god.recommend}}-->
          <!--</div>-->
        <!--</div>-->
      <!--</div>-->
    <!--</div>-->
    <!-- recommend:彩帝推荐 -->
    <div id="documengtary_recommend_wrap">
      <div id="documengtary_recommend_title">
        <div>彩帝推荐</div>
      </div>
      <div class="documengtary_recommend_body" v-for="(plan, index, key) in plans">
        <div class="documengtary_recommend_body_title" @click="recommendClick(index)">
          <img :src="plan.avatar">
          <span class="documengtary_recommend_body_title_user">
            <strong class="user_username">{{plan.nick}}</strong>
            <div class="user_userranking" v-if="plan.hitState != ''">
              近{{(plan.hitState).split('').length}}中{{counts[index]}}
            </div>
            <div class="newUser" v-if="plan.hitState === ''">新手发单</div>
          </span>
          <span class="documengtary_recommend_body_title_time">
            {{plan.followedBet.deadline}} 截止
          </span>
        </div>
        <div class="documengtary_recommend_body_body" @click="recommendBodyClick(index)">
          {{plan.recommendTitle}}
        </div>
        <div class="documengtary_recommend_body_footer_wrap" @click="recommendBodyClick(index)">
          <div class="documengtary_recommend_body_footer_counts">
            <span>场次数</span>
            <strong class="blackstrong">{{plan.matchCounts}}</strong>
          </div>
          <p></p>
          <div class="documengtary_recommend_body_footer_way">
            <span>过关方式</span>
            <strong class="blackstrong">{{plan.parlay}}</strong>
          </div>
          <p></p>
          <div class="documengtary_recommend_body_footer_bet">
            <span>彩帝投注</span>
            <strong class="blackstrong">{{plan.bet}}</strong>
          </div>
          <p></p>
          <div class="documengtary_recommend_body_footer_money">
            <span>已跟投金额</span>
            <strong class="redstrong">{{plan.followedBet.amount}}</strong>
          </div>
        </div>
      </div>
    </div>
    <!-- 背景图片 -->
    <img id="documengtary_bgimg" src="../../assets/tth-documentary/NoDate.png" v-if="this.plans.length <= 0">
    <!-- 暂无彩帝数据 -->
    <div id="documengtary_no" v-if="this.plans.length <= 0">暂无彩帝数据</div>
    <my-footer></my-footer>
  </div>
</template>
<script>
  import MyFooter from '../../components/Footer.vue'
  import {Indicator} from 'mint-ui'
  import NewDocumentaryrank from '../../components/tth-documentary/NewDocumentaryRank.vue'
  export default {
    name: 'Documentary',
    data () {
      return {
//        halfgods: [],
        plans: [],
        counts: [],
        gods: []
      }
    },
    methods: {
//      rankingData () {
//        this.$request({
//          type: 'get',
//          url: 'api/master/master/rank',
//          headers: {},
//          params: {},
//          success: function (res) {
//           console.log(parseInt((res.data.data.gods[2].hitState.split('').length) * parseInt(res.data.data.gods[1].hitRate) / 100))
//            this.halfgods = (res.data.data.gods).slice(0, 5)
//            for (let i = 0; i < (res.data.data.gods).length; i++) {
//              if (res.data.data.gods[i].avatar === '') {
//                res.data.data.gods[i].avatar = '../../src/assets/tth-documentary/tth-user.png'
//              }
//            }
//          },
//          failed: function () {}
//        })
//      },
      recommendData () {
        this.$request({
          type: 'get',
          url: 'api/master/master/plan/recommend',
          headers: {},
          params: {},
          success: function (res) {
//            console.log(res.data.data.plans.length)
            this.plans = res.data.data.plans
            let count = 0
            let counts = []
            for (let i = 0; i < res.data.data.plans.length; i++) {
              let str = res.data.data.plans[i].hitState
              let num1 = '1'
              let regex = new RegExp(num1, 'g')
              let result = str.match(regex)
              count = !result ? 0 : result.length
              counts.push(count)
              if (res.data.data.plans[i].avatar === '') {
                res.data.data.plans[i].avatar = '../../src/assets/tth-documentary/tth-user.png'
              }
            }
            this.counts = counts
            Indicator.close()
          },
          failed: function () {}
        })
      },
//      clicked (index) {
//        let godsrankUid = this.halfgods[index].uid
//        this.$router.push('/particulars/' + godsrankUid)
//        console.log(godsrankUid)
//      },
      recommendClick (index) {
        let godsrankUid = this.plans[index].uid
        this.$router.push('/particulars/' + godsrankUid)
//        console.log(godsrankUid)
      },
      recommendBodyClick (index) {
        let masterSchemeId = this.plans[index].masterSchemeId
        let uId = this.plans[index].uid
        this.$router.push('/deity/' + uId + '/' + masterSchemeId)
      }
    },
    components: {
      MyFooter,
      NewDocumentaryrank
    },
    mounted () {
      Indicator.open('加载中')
//      this.rankingData()
      this.recommendData()
    }
  }
</script>

<style scoped lang="less">
  @import "../../common/css/style.less";

  #documengtary_wrap{
    width: 100%;
    height: 100%;
    background-color: @color-background-gray;
    padding-bottom: 13.33333vmin;
  }
  /* 彩帝跟单 */
  #documengtary_title{
    height: 12vmin;
    background-color: #ff5f5f;
    color: #fff;
    font-size: 4.8vmin;
  }
  h3{
    font-size: 1.17em;
    font-weight: bold;
    text-align: center;
    line-height: 12vmin;
  }

  /* 申请加入 */
  #documengtary_join{
    width: 100%;
    margin-bottom: 2.66667vmin;
  }
  #documengtary_join img{
    width: 100%;
  }

  /* 彩帝排行 */
  /*#documengtary_ranking_wrap{*/
    /*width: 100%;*/
    /*background: @color-background-white;*/
    /*padding: 4vmin 3.2vmin;*/
    /*margin-bottom: 2.66667vmin;*/
    /*box-sizing: border-box;*/
  /*}*/
  /* title */
  /*#documengtary_ranking_wrap h6{*/
    /*display: flex;*/
    /*line-height: 1;*/
    /*height: 4.26667vmin;*/
    /*font-size: 4.26667vmin;*/
    /*border-left: .8vmin solid #ff5f5f;*/
    /*padding-left: 1.6vmin;*/
    /*margin-bottom: 4vmin;*/
  /*}*/
  /*#documengtary_ranking_wrap h6 span{-->*/
    /*<!--width: 85vmin;-->*/
    /*<!--font-weight: bold;-->*/
    /*<!--color: @color-text-black;-->*/
  /*<!--}*/
  /*.icon-arrow-right{*/
    /*color: #CFCFCF;*/
    /*font-size: 4vmin;*/
    /*margin-top: .2vmin;*/
  /*}*/
  /* body */
  /*#documengtary_ranking_body-wrap{*/
    /*width: 100%;*/
    /*overflow: hidden;*/
    /*display: flex;*/
    /*justify-content: space-between;*/
    /*box-sizing: border-box;*/
  /*}*/
  /*.documengtary_ranking_body{*/
    /*width: 21.33333vmin;*/
    /*position: relative;*/
  /*}*/
  /*.now{*/
    /*position: absolute;*/
    /*width: 4vmin;*/
    /*height: 4vmin;*/
    /*line-height: 4vmin;*/
    /*text-align: center;*/
    /*background: #ff5f5f;*/
    /*font-size: 3.2vmin;*/
    /*color: #fff;*/
    /*border-radius: 50%;*/
    /*display: block;*/
    /*top: 0;*/
    /*right: 1.93333vmin;*/
    /*font-weight: 400;*/
  /*}*/
  /*.documengtary_ranking_body_avatar{*/
    /*width: 13.06667vmin;*/
    /*height: 13.06667vmin;*/
    /*margin: 0 auto;*/
  /*}*/
  /*.documengtary_ranking_body_avatar img{*/
    /*width: 13.06667vmin;*/
    /*height: 13.06667vmin;*/
    /*border-radius: 50%;*/
  /*}*/
  /*.documengtary_ranking_body_username{*/
    /*margin-top: 2.06667vmin;*/
    /*font-size: 3.2vmin;*/
    /*height: 3.2vmin;*/
    /*color: #666;*/
    /*line-height: 1;*/
    /*font-weight: 400;*/
    /*text-align: center;*/
  /*}*/
  /*.documengtary_ranking_body_grade{*/
    /*height: 4vmin;*/
    /*background: #ff5f5f;*/
    /*color: #fff;*/
    /*border-radius: 4vmin;*/
    /*margin: 1.33333vmin 1.33333vmin 0;*/
    /*text-align: center;*/
    /*font-weight: 400;*/
    /*font-size: 3vmin;*/
    /*line-height: 1.3;*/
  /*}*/

  /* 彩帝推荐 */
  #documengtary_recommend_wrap{
    width: 100%;
    box-sizing: border-box;
  }
  /* title */
  #documengtary_recommend_title{
    width: 100%;
    padding-left: 2.6vmin;
    background-color: @color-background-white;
    border: 1px solid white;
    box-sizing: border-box;
  }
  #documengtary_recommend_title div{
    border-left: .8vmin solid #ff5f5f;
    padding-left: 1.6vmin;
    font-weight: bold;
    color: @color-text-black;
    margin-top: 4vmin;
    font-size: 4.26667vmin;
    margin-bottom: 1vmin;
  }
  /* body */
  .documengtary_recommend_body{
    width: 100%;
    box-sizing: border-box;
    padding: 4vmin 3.2vmin;
    background: #fff;
    margin-bottom: 2.66667vmin;
  }
  .documengtary_recommend_body_title{
    box-sizing: border-box;
  }
  .documengtary_recommend_body_title img{
    width: 13.06667vmin;
    height: 13.06667vmin;
    border-radius: 50%;
    margin-right: 2.13333vmin;
  }
  .documengtary_recommend_body_title span{
    display: inline-block;
    line-height: 1;
    padding: 1.86667vmin 0;
    vertical-align: top;
    text-align: left;
  }
  .documengtary_recommend_body_title strong{
    font-size: 4.26667vmin;
    vertical-align: middle;
    margin-bottom: 1.6vmin;
  }
  .user_userranking{
    color: #ff5f5f;
    font-size: 3.8vmin;
    margin-top: 1vmin;
  }
  .newUser{
    line-height: 1;
    font-size: 3.73333vmin;
    color: #999;
    margin-top: 1vmin;
  }
  .documengtary_recommend_body_title_time{
    padding: 1.86667vmin 0;
    font-size: 3.73333vmin;
    color: #999;
    font-weight: 400;
    vertical-align: middle;
    margin-top: 1vmin;
    float: right;
  }
  .documengtary_recommend_body_body{
    font-weight: 700;
    padding-left: 4.4vmin;
    display: block;
    margin-top: 3.33333vmin;
    font-size: 4vmin;
    color: black;
  }
  .documengtary_recommend_body_footer_wrap{
    box-sizing: border-box;
    width: 100%;
    margin-top: 5vmin;
    display: flex;
    justify-content: space-between;
  }
  .documengtary_recommend_body_footer_wrap div{
    width: 23%;
  }
  .documengtary_recommend_body_footer_wrap span{
    color: #999;
    display: block;
    line-height: 1;
    margin-bottom: 1.33333vmin;
    font-size: 3.73333vmin;
    text-align: center;
  }
  .redstrong{
    color: red;
    font-size: 4.26667vmin;
    text-align: center;
    display: inline-block;
  }
  .documengtary_recommend_body_footer_wrap strong{
    display: inline-block;
    width: 100%;
    line-height: 1;
    font-weight: 400;
    text-align: center;
    font-size: 3.73333vmin;
  }
  .blackstrong{
    color: black;
  }
  .documengtary_recommend_body_footer_wrap p{
    height: 6vmin;
    border-left: .3vmin solid #EDEDED;
    margin-top: 1.5vmin;
  }
  .user_username{
    color: black;
  }
  /* 背景图 */
  #documengtary_bgimg{
    width: 100%;
    display: block;
    margin: 34.66667vmin 0 13.33333vmin;
  }
  /* 暂无彩帝数据 */
  #documengtary_no{
    width: 100%;
    text-align: center;
    font-size: 4.26667vmin;
    color: @color-text-black;
    margin-bottom: 5vmin;
  }
</style>
