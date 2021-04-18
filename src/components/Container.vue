<template>
  <div class="mycontainer">
    <div class="row">
    <div class="col-md-4">
      <Event :events="events" :isActive="isActive" @clicked="onSelect" />
    </div>
    <div class="col-md-4">
      <Detail :event="event" @deleted="onDeletedTemoin"/>
    </div>
    <div class="col-md-4">
      <Comment :comments="comments" @deleted="onDeletedComment" />
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
  data () {
    return {
      event: null,
      events: [],
      comments: [],
      isActive: false
    }
  },
  created () {
    // Sélection du l'Event 0  ( Par Défaut )
    this.onSelect(dataContext.events[0])
    // Export l'id de l'event sélectionné
    this.isActive = dataContext.events[0].id
  },
  mounted () {
    this.events = dataContext.events
  },
  props: {
    msg: String
  },
  methods: {
    onSelect (event) {
      // Select Event Function
      this.event = event
      this.isActive = this.event.id
      this.comments = dataContext.GetEventComments(event.id)
    },
    onDeletedComment (index) {
      // Delete Comment
      this.comments = this.comments.filter((item, i) => i !== index)
    },
    onDeletedTemoin (index) {
      // Delete Temoin
      this.event.temoins = this.event.temoins.filter((item, i) => i !== index)
    }
  }
}
</script>

<style scoped>
  .mycontainer {
    width: 100vw;
    height: 100vh;
    background: #E5E7EB;
    overflow: hidden;
    padding: 5%;
  }

  .row, .col-md-4 {
    height: 100%;
  }

  @media(max-width: 767px) {
    .mycontainer {
      width: auto;
      height: auto;
    }

    .row, .col-md-4 {
      height: auto;
    }

  }
</style>
