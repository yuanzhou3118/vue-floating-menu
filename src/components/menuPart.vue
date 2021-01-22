<template>
  <div class="popover-content">
    <div
      class="popover-content__item el-button"
      @click.stop.prevent="menuClick('处理中心')"
    >
      <i class="el-icon-odometer icon"></i>处理中心
    </div>
    <div
      class="popover-content__item el-button"
      @click.stop.prevent="menuClick('我的工作台')"
    >
      <i class="el-icon-user icon"></i>我的工作台
    </div>
    <div
      class="popover-content__item el-button"
      @click.stop.prevent="menuClick('资源管理')"
    >
      <i class="iconfont iconnut icon"></i>资源管理
    </div>
  </div>
</template>
<script>
export default {
  name: 'menu-part',
  props: {},
  data() {
    return {};
  },
  methods: {
    menuClick(eventName) {
      switch (eventName) {
        case '处理中心':
          //自定义菜单点击事件
          this.$alert('处理中心', '标题名称', {
            confirmButtonText: '确定',
          });
          //关闭菜单的方法
          this.$emit('closeMenuAndFloating');
          break;
        case '我的工作台':
          //自定义菜单点击事件
          this.$alert('处理中心', '标题名称', {
            confirmButtonText: '确定',
          });
          //关闭菜单的方法
          this.$emit('closeMenuAndFloating');
          break;
        case '资源管理':
          //自定义菜单点击事件
          this.$alert('处理中心', '标题名称', {
            confirmButtonText: '确定',
          });
          //关闭菜单的方法
          // this.$emit('closeMenuAndFloating');
          break;
        default:
          break;
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.popover-content {
  width: 182px;
  overflow: hidden;
  border-radius: 15px;
  box-shadow: 0px 16px 34px 0px rgba(0, 49, 128, 0.2);

  .el-button + .el-button {
    margin-left: 0;
  }

  .el-button:focus,
  .el-button:hover {
    color: var(--themeColor);
  }

  &__item {
    width: 100% !important;
    height: 50px;
    padding: 0;
    border: 0;
    line-height: 50px;
    font-size: 18px;
    border-radius: 0;
    color: var(--themeColor);
    padding: 0 20px;
    text-align: left;

    &.el-button:hover {
      background: var(--shadowColor);
    }

    > .icon {
      margin-right: 10px;
      font-size: 18px;
    }
  }
}
</style>
