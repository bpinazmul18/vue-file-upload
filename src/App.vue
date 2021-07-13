<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <form @submit.prevent="sendFile" enctype="multipart/form-data">
          <div class="form-group">
            <label for="file">File</label>
            <input
              class="form-control"
              type="file"
              ref="file"
              @change="selectedFile"
              id="file"
            />
          </div>

          <div class="form-group">
            <label for="max-downlaod">Max Download</label>
            <input
              class="form-control"
              type="text"
              v-model="maxDownload"
              id="max-download"
            />
          </div>

          <div class="form-group">
            <label for="datetime">Date Time</label>
            <input
              class="form-control"
              type="datetime-local"
              v-model="datetime"
              id="datetime"
            />
          </div>

          <div class="form-group">
            <input class="btn btn-primary" type="submit" value="submit" />
          </div>

          <div class="form-group mt-4 pt-4">
            <img
              v-if="imgUrl"
              class="img-fluid"
              :src="imgUrl"
              alt="file image"
              style="width: 200px; height: 200px"
            />
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "App",
  components: {},
  data() {
    return {
      file: "",
      maxDownload: null,
      datetime: null,
      imgUrl: null,
    };
  },
  methods: {
    selectedFile() {
      this.file = this.$refs.file.files[0];
      const fileReader = new FileReader();
      fileReader.addEventListener("load", () => {
        this.imgUrl = fileReader.result;
      });
      fileReader.readAsDataURL(this.file);
    },
    async sendFile() {
      const formData = new FormData();
      formData.append("file", this.file);
      formData.append("max_download", Number(this.maxDownload));
      formData.append("expiration_date_time", moment(this.datetime).toJSON());
      formData.append("directory", null);

      // const newFile = {
      //   file: this.file,
      //   max_download: Number(this.maxDownload),
      //   expiration_date_time: moment(this.datetime).format(),
      //   directory: null,
      // };

      try {
        const res = await axios.post("api/v1/file_managers/files/", formData);
        const data = await res.json();
        console.log(data);
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
