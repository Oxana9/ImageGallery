<template>
  <div id='app'>
    <h1>Gallery</h1>
    <label>Load your image</label>
    <br>
    <b-form-file class='inp' :state="Boolean(file)" accept='image/*' @change="processFile($event)"></b-form-file>
    <b-form-input class='inp' type='text' placeholder='Image name' v-model='newImageName'></b-form-input>
    <b-button v-on:click='addImage'>Add</b-button>
    <br>
    <br>
    <p>Sort the images</p>
    <b-form-input class='inp' type='search' placeholder='Enter the image name' v-model='imageFilter'></b-form-input>
    <span v-if='imageFilter'>
      {{filteredList.length}}
    </span>
    <table v-if="tableVisible">
      <tr v-for='image in filteredList'>
        <td>
          <img :src='getImageSrc(image.src)'>
          <div v-on:click="hideTooltip(image)">
            <div v-on:click.stop v-if="image.show_tooltip">
              <input type="text" v-model="image.name" @keyup.enter='toggleTooltip(image)' />
            </div>
            <span v-on:click.stop="toggleTooltip(image)">{{image.name}}</span>
          </div>
        </td>
      </tr>
    </table>


  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      newImage: '',
      newImageName: '',
      images: [],
      imageFilter: ''
    }
  },
  methods: {
    addImage: function() {
      if (this.newImage && this.newImageName) {
        this.images.push({src:this.newImage, name:this.newImageName, show_tooltip: false});
        this.newImage = '';
        this.newImageName = '';
      }
    },
    tableVisible: function() {
      return this.images.length > 0;
    },
    processFile(event) {
      this.newImage = event.target.files[0]
    },
    getImageSrc: function(fileObj) {
      return window.URL.createObjectURL(fileObj);
    },
    hideTooltip: function(image){
      image.show_tooltip = false;
    },
    toggleTooltip: function(image){
      image.show_tooltip = !image.show_tooltip;
    }
  },
  computed:{
    filteredList: function(){
      var img = this.imageFilter;
      return this.images.filter(function (elem) {
        if(img === '') return true;
        else return elem.name.indexOf(img) > -1;
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

img {
  max-width: 50%;
}

table {
  width: 100%;
}

.inp {
  width: 50%;
  margin-left: auto;
  margin-right: auto;
}
</style>
