<template>
<div class="section md:section-md" id="about">
    <h1 class="section_title" v-if="custom.title">{{ custom.title }}</h1>
    <div
      class="wrapper md:wrapper-md"
    >
      <ul class="accordion">
        <AccordionItem
          v-for="(item, index) in topics"
          :key="index"
        >
          <template slot="accordion-trigger">
            <h3 class="accordion_title">{{item.title}}</h3>
          </template>
          <template slot="accordion-content">
            <span class="accordion-text" v-html="item.cooked"></span>
          </template>
        </AccordionItem>
      </ul>
    </div>
</div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import AccordionItem from "@/components/AccordionItem.vue";
export default {
  props: ["custom", "baseUrl"],
  components: {
    AccordionItem
  },
  data() {
    return {
      topics: [],
      index: null,
      Accordion: {
        count: 0,
        active: null
      }
    };
  },
  filters: {
    formatDate: function(value) {
      return moment(String(value)).format("dddd, MMMM DD YYYY");
    }
  },
  computed: {
    visible() {
      return this.index == this.Accordion.active;
    },
    cooked: function(value) {
      return value.cooked.replace(
        'class="lightbox-wrapper"',
        'class="lightbox-wrapper hidden"'
      )
    }
  },
  methods: {
    show(value) {
      return this.display.includes(value);
    },
    getTopics(value, filter) {
      axios.get(
        `${this.baseUrl}/webkit_components/topics.json?${filter}=${value}&per=500&serializer=organizer`
      ).then(({ data }) => {
        this.topics = data;
        if (this.custom.sort) {
          this.sortBy(data, this.custom.sort_by.property, this.custom.sort_by.order)
        }
      });
    },
    sortBy(data, value, order) {
      var ord_val = -1;
      if (order == 'ascending') {
        ord_val = 1;
      };
      var sorted = data.sort((a,b) => (a[value] > b[value]) ? ord_val : ((b[value] > a[value]) ? -ord_val : 0));
      this.data = sorted;
    },
    getUsers() {
      axios.get(
        `${this.baseUrl}/webkit_components/users.json?per=500`
      ).then(({ data }) => {
        this.allusers = data.filter(({ bio_raw }) => bio_raw);
        this.visibleCards = this.users;
      });
    },
    open() {
      if (this.visible) {
        this.Accordion.active = null;
      } else {
        this.Accordion.active = this.index;
      }
    },
    start(el) {
      el.style.height = el.scrollHeight + "px";
    },
    end(el) {
      el.style.height = "";
    }
  },
  created() {
    this.index = this.Accordion.count++;
    if (this.custom.tag) {
      this.getTopics(this.custom.tag, 'tags');
    }
  },
  provide() {
    return { Accordion: this.Accordion };
  }
};
</script>

<style lang="scss" scoped>

.accordion {
  list-style: none;
  margin: 0;
  padding: 0;
  width: 100%;

  &__item:last-child {
    border-bottom: none;
  }
}

.accordion__item {
  cursor: pointer;
  padding: 10px 20px 10px 40px;
  border-bottom: 1px solid #ebebeb;
  position: relative;
}

.accordion-text {
  /deep/ p {
    margin: 10px;
  }
}

.accordion__trigger {
  display: flex;
  justify-content: space-between;
}

.accordion-enter-active,
.accordion-leave-active {
  will-change: height, opacity;
  transition: height 0.3s ease, opacity 0.3s ease;
  overflow: hidden;
}

.accordion-enter,
.accordion-leave-to {
  height: 0 !important;
  opacity: 0;
}
</style>

