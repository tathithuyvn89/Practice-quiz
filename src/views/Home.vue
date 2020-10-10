<template>
  <div class="home">
    
    <HelloWorld :blogs="bloglist"></HelloWorld>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'
export default {
  name: 'Home',
  components: {
    HelloWorld
  },

  data () {
    return{
      bloglist: [],
      errors: []
    }
  },
  
  created() {
    axios.get('http://localhost:8080/blog').then(response => {
      this.bloglist = response.data;
      console.log(this.blogs)
    }).catch( e=> {
      this.errors.push(e)
    })
  }
}
</script>
