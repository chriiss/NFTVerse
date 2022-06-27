<script>
import Axios from "axios";
import Pagination from './Pagination.vue';
import Loader from './Loader.vue';
import dataJson from '../data/data.json';
export default {
    components: {
        Pagination,
        Loader
    },
    data() {
        return {
            collectDatas: [],
            searchDatas: "",
            showLoader: false,
            page: 1,
            totalPages: 0,
            totalRecords: 0,
            recordsPerPage: 100,
            enterpage: '',
            datas: dataJson,
            sortBy: null
        }
    },
    created () {
        this.getData()
    },
    methods: {
        async getData() {
            this.showLoader = true;
            await Axios.get(`https://api.opensea.io/api/v1/collections?offset=${this.page}&limit=${this.recordsPerPage}`)
            .then(response => {
                this.collectDatas = response.data.collections;
                console.log(response.data.collections);
                this.showLoader = false
                this.totalPages = Math.floor(response.data.collections / this.recordsPerPage) // Calculate total records
                this.totalRecords = response.data.collections
            })
        },
        onPageChange (page) {
            this.page = page
            this.getData()
        },
        onChangeRecordsPerPage () {
            this.getData()
        },
        gotoPage () {
            if (!isNaN(parseInt(this.enterpage))) {
                this.page = parseInt(this.enterpage)
                this.getData()
            }
        }
    },
    computed: {
        filteredCollections() {
                return this.collectDatas.filter(data => {
                return data.name.toLowerCase().includes(this.searchDatas);
            })
        },
        filterCollections() {
            const sortTypesCollect = {
                1: (a,b) => b.stats.total_supply - a.stats.total_supply,
                2: (a,b) => a.stats.total_supply - b.stats.total_supply
            }

            return this.collectDatas.sort(sortTypesCollect[this.sortBy]);
        }
    }
}
</script>


<template>
    <div id="collect" class="collections bg_color">
        <Loader :loading="showLoader" />
        <h2 class="text_center secondary_color">
             <svg viewBox="0 0 18 18" xmlns="http://www.w3.org/2000/svg" fill="#17EF97" width="15" class="homepage_star"><path d="m9 0 2.756 6.244L18 9l-6.244 2.756L9 18l-2.756-6.244L0 9l6.244-2.756L9 0Z"></path></svg>
            Explore Collections
             <svg viewBox="0 0 18 18" xmlns="http://www.w3.org/2000/svg" fill="#35467E" width="15" class="homepage_star"><path d="m9 0 2.756 6.244L18 9l-6.244 2.756L9 18l-2.756-6.244L0 9l6.244-2.756L9 0Z"></path></svg>
        </h2>
        <div class="button_bloc">
            <a href="#collect">	&#8593;</a>
        </div>
        <div>
            <div class="search_bloc" >
                <input type="search" v-model="searchDatas" placeholder="Search a collection"/>
            </div>
            <ul class="show_items d_flex f_column">
                <li>
                    <label>Show Items</label><br>
                    <select   class="pagination_color" @change="onChangeRecordsPerPage" v-model="recordsPerPage">
                    <option v-for="data in datas.selectItemsCollect" :key="data.id" :value="data.value">{{data.value}}</option>
                    </select>
                </li>
                <li>
                    <label>Go to Page</label><br>
                    <input class="pagination_color" type="text" v-model="enterpage" @keyup.enter="gotoPage"><button class="pagination_color" type="button" @click.prevent="gotoPage">Go</button>
                </li>
                <li>
                    <label>Page {{page}}</label><br>
                    <Pagination v-if="getData" :total-pages="totalPages" :per-page="recordsPerPage" :current-page="page" @pagechanged="onPageChange"/>
                </li>
                <li>
                    <label>Filter by</label><br>
                    <select class="pagination_color" v-model="sortBy" :change="filterCollections">
                        <option v-for="data in datas.selectFilterCollections" :key="data.id" :value="data.value">{{data.name}}</option>
                    </select>
                </li>
            </ul>
        </div>
        <div class="nft_bloc d_flex">
            <div class="nft_bloc_children" v-for="data in filteredCollections" :key="data.id">
                <div>
                    <img :src="data.image_url" :alt="data.name"/>
                </div>
                <div class="text_center">
                    <h3>{{data.name.toLowerCase()}}</h3>
                    <div class="supply_bloc">
                        <p><i class="fab fa-ethereum"></i> Total Supply: {{data.stats.total_supply}}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<style scoped lang="scss">
@use "@/assets" as *;
@include collectionStyle;
@include styleAnimation;

.button_bloc {
    float: right;
    margin-right: 2rem;
    position: sticky;
    top: 25rem;
    z-index: 1;

    a {
        background-color: $primary_color;
        color: $secondary_color;
        font-weight: bold;
        border: 1px solid $secondary_color;
        padding: 1rem;
    }
}
</style>