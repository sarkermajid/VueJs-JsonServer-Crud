<template>
    <section class="py-5">
        <div class="container">
            <div class="row">
                <div class="col-md-8 mx-auto">
                        <div class="card bg-dark text-white">
                        <div class="card-header">
                            <h4>Add New Contact</h4>
                            <router-link to="/contacts" class="btn btn-outline-info">View All Contacts</router-link>
                        </div>
                        <div class="card-body">
                            <h3 class="text-center text-success"></h3>
                            <form action="" method="" @submit.prevent="submitCreate()" >
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for="">Name</label>
                                    </div>
                                    <div class="col-md-9">
                                        <input type="text" v-model="contact.name" class="form-control"  required>
                                    </div>
                                </div>
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for="">Photo Url</label>
                                    </div>
                                    <div class="col-md-9">
                                        <input type="text" v-model="contact.picture" class="form-control"  required>
                                        <img :src="contact.picture" alt="" v-if="contact.picture" class="contact-img mt-3">
                                    </div>
                                </div>
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for="">Email</label>
                                    </div>
                                    <div class="col-md-9">
                                        <input type="email" v-model="contact.email" class="form-control"  required>
                                    </div>
                                </div>
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for="">Mobile</label>
                                    </div>
                                    <div class="col-md-9">
                                        <input type="number" v-model="contact.mobile" class="form-control"  required>
                                    </div>
                                </div>
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for="">Title</label>
                                    </div>
                                    <div class="col-md-9">
                                        <select v-model="contact.title" class="form-control" v-if="groups.length > 0">
                                          <option value="">Select Group</option>
                                          <option :value="group.id" v-for="group in groups" :key="group.id">{{ group.name }}</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="row mb-3">
                                    <div class="col-md-3">
                                        <label for=""></label>
                                    </div>
                                    <div class="col-md-9">
                                        <input type="submit" class="btn btn-outline-success" value="Create">
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import { ContactService } from '@/services/ContactService'
export default {
    name: 'AddContact',
    data: function (){
        return{
            contact: {
                name: '',
                email: '',
                mobile: '',
                picture: '',
                title: '',
            },
            groups:[],
            errorMessage: null
        }
    },

    created: async function(){
        try{
            let response = await ContactService.getAllGroups();
            this.groups = response.data;
        }
        catch(error){
            this.errorMessage = error;
        }
    },

    methods: {
        submitCreate: async function(){
            try{
                let response = await ContactService.createContact(this.contact);
                if(response){
                    return this.$router.push('/');
                }
                else{
                    return this.$router.push('/contacts/add');
                }
            }
            catch(error){
                this.errorMessage = error;
            }
        }
    }
}
</script>

<style>

</style>