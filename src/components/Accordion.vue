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
        'class="lightbox-wrapper hidden"',
        'class="meta"'
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
        if (this.custom.sort_by) {
          this.sortBy(this.topics, this.custom.sort_by.property, this.custom.sort_by.order)
        }
      });
    },
    sortBy(data, value, order) {
      var ord_val = -1;
      if (order == 'ascending') {
        ord_val = 1;
      };
      var sorted = this.topics.sort((a,b) => (a[value] > b[value]) ? ord_val : ((b[value] > a[value]) ? -ord_val : 0));
      this.topics = sorted;
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
  /deep/ p ul h1 h2 h3 a {
    margin: 10px;
  }
  /deep/ h3 {
    @apply border border-black text-base font-heading px-4 py-3 mt-6 leading-normal;
    margin: 10px;
    font-family: "Raleway";
    font-weight: 700;
    width: 130px;
    text-align: center;
    a {
    text-decoration: none;
    }
  }
  /deep/ li {
    margin-left: 30px;
    list-style-type: circle;
  }
  /deep/ video {
    margin: 30px 0px 30px 0px;
    max-width: 400px;
  }
  /deep/ img {
    margin: 30px 0px 30px 0px;
  }
  /deep/ a {
    text-decoration: underline;
    text-decoration-color: orange;
  }
  /deep/ .meta {
    display: none;
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

