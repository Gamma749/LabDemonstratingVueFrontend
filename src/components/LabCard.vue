<script>
export default {
    props: {
        lab: {},
        endComponent: String
    },
};

</script>

<template>
    <div id="labCard" class="card">
        <h2 class="labItem">Lab {{ lab["LabID"] }}</h2>
        <h2 class="labItem">{{ lab["LabName"] }}</h2>
        <h2 class="labItem">Points: {{ lab["Points"] }}</h2>
        <div id="endComponent">
            <slot></slot>
        </div>
        <h4 class="labItem" id="labDesc">{{ lab["Description"] }}</h4>
    </div>
</template>


<style scoped>
#labCard {
    grid-template-columns: 15% 1fr 1fr 15%;
    /* grid-template-rows: 1fr 1fr; */
    border-radius: 1rem/1.5rem;
    gap: 2% 2%;
}

#labDesc {
    grid-column-start: 2;
    grid-column-end: 4;
    grid-row: 2;
}

#endComponent {
    display: flex;
    grid-column-start: 4;
    grid-row-start: 1;
    grid-row-end: 3;
}


</style>

