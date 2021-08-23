<template>
  <div>
    <form>
      <input type="text" placeholder="Digite aqui..." v-model="keyword" />
      <Slug :keyword="keyword" :data="Data" :isShow="isShow" />
    </form>
  </div>
</template>

<script lang='ts'>
import axios from "axios";
import Slug from "./slug/slug.vue";

import { Vue } from "vue-property-decorator";
interface filterOptions {
  subline: string;
  value: string;
  label: string;
  type: string;
  queryField: string;
  tags: string;
}
interface Filters {
  groupTag: string
  
}
export default Vue.extend({
  name: "Form",
  components: {
    Slug,
  },
  data() {
    return {
      Data: [] as filterOptions[],
      keyword: '' as string,
      isShow: false,
      Result: [] as filterOptions[]
    };
  },
  methods: {
    async AllData() {
      await axios
        .get("https://filters.app3.speedio.com.br/api/v3/filters.json")
        .then((res) => {
          let filters = res.data.filters.map((x: any) =>
            x.filters
              .filter(
                (x: Filters) =>
                  x.groupTag == "setores e cnaes" ||
                  x.groupTag == "estados" ||
                  x.groupTag == "municipio"
              )
              .pop()
          );
          let filterOptions = filters;
          this.ReducerElementArray(filterOptions);
          let data: filterOptions[] = [];
          filterOptions.map((x: any) => data.push(x.filterOptions));
          data.map((x: any) =>
            x.slice(0, 7).map((x: any) => this.Data.push({ ...x, bool: false }))
          );
        })
        .catch((err) => console.log("Ocorreu um erro " + err));
    },
    ReducerElementArray(value: filterOptions[]){
      var i = 0;
      while (i < value.length) {
        if (value[i] === undefined) {
          value.splice(i, 1);
        } 
        else {
          ++i;
        }
      }
    },
    VerifyKeywordData(){
      if(this.keyword)
        this.isShow = true
      else this.isShow = false
    },
    TransformText(text: string){
      return text.normalize("NFD").replace(/[^a-zA-Zs]/g, "").toLowerCase()
    },
  },
  mounted() {
    this.AllData();
  },
  updated(){
    this.VerifyKeywordData()
  }
});
</script>
