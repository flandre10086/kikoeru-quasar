<template>
  <q-form @submit="onSubmit">
    <q-card class="q-ma-md">
      <q-toolbar>
        <q-toolbar-title>爬虫相关设置</q-toolbar-title>
      </q-toolbar>

      <q-list>
        <q-item style="height: 70px;"> 
          <q-item-section>
            <q-item-label>封面图片源</q-item-label>
            <q-item-label caption>默认 DLSite</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <div class="q-gutter-sm">
              <q-radio dense v-model="config.coverSource" val="DLite" label="DLite" />
              <q-radio dense v-model="config.coverSource" val="HVDB" label="HVDB" />
            </div>
          </q-item-section>
        </q-item>

        <q-item style="height: 70px;">
          <q-item-section>
            <q-item-label>标签语言</q-item-label>
            <q-item-label caption>从 DLSite 爬取的标签元数据的语言</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <div class="q-gutter-sm">
              <q-radio dense v-model="config.tagLanguage" val="zh-cn" label="简" />
              <q-radio dense v-model="config.tagLanguage" val="zh-tw" label="繁" />
              <q-radio dense v-model="config.tagLanguage" val="ja-jp" label="日" />
            </div>
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>网络请求超时时间</q-item-label>
            <q-item-label caption>默认 2000 毫秒</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.timeout"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>重新请求间隔时间</q-item-label>
            <q-item-label caption>默认 500 毫秒</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.retryDelay"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>请求最大尝试次数</q-item-label>
            <q-item-label caption>默认 5</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.retry"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>爬虫并行任务数量</q-item-label>
            <q-item-label caption>默认 16</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.maxParallelism"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>HTTP 代理服务主机 IP</q-item-label>
            <q-item-label caption>此项为空时默认为本机</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model="config.httpProxyHost"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>HTTP 代理服务端口号 </q-item-label>
            <q-item-label caption>此项为 0 时默认不使用代理</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.httpProxyPort"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>

    <q-card class="q-ma-md">
      <q-toolbar>
        <q-toolbar-title>文件夹扫描相关设置</q-toolbar-title>
      </q-toolbar>

      <q-list>
        <q-item>
          <q-item-section>
            <q-item-label>最大递归扫描深度</q-item-label>
            <q-item-label caption>默认 2</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.scannerMaxRecursionDepth"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>

    <q-card class="q-ma-md">
      <q-toolbar>
        <q-toolbar-title>Web 服务器相关设置</q-toolbar-title>
      </q-toolbar>

      <q-list>
        <q-item>
          <q-item-section>
            <q-item-label>用户验证</q-item-label>
            <q-item-label caption>是否启用用户验证</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-toggle v-model="config.auth" />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>token 过期时间</q-item-label>
            <q-item-label caption>默认 2592000 秒</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.expiresIn"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>

        <q-item>
          <q-item-section>
            <q-item-label>每页显示的音声数量</q-item-label>
            <q-item-label caption>默认 12</q-item-label>
          </q-item-section>

          <q-item-section avatar>
            <q-input
              v-model.number="config.pageSize"
              type="number"
              input-class="text-right"
              style="max-width: 100px;"
            />
          </q-item-section>
        </q-item>
      </q-list>
    </q-card>

    <div class="q-ma-lg row justify-end">
      <q-btn :loading="loading" label="保存" type="submit" color="primary" />
    </div>
  </q-form>
</template>

<script>
export default {
  name: 'Advanced',

  data () {
    return {
      config: {},
      loading: false
    }
  },

  methods: {
    requestConfig () {
      this.$axios.get('/api/config')
        .then((response) => {
          this.config = response.data.config
        })
        .catch((error) => {
          if (error.response) {
            // 请求已发出，但服务器响应的状态码不在 2xx 范围内
            if (error.response.status !== 401) {
              this.showErrNotif(error.response.data.error || `${error.response.status} ${error.response.statusText}`)
            }
          } else {
            this.showErrNotif(error.message || error)
          }
        })
    },

    onSubmit () {
      this.loading = true
      this.$axios.put('/api/config', {
        config: this.config
      })
        .then((response) => {
          this.loading = false
          this.showSuccNotif(response.data.message)
        })
        .catch((error) => {
          this.loading = false
          if (error.response) {
            // 请求已发出，但服务器响应的状态码不在 2xx 范围内
            this.showErrNotif(error.response.data.error || `${error.response.status} ${error.response.statusText}`)
          } else {
            this.showErrNotif(error.message || error)
          }
        })
    },

    showSuccNotif (message) {
      this.$q.notify({
        message,
        color: 'positive',
        icon: 'done',
        timeout: 500
      })
    },

    showErrNotif (message) {
      this.$q.notify({
        message,
        color: 'negative',
        icon: 'bug_report'
      })
    }
  },

  created () {
    this.requestConfig()
  }
}
</script>