<template>
  <v-container text-xs-center>
    <v-layout row>
      <v-dialog v-model="loading" persintent fullscreen>
        <v-container fill-height>
          <v-layout row justify-center align-center>
            <v-progress-circular indeterminate :size="70" :width="7" color="secondary"></v-progress-circular>
          </v-layout>
        </v-container>
      </v-dialog>
    </v-layout>
    <v-flex xs12>
      <v-carousel v-if="!loading && posts.length > 0" v-bind="{ 'cycle': true }" interval="3000">
        <v-carousel-item
          @click.native="goToPost(post._id)"
          v-for="post in posts"
          :key="post._id"
          :src="post.imageUrl"
        >
          <h2 id="carousel__title">{{post.title}}</h2>
        </v-carousel-item>
      </v-carousel>
    </v-flex>
  </v-container>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "home",
  methods: {
    handleGetCarouselPosts() {
      //reachs out Vuex, fires action
      this.$store.dispatch("getPosts");
    },
    goToPost(postId) {
      this.$router.push(`/posts/${postId}`);
    }
  },
  computed: {
    //mapping getters to computed properties
    ...mapGetters(["loading", "posts"])
  },
  created() {
    this.handleGetCarouselPosts();
  }
};
</script>
<style>
#carousel__title {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.3);
  color: white;
  border-radius: 5px 5px 0 0;
  padding: 0.5em;
  /* margin: 0 auto; */
  bottom: 50px;
  left: 0;
  right: 0;
  text-align: center;
}
</style>
