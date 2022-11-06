<script>
import LabEditModal from './LabEditModal.vue'

export default {
    props: {
        lab: {},
    },

    components: {
        LabEditModal
    },

    data() {
        return {
            showModal: false,
        }
    },

    mounted() {
        // console.log(this.lab)
    },
};
</script>

<template>
    <button @click="showModal = true">Edit</button>
    <teleport to="body">
        <LabEditModal :lab=lab :show="showModal" @close="showModal = false" />
    </teleport>
</template>