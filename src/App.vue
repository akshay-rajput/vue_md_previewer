<template>
  <div id="app">
    
    <i-layout>
      <i-layout-header class="_padding-0">
        <i-navbar :collapse = false>
          <i-navbar-brand>
            <span class="">
              <svg xmlns="http://www.w3.org/2000/svg" class="theme_icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
            </span> 
            <span class="logo">ZAP</span>
          </i-navbar-brand>
          <i-nav>
              <i-nav-item>
                <span class="">
                  <svg xmlns="http://www.w3.org/2000/svg" class="theme_icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                  </svg>
                </span>

                <span class="_text-black">
                  <!-- <svg xmlns="http://www.w3.org/2000/svg" class="theme_icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                  </svg> -->
                  <svg xmlns="http://www.w3.org/2000/svg" class="theme_icon" viewBox="0 0 20 20" fill="currentColor">
                    <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
                  </svg>
                </span>
              </i-nav-item>
          </i-nav>
        </i-navbar>
      </i-layout-header>
      
      <i-layout-content>
          <i-container>
            <h3 class="_margin-y-1">Open a file</h3>
            <input type="file" ref="myFile" @change="selectedFile"><br/>
  
            <p class="_margin-y-1">OR</p>
            <h3 class="_margin-y-1">Use online editor</h3>

            <i-row class="._display-flex _align-items-stretch _flex-direction-sm-row">
              <i-column sm="6">
                <label for="md_text">Your Text</label>
                <i-textarea id="md_text" class="_width-100" v-model="mdtext" placeholder="Enter your Markdown text here." rows="15"></i-textarea>
              </i-column>
              
              <i-column sm="6" class="_display-flex _flex-direction-column">
                <label for="">Markdown Preview</label>
                <vue-markdown :source="mdtext"  class="preview-box _flex-grow-1"></vue-markdown>
              </i-column>
            </i-row>

          </i-container>          
      </i-layout-content>
      
      <i-layout-footer class="_background-gray-20 _margin-top-2">
        <div class="_text-center _padding-y-1-4">
          Made by Akshay.
        </div>
      </i-layout-footer>
    </i-layout>
  
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'

export default {
  name: 'App',
  data(){
    return{
      mdtext: ''
    }
  },
  components: {
    VueMarkdown
  },
  methods: {
    selectedFile() {
      console.log('selected a file');
      console.log(this.$refs.myFile.files[0]);
      
      let file = this.$refs.myFile.files[0];
      // get extension of uploaded file
      let fileExtension = file.name.split('.').pop();
      
      if(!file || fileExtension !== 'md') {
        console.log("Invalid FILE: ", file.name);
        return;
      }
      
      // if valid md file, read the contents and display
      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload =  evt => {
        this.mdtext = evt.target.result;
      }
      reader.onerror = evt => {
        console.error(evt);
      }
      
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono&family=Poppins&family=Ranchers&display=swap');/* font-family: 'Bowlby One SC', cursive;
// font-family: 'JetBrains Mono', monospace;
// font-family: 'Poppins', sans-serif; */


$gray100 : #f3f4f6;
$gray200 : #e5e7eb;
$gray400 : #9ca3af;
$gray700 : #374151;

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Poppins', sans-serif;
  height: 100%;
  color: #333;

  .logo{
    font-family: 'Ranchers', cursive;
    font-size: 1.75rem;
    line-height: 1
    }

  h1, h2, h3, h4, h5{
    font-family: 'Poppins', sans-serif;
    font-weight: 500;
    margin-top: 1rem;
  }

  #md_text{
    font-family: 'JetBrains Mono', monospace;
  }

  .theme_icon{
    color: black;
    height: 28px;
    width: 28px;
  }
  .preview-box{
    border: 1px solid #ccc;
    padding-left: 10px;
    /* height: 100%; */
    width: 100%;
    /* height: 15rem; */
  }
}

</style>