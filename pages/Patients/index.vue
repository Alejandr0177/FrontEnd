<template>
    <Patients />
</template>

<script>
import Patients from '~/components/Content/Patients.vue'
export default {
    name: 'IndexPage', 
    components: {
        Patients
    },
    layout: 'dashboard'
}
</script>