<script>
export default {
    props: {
        show: Boolean,
        student: {},
    },

    methods: {
        async submitForm(e) {
            e.preventDefault();
            const formData = Object.fromEntries(new FormData(e.target).entries());
            const responseData = {};
            responseData["StudentCode"] = this.student["StudentCode"];
            responseData["FirstName"] = formData["FirstName"];
            responseData["MiddleNames"] = formData["MiddleNames"];
            responseData["LastName"] = formData["LastName"];

            await fetch("http://localhost:8080/students/"+this.student["StudentCode"], {
                method: "PUT",
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(responseData)
            }).catch(function(error) {
                console.log("ERROR: "+error);
            });
            this.$emit('close');
            await this.$parent.$parent.$parent.refreshStudents()
            return false;
        },
        async deleteStudent(){
            const resp = confirm("WARNING: Delete Student "+this.student["StudentCode"]+"?");
            if (resp === false) {
                return false;
            }

            await fetch("http://localhost:8080/students/"+this.student["StudentCode"], {
                method: "DELETE",
                headers: {
                    'Content-type': 'application/json'
                },
            }).catch(function(error) {
                console.log("ERROR: "+error);
            });

            this.$emit('close');
            await this.$parent.$parent.$parent.refreshStudents()
            return false;
        }
    },
}

</script>

<template>
    <transition name="modal">
        <div v-if="show" class="modal-mask">
            <div class="modal-wrapper" @click="$emit('close')">
                <div class="modal-container" @click.stop="">
                    <div class="modal-header">
                        <slot name="header"><h1>{{student["StudentCode"]}}</h1></slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <form id="studentModalForm" class="modal-form" autocomplete="off" v-on:submit="submitForm">
                                <label for="FirstName">First Name:</label>
                                <input type="text" name="FirstName" id="FirstName" :value="student['FirstName']" required>
                                <label for="MiddleNames">Middle Names:</label>
                                <input type="text" name="MiddleNames" id="MiddleNames" :value="student['MiddleNames']" >
                                <label for="LastName">Last Name:</label>
                                <input type="text" name="LastName" id="LastName" :value="student['LastName']"  required>
                            </form>
                        </slot>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="$emit('close')">Close</button>
                            <button :onclick="deleteStudent" class="deleteButton">Delete</button>
                            <input type="submit" form="studentModalForm" class="updateButton"  value="Update">
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>