<template>
  <div class="container">
    <div>
      <Divider orientation="center">
        <h1> Teams ({{ total }})</h1>

      </Divider>

      <p style="margin-bottom: 10px;">
        We are creative team
      </p>
      <Loading v-if="loading"/>
      <Row v-else>
        <Col flex="1" v-for="row in rows" :key="row.id">
          <Card  style="margin: 4px;">
            <Avatar :src="$images(row.image)" size="large" />
            <h2>{{ row.name }}</h2>
            <p>{{ row.job_title}}</p>
            <template v-if="row.facebook_link">
              <NuxtLink :to="row.facebook_link">
                <Icon type="logo-facebook" size="24"/>
              </NuxtLink>
            </template>
            <template v-if="row.linkedin">
              <NuxtLink :to="row.linkedin">
                <Icon type="logo-linkedin" size="24"/>
              </NuxtLink>
            </template>
          </Card>
        </Col>
      </Row>
      <Button v-if="currentPage !== lastPage" type="primary"  style="margin-top: 5px" :loading="loading" @click="fetchData">
        <span v-if="!loading">Load More </span>
        <span v-else>Loading...</span>
      </Button>
    </div>
  </div>
</template>

<script>
import Loading from '../components/Loading';

export default {
  components: {
    Loading
  },
  data() {
    return {
      rows: [],
      loading: true,
      lastPage: 1,
      nextUrl: '',
      currentPage: 1,
      total:0
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.loading = true;
      let url =  process.env.MODULE_URL+'teams';
      if(this.nextUrl !== '' && this.nextUrl != null){
        url = this.nextUrl;
      }
      this.$axios.$get(url).then((res) => {
        if(this.rows.length > 0) {
          this.rows = [...this.rows, ...res.payload.data];
        }else {
          this.rows = res.payload.data;
        }
        this.nextUrl = res.payload.next_page_url,
          this.lastPage = res.payload.last_page,
          this.currentPage = res.payload.current_page,
          this.total = res.payload.total,
          this.loading = false
      });
    }
  }
}
</script>
