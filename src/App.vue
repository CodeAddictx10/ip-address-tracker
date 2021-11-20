<template>
    <div class="container">
        <div class="upper-section">
            <div class="heading-wrapper">
                <h3>IP ADDRESS TRACKER</h3>
            </div>
            <search @searchIP="getIpInfo" />
        </div>
        <div class="wrapper">
            <ip-information
                :ip="ip"
                :location="location"
                :timezone="timezone"
                :isp="isp"
                v-if="ip != ''"
            />
        </div>
        <div class="map-section">
            <div class="mapid" ref="mapid" id="mapid"></div>
        </div>
    </div>
</template>

<script>
import L from "leaflet";
import axios from "axios";
import Search from "./components/Search.vue";
import IpInformation from "./components/Ip-information.vue";

export default {
    name: "App",
    components: { Search, IpInformation },
    data() {
        return {
            mapid: null,
            ip: "",
            location: "",
            timezone: "",
            isp: "",
        };
    },

    methods: {
        //get search ip address
        async getIpInfo(ip) {
            try {
                const data = await axios.get(
                    `https://geo.ipify.org/api/v1?apiKey=${process.env.VUE_APP_GEO_IP}&ipAddress=${ip}`,
                );

                let ipInfo = {
                    address: data.data.ip,
                    state: data.data.location.region,
                    city: data.data.location.city,
                    timezone: data.data.location.timezone,
                    isp: data.data.isp,
                    lat: data.data.location.lat,
                    lng: data.data.location.lng,
                };
                this.ip = ip;
                this.location = `${ipInfo.city}<br>${ipInfo.state}`;
                this.timezone = ipInfo.timezone;
                this.isp = ipInfo.isp;
                let pointerIcon = L.icon({
                    iconUrl: require("@/assets/images/icon-location.svg"),
                    iconSize: [38, 40], // size of the icon
                    iconAnchor: [22, 94], // point of the icon which will correspond to marker's location
                    popupAnchor: [-3, -76], // point from which the popup should open relative to the iconAnchor
                });
                L.marker([ipInfo.lat, ipInfo.lng], { icon: pointerIcon }).addTo(
                    this.mapid,
                );
                this.mapid.setView([ipInfo.lat, ipInfo.lng], 13);
            } catch (err) {
                console.log(err.message);
            }
        },

        // get user ip address
        getUserIP() {
            axios
                .get("https://api64.ipify.org/?format=json")
                .then((res) => {
                    let ip = res.data.ip;
                    this.getIpInfo(ip);
                })
                .catch((err) => this.getIpInfo("0.0.0.0"));
        },
    },
    mounted() {
        this.getUserIP();
        this.mapid = L.map(this.$refs["mapid"]).setView([51.505, -0.09], 13);
        L.tileLayer(
            `https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${process.env.VUE_APP_MAP_BOX}`,
            {
                attribution:
                    'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
                maxZoom: 18,
                id: "mapbox/streets-v11",
                tileSize: 512,
                zoomOffset: -1,
                accessToken: process.env.VUE_APP_MAP_BOX,
            },
        ).addTo(this.mapid);
    },
};
</script>

<style src="@/assets/css/style.css"></style>
