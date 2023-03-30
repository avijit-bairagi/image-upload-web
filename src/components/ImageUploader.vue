<template>
  
  <h1>Select images to upload.</h1>
    <div>
      <input ref="imageInputId" type="file"  accept="image/png, image/jpg, image/jpeg" multiple @change="onFileChange">
      <button @click="uploadImages">Upload</button>
    </div>
</template>
  
<script>
  import axios from 'axios';
  import Constants from '../constants'

  export default {
    data() {
      return {
        files: [],
      };
    },
    methods: {
      onFileChange(event) {
        this.files = event.target.files;
      },
      async uploadImages() {

        if (this.files.length < 1) {
          alert("Please select file(s) to upload.");
          return;
        }

        const formData = new FormData();
        for (let i = 0; i < this.files.length; i++) {
          formData.append('images', this.files[i]);
        }
        try {
          const response = await axios.post(Constants.SERVER_API_LINK + '/upload/images', formData);
          this.$refs["imageInputId"].value = "";
          console.log(response);
          var data = response.data;
          if(data.code == "000") {
            this.$swal(  'Success!',data.message,'success');
          } else{
            this.$swal('Canceled!',data.message,'error');
          }
        } catch (error) {
          console.error(error);
          this.$swal('Canceled!',error,'error');
        }
      },
    },
  };
</script>
  