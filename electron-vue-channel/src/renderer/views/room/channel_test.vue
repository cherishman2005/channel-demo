<template>
  <div class="app-container">
    <h2 style="text-align:left;">ChannelService调测系统（调用channel js_sdk，提供调测接口）</h2>

    <!-- 初始化channel -->
    <el-row type="flex">
      <el-col :span="24"  style="height:30px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="uid">
            <el-input v-model="uid" disabled></el-input>
          </el-form-item>
          <el-form-item label="appid">
            <el-input v-model="appid" disabled></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="initChannel" style="border-radius: 4px">initChannel</el-button>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="getInstance" style="border-radius: 4px">getInstance</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;" >{{GetInstanceRes}}</p>
    </div>

    <p class="text-unit">A给B发送消息</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="reliable">
            <!--<el-input v-model="SendP2PChatReq.option.reliable"></el-input>-->
            <template>
              <el-select v-model="SendP2PChatReq.option.reliable" placeholder="reliable">
                <el-option
                  v-for="item in reliable"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </template>
          </el-form-item>
          <el-form-item label="content">
            <el-input v-model="SendP2PChatReq.content"></el-input>
          </el-form-item>
          <el-form-item label="receiver">
            <el-input v-model="SendP2PChatReq.receiver"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="sendMessageToUser" style="border-radius: 4px">sendMessageToUser</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{SendP2PChatRes}}</p>
    </div>

    <p class="text-unit">A给群组发消息</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="reliable">
            <!--<el-input v-model="SendP2ChannelReq.option.reliable"></el-input>-->
            <template>
              <el-select v-model="SendP2ChannelReq.option.reliable" placeholder="reliable">
                <el-option
                  v-for="item in reliable"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </template>
          </el-form-item>
          <el-form-item label="content">
            <el-input v-model="SendP2ChannelReq.content"></el-input>
          </el-form-item>
          <el-form-item label="channelId">
            <el-input v-model="SendP2ChannelReq.channelId"></el-input>
          </el-form-item>

          <el-form-item class="search">
            <el-button type="primary" @click="sendMessageToChannel" style="border-radius: 4px">sendMessageToChannel</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{SendP2ChannelRes}}</p>
    </div>

    <p class="text-unit">加入Channel</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="JoinChannelReq.channelId"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary" @click="joinChannel" style="border-radius: 4px">joinChannel</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{JoinChannelRes}}</p>
    </div>

    <p class="text-unit">离开Channel</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="LeaveChannelReq.channelId"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="leaveChannel" style="border-radius: 4px">leaveChannel</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{LeaveChannelRes}}</p>
    </div>

    <p class="text-unit">设置用户属性</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="SetUserAttributesReq.channelId"></el-input>
          </el-form-item>
          <el-form-item label="key">
            <el-input v-model="SetUserAttributesReq.key"></el-input>
          </el-form-item>
          <el-form-item label="prop">
            <el-input v-model="SetUserAttributesReq.prop"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="setUserAttributes" style="border-radius: 4px">setUserAttributes</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;" >{{SetUserAttributesRes}}</p>
    </div>

    <p class="text-unit">删除用户某些属性</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="DeleteUserAttributesReq.channelId"></el-input>
          </el-form-item>
          <el-form-item label="keys">
            <el-input v-model="DeleteUserAttributesReq.keys"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="deleteUserAttributesByKeys" style="border-radius: 4px">deleteUserAttributesByKeys</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;" >{{DeleteUserAttributesRes}}</p>
    </div>

    <p class="text-unit">查询用户列表</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="GetChannelUserListReq.channelId"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="getChannelUserList" style="border-radius: 4px">getChannelUserList</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{GetGroupUserListRes}}</p>
    </div>

    <p class="text-unit">查询某一属性的用户列表</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="channelId">
            <el-input v-model="GetChannelUserListByAttributeReq.channelId"></el-input>
          </el-form-item>
          <el-form-item label="key">
            <el-input v-model="GetChannelUserListByAttributeReq.key"></el-input>
          </el-form-item>
          <el-form-item label="prop">
            <el-input v-model="GetChannelUserListByAttributeReq.prop"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="getChannelUserListByAtrribute" style="border-radius: 4px">getChannelUserListByAtrribute</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{GetGroupUserListByAttributeRes}}</p>
    </div>
    
    <p class="text-unit">查询queryOnlineStatusForUser</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item label="uid">
            <el-input v-model="queryOnlineStatusForUserParams.uid"></el-input>
          </el-form-item>
          <el-form-item class="search">
            <el-button type="primary"  @click="queryOnlineStatusForUser" style="border-radius: 4px">queryOnlineStatusForUser</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;">{{queryOnlineStatusForUserRes}}</p>
    </div>

    <p class="text-unit">清除MQ队列</p>
    <el-row type="flex" class="row-bg">
      <el-col :span="24"  style="height: 45px;text-align:left;" >
        <el-form :inline="true"  size="small">
          <el-form-item class="search">
            <el-button type="primary"  @click="clearMqData" style="border-radius: 4px">clearMqData</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>

    <!--
    <p class="text-unit">接收MQ队列消息</p>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;" >{{mq_data}}</p>
    </div>

    <p class="text-unit">接收MQ Channel队列消息</p>
    <div class="text">
      <p class="rsp-text" type="textarea" contenteditable="true" style="width: 80%;height: 46px; text-align:left;" >{{mq_channel_data}}</p>
    </div>
    -->
  </div>
</template>

<script>

  import { getStorage, setStorage } from '@/utils/BaseUtil'
  //import Hummer from 'hummer-channel';

  const UID = getStorage('uid');
  const AREA = getStorage("area");
  const APPID = getStorage("appid");

  export default {
    name : 'channel-test',
    data() {
      return {
        flag: -1,
        hummer: null,
        channel: null,
        appid: APPID,
        uid: UID,
        mq_data: [],
        mq_channel_data: [],
          reliable: [{
              value: 'yes',
              label: 'yes'
            }, {
              value: 'no',
              label: 'no'
            }],
          GetInstanceRes: {
          },
          GetMaxSeqIdRes: {
          },
          GetGrpSysMaxSeqIdReq: {
            channelId: 'test_channel1',
          },
          GetGrpSysMaxSeqIdRes: {
          },
          JoinChannelReq: {
            channelId: 'test_channel1',
          },
          JoinChannelRes: '',
          LeaveChannelReq: {
            channelId: 'test_channel1',
          },
          LeaveChannelRes: '',
          SetUserAttributesReq: {
            key: 'role',
            prop: 'teacher',
            channelId: 'test_channel1',
          },
          SetUserAttributesRes: '',
          DeleteUserAttributesReq: {
            keys: 'role',
            channelId: 'test_channel1',
          },
          DeleteUserAttributesRes: '',
          GetChannelUserListReq: {
            channelId: 'test_channel1',
            pos: 0,
            num: 100,
          },
          GetGroupUserListRes: '',
          GetChannelUserListByAttributeReq: {
            channelId: 'test_channel1',
            key: 'role',
            prop: 'teacher',
          },
          GetGroupUserListByAttributeRes: '',
          queryOnlineStatusForUserParams: {
            uid: UID,
          },
          SendP2PChatReq: {
            option: { reliable: 'no' },
            content: 'js_sdk SendP2PChat',
            receiver: UID,
          },
          SendP2PChatRes: "",
          SendP2ChannelReq: {
            option: { reliable: 'no' },
            content: 'js_sdk SendP2Channel',
            channelId: 'test_channel1',
          },
          SendP2ChannelRes: "",
          queryOnlineStatusForUserRes: '',
        }
      },
      computed: {
      },
      watch: {
      },
      created() {
        const token = getStorage("token");

        // 1. 初始化Hummer
        this.hummer = new Hummer.Hummer({ appid: this.appid,
                                    uid: this.uid,
                                    token: token,
                                    area: AREA,
                                    onConnectStatus: this.onConnectStatus,
                                    onLoginStatus: this.onLoginStatus,
                                    onerror: (d) => {
                                      console.log('new hummer: d=' + JSON.stringify(d));
                                      this.flag = d.code;
                                    }
                                  });
        
        if (this.flag != 0) {
          this.hummer = null;
          return;
        }

        this.hummer.setLogLevel({level: -1});
      },
      destroyed() {
      },
      mounted() {
      },
      methods: {
        initChannel() {
          if (!this.hummer) {
            console.log("hummer is null");
            return;
          }

          if (this.channel) {
              console.log("channel is ready");
              return;
          }

          this.channel = new Hummer.ChannelService(this.hummer, {
              onReceiveMessage: this.onReceiveMessage,
              onReceiveChannelMessage: this.onReceiveChannelMessage,
              onNotifyJoinChannel: this.onNotifyJoinChannel,
              onNotifyLeaveChannel: this.onNotifyLeaveChannel,
              onNotifyUserAttributesUpdated: this.onNotifyUserAttributesUpdated,
              onNotifyUserAttributesSet: this.onNotifyUserAttributesSet,
              onNotifyUserAttributesDelete: this.onNotifyUserAttributesDelete,
              onerror: (d) => {
                console.log('new channel: d=' + JSON.stringify(d));
                this.flag = d.code;
              }
          });

          if (this.flag != 0) {
            delete this.channel;
            this.channel = null;
            return;
          }
        },
        // ------------------ 测试接口 --------------------
        getInstance() {
          if (!this.channel)
            return;

          this.channel.getInstance().then(res => {
            console.log("getInstance: " + JSON.stringify(res));
            this.GetInstanceRes = res;
          }).catch(err => {
            console.log(err);
          });
        },
        sendMessageToUser() {
          if (!this.channel)
            return;

          let reliable = this.SendP2PChatReq.option.reliable;
          let content = this.SendP2PChatReq.content;
          let receiver = this.SendP2PChatReq.receiver;

          this.channel.sendMessageToUser({
            receiver: receiver, 
            type: "100", 
            content: Hummer.Utify.encodeStringToUtf8Bytes(content),
            option: { reliable: reliable }
          }).then(res => {
            console.log("sendMessageToUser Res: " + JSON.stringify(res));
            this.SendP2PChatRes = JSON.stringify(res);

            console.log("消息队列mq_data: " + JSON.stringify(this.mq_data));
          }).catch((err) => {
            console.log(err)
          })
        },
        sendMessageToChannel() {
          if (!this.channel)
            return;

          let reliable = this.SendP2ChannelReq.option.reliable;
          let content = this.SendP2ChannelReq.content;
          let channelId = this.SendP2ChannelReq.channelId;

          this.channel.sendMessageToChannel({
            channelId: channelId, 
            type: "100", 
            content: Hummer.Utify.encodeStringToUtf8Bytes(content), 
            option: { reliable: reliable }
          }).then(res => {
            console.log("sendMessageToChannel Res: " + JSON.stringify(res));
            this.SendP2ChannelRes = JSON.stringify(res);

            console.log("消息队列mq_channel_data: " + JSON.stringify(this.mq_channel_data));
          }).catch((err) => {
            console.log(err)
          })
        },
        getGrpSysMaxSeqId() {
          if (!this.channel)
            return;

          let channelId = this.GetGrpSysMaxSeqIdReq.channelId;
    
          let params = { channelId };
          console.log("getGrpSysMaxSeqId Req: " + JSON.stringify(params));
          
          this.channel.getGrpSysMaxSeqId(channelId).then(res => {
            console.log("getGrpSysMaxSeqId res:", res);
            this.GetGrpSysMaxSeqIdRes = JSON.stringify(res);
          }).catch(err => {
          });
        },
        joinChannel() {
          if (!this.channel)
            return;

          let channelId = this.JoinChannelReq.channelId;
          let extra = {"Name": "阿武"};
          let params = { channelId, extra };
          console.log("joinChannel Req: " + JSON.stringify(params));
          
          this.channel.joinChannel(params).then(res => {
            console.log("自己进入频道joinChannel res:", res);
            this.JoinChannelRes = JSON.stringify(res);
          }).catch(e => {
            console.log("joinChannel: err=", e);
          });
        },
        leaveChannel() {
          if (!this.channel)
            return;

          let channelId = this.LeaveChannelReq.channelId;
          let extra = {"Name": "阿武"};
          let params = { channelId, extra };
          console.log("leaveChannel Req: " + JSON.stringify(params));

          this.channel.leaveChannel(params).then(res => {
            console.log("自己离开频道leaveChannel res:", res);
            this.LeaveChannelRes = JSON.stringify(res);
          }).catch(e => {
            console.log("leaveChannel: err=", e);
          });
        },
        setUserAttributes() {
          if (!this.channel)
            return;

          let channelId = this.SetUserAttributesReq.channelId;

          let attributes = {
            "Name": "awu",
            "Description": "js_sdk测试",
            "Bulletin": "bull",
            "Extention": "ex"
          };
    
          let key = this.SetUserAttributesReq.key;
          let prop = this.SetUserAttributesReq.prop;
          attributes[key] = prop;
          
          let params = { channelId, attributes };
          this.channel.setUserAttributes(params).then((res) => {
            console.log("setUserAttributes Res: ", res);
            this.SetUserAttributesRes = JSON.stringify(res);
          }).catch((err) => {
            console.log(err)
          })
        },
        deleteUserAttributesByKeys() {
          if (!this.channel)
            return;

          let channelId = this.DeleteUserAttributesReq.channelId;
          let keys_str = this.DeleteUserAttributesReq.keys;

          let keys = [];

          let elements = keys_str.split(",");
          for (let k of elements) {
            keys.push(k);
          }

          let params = { channelId, keys };
          this.channel.deleteUserAttributesByKeys(params).then((res) => {
            console.log("deleteUserAttributesByKeys Res: ", res);
            this.DeleteUserAttributesRes = JSON.stringify(res);
          }).catch((err) => {
            console.log(err)
          })
        },
        getChannelUserList() {
          if (!this.channel)
            return;

          let channelId = this.GetChannelUserListReq.channelId;
          this.channel.getChannelUserList({ channelId }).then(res => {
            console.log("getChannelUserList res:", res);
            this.GetGroupUserListRes = JSON.stringify(res);
          }).catch(err => {
          });
        },
        getChannelUserListByAtrribute() {
          if (!this.channel)
            return;

          let channelId = this.GetChannelUserListByAttributeReq.channelId;
          let key = this.GetChannelUserListByAttributeReq.key;
          let prop = this.GetChannelUserListByAttributeReq.prop;

          this.channel.getChannelUserListByAtrribute({ channelId, key, prop }).then(res => {
            console.log("getChannelUserListByAtrribute res:", res);
            this.GetGroupUserListByAttributeRes = JSON.stringify(res);
          }).catch(err => {
          });
        },
        queryOnlineStatusForUser() {
          if (!this.channel)
            return;

          let uid = this.queryOnlineStatusForUserParams.uid;
          this.channel.queryOnlineStatusForUser({uid: uid}).then(res => {
            console.log("queryOnlineStatusForUser res:", res);
            this.queryOnlineStatusForUserRes = JSON.stringify(res);
          }).catch(err => {
          });

        },
        clearMqData(){
          this.mq_data = [];
          this.mq_channel_data = [];
          this.ReceiveChannelMessage = '';
          this.ReceiveMessage = '';
        },

        /* 消息接收模块 */
        onReceiveMessage(obj) {
          this.ReceiveMessage = JSON.stringify(obj);
          console.log("接收消息ReceiveMessage: " + JSON.stringify(obj));
          obj.message.data = Hummer.Utify.decodeUtf8BytesToString(obj.message.data);
          this.mq_data.push(obj);

          this.$message({
            duration: 3000,
            message: "ReceiveMessage: " + JSON.stringify(obj),
            type: 'success'
          });

          console.log("MQ队列mq_data: " + JSON.stringify(this.mq_data));
        },
        /* 组播消息接收模块 */
        onReceiveChannelMessage(obj) {
          this.ReceiveChannelMessage = JSON.stringify(obj);
          console.log("接收组播消息ReceiveChannelMessage: " + JSON.stringify(obj));

          obj.message.data = Hummer.Utify.decodeUtf8BytesToString(obj.message.data);
          this.mq_channel_data.push(obj);

          this.$message({
            duration: 3000,
            message: "ReceiveChannelMessage: " + JSON.stringify(obj),
            type: 'success'
          });

          console.log("组播MQ队列mq_channel_data: " + JSON.stringify(this.mq_channel_data));
        },
        onNotifyJoinChannel(obj) {
          console.log("接收消息NotifyJoinChannel: " + JSON.stringify(obj));
          /*
          this.$message({
            duration: 3000,
            message: "JoinChannel: " + JSON.stringify(obj),
            type: 'success'
          });
          */
        },
        onNotifyLeaveChannel(obj) {
          console.log("接收消息NotifyLeaveChannel: " + JSON.stringify(obj));
          /*
          this.$message({
            duration: 3000,
            message: "LeaveChannel: " + JSON.stringify(obj),
            type: 'success'
          });
          */
        },
        onNotifyUserAttributesSet(obj) {
          console.log("用户属性设置UserAttributesSet: " + JSON.stringify(obj));
          /*
          this.$message({
            duration: 3000,
            message: "UserAttributesSet: " + JSON.stringify(obj),
            type: 'success'
          });
          */
        },
        onNotifyUserAttributesDelete(obj) {
          console.log("用户属性删除UserAttributesDelete: " + JSON.stringify(obj));
          this.$message({
            duration: 3000,
            message: "UserAttributesDelete: " + JSON.stringify(obj),
            type: 'success'
          });
        },
        onConnectStatus(obj) {
          console.log("===channel status===:", obj);
        },
        onLoginStatus(obj) {
          console.log("===login status===:", obj);
        }
    }
  }
</script>

<style scoped>
.line{
  text-align: center;
}
</style>

