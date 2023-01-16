<template>
  <view class="bg">
    <view class="group-details">
      <view class="calendar-box">
        <view class="title">我的打卡记录</view>
        <view class="calendar-content">
          <calendar
            @getDateList="getDateList"
            @selectDay="selectDay"
            @openChange="openChange"
            :spotMap="spotMap"
            :disabledDate="disabledDateFn"
            :changeTime="changeTime"
          />
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import Vue from 'vue'
import calendar from '../../components/calendar/index.vue'

export default Vue.extend({
  name: 'group-details',
  components: {
    calendar,
  },
  data() {
    return {
      selected: [
        {
          date: '2023-01-12', // date为所需要打点的日期
          type: 'abnormal', // abnormal显示为橙色点
        },
        {
          date: '2023-01-13',
          type: 'normal', // normal显示为蓝色点
        },
      ],
      spotMap: {
        y2023m1d13: 'deep-spot',
        y2023m1d10: 'spot',
      },
      // 需要改变日期时所使用的字段
      changeTime: '',
      // 存储已经获取过的日期
      dateListMap: [],
    }
  },
  computed: {},
  onLoad() {},
  async onShow() {},
  methods: {
    /**
     * 选中日期的触发事件
     */
    dateChange: function (e) {
      console.log(e)
    },
    disabledDateFn({ day, month, year }) {
      // 例子，今天之后的日期不能被选中
      const now = new Date()
      const date = new Date(year, month - 1, day)
      return date > now
    },
    // 获取日期数据，通常用来请求后台接口获取数据
    getDateList(detail) {
      // 检查是否已经获取过该月的数据
      if (this.filterGetList(detail)) {
        // 获取数据
        console.log(detail, '获取数据')
      }
    },
    // 过滤重复月份请求的方法
    filterGetList({ setYear, setMonth }) {
      const dateListMap = new Set(this.dateListMap)
      const key = `y${setYear}m${setMonth}`
      if (dateListMap.has(key)) return false

      dateListMap.add(key)
      this.dateListMap = [...dateListMap]
      return true
    },
    // 日期改变的回调
    selectDay(detail) {
      console.log(detail, 'selectDay detail')
    },
    // 展开收起时的回调
    openChange({ detail }) {
      console.log(detail, 'openChange detail')
    },
  },
})
</script>

<style lang="scss">
.bg {
  position: relative;
  min-height: 100vh;
  background: url('@/static/group-punch/a3.png'),
    linear-gradient(180deg, #ffbd79 77%, #ffbd79 100%);
  background-size: 100%;
  background-repeat: no-repeat;
}

.group-details {
  @mixin boxStyle($topImg) {
    position: relative;
    width: 702rpx;
    margin: 0 auto;
    padding-top: 126rpx;
    padding-bottom: 38rpx;
    background-image: url($topImg), url('@/static/group-punch/a7.png');
    background-size: 100% 128px, 100% 40rpx;
    background-position: left top, left bottom;
    background-repeat: no-repeat, no-repeat;
    .title {
      position: absolute;
      top: 16rpx;
      transform: translate(-50%);
      left: 50%;
      height: 80rpx;
      line-height: 80rpx;
      font-size: 28rpx;
      font-weight: 500;
      color: #fa6044;
    }
  }
  @mixin boxMiddle {
    padding: 0 56rpx;
    background: url('@/static/group-punch/a8.png') repeat-y;
    background-size: 100% 24rpx;
  }

  .calendar-box {
    @include boxStyle('@/static/group-punch/a6.png');
    padding-top: 100rpx;
    height: auto;
    margin-top: 0;
    .calendar-content {
      @include boxMiddle;
      padding: 0 24rpx;
    }
  }
  .rules-box {
    @include boxStyle('@/static/group-punch/a6.png');
    height: auto;
    margin-top: 48rpx;
    .rules-content {
      @include boxMiddle;
      padding: 30px 56rpx;
      color: #e96400;
      font-size: 28rpx;
      .activity-date {
        .tlt {
          font-weight: 500;
          margin-bottom: 16rpx;
        }
        .content {
          white-space: pre-line;
          line-height: 40rpx;
        }
      }
      .mt32 {
        margin-top: 32rpx;
      }
    }
  }
}
</style>
