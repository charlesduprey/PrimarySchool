<template>
    <div>
        <div class="page-header">
            <h1>Gestion des professeurs</h1>
        </div>

        <div class="panel panel-default">
            <div class="panel-body text-right">
                <router-link class="btn btn-primary" :to="`teachers/create`"><i class="glyphicon glyphicon-plus"></i> Ajouter un professeur</router-link>
            </div>
        </div>

        <table class="table table-striped table-hover table-bordered">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Nom</th>
                    <th>Prénom</th>
                    <th>Options</th>
                </tr>
            </thead>

            <tbody>
                <tr v-if="teacherList.length == 0">
                    <td colspan="4" class="text-center">Il n'y a actuellement aucun professeur.</td>
                </tr>

                <tr v-for="i of teacherList">
                    <td>{{ i.teacherId }}</td>
                    <td>{{ i.lastName }}</td>
                    <td>{{ i.firstName }}</td>
                    <td>
                        <router-link :to="`teachers/edit/${i.teacherId}`"><i class="glyphicon glyphicon-pencil"></i></router-link>
                        <router-link :to="`teachers/assign/${i.teacherId}`"><i class="glyphicon glyphicon-pushpin"></i></router-link>
                        <a href="#" @click="deleteTeacher(i.teacherId)"><i class="glyphicon glyphicon-remove"></i></a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    import { mapActions } from 'vuex'
    import TeacherApiService from '../../services/TeacherApiService'

    export default {
        data() {
            return {
                teacherList: []
            }
        },

        async mounted() {
            await this.refreshList();
        },

        methods: {
            ...mapActions(['executeAsyncRequestOrDefault', 'executeAsyncRequest']),

            async refreshList() {
                this.teacherList = (await this.executeAsyncRequestOrDefault(() => TeacherApiService.getTeacherListAsync())) || [];
            },

            async deleteTeacher(teacherId) {
                try {
                    await this.executeAsyncRequest(() => TeacherApiService.deleteTeacherAsync(teacherId));
                    await this.refreshList();
                }
                catch(error) {

                }
            }
        }
    }
</script>

<style lang="less">

</style>