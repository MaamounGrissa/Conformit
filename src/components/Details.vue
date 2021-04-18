<template>
    <div class="details-container">
      <div v-if="event">
        <form class="event-details" @submit.prevent="checkForm" method="post" >
          <h2 v-text="event.title"></h2>
          <p>Crée le {{ creationDate }} &nbsp; {{ creationTime }} par {{ event.createdBy }}</p>
          <div class="form-group">
            <label for="title">Title</label>
            <input type="text" class="form-control" v-model="event.title" name="title" id="title">
          </div>
          <div class="form-group">
            <label for="description">Description</label>
            <textarea class="form-control" id="description" name="description" rows="2" v-model="event.description" >
            </textarea>
          </div>
          <div class="datetime-container">
            <div class="form-group date">
              <label for="date">Date</label>
              <input type="date" class="form-control" v-model="creationDate" name="date" id="date">
            </div>
            <div class="form-group time">
              <label for="time">Time</label>
              <input type="time" class="form-control" v-model="creationTime" name="time" id="time">
            </div>
          </div>
          <div class="form-group">
            <label for="statut">Nom du statut</label>
            <select class="form-control" v-model="event.statusName" name="statut" id="statut">
              <option value="Open">Open</option>
              <option value="InProgress">In Progress</option>
              <option value="Closed">Closed</option>
            </select>
          </div>
          <div class="form-group">
            <label for="employe">Employé Impliqué</label>
            <select class="form-control" name="employe" id="employe">
                  <option value="Emp" selected>
                 {{ getEmploye (event.involvedEmployeeId).firstname }} &nbsp;
                 {{ getEmploye (event.involvedEmployeeId).lastname }} &nbsp;
                 ( {{ getEmploye (event.involvedEmployeeId).id }} )
                  </option>
            </select>
          </div>
          <div>
            <label for="temoins">Témoins</label>
            <div class="temoins-container">
              <div v-for="(temoin, index) in event.temoins" :key="index" class="temoin">
                 {{ temoin }}
                <span @click.prevent="onDelete(index)">X</span>
              </div>
            </div>
          </div>
          <button type="submit" class="mybtn">Submit</button>
        </form>
      </div>
    </div>
</template>

<script>
import moment from 'moment'
import { employees } from '../data.js'

export default {
  name: 'Detail',
  data () {
    return {
      creationDate: null,
      creationTime: null
    }
  },
  created () {
    this.formatDate()
  },
  updated () {
    this.formatDate()
  },
  props: ['event'],
  methods: {
    onDelete (index) {
      this.$emit('deleted', index)
    },
    checkForm () {
      console.log('event', this.event)
    },
    getEmploye (index) {
      return employees.find(x => x.id === index)
    },
    formatDate () {
      this.creationTime = moment(String(this.event.creationDate)).format('hh:mm:ss')
      this.creationDate = moment(String(this.event.creationDate)).format('YYYY-MM-DD')
    }
  }
}
</script>

<style>
  .details-container {
    height: 100%;
    overflow-y: auto;
    padding: 10px 20px 10px 0;
  }

  .event-details h2 {
    font-size: 22px;
    font-weight: 600;
    margin-bottom: 5px;
  }

  .event-details p {
    font-size: 16px;
    margin-bottom: 15px;
  }

  .event-details textarea {
    resize: none;
  }

  .datetime-container {
    display: flex;
  }

  .event-details .form-group {
    margin-bottom: 10px;
  }

  .form-group label {
    font-size: 14px;
    margin-bottom: 3px;
  }

  .form-group input, .form-group select {
    border-radius: 10px;
    border: none;
    font-size: 15px;
  }

  .form-group input:focus, .form-group select:focus {
    outline: none;
    border: 1px solid #93C5FD;
  }

  .form-group textarea {
    border-radius: 10px;
    border: none;
  }

  .form-group.date {
    width: 50%;
    margin-right: 10%;
  }

  .form-group.time {
    width: 40%;
  }

  .temoins-container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    padding: 10px;
    border: 1px solid #93C5FD;
    border-radius: 10px;
    margin-bottom: 20px;
    min-height: 60px;
  }

  .temoin {
    width: fit-content;
    padding: 2px 35px 2px 10px;
    position: relative;
    background: rgba(219, 234, 254, 0.8);
    border: 1px solid #93C5FD;
    border-radius: 10px;
    font-size: 14px;
    margin-right: 5px;
    margin-bottom: 5px;
  }

  .temoin>span {
    position: absolute;
    top: 1px;
    right: 8px;
    font-weight: bold;
    color: #60A5FA;
    font-size: 1rem;
    cursor: pointer;
  }

  .mybtn {
    float: right;
    text-align: center;
    padding: 5px 30px;
    background: rgb(78,169,228);
    background: linear-gradient(90deg,
                rgba(78,169,228,1) 0%,
                rgba(102,193,236,0.7959558823529411) 88%,
                rgba(151,205,215,1) 100%);
    border: none;
    border-radius: 10px;
  }

  @media(max-width: 767px) {
    .details-container {
      padding: 20px;
    }
  }
</style>
