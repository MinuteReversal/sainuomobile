<template>
    <div></div>
</template>
<script>
import axios from "axios"
import apiConfig from "~/static/apiConfig"
import webConfig from "~/static/webConfig"
/*
* 1、通过openid直接登录
 */
export default {
    methods:{
        loginByOpenId(openId){
            let me=this;
            axios.get(apiConfig.wechatOAuth_get,{params:{openid:openId}}).then(response=>{
                let data=response.data;
                axios.defaults.headers.common['authorization'] =`Bearer ${data.token}`;
                me.$store.dispatch("modules/userinfo/updateUserInfo",data.userInfo);
                me.$store.dispatch("modules/userinfo/updateOpenId",openId);
                
                switch(data.userType){
                    case 1:
                        me.$router.replace("/doctor");
                        break;
                    case 2:
                        me.$router.replace("/tester");
                        break;
                    default:
                        me.$router.replace("/guest");
                        break;
                }
            });
        }
    },
    mounted(){
        let me=this;
        let openid = me.$route.query.openid;
        if(openid){
            me.loginByOpenId(openid);
        }
        else{
            me.$toast.console.fail("获取用户信息失败");
        }
    }
}
</script>

