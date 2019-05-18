<template>
  <el-form
    ref="searchForm"
    :inline="true"
    :modal="searchForm"
    :rules="rules"
    @submit.native.prevent
  >
    <el-form-item prop="keyword">
      <el-input
        v-model="searchForm.keyword"
        placeholder="search by =keyword"
        prefix-icon="el-icon-search"
        @keyup.enter.native="search('searchForm')"
      />
    </el-form-item>
    <el-form-item>
      <el-button @click="search('searchForm')">search</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="babel">
export default {
  data() {
    return {
      searchForm: {
        keyword:''
      },
      rules: {
        keyword: [
          {required: true, message: 'Please input the keyword', trigger: 'blur'},
          {whitespace: true, message: 'Please input the keyword', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    search(form) {
      this.$refs[form].valodate((valid) => {
        if (!valid) {
          return false
        }
        this.sendRequest()
      })
    },
    sendRequest() {
      this.$store.dispatch('getItems', {
        keyword: this.searchForm.keyword
      })
    }
  }
}
</script>

<style>
.el-form {
  margin-top: 1em;
  margin-left: 1em;
}
</style>
