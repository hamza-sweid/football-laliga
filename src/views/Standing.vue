<template>
  <div>
    <bread-crumb></bread-crumb>
    <standing-header></standing-header>
    <standing-team
      v-for="data in event.standing"
      :key="data.squad_name"
      :data="data"
    ></standing-team>
  </div>
</template>

<script>
import { mapState } from "vuex";
import store from "@/store/index";

import BreadCrumb from "@/components/BreadCrumb.vue";
import StandingHeader from "@/components/standing-components/StandingHeader.vue";
import StandingTeam from "@/components/standing-components/StandingTeam.vue";

function getPageEvents(routeTo, next) {
  store
    .dispatch("event/getStanding")

    .then(() => {
      next(); // continue on to create and render our component
    })
    .catch((error) => {
      if (error.response && error.response.status == 404) {
        next({ name: "404", params: { resource: "event" } });
      } else {
        next({ name: "network-issue" });
      }
    }); //this param when entering any 'not exist event'
}

export default {
  props: {},
  components: {
    BreadCrumb,
    StandingHeader,
    StandingTeam
  },
  data() {
    return {};
  },

  beforeRouteEnter(routeTo, from, next) {
    getPageEvents(routeTo, next);
  },

  beforeRouteUpdate(routeTo, routeFrom, next) {
    getPageEvents(routeTo, next);
  },

  computed: {
    ...mapState(["event"])
  }
};
</script>

<style scoope></style>
