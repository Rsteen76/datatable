<template>
  <div class="table-container">
    <table >
      <thead>
        <tr class="dataTable">
          <th v-for="(value, key) in userData[0]" :key="key" @click="sortBy(key), addSortedClass($event)">
            {{key}} <i class="fas fa-sort-up sorted not-active"></i><i class="sorted fas fa-sort-down not-active"></i>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(rows, index) in orderedData" :key="index" :id="index">
          <td>{{ rows.ID }}</td>
          <td>{{ rows.Name }}</td>
          <td @click="updateData" @input="rows.Description=$event.target.innerHTML" class="description-input">{{ rows.Description }}</td>
          <td>{{ moment((rows.Date)).format('MM/DD/YY') }}</td>
          <td>{{ rows.Amount }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import userData from '@/assets/briteCore.json'
  import moment from 'moment'
  var _ = require('lodash')

  export default {
    data() {
      return {
        userData: userData,
        orderDirection: 'asc',
        value: ''
      }
    },
    methods: {

      //Allow editing of the Description field
      updateData(evt) {
          evt.target.setAttribute('contenteditable', 'true')
      },

      // Initialize moment.js
      moment: function (date) {
        return moment(date);
      },

      //Sort data by user selected fields.
      sortBy(key) {
        this.value = key
        if (this.orderDirection == 'asc') {
          this.orderDirection = 'desc'
        } else {
          this.orderDirection = 'asc'
        }
      },

      //Add and remove sorting icons.
      addSortedClass(evt) {
      var columns = document.querySelectorAll('.sorted')
      var targetChild = evt.currentTarget.children
      for (var i = 0; i < columns.length; i++) {
        columns[i].classList.remove('active')
        columns[i].classList.add('not-active')
      }
        if (this.orderDirection == 'desc') {
          targetChild[0].classList.add('active')
        } else {
          targetChild[1].classList.add('active')
        }
      },
      },
    

    // Use lodash to order data
    computed: {
      orderedData: function () {
        return _.orderBy(this.userData, [this.value],[this.orderDirection])
      }
    }
  }
</script>
<style>

.table-container {
  max-width: 90vw;
  margin: 0 auto;  
  align-content: center;
}

table {
  width: 100%;
  border-radius: 15px;
  overflow: hidden;
}

th {
  background-color: lightgray;
  white-space: nowrap;
}

table, th, td {
  border: 2px solid gray;
  border-collapse: collapse;
  margin: 0;
}

tr:nth-child(even) {
  background-color: #eee;
}

th {
  cursor: pointer;
  padding: 10px;
}

td {
  cursor: default
}

.not-active {
  color: black;
  opacity: .2;
}

.active {
  color:black;
  opacity: 1;
}

.description-input {
  cursor: text;
}
</style>
