<template>
  <div class="column">
    <b-field>
      <b-upload v-model="dropFile" drag-drop>
        <section class="section">
          <div class="content has-text-centered">
            <p>
              <b-icon icon="upload" size="is-large" />
            </p>
            <p>
              ファイルをドロップするか、ここをクリックしてアップロードするファイルを選択してください。
            </p>
          </div>
        </section>
      </b-upload>
    </b-field>
    <div v-if="dropFile" class="block">
      <div class="tags">
        <span class="tag is-primary">
          {{ dropFile.name }}
          <button
            class="delete is-small"
            type="button"
            @click="deleteDropFile()"
          />
        </span>
      </div>
      <button class="button is-primary" @click="uploadedData()">
        読み込む
      </button>
    </div>
    <div v-if="dropFile" class="table-wrapper">
      <table class="table">
        <thead>
          <tr>
            <th>管理番号</th>
            <th>hoge</th>
            <th>fuga</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in csvData" :key="item.id">
            <td>{{ item.kanri_id }}</td>
            <td>{{ item.hoge }}</td>
            <td>{{ item.fuga }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import iconv from "iconv-lite";

export default {
  data() {
    return {
      dropFile: null,
      csvData: [] // csvデータ格納用
    };
  },
  methods: {
    // アップロードしたファイル削除用
    deleteDropFile() {
      console.log("start drop file..");
      this.dropFile = null;
    },
    // アップロードしたcsvファイルをcsvDataに格納する用
    uploadedData() {
      console.log("start upload..");
      this.csvData = []; // csvDataを初期化
      const csvfile = this.dropFile;
      const reader = new FileReader();

      const loadCSV = () => {
        const sjisCsv = iconv.decode(reader.result, "Shift_JIS");
        const parsedCsv = this.$papa.parse(sjisCsv);
        const lines = parsedCsv.data;
        lines.shift(); // csvファイルの先頭（ヘッダ）を削除
        // csvファイルの各行をcsvDataにオブジェクトとしてpushする
        lines.forEach((element, index) => {
          this.csvData.push({
            id: index,
            kanri_id: element[1],
            hoge: element[5],
            fuga: element[7]
          });
        });
      };
      reader.onload = loadCSV;
      reader.readAsBinaryString(csvfile);
    },
    getCsv() {
      return this.csvData;
    }
  }
};
</script>
