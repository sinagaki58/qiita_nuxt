<template>
  <div>
    <template v-if="lists.length === 0 && !isLoading">
      <i class="el-icon-warning">&nbsp;No results found for your keyword.</i>
    </template>
    <template v-else>
      <el-col
        v-for="(element, index) in lists"
        :key="index"
        :span="6"
        class="col-style"
      >
        <el-card :body-style="{ padding: '15px' }" class="box-card">
          <div slot="header">
            <a :href="element.url" target="_blank">{{ element.title }}</a>
          </div>
          <div class="bottom content-style text">
            <div>{{ element.created_at | formatDate }}</div>
            <span>
              <img
                :src="element.user.profile_image_url"
                width="15"
                height="15"
              />
              <template v-if="element.user.description">
                <el-popover
                  slot="description"
                  placement="top-start"
                  width="300"
                  trigger="hover"
                  :content="element.user.description"
                >
                  <span slot="reference">&nbsp;{{ element.user.id }}</span>
                </el-popover>
              </template>
              <template v-else>
                <span>&nbsp;{{ element.user.id }}</span>
              </template>
            </span>
            &nbsp;
            <span>
              <i class="el-icon-star-off">{{ element.likes_count }}</i>
            </span>
            <div>{{ element.body | description }}</div>
            <el-tag
              v-for="(tag, index) in element.tags"
              :key="index"
              size="mini"
              type="info"
              class="tab-style"
              >{{ tag.name }}</el-tag
            >
          </div>
        </el-card>
      </el-col>
      <!-- ページトップへスクロール用のボタン -->
      <div v-if="250 < scrollY" class="page-component-up">
        <transition name="fade">
          <i class="el-icon-caret-top" @click="scrollTop" />
        </transition>
      </div>
    </template>
  </div>
</template>

<script lang="babel">
import { mapState } from "vuex";
export default {
  data() {
    return {
      scrollY: 0
    }
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll)
  },
  computed: mapState(['lists', 'isLoading']),
  methods: {
    handleScroll: () => {
      this.scrollY = window.scrollY
    },
    scrollTop: () => {
      document.body.scrippTop = 0
      document.documentElement.scrippTop = 0
    }
  }
}
</script>

<style>
.content-style {
  line-height: 30px;
  font-size: 14px;
}

.tab-style {
  margin-right: 5px;
}

.box-card {
  height: 360px;
  font-size: 15px;
}

.col-style {
  padding: 10px;
}

.page-component-up {
  background-color: #59bb0c;
  position: fixed;
  right: 80px;
  bottom: 80px;
  width: 40px;
  height: 40px;
  border-radius: 20px;
  cursor: pointer;
  transition: 0.3s;
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.12);
}

.page-component-up i {
  color: #fff;
  display: block;
  line-height: 40px;
  text-align: center;
  font-size: 18px;
}

a:link,
a:visited {
  color: #59bb0c;
}

a:hover {
  color: #3b8070;
}
</style>
