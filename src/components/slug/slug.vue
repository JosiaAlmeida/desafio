<template>
  <div>
    <form>
      <div v-if="isShow">
        <div v-for="(dados, i) in Result" :key="i">
          <label for=""> {{ dados.label }} </label>
          <input
            type="checkbox"
            :value="dados.value"
            v-model="dados.bool"
            @change="Save()"
          />
        </div>
      </div>
      <div v-else>
        <div v-for="(dados, i) in AllDatas" :key="i">
          <label for=""> {{ dados.label }} </label>
          <input
            type="checkbox"
            :value="dados.value"
            v-model="dados.bool"
            @change="Save()"
          />
        </div>
      </div>
    </form>
  </div>
</template>

<script lang='ts'>
const STOREGE_KEY = "STOREGE_KEY";
import { Vue } from "vue-property-decorator";
interface Data {
  id: number;
  label: string;
  value: string;
  subline: string;
  isSelected?: boolean;
}

export default Vue.extend({
  name: "Slug",
  props: {
    data: {
      type: Array,
    },
    ["keyword"]: String,
    ["isShow"]: Boolean
  },
  data() {
    return {
      AllDatas: [] as Data[],
      Result: [] as Data[]
    };
  },
  methods: {
    Save() {
      localStorage.setItem(STOREGE_KEY, JSON.stringify(this.AllDatas));
    },
    NewData() {
      let _data: any = this.data;
      this.AllDatas = _data;
    },
    VerifySelected() {
      this.AllDatas = this.AllDatas.filter(
        (x: Data) => !this.Result.includes({ ...x })
      ).concat(
        this.Result.filter((x: Data) => !this.AllDatas.includes({ ...x }))
      );
      this.Save();
    },
    searchData() {
      this.Result = this.keyword
        ? this.AllDatas.filter(
            (x) =>
              this.TransformText(x.label).includes(
                this.TransformText(this.keyword)
              ) ||
              x.subline.includes(this.keyword)
          )
        : this.Result;
    },
    TransformText(text: string) {
      return text
        .normalize("NFD")
        .replace(/[^a-zA-Zs]/g, "")
        .toLowerCase();
    },
  },

  mounted() {
    const data: any = localStorage.getItem(STOREGE_KEY);
    if (localStorage.getItem(STOREGE_KEY)) {
      this.AllDatas = JSON.parse(data);
    }
  },
  created() {
    if(!this.keyword) this.NewData();
    if (this.isShow) this.VerifySelected();
  },
  beforeUpdate() {
    this.searchData();
  },
});
</script>
