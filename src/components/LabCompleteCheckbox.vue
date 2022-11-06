<script>


export default {
    props: {
        lab: {},
        student: {},
        complete: false,
    },

    data() {
        return {
            studentLabCompletions: [],
        }
    },

    async created() {
    },

    methods: {
        async submitLab(event) {
            if (event.target.checked) {
                // Lab marked off, checkbox CHECKED
                event.preventDefault();
                const responseData = {};
                responseData["StudentCode"] = this.student.StudentCode;
                responseData["LabID"] = this.lab.LabID;

                await fetch("http://localhost:8080/labCompletions", {
                    method: "POST",
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify(responseData)
                });
            } else {
                await fetch("http://localhost:8080/labCompletions/" + this.student.StudentCode + "/" + this.lab.LabID, {
                    method: "DELETE",
                    headers: {
                        'Content-type': 'application/json'
                    },
                });
            }
            await this.$parent.$parent.$parent.refreshLabCompletions();
            return false;
        }
    }
};
</script>

<template>
    <input type="checkbox" v-on:change="submitLab" :checked="complete">
</template>