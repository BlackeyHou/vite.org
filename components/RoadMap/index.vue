<template>
  <section>
    <h1 class="roadmap-title">{{ $t('roadmap.title')}}</h1>
    <div class="is-hidden-touch">
      <div class="container is-flex roadmap__timeline">
        <timeline :time-lines="tableList(item)" :type="item" v-for="(item, index) in tabs" :key="index"></timeline>
      </div>
    </div>

    <div class="is-hidden-desktop">
      <div class="tab-wrapper is-flex">
        <div
          v-for="(item, index) in tabs"
          :key="index"
          class="tab-content"
          :class="{'is-active': tabParams === item}"
          @click="clickTab(item)">
          {{ $t(`roadmap.${item}`) }}
        </div>
      </div>
      <div class="container is-flex roadmap__timeline roadmap__touch">
        <timeline :time-lines="list" :type="tabParams" ></timeline>
      </div>
    </div>

  </section>
</template>

<script type="text/babel">
import timeline from './timeline';

export default {
  components: {
    timeline,
  },
  data() {
    return {
      tabParams: 'inProgress',
      tabs: ['completed', 'inProgress', 'nextStep']
    };
  },
  computed: {
    list() {
      if (this.tabParams === 'completed') {
        return this.getTimelines().slice(0, 12);
      }
      if (this.tabParams === 'inProgress') {
        return this.getTimelines().slice(12, 14);
      }
      if (this.tabParams === 'nextStep') {
        return this.getTimelines().slice(14);
      }
      return [];
    },
  },
  methods: {
    tableList(item) {
      for (let i = 0; i < this.tabs.length; i++) {
        if (item === 'completed') {
          return this.getTimelines().slice(0, 12);
        }
        if (item === 'inProgress') {
          return this.getTimelines().slice(12, 14);
        }
        if (item === 'nextStep') {
          return this.getTimelines().slice(14);
        }
      }
    },
    clickTab(str) {
      this.tabParams = str;
    },
    getTimelines() {
      const roadmaps = this.$t('roadmap.timelines');
      if (!Array.isArray(roadmaps)) {
        return [];
      }
      return roadmaps.map((item) => {
        if (!Array.isArray(item.description)) {
          return {
            ...item,
            description: [item.description],
          };
        }
        return item;
      });
    },
  },
};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
@import "~assets/vars";

section {
  margin-bottom: 100px;
  .roadmap-title {
    text-align: center;
    padding-top: 109px;
    @include mobile {
      padding-top: 30px;
    }
  }
  .is-hidden-touch {
    padding-right: 150px;
  }
  .roadmap__timeline {
    margin-top: 50px;
    justify-content: space-between;
    @include touch {
      margin-top: 0px;
    }
  }
  .roadmap__touch {
    justify-content: center;
  }
  .is-active {
    color: #171C34;
  }
  .tab-wrapper {
    box-sizing: border-box;
    padding: 0 40px;
    justify-content: space-between;
  }

}
</style>
