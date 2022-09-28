<template>
  <div class="container">
    <div>
      <Loading v-if="loading"/>
      <Row v-else>
        <Col flex="1" style="text-align: left">
            <h1 style="margin-bottom: 20px;">{{ rows.title }}</h1>
            <img style="margin-bottom: 20px" :src="$images(rows.image,'org')" alt="" width="288">
            <p v-html=" rows.body "></p>
        </Col>
      </Row>
    </div>
  </div>
</template>

<script>
import Loading from '../../components/Loading';

export default {
  components: {
    Loading
  },
  data() {
    return {
      rows: {},
      loading: true,
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.loading = true;
      let url = process.env.moduleUrl + 'pages/' + this.$route.params.id;
      this.$axios.$get(url).then((res) => {
        this.rows = res.payload;
        this.loading = false;
      })
    }
  }

}
</script>
