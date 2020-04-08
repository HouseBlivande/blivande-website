<template>
  <div class="section md:section-md" id="users">
    <div class="section_title">
      {{ custom.title }}
    </div>
    <div class="wrapper md:wrapper-md" v-if="view == 'cards'">
      <Stack
        v-if="$mq == 'sm'"
        :cards="visibleCards"
        @cardAccepted="handleCardAccepted"
        @cardRejected="handleCardRejected"
        @cardSkipped="handleCardSkipped"
        @hideCard="removeCardFromDeck"
        :key="componentKey"
      />

      <Row v-else :users="allusers" />
    </div>

    <div
      class="wrapper md:wrapper-md"
      v-if="allusers && (view == 'list') | (view == 'grid')"
    >
      <div class="user_list md:user_list-md" v-if="view == 'list'">
        <ul>
          <li
            v-for="(item, index) in allusers"
            :key="index"
            @click="setActive(index)"
            :class="{ active: selected === index }"
          >
            <div
              class="user_avatar"
              :style="{ backgroundImage: 'url(' + item.avatar_url + ')' }"
            ></div>
            <p v-if="item.name">{{ item.name }}</p>
            <p v-else>{{ item.username }}</p>
          </li>
        </ul>
      </div>

      <div class="w-full px-6" v-if="allusers[selected]">
        <a
          class="user_name"
          :href="'https://forum.blivande.com/u/' + allusers[selected].username"
        >
          <span class="mr-1">
            {{ allusers[selected].name }}
          </span>
          <span v-if="allusers[selected].name" class="font-normal text-lg"
            >@{{ allusers[selected].username }}</span
          >
          <span v-else> @{{ allusers[selected].username }} </span>
        </a>
        <div v-if="allusers[selected].bio_raw" class="user_bio md:user_bio-md" v-html="allusers[selected].bio_raw"></div>
        <div v-else class="user_bio md:user_bio-md">
          <p>No biography for this community member yet, but you can click their name to see their profile of on the forum. Members can update their profiles by going to <a :href="'https://forum.blivande.com/u/' + allusers[selected].username + '/summary' ">their profile</a>.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Stack from "@/components/Stack.vue";
import Row from "@/components/Row.vue";
export default {
  props: ["custom", "baseUrl"],
  data() {
    return {
      selected: 0,
      view: "list",
      visibleCards: [],
      allcards: [],
      componentKey: 0,
      bio: null,
      allusers: null,
      memberusers: null,
      filtered_users: false
    };
  },
  components: { Stack, Row },
  methods: {
    toggleView(view) {
      this.view = view;
    },
    forceRerender() {
      this.componentKey += 1;
    },
    handleCardAccepted() {
      window.console.log("handleCardAccepted");
    },
    handleCardRejected() {
      window.console.log("handleCardRejected");
    },
    handleCardSkipped() {
      window.console.log("handleCardSkipped");
    },
    removeCardFromDeck() {
      this.visibleCards.shift();
      if (this.visibleCards.length == 0) {
        const newCards = this.filtered_users.slice();
        this.visibleCards = newCards;
        this.forceRerender();
      }
    },
    getUsers() {
      axios.get(
        `${this.baseUrl}/g/blivande-members/members.json?limit=500`
      ).then(({ data }) => {
        this.members = data.members
        axios.get(
          `${this.baseUrl}/webkit_components/users.json?per=500`
        ).then(({ data }) => {
          this.allusers = data.filter(user => this.members.find(member => user.id === member.id))
          this.sortBy(this.allusers, 'last_seen_at', 'descending')
          this.visibleCards = this.users;
        });
      });
    },
    setActive(index) {
      this.selected = index;
    },
    sortBy(data, value, order) {
      var ord_val = -1;
      if (order == 'ascending') {
        ord_val = 1;
      };
      var sorted = data.sort((a,b) => (a[value] > b[value]) ? ord_val : ((b[value] > a[value]) ? -ord_val : 0));
      this.data = sorted;
    }
  },
  mounted: function() {
    this.getUsers(this.custom.category);
  }
};
</script>
<style lang="scss">
@import "../assets/index.scss";
</style>
