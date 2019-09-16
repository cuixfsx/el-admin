<template>
  <el-tabs v-model="activeName" style="padding-left: 5px;">
    <el-tab-pane label="邮箱配置" name="first">
      <Config/>
    </el-tab-pane>
    <el-tab-pane label="发送邮件" name="second">
      <Send/>
    </el-tab-pane>
    <el-tab-pane label="使用说明" name="third">
      <Description/>
    </el-tab-pane>
    <el-tab-pane label="普通文本" name="four">
      <Msg/>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import Config from './module/config'
import Send from './module/send'
import Description from './module/description'
import Msg from './module/text'
export default {
  components: { Config, Send, Description, Msg },
  data() {
    return {
      activeName: 'second'
    }
  }
}
</script>

<style scoped>
</style>
