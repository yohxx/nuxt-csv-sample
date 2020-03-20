<template>
  <section class="section">
    <b-field>
      <b-upload
        v-model="dropFile"
        drag-drop
      >
        <section class="section">
          <div class="content has-text-centered">
            <p>
              <b-icon
                icon="upload"
                size="is-large"
              />
            </p>
            <p>ファイルをドロップするか、ここをクリックしてアップロードするファイルを選択してください。</p>
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
            @click="deleteDropFile(index)"
          />
        </span>
      </div>
      <button class="button is-primary" @click="uploadedData()">
        読み込む
      </button>
    </div>
    <div class="table-wrapper">
      <table class="table">
        <thead>
          <tr>
            <th>氏名</th>
            <th>カタカナ</th>
            <th>生年月日</th>
            <th>会社名</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in csvData" :key="item.id">
            <td>{{ item.lastname }} {{ item.firstname }}</td>
            <td>{{ item.lastkana }} {{ item.firstkana }}</td>
            <td>{{ item.birthday }}</td>
            <td>{{ item.company }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  name: 'UploadForm',
  data () {
    return {
      dropFile: null,
      csvData: [] // csvデータ格納用
    }
  },
  methods: {
    // アップロードしたファイル削除用
    deleteDropFile (index) {
      this.dropFile = null
    },
    // アップロードしたcsvファイルをcsvDataに格納する用
    uploadedData () {
      console.log("start upload..");
      this.csvData = [] // csvDataを初期化
      const csvfile = this.dropFile
      const reader = new FileReader()

      const loadCSV = () => {
        const lines = reader.result.split('\n') // 改行毎にデータを分ける
        lines.shift() // csvファイルの先頭（ヘッダ）を削除
        // csvファイルの各行をcsvDataにオブジェクトとしてpushする
        lines.forEach((element, index) => {
          const workerData = element.split(',') // 区切り文字はカンマ
          if (workerData.length !== 6) { return } // 読み込んだ行のデータが欠けている場合は無視
          this.csvData.push(
            {
              id: index,
              lastname: workerData[0],
              firstname: workerData[1],
              lastkana: workerData[2],
              firstkana: workerData[3],
              birthday: new Date(workerData[4]),
              company: workerData[5]
            }
          )
        })
      }
      reader.onload = loadCSV
      reader.readAsText(csvfile)
    }
  }
}
</script>