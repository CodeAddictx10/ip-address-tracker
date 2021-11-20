<template>
    <div class="information-box">
        <div class="desc-box">
            <div class="box">
                <small>IP ADDRESS</small>
                <h4>{{ ip }}</h4>
            </div>
            <div class="box">
                <small>LOCATION</small>
                <h4 v-html="location"></h4>
            </div>
            <div class="box">
                <small>TIMEZONE</small>
                <h4>{{ timezone }}</h4>
            </div>
            <div class="box">
                <small>ISP</small>
                <h4>{{ isp }}</h4>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "IpInfo",
    props: {
        ip: String,
        location: String,
        timezone: String,
        isp: String,
    },
};
</script>

<style></style>
