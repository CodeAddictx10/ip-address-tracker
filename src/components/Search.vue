<template>
    <div class="ip-search-wrapper">
        <input
            type="search"
            name=""
            id=""
            class="ip-search-input"
            placeholder="Search for an ip address or domain"
            v-model="ip"
        />
        <div class="icon-wrapper" @click="searchIp">
            <img src="@/assets/images/icon-arrow.svg" alt="search icon" />
        </div>
    </div>
</template>

<script>
export default {
    name: "Search",
    data() {
        return {
            ip: "",
        };
    },
    methods: {
        searchIp(event) {
            this.$emit("searchIP", this.ip);
        },
    },
};
</script>
