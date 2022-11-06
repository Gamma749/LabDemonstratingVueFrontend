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
            responseData["LabName"] = formData["LabName"];
            responseData["Description"] = formData["Description"];
            responseData["Points"] = parseInt(formData["Points"],10);

            await fetch("http://localhost:8080/labs", {
                method: "POST",
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(responseData)
            }).catch(function(error) {
                console.log("ERROR: " + error)
            });
            this.$emit('close');
            await this.$parent.refreshLabs()
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
                        <slot name="header"><h1>New Lab</h1></slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <form id="labModalForm" class="modal-form" autocomplete="off" v-on:submit="submitForm">
                                <label for="LabName">Lab Name:</label>
                                <input type="text" name="LabName" id="LabName" required>
                                <label for="Description">Lab Description:</label>
                                <input type="text" name="Description" id="Description"
                                    required>
                                <label for="Points">Lab Points:</label>
                                <input type="number" step=1 name="Points" id="Points" required>
                            </form>
                        </slot>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="$emit('close')">Close</button>
                            <input type="submit" form="labModalForm" value="Create">
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>