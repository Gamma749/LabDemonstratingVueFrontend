<script>
import StudentEditModal from './StudentEditModal.vue'

export default {
    props: {
        student: {},
    },

    components: {
        StudentEditModal
    },

    data() {
        return {
            showModal: false,
        }
    },

    methods: {
        clicked(event) {
            event.stopPropagation();
            this.showModal = true;
        }
    }
};
</script>

<template>
    <button @click="clicked">Edit</button>
    <teleport to="body">
        <StudentEditModal :student=student :show="showModal" @close="showModal = false"/>
    </teleport>
</template>