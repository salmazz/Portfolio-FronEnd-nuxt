<template>
  <div>
    <Divider dashed ><h2>Service</h2></Divider>
    <Row>
      <Col span="4" v-for="row in rows" :key="row.id">
        <Card style="text-align: center" >
          <p slot="title">
            <NuxtLink :to="'/services/'+row.id">{{ row.name }}</NuxtLink>
          </p>
          <img :src="$images(row.image,'small')" alt="" width="100%">
        </Card>
      </Col>
    </Row>
  </div>
</template>
<script>
export default {
  data () {
    return {
      rows:[],
      loading:true,
      value1:0
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.loading = true;
      let url = process.env.moduleUrl + 'services';
      this.$axios.$get(url).then((res) => {
        this.rows = res.payload.data;
        this.loading = false;
      })
    }
  }
}
</script>
