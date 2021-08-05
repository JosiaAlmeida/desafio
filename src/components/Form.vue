<template>
    <div>
        <form >
            <input type="text" placeholder="Digite aqui...">
            <div v-for="dados in data" :key="dados.id">
                <Slug :data="" />
                <input type="checkbox" name="inp" id="">
            </div>
        </form>
    </div>
</template>

<script lang='ts'>
import axios from 'axios'
import Slug from './slug/slug.vue'

import {Component, Prop, Vue} from 'vue-property-decorator'
export default Vue.extend({
    name: 'Form'
    components:{
        Slug
    }
})
</script>
