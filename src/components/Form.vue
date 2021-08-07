<template>
    <div>
        <form >
            <input type="text" placeholder="Digite aqui...">
            <div v-for="dados in data" :key="dados.id">
                <Slug :data="data" />
                <input type="checkbox" name="inp" id="">
            </div>
        </form>
    </div>
</template>

<script lang='ts'>
import axios from 'axios'
import Slug from './slug/slug.vue'

import {Component, Prop, Vue} from 'vue-property-decorator'
interface filterOptions {
    subline: string,
    value: string,
    label: string,
    type: string,
    queryField: string,
    tags: string
}
export default Vue.extend({
    name: 'Form',
    components:{
        Slug
    },
    data(){
        return{
            data: [] as filterOptions[]
        }
    },
    methods:{
        async AllData(){
            await axios.get<any>('https://filters.app3.speedio.com.br/api/v3/filters.json')
            .then(res=> res.data.filters.map(x=> console.log(x.filters)))
        }
    },
    mounted(){
        this.AllData()
    }
})
</script>
