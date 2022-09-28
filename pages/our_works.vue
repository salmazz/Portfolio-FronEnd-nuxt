<template>
  <div class="container">
    <div>
      <Divider orientation="center">
        <h1> Our Works ({{ total }})</h1>

      </Divider>

      <p style="margin-bottom: 10px;">
        Happy for us
      </p>
      <Loading v-if="loading"/>
            <template>
              <List item-layout="vertical">
                <ListItem v-for="row in rows" :key="row.id">
                  <ListItemMeta :title="row.title" :description="row.description" />
                  {{ row.description }}
                  <template #action>
                    <li v-if="row.link">
                      <a :href="row.link">
                        <Icon type="md-eye" />
                        Visit</a>
                    </li>
                    <li>
                      {{ row.created_at }}
                      <Icon type="md-calendar" />
                    </li>
                  </template>
                  <template #extra>
                    <img :src="$images(row.image,'small')" style="width: 280px">
                  </template>
                </ListItem>
              </List>
            </template>
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
      let url =  process.env.MODULE_URL+'our_works';
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
