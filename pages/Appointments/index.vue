<template>
    <Appointments />
</template>

<script>
import Appointments from '@/components/Content/Appointments.vue'
export default {
    name: 'IndexPage', 
    components: {
        Appointments
    },
    layout: 'dashboard'
}
</script>