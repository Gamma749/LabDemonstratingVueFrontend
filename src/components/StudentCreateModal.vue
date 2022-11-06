<script>
export default {
    props: {
        show: Boolean,
    },

    methods: {
        async submitForm(e) {
            e.preventDefault();
            const formData = Object.fromEntries(new FormData(e.target).entries());
            const responseData = {};
            responseData["StudentCode"] = formData["StudentCode"];
            responseData["FirstName"] = formData["FirstName"];
            responseData["MiddleNames"] = formData["MiddleNames"];
            responseData["LastName"] = formData["LastName"];

            await fetch("http://localhost:8080/students", {
                method: "POST",
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(responseData)
            });
            this.$emit('close');
            await this.$parent.refreshStudents()
            await this.$parent.refreshStudents()
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
                        <slot name="header">
                            <h1>New Student</h1>
                        </slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <form id="studentModalForm" class="modal-form" autocomplete="off" v-on:submit="submitForm">
                                <label for="StudentCode">Student Code:</label>
                                <input type="text" name="StudentCode" id="StudentCode" required>
                                <label for="FirstName">First Name:</label>
                                <input type="text" name="FirstName" id="FirstName" required>
                                <label for="MiddleNames">Middle Names:</label>
                                <input type="text" name="MiddleNames" id="MiddleNames">
                                <label for="LastName">Last Name:</label>
                                <input type="text" name="LastName" id="LastName" required>
                            </form>
                        </slot>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="$emit('close')">Close</button>
                            <input type="submit" form="studentModalForm" value="Create" class="createButton">
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>