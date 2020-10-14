<!--
 * @Descripttion: 
 * @version: 
 * @Author: ankeji
 * @Date: 2020-06-30 13:25:15
 * @LastEditors: ankeji
 * @LastEditTime: 2020-10-14 14:47:39
--> 
<template>
  <div>
    <div id="captcha"></div>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      geetest_type: ""
    };
  },
  computed: {},
  watch: {},
  mounted() {
    let self = this;
    var name = new Date().getTime();
    self.$api.GET_GEETEST_ONE(name).then(res => {  //这里请求后端部署的接口
      console.log(res);
      var data = res.data;
      initGeetest(
        {
          gt: data.gt,
          challenge: data.challenge,
          product: "bind",
          offline: !data.success,
          new_captcha: data.new_captcha, // 用于宕机时表示是新验证码的宕机
          width: "300px",
          https: true
        },
        captchaObj => {
          captchaObj.onReady(function() {});
          captchaObj.onSuccess(function() {
            var validate = captchaObj.getValidate();
            console.log(validate);
            self.$bus.$emit("geetest", {
              validate,
              geetest_type: self.geetest_type,
              dataGestest:data
            });
          });
          captchaObj.onError(function() {
            captchaObj.reset();
          });
          self.$bus.$on("login_geetest", data => {
            console.log(data);
            self.geetest_type = "";
            self.geetest_type = data;
            captchaObj.verify();
          });
        }
      );
    });
  },
  methods: {},
  created() {},
  beforeDestroy() {}, //生命周期 - 销毁之前
};
</script>

<style>
</style>

