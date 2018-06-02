<template>
  <div class='page'>
    <div class="content">
      <div class="content-header">
        <div class="top">抽奖规则: 推广成交人次 = 可抽奖次数</div>
        <div class="bottom">
          <button>可抽奖次数: 998</button>
          <button>已抽中奖品</button>
        </div>
      </div>
      <div class="content-body" id="lottery">
        <ul>
          <li class="item lottery-unit lottery-unit-0">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-1">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-2">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-7">
            <div class="img"></div>
          </li>
          <li class="">
            <button class="draw-btn" @click="start"></button>
          </li>
          <li class="item lottery-unit lottery-unit-3">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-6">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-5">
            <div class="img"></div>
          </li>
          <li class="item lottery-unit lottery-unit-4">
            <div class="img"></div>
          </li>
        </ul>
      </div>
    </div>
    <div class="footer">
      <div class="footer-title">活动详情</div>
      <ul class="footer-detail">
        <li><p>想要的奖品,直接找好有钱要就好了,他很大方的</p></li>
        <li><p>想要的奖品,直接找好有钱要就好了,他很大方的</p></li>
        <li><p>想要的奖品,直接找好有钱要就好了,他很大方的</p></li>
        <li><p>想要的奖品,直接找好有钱要就好了,他很大方的</p></li>
      </ul>
    </div>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        index: 3,    //当前转动到哪个位置，起点位置
        count: 0,     //总共有多少个位置
        timer: 0,     //setTimeout的ID，用clearTimeout清除
        speed: 20,    //初始转动速度
        times: 0,     //转动次数
        cycle: 50,    //转动基本次数：即至少需要转动多少次再进入抽奖环节
        prize: -1,    //中奖位置
        obj: null,
        click: false
      }
    },
    methods: {
      start() {
        if (this.click) { //click控制一次抽奖过程中不能重复点击抽奖按钮，后面的点击不响应
          return false;
        } else {
          this.speed = 100;//初始化速度
          this.froll(); //转圈过程不响应click事件，会将click置为false
          this.click = true; //一次抽奖完成后，设置click为true，可继续抽奖
          return false;
        }
      },
      init(id) {
        if (document.getElementById(id).getElementsByClassName('lottery-unit').length > 0) {
          let lottery = document.getElementById(id);
          let units = lottery.getElementsByClassName('lottery-unit');
          this.obj = lottery;
          this.count = units.length;
          lottery.getElementsByClassName('lottery-unit-' + this.index)[0].classList.add('active');
        }
      },
      roll: function () {
        let index = this.index;
        let count = this.count;
        let lottery = this.obj;
        lottery.getElementsByClassName('lottery-unit-' + index)[0].classList.remove('active');
        index += 1;
        if (index > count - 1) {
          index = 0;
        }
        lottery.getElementsByClassName('lottery-unit-' + index)[0].classList.add('active');
        this.index = index;
      },
      stop: function (index) {
        this.prize = index;
        return
      },
      froll() {
        this.times += 1;
        this.roll(); //转动过程调用的是lottery的roll方法，这里是第一次调用初始化

        if (this.times > this.cycle + 10 && this.prize == this.index) {//转动到达60次并且中奖位置正确 则停止转动
          clearTimeout(this.timer);
          this.prize = -1;
          this.times = 0;
          this.click = false;
        } else {//没有中奖则继续判断
          if (this.times < this.cycle) {//如果转动次数没有到达,则将速度逐渐变快(降低定时器调用函数的时间)
            this.speed -= 10;
          } else if (this.times == this.cycle) {//如果转动次数达到50次,则发送网络请求,拿到奖品的随机数
            let index = Math.random() * (this.count) | 0; //静态演示，随机产生一个奖品序号，实际需请求接口产生
            console.log(index);
            this.prize = index;//选中中奖位置
          } else {//如果超过60次
            if (this.times > this.cycle + 10 && ((this.prize == 0 && this.index == 7) || this.prize == this.index + 1)) {
              this.speed += 110;
            } else {
              this.speed += 20;
            }
          }
          if (this.speed < 40) {//限制速度,最快不能小于40
            this.speed = 40;
          }
          this.timer = setTimeout(this.froll, this.speed); //递归调用
//          this.timer = setTimeout(this.roll, this.speed); //傻逼代码
        }
      }
    },
    created() {
    },
    mounted() {
      this.init('lottery')
    }
  }
</script>
<style scoped lang="less" type="text/less">
  @import '../styles/luckyWheel/lucky_wheel.less';
</style>
