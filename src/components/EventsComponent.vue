<template>
    <div class="events-container">
      <h3>Liste d'évennements</h3>
      <ul class="events-list">
        <li v-for="event in events" :key="event.id" :class="event.id === isActive ? 'active' : '' " @click="onSelect(event)">
          {{ event.title }}
        </li>
      </ul>
    </div>
</template>

<script>
import * as data from '../data.js'

export default {
  name: 'EventsComponent',
  props: ['events', 'isActive'],
  methods: {
    onSelect (event) {
      // Appel du fonction onSelect (in the parent component)
      this.$emit('clicked', event)
    }
  }
}
</script>

<style>
  .events-container {
    height: 100%;
    box-shadow: 0 10px 15px -3px rgba(163, 157, 157, 0.1),
                0 4px 6px -2px rgba(0, 0, 0, 0.05);
    border-radius: 20px;
    background: rgb(78,169,228);
    background: linear-gradient(90deg,
                rgba(78,169,228,1) 0%,
                rgba(102,193,236,0.7959558823529411) 100%,
                rgba(96, 158, 230, 0.555) 100%);
    padding: 30px 20px;
    overflow-y: auto;
  }

  .events-container>h3 {
    margin-bottom: 50px;
    text-align: center;
  }

  .events-list {
    list-style-type: none;
    padding: 0;
  }

  .events-list li {
    margin: 10px 0;
    padding: 8px 0;
    text-align: center;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
                0 2px 4px -1px rgba(0, 0, 0, 0.06);
    background: rgba(255,255,255,0.5);
    border-radius: 10px;
    font-weight: 500;
    cursor: pointer;
  }

  .events-list li.active {
    background: #fff;
  }

  .events-list li:hover {
    background: #fff;
  }
</style>
