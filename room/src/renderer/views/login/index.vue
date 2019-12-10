<template>
  <div class="login-container">
    <!--<iframe src="javascript:void(0);" ref="regIframe"  frameborder="0" class="app-login"  style="width: 100%;height: 302px;" scrolling=auto></iframe>-->
    <h2 style="text-align:left;">输入UID</h2>
    <el-col :span="24"  style="height: 45px;text-align:left;" >
      <el-form :inline="true" size="small">
        <el-form-item label="appid">
          <!--<el-input v-model="appid"></el-input>-->
          <template>
            <el-select v-model="appid" placeholder="appid">
              <el-option
                v-for="item in appids"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </el-form-item>

        <el-form-item label="area">
          <!--<el-input v-model="area"></el-input>-->
          <template>
            <el-select v-model="area" placeholder="area">
              <el-option
                v-for="item in areas"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </el-form-item>

        <el-form-item label="uid">
          <el-input v-model="uid"></el-input>
        </el-form-item>
        <el-form-item class="search">
          <el-button type="primary"  @click="getUserToken" style="border-radius: 4px"><span class="el-icon-search margin"></span>进入房间</el-button>
        </el-form-item>
      </el-form>
    </el-col>

  </div>
</template>

<script>
  import { APPID, authURL, redirectURL } from '@/global.js';
  import { getBeforeLoginUrl, removeBeforeLoginUrl } from '@/utils/auth'
  import { getStorage, setStorage } from '@/utils/BaseUtil'
  import axios from 'axios'

  export default {
    name: 'author',
    data() {
        return {
          uid: '',
          appid: 1504984159,
          area: 'CN',
          token: '',
          reportType: 0,
          appids: [{
              value: 1504984159,
              label: '1504984159'
            }, {
              value: 1350626568,
              label: '1350626568'
            }],
          areas: [{
              value: 'CN',
              label: 'CN'
            }, {
              value: 'CN-TEST',
              label: 'shenzhen/test/2'
            }, {
              value: 'CN-TEST1',
              label: 'shenzhen/test/1'
            }, {
              value: 'CN-INNER',
              label: 'inner'
            }],
      }
    },
    created() {
    },
    mounted () {
      let uid = getStorage('uid');
      let token = getStorage('token');
      console.log("author uid=" + uid + ", token=" + token);
      if (uid && token) {
          this.reportType = 1;
          let redirect = getBeforeLoginUrl() || '/';
          this.$router.push({ path: redirect });
          removeBeforeLoginUrl();
      } else {
        this.reportType = 0;
        this.getUserToken(this.uid);
        /*let token = 'ADp4VYnqABhfObBXLeUAAAFtQoNAaAAAC7gACnthOumbhue-pH26R8M3HCw2oPLBqZQExg9PIVQTqQ';
        let uid = '6860100817333733';
        setStorage("uid", uid);
        setStorage("token", token);
        */
      }
    },
    beforeDestroy() {

    },
    methods: {
      getUserToken(uid) {
        const appid = this.appid || APPID;
        axios.get(authURL + '/user/token?uid=' + this.uid+'&appid=' + appid)
          .then(res => {
            if (res.status === 200) {
              console.log("res.data: " + JSON.stringify(res.data));
              let body = res.data;
              if (body.uid && body.token) {
                setStorage("uid", body.uid.toString());
                setStorage("token", body.token);
                setStorage("area", this.area);
                setStorage("appid", appid);
                
                let redirect = getBeforeLoginUrl() || '/';
                this.$router.push({ path: redirect });
                removeBeforeLoginUrl();
              }
            }
          })
          .catch(error => {
            console.log("error: " + JSON.stringify(error));
          });
      },
    }
  }
</script>
<style rel="stylesheet/scss" lang="scss">
$bg:#2d3a4b;
$light_gray:#eee;

/* reset element-ui css */
.login-container {
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;
    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      &:-webkit-autofill {
        -webkit-box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: #fff !important;
      }
    }
  }
  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}

</style>

<style rel="stylesheet/scss" lang="scss" scoped>
$bg:#2d3a4b;
$dark_gray:#889aa4;
$light_gray:#eee;
.login-container {
  position: fixed;
  height: 100%;
  width: 100%;
  background-color: $bg;
  .login-form {
    position: absolute;
    left: 0;
    right: 0;
    width: 520px;
    padding: 35px 35px 15px 35px;
    margin: 120px auto;
  }
  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;
    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }
  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
    &_login {
      font-size: 20px;
    }
  }
  .title {
    font-size: 26px;
    font-weight: 400;
    color: $light_gray;
    margin: 0px auto 40px auto;
    text-align: center;
    font-weight: bold;
  }
  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
