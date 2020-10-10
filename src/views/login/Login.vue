<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">
        <h3 class="title">
            {{$t('login.title')}}
        </h3>
        <lang-select class="set-language"/>

        <el-form-item prop="email">
             <span class="svg-container">
                 <svg-icon icon-class="user"/>
             </span>
             <el-input v-model="loginForm.email" name="email" type="text" auto-complete="on" :placeholder="$t('login.email')"/>
        </el-form-item>
         
         <el-form-item prop="password">
              <span class="svg-container">
                 <svg-icon icon-class="password"/>
             </span>
             <el-input
             v-model="loginForm.password"
             :type="pwType"
             name="password"
             auto-complete="on"
             :placeholder="$t('login.password')"
             @keyup.enter.native="handleLogin"
             />
             <span class="show-pwd" @click="showPwd">
                 <svg-icon icon-class="eye"/>
             </span>

         </el-form-item>

         <el-form-item>
             <el-button :loading="loading" type="primary" style="width:100;" @click.native.prevent="handleLogin">
                {{$t('login.signIn')}}
             </el-button>

         </el-form-item>

         <el-form-item>
            <el-button type="primary" style="width:100%;" @click="redirectLoginAzure()">
               Login with Azure (Respondent Only)
            </el-button>
         </el-form-item>
         <div>
             <span style="margin-right:20px;">Email admin@laravue.dev</span>
             <span>password: laravue</span>
         </div>
             
    </el-form>
  </div>
</template>

<script>
 import LangSelect from '@/components/LangSelect'
 import {validEmail} from '@/utils/validate'
 import {csrf} from '@/api/auth'

export default {
 name: 'Login',
 components: {
     LangSelect
 },

 data() {
     const validateEmail = (rule, value, callback) => {
         if(!validEmail(value)) {
             callback(new Error(this.$t('login.validateEmail')))
         } else {
             callback();
         }
     };

     const validatePass = (rule, value, callback) => {
         if (value.length<6) {
             callback(new Error(this.$t('login.validatePassword')));
         } else {
             callback();
         }
     };

     return {
         loginForm : {
             email: 'admin@laravue.dev',
             password: 'laravue'
         },

         loginRules: {
             email: [{required:true, trigger: 'blur', validator: validateEmail}],
             password: [{required:true, trigger: 'blur', validator: validatePass}]
         },

         loading: false,
         pwdType: 'password',
         redirect: undefined,
         otherQuery: {}
     };
 },
   computed: {
       lang() {
           return this.$store.getters.language;
       },
   },

   watch : {
       $route : {
           handler: function(route) {

               const query = route.query;
              if (query) {
                  this.redirect = query.redirect;
                  this.otherQuery = this.otherQuery(query);
              }
              
           },
           immediate: true,
       },

       lang() {

               var lang = this.$store.getters.language;
           
           if (lang === 'en') {
               document.documentElement.style.setProperty('--frontWeb', 'Graphiz Web Regular');

           } else if (lang === 'ja') {
               document.documentElement.style.setProperty('--frontWeb', 'Meiryo UI');
           }
             
       },

       created() {
           this.getFont();
       },

       methods: {
           getFont(){

               var lang = this.$store.getters.language;
               if (lang === 'en') {
                    document.documentElement.style.setProperty('--frontWeb', 'Graphiz Web Regular');
               } else if (lang === 'ja') {
               document.documentElement.style.setProperty('--frontWeb', 'Meiryo UI');
           }
           },
           redirectLoginAzure() {
               window.location.href
           },
           showPwd() {
               if (this.pwdType==='password') {
                   this.pwdType = '';
               } else {
                   this.pwdType = 'password'
               }
           },

           handleLogin() {
               this.$refs.loginForm.validate(valid => {
                   if (valid) {
                       this.loading = true;
                       csrf.then(() => {
                           this.$store.dispath('user/login', this.loginForm)
                           .then(() => {

                               var
                           })
                       })
                   }
               })
           }
       }
   }

}
</script>

<style>

</style>