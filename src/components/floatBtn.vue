<template>
  <div
    ref="page"
    :class="{ 'floating-window': true, zIndexTop: moveFlags }"
    @mouseup.stop.prevent="moveEnd"
  >
    <div
      class="popover-content__trigger floatBtn"
      ref="floatBtn"
      :style="{
        transition: !moveFlags ? 'all 300ms ease' : 'unset',
        transform: `translate3d(${transform.left}px,${transform.top}px,0)`,
      }"
      @mousedown.stop.prevent="moveStart"
      @mouseover.stop.prevent="showButton"
      @click.stop.prevent="toggleMenu"
    >
      <span
        @mouseleave.stop.prevent="hideButton"
        :style="{
          left: transform.left < 50 ? '-50px' : 0,
          width: moveFlags ? '50px' : '100px',
        }"
        class="hover-area"
      ></span>
      <transition name="floating-popover-fade-in-out">
        <div
          ref="popoverContent"
          class="floating-popover"
          :style="menuPosition"
          v-if="popoverShow"
        >
          <slot>
            <div class="popover-content">
              <div
                class="popover-content__item el-button"
                @click.stop.prevent="
                  handleClickMenuAction();
                  closeMenuAndFloating();
                "
              >
                <!-- <img src="../assets/float-icon.png" />自动标准化 -->
                <i class="iconfont iconnut icon"></i>自动标准化
              </div>
            </div>
          </slot>
        </div>
      </transition>
      <i
        :style="{ fontSize: `${form.fontSize}px` }"
        class="iconfont iconnut icon"
      ></i>
      <!-- <img class="icon" src="../assets/float-icon.png" /> -->
    </div>
  </div>
</template>
<script>
export default {
  name: 'floating-window',
  props: {
    form: Object,
  },
  data() {
    return {
      moveFlags: false,
      clickFlag: false,
      hoverFlag: false,
      // 初始的位置记录
      movePosition: { x: 0, y: 0 },
      nx: '',
      ny: '',
      dx: '',
      dy: '',
      xPum: '',
      yPum: '',
      popoverShow: false,
      //浮窗的位置
      transform: {
        top: document.body.offsetHeight - 300,
        left: document.body.offsetWidth - 25,
      },
      lastMoveIndex: 0, //  拖拽计数
      curMoveIndex: 0, //  历史计数
      menuPosition: {
        top: 0,
        left: 0,
      },
    };
  },
  created() {
    document.addEventListener('mouseup', (e) => {
      if (
        e.clientY > window.innerHeight ||
        e.clientY < 60 ||
        e.clientX < 0 ||
        e.clientX > window.innerWidth
      ) {
        this.moveEnd();
      }
    });
  },
  mounted() {
    window.addEventListener('resize', () => {
      this.$set(this.transform, 'top', document.body.offsetHeight - 300);
      this.$set(this.transform, 'left', document.body.offsetWidth - 25);
    });
    const floatBtn = this.$refs.floatBtn;
    document.addEventListener('click', (event) => {
      if (!floatBtn) return;
      var tDom = event.target;
      if (floatBtn !== tDom && !floatBtn.contains(tDom)) {
        this.closeMenuAndFloating();
      }
    });
  },
  methods: {
    handleClickMenuAction() {
      this.$alert('点击菜单了', '标题名称', {
        confirmButtonText: '确定',
      });
      return;
    },
    closeMenuAndFloating() {
      console.log('closeMenuAndFloating');
      this.popoverShow = false;
      this.hoverFlag = false;
      this.moveFlags = false;
      this.resetFloatBtnLocation();
    },
    async toggleMenu() {
      //  如果上一次down事件到下一次click事件中 相同即为点击事件
      if (this.lastMoveIndex == this.curMoveIndex) {
        console.log(this.lastMoveIndex - this.curMoveIndex);
        console.warn('点击');
        this.popoverShow = !this.popoverShow;
        await this.$nextTick();
        if (this.popoverShow) {
          document.onmousemove = null;
          const { top, left } = this.transform;
          if (top >= 60) {
            this.menuPosition.top = `-${this.$refs.popoverContent.offsetHeight +
              10}px`;
          } else {
            this.menuPosition.top = '60px';
          }
          if (left > 200) {
            this.menuPosition.left = '-132px';
          } else {
            this.menuPosition.left = 0;
          }

          console.log(`top:${top},left:${left}`);
        }
      }
      this.curMoveIndex = this.lastMoveIndex;
    },
    hideButton() {
      console.log('mouse-leave');
      if (this.popoverShow) return;
      this.hoverFlag = false;
      //移动过程中不复位 按钮；
      if (this.moveFlags) return;
      this.resetFloatBtnLocation();
    },
    showButton() {
      console.log('mouse-over');
      if (this.moveFlags) return (this.hoverFlag = false);
      this.hoverFlag = true;
      const { left, top } = this.transform;
      if (left > 0 && left < -25) return;
      let bodyWidth = document.body.clientWidth;
      if (left < bodyWidth / 2) {
        this.generateTransform({ top: top, left: 20 });
      } else {
        this.generateTransform({ top: top, left: bodyWidth - 70 });
      }
    },
    moveStart(e) {
      console.log('moveStart');
      if (event.button == 2) return;
      if (this.popoverShow) {
        this.moveFlags = false;
        return;
      }
      this.clickFlag = true;
      let floatBtn = this.$refs.floatBtn; //获取目标元素

      this.movePosition.x = e.clientX;
      this.movePosition.y = e.clientY;

      //计算鼠标相对元素的位置
      const { left, top } = this.transform;
      this.dx = e.clientX - left;
      this.dy = e.clientY - top;

      this.moveFlags = true;

      document.onmousemove = async (e) => {
        this.clickFlag = false;
        this.moveFlags = true;
        console.log('onmousemove');
        let bodyWidth = document.body.clientWidth;
        let bodyHeight = document.body.clientHeight;
        let moveMaxHeight = bodyHeight - 30;
        let moveMaxWidth = bodyWidth - floatBtn.offsetWidth / 2 + this.dx;

        this.nx = e.clientX;
        this.ny = e.clientY;
        this.xPum = (this.nx > moveMaxWidth ? moveMaxWidth : this.nx) - this.dx;
        this.yPum =
          (this.ny > moveMaxHeight ? moveMaxHeight : this.ny) - this.dy;
        await this.$nextTick();
        this.lastMoveIndex++;
        this.generateTransform({
          left: this.xPum > -25 ? this.xPum : -25,
          top: this.yPum > 0 ? this.yPum : 0,
        });
      };
      document.onmouseup = () => {
        document.onmousemove = null;
        document.onmouseup = null;
        this.moveEnd();
      };
    },
    generateTransform({ top, left }) {
      let floatBtn = this.$refs.floatBtn; //获取目标元素
      if (!floatBtn) return;
      this.$set(this.transform, 'left', left);
      this.$set(this.transform, 'top', top);
    },
    resetFloatBtnLocation() {
      console.log('------reset');
      let floatBtn = this.$refs.floatBtn; //获取目标元素
      if (!floatBtn) return;
      let bodyWidth = document.body.clientWidth;
      const { left, top } = this.transform;
      if (left < bodyWidth / 2) {
        this.generateTransform({ top, left: `-25` });
      } else {
        this.generateTransform({ top, left: bodyWidth - 25 });
      }
    },
    moveEnd() {
      console.log(`moveEnd`);
      this.moveFlags = false;
      if (this.hoverFlag) return;
      document.onmousemove = null;
      this.resetFloatBtnLocation();
    },
  },
};
</script>
<style lang="scss" scoped>
.zIndexTop {
  z-index: 1000;
}
.floating-window {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  pointer-events: none;

  .floatBtn {
    position: absolute;
    z-index: 1000;
    cursor: pointer;

    .icon {
      color: var(--themeColor);
      font-size: 24px;
      -webkit-user-drag: none;
    }

    /deep/ .hover-area {
      position: absolute;
      width: 100px;
      height: 100%;
    }

    span:last-child {
      z-index: 2;
    }
  }
  .popover-content {
    width: 100% !important;

    &__trigger {
      width: 50px;
      height: 50px;
      background: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 40px;
      box-shadow: 0px 16px 34px 0px rgba(0, 49, 128, 0.2);
      pointer-events: auto;
    }
  }
}
</style>
<style lang="scss" scoped>
.floating-popover-fade-in-out {
  &-enter-active,
  &-leave-active {
    transition-timing-function: ease;
    transition-duration: 300ms;
    transition-property: opacity, transform;
  }

  &-enter,
  &-leave-to {
    opacity: 0;
    transform: translate3d(0, -10px, 0);
  }
}
.floating-popover {
  position: absolute;
  /* width: 182px; */
  padding: 0 !important;
  border: 0 !important;
  /* border-radius: 40px !important; */
  /* box-shadow: 0px 16px 34px 0px rgba(0, 49, 128, 0.2); */
}
</style>
