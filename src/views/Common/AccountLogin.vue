<script setup>
import { ref } from 'vue'
//subscriber management system用户管理系统

//smsRules用于验证手机号和验证码的规则
const smsRules = {
  //定义了验证手机号的规则
  phone: [
        //第一个规则对象指定了是必填项，并指定当输入框失去焦点时触发验证（trigger: 'blur'）。当手机号为空时，显示的错误提示信息:'请输入手机号'
    { required: true, message: '请输入手机号', trigger: 'blur' },
        //第二个规则对象指定手机号的正则表达式模式（pattern），限制只能输入中国大陆的手机号码，同样在输入框失去焦点时触发验证。
    {
      pattern: /^1[3456789]\d{9}$/, //正则表达式，1开头，后跟3,4,5,6,7,8,9中的任意一个数字，再接着任意的9个数字，共计11位
      message: '目前只支持中国大陆的手机号码',//当手机号格式不符合要求时，显示的错误提示信息。
      trigger: 'blur'
    }
  ],
  //定义了验证验证码的规则
  code: [
        //这个规则对象同样指定了必填项，并在输入框失去焦点时触发验证,当验证码为空时，显示的错误提示信息:'请输入验证码'
    { required: true, message: '请输入验证码', trigger: 'blur' }
  ]
}

const formData = ref({
  code: null,
  phone: null,
  checked: false
})

const smsText = ref('发送验证码')
const smsDisabled = ref(false)

function handleSmsCode(e) {
  console.log(e)
}

function handleSubmit(e) {
  console.log(e)
}
</script>
<template>
  <div class="login-page">
    <div class="company-container">
      <img class="company-info-logo" src="@/assets/images/header-logo.svg" />
      <div class="company-info-desc">小具影响力的 Web 设计规范</div>
    </div>
    <div class="form-container">
      <div class="login-form-phone">
        <!-- Element UI 的表单组件 将表单组件与 formData 数据对象进行双向数据绑定，指定表单的验证规则为 smsRules 对象， -->
        <el-form :model="formData" :rules="smsRules">
            <!-- prop 属性：指定表单项的字段名，用于验证规则的匹配。 -->
          <el-form-item prop="phone">
            <!-- Element UI 的输入框组件，autocomplete="off"关闭自动填充功能 -->
            <el-input
              type="number"
              placeholder="请输手机号"
              v-model="formData.phone"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <el-form-item prop="code">
            <!-- <el-col>：Element UI 的栅格布局组件，用于实现表单项的布局；:span="14"：指定栅格列的宽度比例，这里表示该栅格宽度为14个列 -->
            <el-col :span="14">
              <el-input
                type="text"
                placeholder="请输入验证码"
                v-model="formData.code"
                autocomplete="off"
              ></el-input>
            </el-col>
            <el-col :span="1"> </el-col>
            <el-col :span="5">
                <!-- <el-button>：Element UI 的按钮组件；:smsDisabled="smsDisabled"：绑定按钮的禁用状态到 smsDisabled 变量，根据该变量的值决定按钮是否可点击。 -->
              <el-button :smsDisabled="smsDisabled" @click="handleSmsCode">{{ smsText }}</el-button>
            </el-col>
          </el-form-item>
          <el-form-item prop="checked">
            <el-checkbox v-model="formData.checked">自动登录</el-checkbox>
          </el-form-item>
          <el-form-item>
            <!-- type="primary"：设置按钮的样式为主要样式 -->
            <el-button style="width: 100%" type="primary" @click="handleSubmit">登录</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.login-page {
  display: flex;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f0f2f5;
  background-position: center;
  padding-top: 160px;
  box-sizing: border-box;
}

.company-container {
  text-align: center;
  .company-info-logo {
    height: 44px;
  }
  .company-info-desc {
    margin-top: 12px;
    font-size: 14px;
    opacity: 0.5;
    text-align: center;
  }
}
.form-container {
  text-align: center;
  max-width: 388px;
  margin: 40px auto;
}
</style>