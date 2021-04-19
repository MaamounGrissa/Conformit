/**************************************************************/
/****************** EVENT DETAILS COMPONENT *******************/
/**************************************************************/
<template>
    <div class="details-container">
      <div v-if="event">
        <form class="event-details" @submit.prevent="checkForm" method="post" >
          <h2 v-text="event.title"></h2>
          <p>Crée le {{ event.creationDate }} par {{ event.createdBy }}</p>
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
              <input type="date" class="form-control" v-model="myDate" name="date" id="date">
            </div>
            <div class="form-group time">
              <label for="time">Time</label>
              <input type="time" class="form-control" v-model="myTime" name="time" id="time">
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
            <select class="form-control" v-model="event.involvedEmployeeId" name="employe" id="employe">
                  <option v-for="emp in this.emps" :key="emp.id" :value="emp.id">
                    {{ emp.firstname }} &nbsp; {{ emp.lastname }} &nbsp; ({{ emp.id }})
                  </option>
            </select>
          </div>
          <div class="form-group">
            <label for="temoins">Témoins</label>
            <div class="temoins-container">
              <div v-for="(temoin, index) in event.temoins" :key="index" class="temoin">
                 <input type="text" class="temoin-input" ref="temoin" v-model="event.temoins[index]" />
                 <span @click.prevent="onDelete(index)">X</span>
              </div>
              <div class="add-temoin" @click="addTemoin()">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
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
  props: ['event'],
  data () {
    return {
      myDate: null,
      myTime: null,
      emps: null,
      newTemoin: String
    }
  },
  created () {
    // Format Date à la création du component
    this.formatDate()
    this.emps = employees
  },
  updated () {
    // Format Date à l'update du component
    this.formatDate()
  },
  methods: {
    onDelete (index) {
      // Supprimer les témoins
      this.$emit('deleted', index)
    },
    checkForm (e) {
      // Submit Form
      console.log('Connection To API (AXIOS)')
      console.log('Send Data:')
      console.log('events', this.events)
      console.log('Comments')
    },
    getEmploye (index) {
      // l'employé impliqué
      return employees.find(x => x.id === index)
    },
    formatDate () {
      // Format Date Function
      this.myTime = moment(String(this.event.creationDate)).format('HH:mm')
      this.myDate = moment(String(this.event.creationDate)).format('YYYY-MM-DD')
    },
    addTemoin () {
      // Add New Temoin
      this.newTemoin = ''
      this.event.temoins.push(this.newTemoin)
      this.$refs.temoin[this.event.temoins.length - 1].focus()
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
    margin-left: 4px;
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

  .temoin-input {
    width: 90px;
    background: transparent;
    border: none;
    resize: none;
    padding: 1px;
    height: 25px;
  }

  .temoin-input:focus {
    outline: 0 !important;
    border: 2px dotted #93C5FD !important;
  }

  .add-temoin {
    width: 25px;
    height: 25px;
    margin-left: 5px;
    margin-top: 3px;
    cursor: pointer;
    transition: transform 0.3s ease-in-out;
  }

  .add-temoin:hover {
    transform: scale3d(1.1,1.1,1.1)
  }

  .add-temoin svg {
    width: 100%;
    height: 100%;
    color: #fff;
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

  .form-control[readonly] {
    background-color: #fff !important;
  }

  @media(max-width: 767px) {
    .details-container {
      padding: 20px;
    }
  }
</style>
