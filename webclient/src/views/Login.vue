<template>
  <div class="login">
    <div class="login-box">
      <Card shadow style="background: #fff;">
        <Form ref="formInline" :model="formInline" :rules="ruleInline" inline>
          <FormItem prop="user">
            Username
            <Input type="text" v-model="formInline.user" placeholder="Username">
              <Icon type="ios-person-outline" slot="prepend"></Icon>
            </Input>
          </FormItem>
          <FormItem prop="password">
            Password
            <Input type="password" v-model="formInline.password" placeholder="Password">
              <Icon type="ios-lock-outline" slot="prepend"></Icon>
            </Input>
          </FormItem>
          <br>
          <FormItem>
            <Button class="submit" type="primary" @click="handleSubmit('formInline')">Signin</Button>
          </FormItem>
        </Form>
      </Card>
    </div>
  </div>
</template>

<script>
// import HelloWorld from '@/components/HelloWorld.vue'
import { setCookie } from '../common/tools'

export default {
  props: {},
  data() {
    return {
      formInline: {
        user: "",
        password: ""
      },
      ruleInline: {
        user: [
          {
            required: true,
            message: "Please fill in the user name",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "Please fill in the password.",
            trigger: "blur"
          },
          {
            type: "string",
            min: 6,
            message: "The password length cannot be less than 6 bits",
            trigger: "blur"
          }
        ]
      }
    };
  },
  mounted() {},
  created() {},
  methods: {
    handleSubmit(name) {
      this.$refs[name].validate(valid => {
        if (valid) {
          let userMsg = {
            userName: this.formInline.user,
            token: 123123
          }
          setCookie("userMsg", JSON.stringify(userMsg), 30)
          // this.$Message.success("Success!");
          this.$router.replace({path: "/home", query: {childNode: "NE-Reboot", openNames: "5", activeIndex: "5-1"}})
        } else {
          this.$Message.error("Fail!");
        }
      });
    }
  },
  watch: {},
  components: {}
};
</script>

<style scoped lang="scss">
.login {
  background: #f8f8f9 !important;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.login-box {
  margin: 50px auto;
  width: 400px;
  background-color: #fff !important;
}
.ivu-form-inline .ivu-form-item{
  width: 100%;
}
.submit{
  width: 100%;
}
</style>