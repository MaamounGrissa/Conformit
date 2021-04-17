<template>
  <div class="mycontainer">
    <div class="row">
    <div class="col-4">
      <Event :events ="events" @clicked="onSelect" />
    </div>
    <div class="col-4">
      <Detail :event ="event" />
    </div>
    <div class="col-4">
      <Comment :comments ="comments" @deleted="onDeleted" />
    </div>
  </div>
  </div>
</template>

<script>
import Event from './EventsComponent.vue'
import Detail from './Details.vue'
import Comment from './Comments.vue'
import * as dataContext from '../data.js'

export default {
  name: 'Container',
  components: {
    Event, Detail, Comment
  },
  data: function () {
    return {
      event: null,
      events: [],
      comments: []
    }
  },
  mounted: function () {
    this.events = dataContext.events
  },
  props: {
    msg: String
  },
  methods: {
    onSelect (event) {
      this.event = event
      this.comments = dataContext.GetEventComments(event.id)
    },
    onDeleted (index) {
      this.comments = this.comments.filter((item, i) => i !== index)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .mycontainer {
    width: 100vw;
    height: 100vh;
    background: #E5E7EB;
    overflow: hidden;
    padding: 5%
  }

  .row, .col-4 {
    height: 100%
  }

  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
