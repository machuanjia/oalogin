<template>
  <Row>
    <Col span="8" offset="8">
    <Form ref="formCustom" :model="formCustom" :rules="ruleCustom" :label-width="80">
      <FormItem label="domain" prop="domain" v-if="!isDomain">
        <Input type="text" v-model="formCustom.domain"></Input>
      </FormItem>

      <FormItem label="phone" prop="phone" v-if="isDomain">
        <Input type="text" v-model="formCustom.phone"></Input>
      </FormItem>
      <FormItem label="pass" prop="pass" v-if="isDomain">
        <Input type="text" v-model="formCustom.pass"></Input>
      </FormItem>

      <FormItem v-if="!isDomain">
        <Button type="primary" @click="handleStep()">下一步</Button>
      </FormItem>
      <FormItem v-if="isDomain">
        <Button type="primary" @click="handleSubmit('formCustom')">登录</Button>
      </FormItem>
    </Form>
    </Col>
  </Row>

</template>
<script>
  import axios from 'axios'
  import qs from 'qs'

  export default {
    data() {
      const validateDomain = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('Please enter your domain'))
        } else {
          callback()
        }
      }
      const validatePhone = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('Please enter your phone'))
        } else {
          callback()
        }
      }
      const validatePass = () =>{
        if (value === '') {
          callback(new Error('Please enter your pass'))
        } else {
          callback()
        }
      }
      return {
        isDomain: false,
        formCustom: {
          domain: '',
          phone: '',
          pass:''
        },
        ruleCustom: {
          domain: [{
            validator: validateDomain, trigger: 'blur'
          }],
          phone: [{
            validator: validatePhone, trigger: 'blur'
          }],
          pass: [{
            validator: validatePass, trigger: 'blur'
          }]
        }
      }
    },
    methods: {
      handleSubmit(name) {
        this.$refs[name].validate((valid) => {
          if (valid) {

            this.$$api.login.login({
              name:this.formCustom.phone,
              pwd:this.formCustom.pass
            }).then((rv)=>{
              console.log(rv);
            }).catch((err)=>{
            });



//            axios.post('http://localhost:3000/users/login', qs.stringify(this.formCustom))
//              .then((res) => {
//                console.log(res)
//                window.location = '/'
//              })
//              .catch((err) => {
//                if (err) {
//                  console.log(err)
//                }
//              })

          } else {
            this.$Message.error('Fail!')
          }
        })
      },
      handleStep() {
        this.isDomain = true
      }
    }
  }
</script>

