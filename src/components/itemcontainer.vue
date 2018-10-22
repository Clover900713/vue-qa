<template>
  <section>
    <header class="top_tips">
      <span class="num_tip" v-if="fatherComponent == 'home'">{{level}}</span>
      <span class="num_tip" v-if="fatherComponent == 'item'">题目{{itemNum}}</span>
    </header>

    <div v-if="fatherComponent == 'home'">
      <div class="home_logo item_container_style"></div>
      <router-link to="item" class="start button_style"></router-link>
    </div>

    <div v-if="fatherComponent == 'item'">
      <div class="item_back item_container_style">
        <div class="item_list_container" v-if="itemDetail.length > 0">
          <header class="item_title">{{itemDetail[itemNum - 1].topic_name}}</header>
          <ul>
            <li v-for="(item, index) in itemDetail[itemNum - 1].topic_answer" v-bind:key="index" @click="choosed(index, item.topic_answer_id)" class="item_list">
              <span class="option_style" :class="{'has_choosed': index==choosedNum}">{{chooseType(index)}}</span>
              <span class="option_detail">{{item.answer_name}}</span>
            </li>
          </ul>
        </div>
      </div>

      <span class="next_item button_style" @click="nextItem" v-if="itemNum < itemDetail.length"></span>
      <span class="submit_item button_style" @click="submitAnswer" v-else></span>
    </div>
  </section>
</template>

<script>
  import {mapState, mapActions} from 'vuex'
  export default {
    name: 'itemcontainer',
    data() {
      return {
        itemId: null, //题目id
        choosedNum: null, //选中的选项索引
        choosedId: null, //选中答案id
      }
    },
    props: ['fatherComponent'],
    computed: mapState([
      'itemNum', //第几题
      'level', //第几周
      'itemDetail', //题目详情
      'timer', //计时器
    ]),
    methods: {
      ...mapActions([
        'addNum', 'initializeData',
      ]),
      //点击下一题
      nextItem() {

      },
      //索引0-3对应A-D
      chooseType: type => {
        switch (type) {
          case 0: return 'A'; break;
          case 1: return 'B'; break;
          case 2: return 'C'; break;
          case 3: return 'D'; break;
        }
      },
      //选中的答案信息
      choosed(type, id) {
        this.choosedNum = type;
        this.choosedId = id;
      },
      //到达最后一题，交卷，清空定时器，跳转分数页面
      submit() {

      }
    },
    created() {
      //初始化信息
      if (this.fatherComponent == 'home') {
        this.initializeData();
        document.body.style.backgroundImage = 'url(./static/img/1-1.jpg)';
      }
    }
  }
</script>

<style lang="less">
  .top_tips {
    position: absolute;
    height: 7.35rem;
    width: 3.25rem;
    top: -1.6rem;
    right: 1.3rem;
    background: url('../images/WechatIMG2.png') no-repeat;
    background-size: 100% 100%;
    z-index: 10;
    .num_tip {
      position: absolute;
      left: 0.76rem;
      bottom: 1.1rem;
      font-size: 0.6rem;
      font-weight: 600;
      font-family: '黑体';
      height: 0.7rem;
      color: #a57c50;
    }
  }

  .item_container_style {
    position: absolute;
    top: 4.1rem;
		left: 1rem;
    height: 11.625rem;
    width: 13.15rem;
    background: no-repeat;
  }

  .home_logo {
    background-image: url(../images/1-2.png);
    background-size: 13.142rem 100%;
    background-position: right center;
  }

  .item_back {
    background-image: url(../images/2-1.png);
    background-size: 100% 100%;
  }

  .button_style {
    position: absolute;
    top: 16.5rem;
    left: 50%;
    width: 4.35rem;
    height: 2.1rem;
    margin-left: -2.2rem;
    background-size: 100% 100%;
    background-repeat: no-repeat;
  }

  .start {
    background-image: url('../images/1-4.png');
  }

  .next_item {
    background-image: url(../images/2-2.png);
  }

  .item_list_container {
    position: absolute;
    width: 8rem;
    left: 3rem;
  }

  .item_title {
    font-size: 0.65rem;
    height: 0.7rem;
    color: #fff;
  }

  .item_list {
    width: 10rem;
    span {
      display: inline-block;
      font-size: 0.6rem;
      color: #fff;
      vertical-align: middle;
    }
    .option_style {
      width: 0.725rem;
      height: 0.725rem;
      border:1px solid #fff; 
      border-radius: 50%;
      line-height: 0.725rem;
      text-align: center;
      font-size: 0.5rem;
      font-family: 'Arial';
    }
    .has_choosed {
      background-color: #ffd400;
      border-color: #ffd400;
      color: #575757;
    }
    .option_detail {
      width: 8rem;
    }
  }
</style>

