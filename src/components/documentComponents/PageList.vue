<template>
  <div class="list">
    <ul ref="unorderedList" v-if="listType === 'unordered'" :class="classList">
    </ul>
    <ol ref="orderedList" v-if="listType === 'ordered'" :class="classList">
    </ol>
  </div>
</template>

<script>
export default {
  name: 'PageList',
  props: {
    listType: {
      type: String,
      required: true,
    },
    listComponents: {
      type: Array,
      required: true,
    },
    pageTheme: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      classList: [ this.pageTheme ],
      initialized: false,
    };
  },
  mounted() {
    if (!this.initialized) {
      const list = this.listType === 'unordered' ? this.$refs.unorderedList : this.$refs.orderedList;

      for (let i = 0; i < this.listComponents.length; i++) {
        if (this.listComponents[i].$el.classList.contains('list')) {
          list.appendChild(this.listComponents[i].$el);
        } else {
          if (i === 0) {
            this.listComponents[i].classList.push('first');
          }

          this.listComponents[i].classList.push('list-item');

          if (i === this.listComponents.length - 1) {
            this.listComponents[i].classList.push('last');
          }

          const listItem = document.createElement('li');
          listItem.appendChild(this.listComponents[i].$el);
          list.appendChild(listItem);
        }
      }
      this.initialized = true;
    }
  },
};
</script>

<style lang="scss" scoped>
.list {
  margin-bottom: 10px;

  ul, ol {
    margin-bottom: 0;

    ::v-deep li {
      font-size: 9pt;  
    }
  }

  ::v-deep .list {
    margin-bottom: 0;
  }
}
</style>
