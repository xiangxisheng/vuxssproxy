<template>
  <div>
    <group v-if="showGroup==='email'" :title="$t('inputemail')" style="padding: 15px;">
      <x-input type="email" :title="$t('email')" :is-type="checkEmail" :placeholder="$t('inputemail')" v-model="formData.email" :key-enter="emailCheck"></x-input>
      <x-button @click.native="emailCheck" type="primary" :disabled="!valid1">{{$t('next')}}</x-button>
      <div align="left" style="padding: 10px; font-size: 14px;">
        提示：系统会自动判断您的邮箱是否注册，<br />
        已注册过的会提示让您输入密码进行登录，<br />
        没注册过的邮箱会提示让你接收验证码。
      </div>
    </group>
    <group v-if="showGroup==='login'" :title="$t('UserLogin')" style="padding: 15px;">
      <x-input type="email" :title="$t('email')" :is-type="checkEmail" :placeholder="$t('inputemail')" v-model="formData.email" :key-enter="emailCheck"></x-input>
      <x-input :title="$t('LoginPass')" :placeholder="$t('PleaseTypeYourPassword')" type="password" v-model="formData.password" :key-enter="emailCheck"></x-input>
      <x-button @click.native="emailCheck" type="primary" :disabled="!valid1">{{$t('Login')}}</x-button>
      <x-button @click.native="sendvcode">{{$t('ResetPasswd')}}</x-button>
      <x-button @click.native="showGroup='email'">{{$t('BackToEmailCheck')}}</x-button>
    </group>
    <group v-if="showGroup==='reg'" :title="$t('UserRegister')" style="padding: 15px;">
      <x-input type="email" :title="$t('email')" :is-type="checkEmail" :placeholder="$t('inputemail')" v-model="formData.email"></x-input>
      <x-input :title="$t('VerCode')" :placeholder="$t('TypeVerCode')" v-model="formData.vcode"></x-input>
      <x-input :title="$t('SetUsername')" :placeholder="$t('SetYourUsername')" v-model="formData.username"></x-input>
      <x-input :title="$t('SetLoginPassword')" :placeholder="$t('TypeYourPassword')" type="password" v-model="formData.password" :key-enter="emailCheck"></x-input>
      <x-button @click.native="emailCheck" type="primary" :disabled="!valid1">{{$t('FinishReg')}}</x-button>
      <x-button @click.native="showGroup='email'">{{$t('BackToEmailCheck')}}</x-button>
    </group>
    <group v-if="showGroup==='setpwd'" :title="$t('ResetPasswd')" style="padding: 15px;">
      <x-input type="email" :title="$t('email')" :is-type="checkEmail" :placeholder="$t('inputemail')" v-model="formData.email"></x-input>
      <x-input :title="$t('VerCode')" :placeholder="$t('TypeVerCode')" v-model="formData.vcode"></x-input>
      <x-input :title="$t('ResetLoginPassword')" :placeholder="$t('TypeYourNewPassword')" type="password" v-model="formData.password" :key-enter="emailCheck"></x-input>
      <x-button @click.native="emailCheck" type="primary" :disabled="!valid1">{{$('Submit')}}</x-button>
      <x-button @click.native="showGroup='email'">{{$t('BackToEmailCheck')}}</x-button>
    </group>
    <div align="left" style="padding: 0px 13px; font-size: 14px; color: #000099">
    </div>
  </div>
</template>

<i18n>
VerCode:
  en: Verification Code
  zh-CN: 收到的验证码
TypeVerCode:
  en: Type In Verification Code
  zh-CN: 请输入您收到的验证码
SetUsername:
  en: Set Your Username
  zh-CN: 设置用户名
SetYourUsername:
  en: Using 4 to 10 char for your Username
  zh-CN: 可以是2至5个中文汉字或4至10个英文字母
SetLoginPassword:
  en: SetLoginPassword
  zh-CN: 设置登录密码
TypeYourPassword:
  en: Type Your Password
  zh-CN: 请输入您要设置的登录密码
ResetLoginPassword:
  en: Reset Login Password
  zh-CN: 重设登录密码
TypeYourNewPassword:
  en: Type Your New Login Password
  zh-CN: 请输入您要设置的登录密码


Login:
  en: Login
  zh-CN: 登录
Submit:
  en: Submit
  zh-CN: 提交
inputemail:
  en: Please type your E-mail
  zh-CN: 请输入您的常用E-mail邮箱
email:
  en: E-mail
  zh-CN: E-mail邮箱
next:
  en: Next
  zh-CN: 下一步
LoginPass:
  en: Password
  zh-CN: 登录密码
PleaseTypeYourPassword:
  en: Please Type Your Password
  zh-CN: 请输入您的登录密码
UserLogin:
  en: UserLogin
  zh-CN: 用户登录
ResetPasswd:
  en: Reset Password
  zh-CN: 找回密码
BackToEmailCheck:
  en: Back To Email Check
  zh-CN: 返回邮箱地址检查
FinishReg:
  en: Finish Register
  zh-CN: 完成注册
UserRegister:
  en: User Register
  zh-CN: 用户名注册
InvalidEmail:
  en: Invalid Email
  zh-CN: 邮箱格式不正确
PleaseCheckYourEmail:
  en: Please Check Your Email
  zh-CN: 请核对您的邮箱地址
TheEmailNotRegister:
  en: The Email Is Not Registered
  zh-CN: 您输入的邮箱地址尚未注册到本系统
TheEmailNotRegister2:
  en: if Correct，Click [Confirm]，<br />your will take a Verification Code，<br />if incorrect, you can[Cancel]
  zh-CN: 如果确认无误，请点击[确定]，<br />系统将会发送[验证码]到此邮箱，<br />如果输入有误，请点击[取消]
</i18n>

<script>
import { XInput, Group, XButton, Cell } from 'vux'
import md5 from 'blueimp-md5'

export default {
  components: {
    XInput,
    XButton,
    Group,
    Cell
  },
  data () {
    let that = this
    return {
      showGroup: 'email',
      formData: {},
      valid1: false,
      checkEmail: function (value) {
        that.valid1 = /^[a-z0-9]{1,20}([_\-.][a-z0-9]{1,20})?@[a-z0-9-]{1,20}(\.[a-z0-9-]{1,20}){0,2}(\.[a-z]{2,6})$/i.test(value)
        return {
          valid: that.valid1,
          msg: that.$t('InvalidEmail')
        }
      }
    }
  },
  methods: {
    emailCheck () {
      let that = this
      var path = '/public/email/check.php'
      let formData = window.clone(that.formData)
      formData.password = md5(formData.password)
      if (that.showGroup === 'login') {
        path = '/public/email/login.php'
      }
      if (that.showGroup === 'reg') {
        path = '/public/email/verify/reg.php'
      }
      if (that.showGroup === 'setpwd') {
        path = '/public/email/verify/setpwd.php'
      }
      window.api.post(path, formData, function (data) {
        console.log(data)
        if (data.flag === 'blocked') {
          var msg3 = '<font color="red">' + formData.email + '</font><br />'
          msg3 += '您输入的账号异常' + '<br />'
          msg3 += '原因是: 密码尝试次数过多' + '<br />'
          msg3 += '请与客服联系'
          window.$vuf.alert(msg3, function () {
          })
          return
        }
        if (data.flag === 'registered') {
          that.showGroup = 'login'
          return
        }
        if (data.flag === 'unregistered') {
          var msg1 = '<font color="red">' + formData.email + '</font><br />'
          msg1 += that.$t('TheEmailNotRegister') + '<br />'
          msg1 += that.$t('TheEmailNotRegister2')
          window.$vuf.confirm(msg1, function () {
            that.showGroup = 'reg'
            window.api.post('/public/email/sendvcode.php', formData, function (data2) {
            })
          }, that.$t('PleaseCheckYourEmail'))
          return
        }
        if (data.flag === 'success') {
          that.$router.push({path: '/user'})
          return
        }
        if (data.flag === 'logined') {
          that.$router.push({path: '/user'})
          return
        }
      })
    },
    sendvcode () {
      let that = this
      let formData = window.clone(that.formData)
      var msg1 = ''
      msg1 += that.$t('PleaseCheckYourEmail') + '<br />'
      msg1 += '<font color="red">' + formData.email + '</font><br />'
      msg1 += that.$t('TheEmailNotRegister2')
      window.$vuf.confirm(msg1, function () {
        that.showGroup = 'setpwd'
        window.api.post('/public/email/sendvcode.php', formData, function (data) {
        })
      }, that.$t('ResetPasswd'))
    }
  }
}
</script>
<style scoped>
.red {
  color: red;
}
.green {
  color: green;
}
</style>
