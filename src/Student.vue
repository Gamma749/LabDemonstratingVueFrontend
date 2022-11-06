<script>
import axios from 'axios'
import MyHeader from './components/MyHeader.vue'
import StudentCard from './components/StudentCard.vue'
import StudentEditButton from './components/StudentEditButton.vue'
import StudentCreateModal from './components/StudentCreateModal.vue'

export default {

  components: {
    MyHeader,
    StudentCard,
    StudentEditButton,
    StudentCreateModal,
  },

  data() {
    return {
      search: "",
      allStudents: [],
      showCreateModal: false,
      showLabCompletionsModal: false,
    }
  },

  methods: {
    async refreshStudents() {
      await axios.get('http://localhost:8080/students')
        .then(response => this.allStudents = response.data)
      this.allStudents.sort(function (a, b) {
        return a.LastName.localeCompare(b.LastName)
      })
    }
  },

  async created() {
    await this.refreshStudents();
  },


  computed: {
    filteredList() {
      var filteredList = this.allStudents.filter(student => {
        return student["FirstName"].toLowerCase().includes(this.search.toLowerCase()) ||
          student["MiddleNames"].toLowerCase().includes(this.search.toLowerCase()) ||
          student["LastName"].toLowerCase().includes(this.search.toLowerCase()) ||
          student["StudentCode"].toLowerCase().includes(this.search.toLowerCase())
      })
      return filteredList
    },
  },
}

</script>

<template>
  <MyHeader title="Students" />
  <button @click="showCreateModal = true" class="createButton bigButton">Create Student</button>
  <div class="search-wrapper">
    <input type="text" v-model="search" placeholder="Student..." />
  </div>
  <teleport to="body">
    <StudentCreateModal :show="showCreateModal" @close="showCreateModal = false" />
  </teleport>
  <template v-for="student in filteredList" :key="student">
    <StudentCard :student=student>
      <StudentEditButton :student="student"></StudentEditButton>
    </StudentCard>

  </template>
</template>