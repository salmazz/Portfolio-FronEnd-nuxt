<template>
    <Carousel autoplay v-model="value" loop>
      <CarouselItem v-for="row in rows" :key="'slider_'+row.id">
        <div class="demo-carousel rel">
          <template>
            <h2 v-if="row.name" class="sliderTitle">{{ row.name }}</h2>
            <p v-if="row.des" class="sliderDes"> {{ row.des }}</p>
            <img :src="$images(row.image)" alt="" style="width:100%;max-height: 300px;">
            <a v-if="row.link" class="more" :href="row.link">
             More
            </a>
          </template>
        </div>
      </CarouselItem>
    </Carousel>
</template>

<script>
export default {
  data() {
    return {
      rows: [],
      value:0
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      let url =  process.env.MODULE_URL+'sliders';
      this.$axios.$get(url).then((res) => {
        console.log(res);
          this.rows = res.payload.data;
      });
    }
  }
}
</script>
<style>
.rel {
  position: relative;
}

.sliderTitle {
  position: absolute;
  z-index: 10;
  top: 20%;
  left: 10%;
  font-size: 50px;
  color: #fff;
  background: gray;
  opacity: .5;
  padding: 10px ;
  margin-bottom: 5px;
  text-transform: capitalize;
}
.sliderDes{
  position: absolute;
  z-index: 10;
  left: 10%;
  top: 50%;
  width: 500px;
  font-size: 15px;
  color: red;
  background: gray;
  margin-bottom: 5px;

  opacity: .5;
  padding: 10px ;
  text-transform: capitalize;
}

.more {
  background: lightseagreen;
  position: absolute;
  padding: 10px;
  text-align: center;
  left: 40%;
  top: 80%;
  border-radius: 5px;
  color: white;
}

.more:hover{
  color: white;
}
</style>
