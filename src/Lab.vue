<script>
import axios from 'axios'
import MyHeader from './components/MyHeader.vue'
import LabCard from './components/LabCard.vue'
import LabEditButton from './components/LabEditButton.vue'
import LabCreateModal from './components/LabCreateModal.vue'

export default {
  data() {
    return {
      labs: {},
      showCreateModal: false,
    }
  },

  components: {
    MyHeader,
    LabCard,
    LabEditButton,
    LabCreateModal,
  },

  methods: {
    async refreshLabs() {
      await axios.get('http://localhost:8080/labs')
        .then(response => this.labs = response.data)
      // console.log(this.labs)
    },

  },

  async created() {
    await this.refreshLabs();
  },
}

</script>

<template>
  <MyHeader title="Labs" />
  <button @click="showCreateModal = true" class="createButton bigButton">Create Lab</button>
  <teleport to="body">
    <!-- use the modal component, pass in the prop -->
    <LabCreateModal :show="showCreateModal" @close="showCreateModal = false" />
  </teleport>
  <template v-for="lab in this.labs">
    <LabCard :lab=lab>
      <LabEditButton :lab=lab></LabEditButton>
    </LabCard>
  </template>
</template>