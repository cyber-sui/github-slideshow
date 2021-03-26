<template>
<div id="app">
    <el-row>
        <el-col :span="24">
            <div class="grid-content bg-blue-dark" style="color:#FFFFFF;height:50px;">
                <div style="padding-top:5px;float:right;padding-right:10px;">
                    <el-button type="primary" @click="guideAction">接続ガイド</el-button>
                    <el-button type="warning" @click="checkAction">確認開始</el-button>
                </div>
                <div class="grid-content" style="color:#FFFFFF;padding-top:15px;padding-left:10px">i-web Live 環境確認</div>
            </div>
        </el-col>
    </el-row>
    <el-col :span="24">
        <div class="grid-content" style="font-size:20px;padding-left:10px;padding-right:10px">i-web LIVEをご利用頂くために、利用環境の確認を行います。画面右上の「確認開始」ボタンをクリックしてください。環境確後に「入室」ボタンが表示されます。</div>
    </el-col>
    <el-row>
        <div class="grid-content" style="font-size:12px">※環境確認をスキップする場合は
            <a href="">こちら</a></div>
    </el-row>
    <div>
        <el-collapse v-loading="startLoading">
            <el-collapse-item title="カメラ" :disabled="cameraCheck" name="1">
                <div style="color:#FF0000" v-show="!cameraCheck">カメラを許可してください。</div>
            </el-collapse-item>
            <el-collapse-item title="マイク" :disabled="audioCheck" name="2">
                <div style="color:#FF0000" v-show="!audioCheck">マイクを許可してください。</div>
            </el-collapse-item>
            <el-collapse-item title="スピーカー" :disabled="volumeCheck" name="3">
                <div style="color:#FF0000" v-show="!volumeCheck">音量が0です。音量を上げてください。</div>
            </el-collapse-item>
            <el-collapse-item title="ネットワーク環境" :disabled="netCheck" name="4">
                <div style="color:#FF0000" v-show="!netCheck">ネットワーク接続に失敗しました、確認後にもう一度お試しください。</div>
            </el-collapse-item>
        </el-collapse>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            startLoading: false,
            cameraCheck: true,
            audioCheck: true,
            volumeCheck: true,
            netCheck: true
        }
    },
    methods: {
        guideAction: function () {

        },

        checkAction: function () {
            // this.startLoading = true
            //カメラの権限を検出する
            console.log(this.video)
            navigator.mediaDevices.getUserMedia({
                video: true
            }).then((stream) => {
                this.cameraCheck = true
                this.video.srcObject = stream
                this.video.setAttribute('playsinline', true)
                this.video.play()
                requestAnimationFrame(this.tick)
            }).catch((error) => {
                if (error) {
                    this.cameraCheck = false
                }
            })

            // マイクの権限を検出する
            navigator.mediaDevices.getUserMedia({
                audio: true
            }).then((stream) => {
                this.audioCheck = true
            }).catch((error) => {
                if (error) {
                    this.audioCheck = false
                }
            })
            //ボリュームを検出する

            //ネットワーク接続を検出する
            if(window.navigator.onLine){
                this.netCheck = true
            }else{
                this.netCheck = false
            }
        },

    },
    mounted: function () {

    }
}
</script>

<style>
.el-row {
    margin-bottom: 30px;
}

.el-col {
    border-radius: 4px;
}

.bg-blue-dark {
    background: #0C69AF;
}

.bg-purple {
    background: #d3dce6;
}

.bg-purple-light {
    background: #e5e9f2;
}

.grid-content {
    border-radius: 4px;
    min-height: 36px;
}

.row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
}

.el-button--warning {
    color: #fff;
    background-color: #f9fafc;
    border-color: #f9fafc;
  }
  .el-button--warning:hover{
    background-color: #f9fafc;
    border-color: #f9fafc;  
  }
  .el-button--warning:focus{
    background-color: #f9fafc;
    border-color: #f9fafc;  
  }
</style>
