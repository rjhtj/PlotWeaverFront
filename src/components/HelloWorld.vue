<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div v-if="loading">加载中...</div>
    <div v-else-if="error" class="error">请求出错：{{ error }}</div>
    <div v-else class="result">
      <pre>{{ apiData }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
  ,data() {
    return {
      apiData: null,
      loading: true,
      error: null
    }
  },
  mounted() {
    fetch('/api')
      .then(res => {
        if (!res.ok) throw new Error(res.status + ' ' + res.statusText)
        return res.text().then(text => {
          try {
            return JSON.parse(text)
          } catch (e) {
            return text
          }
        })
      })
      .then(data => {
        this.apiData = data
        this.loading = false
      })
      .catch(err => {
        this.error = err.message || String(err)
        this.loading = false
      })
  }
}
</script>

<style scoped>
.hello {
  padding: 16px;
}
.error {
  color: #e54d42;
}
.result {
  white-space: pre-wrap;
  background: #f6f8fa;
  padding: 12px;
  border-radius: 6px;
  margin-top: 8px;
}
</style>
