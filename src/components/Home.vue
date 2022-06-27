<script>
import Axios from 'axios';
import moment from 'moment';
import dataJson from '../data/data.json';

export default {
    data() {
        return {
            titleText:["Join the Movement and Unlock Unique Art"],
            statsDatas: [],
            datas: dataJson.homeNFTComponent
        }
    },
    methods: {
        async getData() {
            await Axios.get("https://api.opensea.io/api/v1/collections?offset=0&limit=10")
            .then(response => {
                this.statsDatas = response.data.collections;
            })
        },
        formatDate(value) {
            if (value) {
                return moment(String(value)).format('DD/MM/YYYY hh:mm:ss')
            }
        }
    },
    mounted() {
        this.getData();
    }
}
</script>


<template>
    <div class="home secondary_color">
        <div class="home_background">
            <div class="home_background_title">
                <h1>
                    <vue-writer class="text-highlight" :array="titleText" :eraseSpeed="50" :typeSpeed="100"></vue-writer>
                </h1>
                <p>Discover our work below</p>
                <router-link to="/Collections" class="btn">Collection</router-link>
            </div>
        </div>
        <div class="home_main  bg_color text_center">
            <h2>
                <svg viewBox="0 0 18 18" xmlns="http://www.w3.org/2000/svg" fill="#17EF97" width="15" class="homepage_star"><path d="m9 0 2.756 6.244L18 9l-6.244 2.756L9 18l-2.756-6.244L0 9l6.244-2.756L9 0Z"></path></svg>
                Recently listed
                <svg viewBox="0 0 18 18" xmlns="http://www.w3.org/2000/svg" fill="#35467E" width="15" class="homepage_star"><path d="m9 0 2.756 6.244L18 9l-6.244 2.756L9 18l-2.756-6.244L0 9l6.244-2.756L9 0Z"></path></svg>
            </h2>
            <div class="nft_bloc d_flex">
                <div class="nft_bloc_children" v-for="data in statsDatas" :key="data.id">
                    <div>
                         <img :src="data.image_url" :alt="data.name"/>
                    </div>
                    <div>
                        <h3>{{data.name.toLowerCase()}}</h3>
                        <h3>Add: {{formatDate(data.created_date)}}</h3>
                    </div>
                </div>
            </div>
        </div>
        <div class="home_main  bg_color">
            <div class="nft_bloc d_flex">
                <div class="nft_bloc_children" v-for="data in datas" :key="data.id">
                    <div class="nft_bloc_children_bis">
                        <img :src="data.img_link" :alt="data.name"/>
                        <div class="text_center">
                        <h2 class="secondary_color">{{data.title}}</h2>
                        <p>{{data.description}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<style scoped lang="scss">
@use "@/assets" as *;
@include homeStyle;
@include styleAnimation;
</style>