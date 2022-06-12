<template>
  <el-dialog
    title="关于"
    class="about__dialog"
    :visible="value"
    @close="$emit('input', false)"
    width="30%"
    center
  >
    <div style="text-align: center">
      <h3>一款高度简洁的公众号 Markdown 编辑器</h3>
    </div>
    <div style="text-align: center; margin-top: 10px">
      <p>扫码关注我的微信公众号，第一时间推送全栈开发的最新动态！</p>
      <img
        src="https://www.techgrow.cn/img/wx_mp_qr.png"
        style="width: 40%; display: block; margin: 20px auto 10px"
      />
    </div>
  </el-dialog>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onRedirect(url) {
      window.open(url)
    },
  },
}
</script>

<style lang="less" scoped>
/deep/ .el-dialog {
  min-width: 420px;
}
</style>
