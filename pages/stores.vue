<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Stores</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Stores</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="store in stores" :key="store.id" @click="onSelected(store)">
                            {{store.brand}} <span class="float-right">{{store.description}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <StoreView :store="selectedStore" @saved="onChange" @newStore="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            stores: [],
            selectedStore: {}
        }
    },
    methods: {
        async getMyStores() {
            await this.$axios.get('/api/stores')
            .then((res)=>{
                if(res.status==200) {
                    this.stores = res.data
                }
            })
        },
        onChange() {
            this.getMyStores()
            this.selectedStore = {}
        },
        onSelected(store) {
            this.selectedStore = store;
        },
        onNew() {
            this.selectedStore = {}
        },
    },
    created() {
        this.getMyStores()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: antiquewhite;
}
</style>