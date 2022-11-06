<script>
import axios from 'axios'
import LabCard from './LabCard.vue'
import LabCompleteCheckbox from './LabCompleteCheckbox.vue';

export default {
    props: {
        show: Boolean,
        student: {},
    },

    async created() {
        await axios.get('http://localhost:8080/labs')
            .then(response => this.labs = response.data);
        await axios.get('http://localhost:8080/labCompletions/' + this.student.StudentCode)
            .then(response => {
                var completion;
                var labCompletionArr = [];
                for (var i = 0; i < response.data.length; i++) {
                    completion = response.data[i]
                    labCompletionArr.push(completion["LabID"])
                }
                this.labCompletions = labCompletionArr;
            })

        // console.log(this.labCompletions)
    },

    data() {
        return {
            labs: [],
            labCompletions: [],
        }
    },

    components: {
        LabCard,
        LabCompleteCheckbox,
    },

    methods: {
        async refreshLabCompletions() {
            await axios.get('http://localhost:8080/labCompletions/' + this.student.StudentCode)
                .then(response => {
                    var completion;
                    var labCompletionArr = [];
                    for (var i = 0; i < response.data.length; i++) {
                        completion = response.data[i]
                        labCompletionArr.push(completion["LabID"])
                    }
                    this.labCompletions = labCompletionArr;
                }).catch(function (error) {
                    if (error.response) {
                        console.log(error.response.data);
                        console.log(error.response.status);
                        console.log(error.response.headers);
                    } else if (error.request) {
                        console.log(error.request);
                    } else {
                        console.log('Error', error.message);
                    }
                    console.log(error.config);
                })
        },

        async submitForm(e) {
            e.preventDefault();
            const formData = Object.fromEntries(new FormData(e.target).entries());
            const responseData = {};
            responseData["StudentCode"] = this.student["StudentCode"];
            responseData["FirstName"] = formData["FirstName"];
            responseData["MiddleNames"] = formData["MiddleNames"];
            responseData["LastName"] = formData["LastName"];

            await fetch("http://localhost:8080/students/" + this.student["StudentCode"], {
                method: "PUT",
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(responseData)
            }).catch(function (error) {
                console.log("ERROR: " + error)
            });
            this.$emit('close');
            await this.$parent.$parent.$parent.refreshStudents()
            return false;
        },

        labIDCompleted(LabID) {
            return -1 !== this.labCompletions.indexOf(LabID)
        },
    },
}

</script>

<template>
    <transition name="modal">
        <div v-if="show" class="modal-mask">
            <div class="modal-wrapper" @click="$emit('close')">
                <div class="modal-container" @click.stop="">
                    <div class="modal-header">
                        <slot name="header">
                            <h1>Lab Completions: {{ student["StudentCode"] }}</h1>
                            <h4>Total Labs Complete: {{ this.labCompletions.length }}</h4>
                        </slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <template v-for="lab in labs">
                                <LabCard :lab=lab>
                                    <LabCompleteCheckbox :lab="lab" :student="student"
                                        :complete="labIDCompleted(lab.LabID)"></LabCompleteCheckbox>
                                </LabCard>
                            </template>
                        </slot>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="$emit('close')">Close</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>