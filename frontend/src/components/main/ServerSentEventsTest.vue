<template>
    <div class="test-wrapper">

        <h2>Server Sent Events Test-View</h2>

        <div class="sse-data-wrapper">
            <button class="id-btn" @click="setupStream(1)">1번</button>
            <button class="id-btn" @click="setupStream(2)">2번</button>
            <button class="id-btn" @click="setupStream(3)">3번</button>
        </div>

    </div>
</template>

<script>

    export default {
        name: "ServerSentEventsTest",
        data() {
            return {
                eventSource: null,
            }
        },
        methods: {
            setupStream(id) {

                console.log("==> 이벤트 소스 수행");

                this.eventSource = new EventSource('http://localhost:8080/api/sse-server/stream-sse/' + id, { withCredentials: true });

                this.eventSource.onopen = (function (e) {
                    // console.log("이벤트 소스 오픈");
                });

                this.eventSource.onmessage = (function (e) {
                    console.log("이벤트 소스 메시지 도착");
                    console.log(e);
                });

                this.eventSource.onerror = (function (e) {
                    console.log("이벤트 소스 에러");
                    console.log(e);
                });

            },

            unSetupStream() {
                if (this.eventSource === null) {
                    return;
                }

                console.log("==> 이벤트 소스 종료");
                this.eventSource.close();
            }
        },
        created() {
            // this.setupStream();
        },
        beforeDestroy() {
            this.unSetupStream();
        }
    }
</script>

<style scoped>
    .id-btn{
        padding:10px 25px;
        border:1px solid royalblue;
        border-radius: 15px;
        margin-left:20px;
        margin-right:20px;
    }

    .id-btn:hover{
        color:white;
        background-color: royalblue;
    }
</style>