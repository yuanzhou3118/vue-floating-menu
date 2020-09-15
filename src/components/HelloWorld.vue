<template>
  <div class="hello">
    <h1>简单的浮窗菜单 DEMO</h1>
    <p>
      其实就是一个简单地浮窗菜单功能，写的过程发现还是有点东西，所以写了一个demo来分享给大家
    </p>
    <div class="container">
      <el-form ref="form" :model="form" v-if="form" label-width="120px">
        <el-form-item label="浮窗主题色">
          <swatches-picker v-model="form.themeColor"></swatches-picker>
        </el-form-item>
        <el-form-item label="浮窗图标大小">
          <el-slider v-model="form.fontSize" :min="12" :max="50"></el-slider>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { Swatches } from 'vue-color';
export default {
  name: 'HelloWorld',
  props: {
    form: Object,
  },
  components: {
    'swatches-picker': Swatches,
  },
};
</script>
<style scoped lang="scss">
p {
  margin-top: 10px;
  font-size: 18px;
}

.container {
  text-align: left;
  width: 1000px;
  margin: 30px auto;

  .el-slider {
    width: 300px;

    ::v-deep .el-slider__bar {
      background-color: var(--themeColor);
    }

    ::v-deep .el-slider__button {
      border-color: var(--themeColor);
    }
  }
}
</style>
