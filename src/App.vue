<template>
  <form @submit.prevent="sendFile" enctype="multipart/form-data">
    <input type="file" ref="file" @change="selectedFile" />
    <input type="submit" value="submit" />
    <img :src="imgUrl" alt="file image" />
  </form>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      file: "",
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
    sendFile() {
      console.log("submited");
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
