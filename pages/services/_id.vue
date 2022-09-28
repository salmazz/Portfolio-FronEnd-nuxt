<template>
  <div class="container">
    <div>
      <Loading v-if="loading"/>
      <Row v-else>
        <Col flex="1">
          <Card  style="margin: 4px;">
            {{ rows.name }}
            <img :src="$images(rows.image,'org')" alt="" width="288">
            <p v-html="rows.description"></p>
          </Card>
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
      let url = process.env.moduleUrl + 'services/' + this.$route.params.id;
      this.$axios.$get(url).then((res) => {
        this.rows = res.payload;
        this.loading = false;
      })
    }
  }

}
</script>
