<template>
  <div class="activity-list">
    <h2>aktivity</h2>
    <div class="filters">
      <label for="show-completed">Show completed:</label>
      <input type="checkbox" id="show-completed" v-model="showCompleted">
      <label for="sort-by">Sort by:</label>
      <select id="sort-by" v-model="sortBy">
        <option value="name">Name</option>
        <option value="completed">Completed</option>
        <option value="dueDate">Due Date</option>
      </select>
    </div>
    <ul>
      <li v-for="(activity, index) in sortedActivities" :key="index">
        <input type="checkbox" v-model="activity.completed" @change="markAsCompleted(activity)">
        <span :class="{ completed: activity.completed }">{{ activity.name }}</span>
        <span v-if="activity.dueDate"> (Due: {{ activity.dueDate }})</span>
        <button @click="cancelActivity(index)">Cancel</button>
      </li>
    </ul>
    <div class="add-activity">
      <label for="new-activity-name">Name:</label>
      <input type="text" id="new-activity-name" v-model="newActivityName">
      <label for="new-activity-dueDate">Due Date:</label>
      <input type="date" id="new-activity-dueDate" v-model="newActivityDueDate">
      <button @click="addActivity">Add Activity</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activities: [
        { name: 'Go to the gym', completed: false, dueDate: '2023-03-01' },
        { name: 'Date with honey', completed: false, dueDate: '2023-03-05' },
      ],
      showCompleted: false,
      sortBy: 'name',
      editing: false,
      editingActivity: {},
      newActivityName: '',
      newActivityDueDate: ''
    }
  },
  computed: {
    sortedActivities() {
      return this.activities.slice().sort((a, b) => {
        if (this.sortBy === 'name') {
          return a.name.localeCompare(b.name)
        } else if (this.sortBy === 'completed') {
          return a.completed - b.completed
        } else if (this.sortBy === 'dueDate') {
          return new Date(a.dueDate) - new Date(b.dueDate)
        }
      })
    }
  },
  methods: {
    addActivity() {
      if (this.newActivityName.trim() === '') return
      this.activities.push({ name: this.newActivityName, completed: false, dueDate: this.newActivityDueDate })
      this.newActivityName = ''
      this.newActivityDueDate = ''
    },
    cancelActivity(index) {
      this.activities.splice(index, 1)
    },
    markAsCompleted(activity) {
      if (activity.name.trim() === '') return
      activity.completed = true
    },
    editActivity(index) {
      this.editing = true
      this.editingActivity = this.activities[index]
    },
    saveActivity() {
      this.editing = false
    },
    cancelEdit() {
      this.editing = false
    }
  }
}
</script>
<style>
.background {
  background-image: url("./mobil.jpg");
  background-position: center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
} 

.activity-list {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  background-color: #f9f9f9; /* Perubahan: Warna latar belakang sedikit lebih terang */
}

.filters {
  margin-bottom: 20px; /* Perubahan: Jarak antara bagian atas filter dan daftar kegiatan */
}

.filters label {
  margin-right: 10px;
}

.filters select {
  padding: 5px;
  border-radius: 5px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between; /* Perubahan: Membuat tombol dan teks berada di ujung kanan */
  margin-bottom: 10px;
  padding: 10px;
  background-color: #ffffff; /* Perubahan: Warna latar belakang daftar */
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Perubahan: Bayangan yang lebih ringan */
}

input[type="checkbox"] {
  margin-right: 10px;
}

.completed {
  text-decoration: line-through;
  color: #888; /* Perubahan: Warna teks yang lebih gelap */
}

button {
  padding: 5px 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #3e8e41;
}

.add-activity {
  margin-top: 20px;
}

.add-activity label {
  margin-bottom: 5px;
}

.add-activity input[type="text"],
.add-activity input[type="date"] {
  padding: 8px; /* Perubahan: Padding yang sedikit lebih kecil */
  border-radius: 5px;
  border: 1px solid #ccc; /* Perubahan: Garis yang lebih jelas */
}
</style>

