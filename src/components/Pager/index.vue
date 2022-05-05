<template>
  <!-- 只有总页数大于1时才显示 -->
  <div class="pager-container" v-if="pageNumber > 1">
    <a @click="handleClick(1)" class="cmponentStyles" :class="{ disabled: current === 1 }">
      <!-- |&lt;&lt; -->
      首页
    </a>
    <a @click="handleClick(current - 1)" class="prevStyles" :class="{ disabled: current === 1 }">
      <!-- &lt;&lt; -->
      <Icon type="arrowDown" />
    </a>
    <a
      @click="handleClick(n)"
      v-for="(n, i) in numbers"
      :key="i"
      :class="{ active: n === current }"
    >
      {{ n }}
    </a>

    <a
      @click="handleClick(current + 1)"
      :class="{ disabled: current === pageNumber }"
      class="nextStyles"
    >
      <!-- &gt;&gt; -->
      <Icon type="arrowUp" />
    </a>
    <a
      @click="handleClick(pageNumber)"
      :class="{ disabled: current === pageNumber }"
      class="cmponentStyles"
    >
      <!-- &gt;&gt;| -->尾页
    </a>
  </div>
</template>

<style lang="less" scoped>
@import "~@/styles/var.less";
.pager-container {
  display: flex;
  justify-content: center;
  margin: 25px 0;
  a {
    color: @words;
    margin: 0 5px;
    display: inline-block;
    line-height: 40px;
    text-align: center;
    height: 40px;
    width: 40px;
    background: #f4f4f5;
    border-radius: 5px;
    cursor: pointer;
    &.disabled {
      color: @lightWords;
      cursor: not-allowed;
    }
    &.active {
      color: #f4f4f5;
      font-weight: bold;
      cursor: text;
      background: @primary;
    }
  }
  .prevStyles {
    transform: rotate(90deg);
  }
  .nextStyles {
    transform: rotate(90deg);
  }
  .cmponentStyles {
    font-size: 14px;
  }
}
</style>

<script>
import Icon from '@/components/Icon/index'
export default {
  props: {
    current: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 0,
    },
    limit: {
      type: Number,
      default: 10,
    },
    visibleNumber: {
      type: Number,
      default: 10,
    },
  },
  components: {
    Icon
  },
  computed: {
    // 总页数
    pageNumber() {
      return Math.ceil(this.total / this.limit);
    },
    // 得到显示的最小数字
    visibleMin() {
      let min = this.current - Math.floor(this.visibleNumber / 2);
      if (min < 1) {
        min = 1;
      }
      return min;
    },
    visibleMax() {
      let max = this.visibleMin + this.visibleNumber - 1;
      if (max > this.pageNumber) {
        max = this.pageNumber;
      }
      return max;
    },
    numbers() {
      let nums = [];
      for (let i = this.visibleMin; i <= this.visibleMax; i++) {
        nums.push(i);
      }
      return nums;
    },
  },
  methods: {
    handleClick(newPage) {
      if (newPage < 1) {
        newPage = 1;
      }
      if (newPage > this.pageNumber) {
        newPage = this.pageNumber;
      }
      if (newPage === this.current) {
        return;
      }
      // 抛出一个事件
      this.$emit("pageChange", newPage);
    },
  },
};
</script>
