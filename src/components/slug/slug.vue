<template>
    <div>
        <form >
            <input type="text" placeholder="Digite aqui...">
            <div v-for="dados in data" :key="dados.id">
                <label for="">Nome</label>
                <input type="checkbox" name="inp" id="">
            </div>
        </form>
    </div>
</template>

<script lang='ts'>

import { Prop, Vue} from 'vue-property-decorator'
export default Vue.extend({
    name: 'Slug',
    props:{
        data: {
            type: Object
        }
    }
})
</script>
