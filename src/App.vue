<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <TinymceEditor v-model="editor"/>
     {{editor}}
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TinymceEditor from './components/tinymce_editor/tinymceeditor.vue'

export default {
  name: 'App',
  data(){
    return{
      editor:'123'
      }   
  },
  components: {
    TinymceEditor
  }
}
</script>
