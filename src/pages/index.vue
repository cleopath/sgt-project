<template>
  <div class="section main-container">
    <Header />
    
    <main>
      <Section1 />
      
      <Section2 />
    </main>
    
    <Etc />
  </div>
</template>

<script>
// Import semua bagian website di sini
import Header from '../components/header.vue'
import Section1 from '../components/section-1.vue'
import Section2 from '../components/section-2.vue'
import Etc from '../components/etc.vue'

export default {
  name: 'HomePage',
  components: {
    Header,
    Section1,
    Section2,
    Etc
  }
}
</script>

<style scoped>
.section {
  min-height: 100vh;
  width: 100%;
  /* Optional: padding, background, dsb */
}
</style>