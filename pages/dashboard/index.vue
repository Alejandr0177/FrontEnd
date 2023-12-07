<template>
    <Dashboard />
</template>

<script>
import Dashboard from '@/components/Content/index.vue'
export default {
    name: 'IndexPage', 
    components: {
        Dashboard
    },
    layout: 'dashboard',
    // created() {
    //     const docId = this.$route.query.doc_id;
    //     if (!docId) {
    //     // Si no hay doc_id, redirigir al inicio de sesión
    //     this.$router.push('/');
    // } else {
    //     // Puedes hacer algo con docId si es necesario
    //     console.log('Se ha encontrado un doc_id en la URL:', docId);
    //     this.docId = docId;
    // }
    // },
}
</script>