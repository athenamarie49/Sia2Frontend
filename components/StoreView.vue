<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Store
        </button>
        <h5>Store</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Brand">
                <b-form-input v-model="store.brand"></b-form-input>
            </b-form-group>
            <b-form-group label="Description">
                <b-form-input v-model="store.description"></b-form-input>
            </b-form-group>
            <b-form-group label="Product">
                <b-form-input v-model="store.product"></b-form-input>
            </b-form-group>
            <b-form-group label="Value">
                <b-form-input v-model="store.value"></b-form-input>
            </b-form-group>
            <b-form-group>
                <button class="btn btn-info " type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="store.id">Delete This Store</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        store: {}
    },
    methods: {
        async onSave() {
            try {
                if(!this.store.id) {
                    await this.$axios.post('/api/stores', this.store)
                }else{
                    await this.$axios.put('/api/stores/' + this.store.id, this.store)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newstore')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/stores/' + this.store.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>