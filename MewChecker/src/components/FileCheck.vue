<template>

    <div v-if="checkComplete">
        <div v-if="fileCheckResult" class="page-title">
            <div class="page-container flex-box-item-center">
                <div class="title">
                    <h1>DNS Watcher</h1>
                    <h4>status.myetherwallet.com</h4>
                </div>
                <div class="current-status ">
                    <!--<p class="for-desktop" style="color: #28e2b9"><i class="fa fa-check-circle-o" aria-hidden="true"></i> Valid</p>-->
                  <p class="good-files"><i class="fa fa-check-circle-o" aria-hidden="true"></i> Valid</p>
                </div>
            </div>
        </div>
        <div v-else class="page-title">
            <div class="page-container flex-box-item-center">
                <div class="title">
                    <h1>DNS Watcher</h1>
                    <h4>status.myetherwallet.com</h4>
                </div>
                <div class="current-status ">
                    <!--<p class="for-desktop" style="color: #e20000"><i class="fa fa-times-circle-o" aria-hidden="true"></i> ERROR</p>-->
                  <p class="bad-files"><i class="fa fa-times-circle-o" aria-hidden="true"></i> ERROR</p>
                </div>
            </div>
        </div>
    </div>
    <div v-else class="page-title">
        <div class="page-container flex-box-item-center">
            <div class="title">
                <h1>DNS Watcher</h1>
                <h4>status.myetherwallet.com</h4>
            </div>
            <div class="current-status">
                <!--<p class="for-desktop" style="color: #ffbf29"><i class="fa fa-refresh" aria-hidden="true"></i> checking</p>-->
              <p class="checking-files"><i class="fa fa-refresh" aria-hidden="true"></i> checking</p>

            </div>
        </div>
    </div>
</template>

<script>

    import fileCheck from '../inBrowserFileCheck'

    let files = [
        '/index.html',
        '/embedded.html',
        '/helpers.html',
        '/signmsg.html',
        '/bin/startMEW.js',
        '/css/etherwallet-master.min.css',
        // fontsBold: "",
        // fontsLight: "",
        // fontsRegular: "",
        '/js/etherwallet-master.js',
        '/js/etherwallet-static.min.js',
        '/js/jquery-1.12.3.min.js'
    ]

    export default {
        name: 'file-check',
        data: function () {
            return {
                checkComplete: false,
                fileCheckResult: false,
                poolingFileCheck: ''
            }
        },
        methods: {
            startPolling() {
                this.poolingFileCheck = setInterval(() => {
                    this.checkComplete = false
                    fileCheck(files)
                        .then(result => {
                            console.log('polling check complete')
                            this.fileCheckResult = result
                            this.checkComplete = true
                        })
                        .catch(error => {
                            console.error(error)
                            clearInterval(this.poolingFileCheck)
                        })
                }, 300000) // checks file status every 5 minutes
            }
        },
        mounted() {
            this.$nextTick(() => {
                fileCheck(files)
                    .then(result => {
                        console.log('complete')
                        this.fileCheckResult = result
                        this.checkComplete = true
                        this.startPolling()
                    })
                    .catch(error => {
                        console.error(error)
                        // clearInterval(this.fileCheckResult);
                    })
            })
        }
    }
</script>


