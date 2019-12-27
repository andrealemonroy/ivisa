<template>
  <section>
    <Header />
    <Banner />
    <div>
      <nuxt />
    </div>
  </section>
</template>
<script>
import "../assets/css/style.css";
import Header from "@/components/Header";
import Banner from "@/components/Banner";
// import Footer from '@/components/Footer'
export default {
  name: "default",
  components: {
    Header,
    Banner
  }
};
</script>
