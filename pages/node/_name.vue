<template>
  <section class="container">
    <mu-card class="node-card">
      <mu-card-header :title="node.title" :subTitle="node.header">
        <mu-avatar :src="node.avatar_normal | image" slot="avatar"/>
      </mu-card-header>
      <mu-card-actions>
        <div class="chip-container">
          <mu-chip class="chip" backgroundColor="amberA100">
            <mu-avatar :size="32" icon="star" backgroundColor="orangeA400" />{{ node.stars }}
          </mu-chip>
          <mu-chip class="chip" backgroundColor="blue300">
            <mu-avatar :size="32" icon="schedule" backgroundColor="indigo900" />{{ node.created | format }}
          </mu-chip>
        </div>
      </mu-card-actions>
    </mu-card>
    <topic-list :topicList="topicList" />
  </section>
</template>

<script>
import axios from 'axios'
import TopicList from '~components/TopicList'
import { format, image } from '~plugins/filters'

export default {
  asyncData ({ app, params, error }) {
    return axios.all([
      app.$axios.get(`nodes/show.json?name=${params.name}`),
      app.$axios.get(`topics/show.json?node_name=${params.name}`)
    ])
    .then(axios.spread(function (node, topicList) {
      return {
        node: node.data,
        topicList: topicList.data
      }
    }))
    .catch(error => console.log(error))
  },
  components: {
    TopicList
  }
}
</script>

<style lang="scss">
.node-card {
  margin: -10px;
}
</style>
