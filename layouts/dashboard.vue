<template>
    <v-app dark>
        <v-navigation-drawer
        permanent
        fixed
        app
        >
            <v-img style="width: 112.969px; height: 45px; flex-shrink: 0; margin: 20px 68.1px auto auto;" :src="require('/assets/images/logo.png')" />
            <div style="width: 160px; height: 135px; margin: 40px auto 0 46px; position: relative">
                <img style="left: 26px; top: 0px; position: absolute; border-radius: 9999px" :src="require('/assets/images/profile.png')" />
                <div style="left: 10px; top: 110px; position: absolute; color: black; font-size: 18px; font-family: Open Sans; font-weight: 400; line-height: 25px; letter-spacing: 0.36px; word-wrap: break-word">
                    Dr. Anushka Singh
                </div>
            </div>

            <div style="width: 240px; height: 245px; margin: 70px auto 0 5px; position: relative">
                <v-list>
                    <v-list-item
                    v-for="(item, i) in items"
                    :key="i"
                    :to="item.to"
                    >
                    <v-list-item-action>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                    </v-list-item>
                </v-list>
            </div>
        </v-navigation-drawer>

        <v-app-bar
        fixed
        app
        >
            <v-toolbar-title>{{ title }}</v-toolbar-title>
            <v-spacer />
        </v-app-bar>

        <v-main>
            <v-container>
                <Nuxt />
            </v-container>
        </v-main>

        <!-- <v-footer
        :absolute="!fixed"
        app
        > -->
        <!-- <span>&copy; {{ new Date().getFullYear() }}</span>
        </v-footer> -->
    </v-app>
</template>

<script>
export default {
    name: 'DefaultLayout',
    data () {
        return {
        items: [
            {
            icon: 'mdi-view-dashboard',
            title: 'Dashboard',
            to: `/dashboard?id_doc=${this.id_doc}`
            },
            {
            icon: 'mdi-notebook',
            title: 'Appointments',
            to: `/Appointments?id_doc=${this.id_doc}`
            },
            {
            icon: 'mdi-account-group',
            title: 'Patients',
            to: `/Patients?id_doc=${this.id_doc}`
            },
            {
            icon: 'mdi-timetable',
            title: 'Schedule',
            to: `/Schedule?id_doc=${this.id_doc}`
            }
        ],
        title: 'Vuetify.js'
        }
    }, 
    mounted() {
    // Escuchar el evento emitido por el componente hijo
        this.$root.$on('docIdObtained', (docId) => {
        this.id_doc = docId;
        });
    },
}
</script>