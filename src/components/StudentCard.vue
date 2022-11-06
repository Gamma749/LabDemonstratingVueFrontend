<script>
import LabCompletionsModal from './LabCompletionsModal.vue'

export default {
    props: ['student'],

    data() {
        return {
            showLabCompletionsModal: false,
        }
    },

    mounted() {
        
    },

    components: {
        LabCompletionsModal
    }
};

</script>

<template>
    <div id="studentCard" class="card clickableCard" @click="showLabCompletionsModal = true">
        <h2 class="studentItem">{{ student["StudentCode"] }}</h2>
        <h2 class="studentItem">{{ student["LastName"] }}, {{ student["FirstName"] }} {{ student["MiddleNames"] }}</h2>
        <div id="endComponent">
            <slot></slot>
        </div>
        <teleport to="body">
            <LabCompletionsModal :student="student" :show="showLabCompletionsModal"
                @close="showLabCompletionsModal = false"></LabCompletionsModal>
        </teleport>
    </div>
</template>

<style scoped>
#studentCard {
    grid-template-columns: 15% 1fr 15%;
    grid-template-rows: 1fr;
}
</style>