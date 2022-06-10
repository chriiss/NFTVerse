<script>
import dataJson from "../data/data.json";
export default {
    data() {
        return {
            datasFooter: dataJson.footerComponent
        }
    }
}
</script>

<template>
    <div class="footer  bg_color" v-for="data in datasFooter" :key="data.id">
        <p class="text_center">{{data.copyright}}</p>
    </div>
</template>

<style scoped lang="scss">
@use "@/assets" as *;
@include footerStyle;
</style>