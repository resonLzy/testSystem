<template>
  <div class="mian1">
    <el-row class="main">
      <el-row class="public">
        <el-card>
          <img src="../../../assets/img/公开发布.png" alt="">
          <div class="title">公开发布</div>
        </el-card>
        <div style="margin-top:10px">在报考管理中进行审核</div>
        <el-button @click="piblishPaper(0)" size="small" type="primary" plain>公 开 发 布</el-button>
      </el-row>
      <el-row class="public">
        <el-card>
          <img src="../../../assets/img/验证码发布.png" alt="">
          <div class="title">验证码发布</div>
          <div>{{textpaperShare}}</div>
          <div>复制验证码</div>
          <div @click="updataCode">刷新验证码</div>
        </el-card>
        <div style="margin-top:10px">获取验证码可直接考试</div>
        <el-button @click="piblishPaper(1)" size="small" type="primary" plain>验 证 码 发 布</el-button>
      </el-row>
</el-row>
  </div>
</template>



<script>
  export default {
    props:['paperID'],
    data(){
      return{
        textpaperShare:''
      }
    },
    created(){
      this.paperShare(this.paperID)
      
    },
    methods:{
      piblishPaper(method){
        this.$http.put('testPaper/updateTestStatus',{
         
          id: this.paperShareID,
          test_method:method,
          test_status:1
          
        })
          .then(res => {
            if (res.data.code != 0) return this.$message.error(res.data.msg)
           this.$message.success(res.data.msg)
            setTimeout(()=>{
              this.$emit('piblishPaper')
            },1500)
          }).catch(err => {
            this.$message({
              dangerouslyUseHTMLString: true,
              showClose: true,
              message: err.response.data.data.join('<br><br>'),
              type: 'error'
            });
          })
      },
      paperShare(id) {
        this.paperShareID = id
        this.$http.get('testPaper/findShareCode',{
          params:{
            id:id
          }
        })
          .then(res => {
            if (res.data.code != 0) return this.$message.error(res.data.msg)
            console.log(this.$decryptData(res.data.data));
            this.textpaperShare = this.$decryptData(res.data.data)[0].share_code
          }).catch(err => {
            this.$message({
              dangerouslyUseHTMLString: true,
              showClose: true,
              message: err.response.data.data.join('<br><br>'),
              type: 'error'
            });
          })
      },
      updataCode(){
        this.$http.put('testPaper/updateVeriCode',this.$qs.stringify({id:this.paperID}))
          .then(res => {
            if (res.data.code != 0) return this.$message.error(res.data.msg)
            console.log(this.$decryptData(res.data.data));
            this.textpaperShare = this.$decryptData(res.data.data)
          }).catch(err => {
            // console.log(err);
            this.$message({
              dangerouslyUseHTMLString: true,
              showClose: true,
              message: err.response.data.data.join('<br><br>'),
              type: 'error'
            });
          })
      }
    }

  }
</script>
<style lang="less" scoped>
.mian1{
     width: 100%;
    height: 100%;
}
  .main {

    display: flex;
    justify-content: space-around;
    margin-top: 100px;

    .public {
      text-align: center;
      background: #e7e7e7;
      height: 300px;
      border-radius: 5px;
      .el-button{
        margin: 0 auto;
        margin-top: 40px;
      }

      .el-card {
        height: 86%;
        width: 100%;

        img {
          margin-top: 10px;
          width: 60%;
        }

        .title {
          margin-top: 10px;
          background: linear-gradient(to right, #24c9e3, blue);
          -webkit-background-clip: text;
          color: transparent;
          font-size: 22px;
        }

        div:nth-of-type(2) {
          color: #999;
          font-size: 14px;
        }

        div:nth-of-type(3) {

          color: #333;
          margin: 0 auto;
          font-size: 14px;
          border-radius: 50px;
          width: 100px;
          height: 30px;
          margin-top: 5px;
          background: #24c9e3;
          cursor: pointer;
          box-sizing: border-box;
          padding: 5px;
          color: #fff;
        }

        div:nth-of-type(4) {
          color: #333;
          margin: 0 auto;
          font-size: 14px;
          border-radius: 50px;
          width: 100px;
          margin-top: 5px;
          cursor: pointer;
          height: 30px;
          background: #f4c521;
          box-sizing: border-box;
          padding: 5px;
          color: #fff;
        }
      }

      >div {
        text-align: center;
        font-size: 16px;
        color: #333;
      }

    }
  }
</style>