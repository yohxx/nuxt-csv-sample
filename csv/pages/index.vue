<template>
  <section class="section">
    <div class="download">
      <b-button type="is-warning" @click="calculation()">
        計算
      </b-button>
      <vue-json-to-csv :json-data="[]">
        <b-button type="is-success">
          ダウンロード
        </b-button>
      </vue-json-to-csv>
    </div>
    <div class="columns">
      <csv-uploader ref="allItem" />
      <csv-uploader ref="excludeItem" />
    </div>
  </section>
</template>

<script>
import Vue from "vue";
import VueJsonToCsv from "vue-json-to-csv";
import CsvUploader from "~/components/CsvUploader.vue";
Vue.use(VueJsonToCsv);

export default {
  name: "UploadForm",
  data() {
    return {
      resultCsv: [], // 計算結果データ
      allItemCsv: [], // csvデータ格納用
      excludeItemCsv: [] // csvデータ格納用
    };
  },
  components: {
    VueJsonToCsv,
    CsvUploader
  },
  methods: {
    downloadCsv() {
      console.log("Download CSV");
    },
    calculation() {
      console.log("Calculation..");
      this.allItemCsv = this.$refs.allItem.getCsv();
      this.excludeItemCsv = this.$refs.excludeItem.getCsv();
      let items = [];
      this.allItemCsv.forEach(item => {
        if (item.kanri_id == undefined) {
          return;
        }
        items[item.kanri_id] = item;
      });

      this.excludeItemCsv.forEach(element => {
        if (element.kanri_id == undefined) {
          return;
        }
        console.log(element.kanri_id);

        if (items[element.kanri_id] != undefined) {
          console.log("delete:" + element.kanri_id);
          delete items[element.kanri_id];
        }
      });
      console.log(items);
      this.resultCsv.push(items);
      console.log(this.resultCsv);
    }
  }
};
</script>
