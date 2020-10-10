<template>
 <el-form>
   <span class="demonstrate" v-for="blog in blogs" :key="blog.id">{{blog.title}}</span>
   <el-input
  type="textarea"
  :rows="5"
  placeholder="Please input"
  v-model="textarea">
</el-input>
 </el-form>

</template>

<script>
  export default {
    name: 'HelloWord',
  props: ['blogs'],
     data() {
    return {
      textarea: ''
    }
  }
     
   }

</script>

<style>


</style>