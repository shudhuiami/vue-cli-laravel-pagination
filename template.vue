<template>
  <div class="vue-pagination-wrapper no_select"
       :class="{'vue-pagination-left': align == 'left', 'vue-pagination-right': align == 'right', 'vue-pagination-center': align == 'center'}">
    <ul class="vue-pagination">
      <li class="vue-page-item">
        <a @click="prevPage"
           :disabled="data.prev_page_url == null" class="vue-page-link" v-html="prev_btn"></a>
      </li>
      <li v-for="(page, index) in buttons" class="vue-page-item" :class="{'_active_': page.title == current_page}">
        <a @click="pageChange(page.options)" class="vue-page-link" href="#">{{ page.title }}</a>
      </li>
      <li class="vue-page-item">
        <a @click="nextPage()"
           :disabled="data.next_page_url == null" class="vue-page-link" v-html="next_btn"></a>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      buttons: [],
      current_page: 0,
      total_pages: 0,
      button_limit: 5,
      start: 0,
      end: 5,
      prev_btn: 'Previous',
      next_btn: 'Next',
    }
  },
  name: 'App',
  props: ['data', 'align', 'onChange', 'buttonLimit', 'prevBtn', 'nextBtn'],
  components: {},
  methods: {
    prevPage() {
      if (this.data.prev_page_url != null) {
        if (this.start == this.current_page - 1) {
          if (parseInt(this.current_page) > 1) {
            this.current_page = this.start;
            this.start = this.start - this.button_limit;
            let options = {page: this.current_page}
            this.onChange(options)
            this.reRenderButtons()
          }
        } else {
          let options = {
            page: 1
          }
          if (parseInt(this.current_page) > 1) {
            options.page = parseInt(this.current_page) - 1
            this.current_page = options.page;
            this.onChange(options)
          }
        }
      }
    },
    nextPage() {
      if (this.data.next_page_url != null) {
        if (this.end == this.current_page) {
          if (this.current_page < this.total_pages) {
            this.current_page = this.start + this.button_limit + 1;
            this.start = this.start + this.button_limit;
            let options = {page: this.current_page}
            this.onChange(options)
            this.reRenderButtons()

          }
        } else {
          let options = {page: 1}
          if (this.current_page < this.total_pages) {
            options.page = parseInt(this.current_page) + 1
            this.current_page = options.page;
            this.onChange(options)
          }
        }

      }
    },
    pageChange(options) {
      if (options.cross_limit == 1) {
        this.current_page = this.start + this.button_limit + 1;
        this.start = this.start + this.button_limit;
        this.onChange(options)
        this.reRenderButtons()
      } else if (options.cross_limit == -1) {
        this.current_page = this.start;
        this.start = this.start - this.button_limit;
        this.onChange(options)
        this.reRenderButtons()
      } else {
        this.current_page = options.page;
        this.onChange(options)
      }
    },
    initButtons() {
      let pageCount = Math.ceil((this.data.total / this.data.per_page))
      this.total_pages = pageCount;
      console.log(this.total_pages)
      this.current_page = this.data.current_page;
      if (pageCount < this.button_limit) {
        for (let i = 0; i < pageCount; i++) {
          this.buttons.push({
            title: (i + 1).toString(),
            options: {
              page: (i + 1),
              cross_limit: 0
            }
          })
        }
      } else {
        for (let i = 0; i < pageCount; i++) {
          if (i < this.button_limit) {
            this.buttons.push({
              title: (i + 1).toString(),
              options: {
                page: (i + 1),
              }
            })
          }
        }
        this.buttons.push({
          title: '...',
          options: {
            page: this.end + 1,
            cross_limit: 1
          }
        })
      }
    },
    reRenderButtons() {
      this.buttons = [];
      let pageCount = parseInt(this.start) + parseInt(this.button_limit)
      if (pageCount > this.total_pages) {
        pageCount = this.total_pages;
      }
      let start_loop_point = this.start;
      if (start_loop_point > 0) {
        this.buttons.push({
          title: '...',
          options: {
            page: this.current_page - 1,
            cross_limit: -1
          }
        });
      }
      this.end = pageCount;
      for (let i = start_loop_point; i < pageCount; i++) {
        this.buttons.push({
          title: (i + 1).toString(),
          options: {
            page: (i + 1),
          }
        })
      }
      if (this.total_pages > pageCount) {
        this.buttons.push({
          title: '...',
          options: {
            page: this.end + 1,
            cross_limit: 1
          }
        })
      }
    }
  },
  created() {
    if (this.prevBtn !== undefined) {
      this.prev_btn = this.prevBtn;
    }
    if (this.nextBtn !== undefined) {
      this.next_btn = this.nextBtn;
    }

    this.button_limit = parseInt(JSON.stringify(JSON.parse(this.buttonLimit)))
    this.end = parseInt(JSON.stringify(JSON.parse(this.buttonLimit)))
    this.initButtons()
  }
}
</script>
<style>
.vue-pagination-wrapper {
  width: 100%;
  display: inline-block;
  text-align: center;
  padding: 20px 0;
}

.vue-pagination-left {
  text-align: left !important;
}

.vue-pagination-right {
  text-align: right !important;
}

.vue-pagination-center {
  text-align: center !important;
}

.vue-pagination-wrapper .vue-pagination {
  display: inline-block;
  margin: 0;
  padding: 0;
}

.vue-pagination-wrapper .vue-pagination .vue-page-item {
  float: left;
  list-style: none;
  cursor: pointer;
}

.vue-pagination-wrapper .vue-pagination .vue-page-item .vue-page-link {
  color: #222 !important;
  background-color: #fff;
  border: 1px solid #dfdfdf;
  cursor: pointer;
  padding: 8px 14px;
  float: left;
}

.vue-pagination-wrapper .vue-pagination ._active_ .vue-page-link {
  color: #fff !important;
  background-color: #6d78fa;
  border: 1px solid #6d78fa;
}

.no_select {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
  /* Non-prefixed version, currently
                                   supported by Chrome, Edge, Opera and Firefox */
}
</style>
