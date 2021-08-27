<template>
  <div>
    
    <div class="row">
      <div class="col-7 input">
        
        <div class="row mb-3 mx-1">
          <div class="col-1 px-0">
            <label for="api-key">API KEY:</label>
          </div>
          <div class="col-5 px-0">
            <b-form-input v-model="apiKey" id="api-key" class="col-4"></b-form-input>
          </div>
          <a class="col-3" href="https://services.metricsamsi.com/v1.0/dealers/swagger/index.html" target="_blank">Reference URL</a>
        </div>
        <div class="row mb-3 mx-1">
          <div class="col-1 px-0">
            <b-form-select class="http-category" v-model="selected" :options="options"></b-form-select>
          </div>
          <div class="col-5 px-0">
            <b-form-input v-model="baseUrl" placeholder="baseURL"></b-form-input>
          </div>
          <div class="col-5 px-0">
            <b-form-input v-model="urlPath" placeholder="Enter only the relative path here, do not enter the apiKey."></b-form-input>
          </div>
          <div class="col-1 px-0">
            <b-button variant="primary" v-on:click="onSend">Send</b-button>
          </div>
        </div>
        <b-row>
          <b-col md="2">
            <label for="input-param">Parameter:</label>
          </b-col>
          <b-col md="10">
            <b-form-textarea
              id="input-param"
              placeholder="Input Parameter"
              rows="27"
              max-rows="27"
              v-model="postData"
            ></b-form-textarea>
          </b-col>
        </b-row>
      </div>
      <div class="col-5 output">
        <label>Response</label>
        <pre class="form">{{ resData | pretty}}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'PostMan',
  data () {
    return {
      baseUrl:'https://services.metricsamsi.com/v1.0/dealers/Options/',
      apiKey: '81c14de2-6891-461b-9ea6-3ed218675b8f',
      urlPath: '',
      postData: '{}',
      selected: 'GET',
      options: [
        { value: 'GET', text: 'GET' },
        { value: 'POST', text: 'POST' },
        // { value: 'PUT', text: 'PUT' },
        { value: 'PATCH', text: 'PATCH' },
        { value: 'DELETE', text: 'DELETE' }
      ],
      resData: '',
    }
  },
  filters: {
    pretty: function(value) {
      return JSON.stringify(value, null, 2);
    }
  },
  methods: {
    onSend() {
      let url = this.baseUrl + this.urlPath + '?apiKey=' + this.apiKey;
      let postData = JSON.parse(this.postData);
      this.resData = '';
      if (this.selected === 'GET') {
        axios.get(url).then((result) => {
          this.resData = result.data;
        })
        .catch(error => {
          this.resData = error;
        });
      } else if (this.selected === 'POST') {
        axios.post(url, postData).then((result) => {
          this.resData = result.data;
        })
        .catch(error => {
            this.resData = error;
        });
      // } else if (this.selected === 'PUT') {
      //   axios.put(url, postData).then((result) => {
      //     this.resData = result.data;
      //   })
      //   .catch(error => {
      //     this.resData = error;
      //   });
      } else if (this.selected === 'PATCH') {
        axios.patch(url, postData).then((result) => {
          this.resData = result.data;
        })
        .catch(error => {
          this.resData = error;
        });
      } else if (this.selected === 'DELETE') {
        axios.get(url).then((result) => {
          this.resData = result.data;
        })
        .catch(error => {
          this.resData = error;
        });
      }
    }
  }
}
</script>

<style scoped>
.http-category {
  height: 100%;
}
.output textarea {
  display: block;
}
</style>
