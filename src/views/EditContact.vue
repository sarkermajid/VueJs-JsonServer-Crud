<template>
    <section class="py-5">
        <div class="container">
            <div class="row">
                <div class="col-md-8 mx-auto">
                    <div class="card bg-dark text-white">
                        <div class="card-header">
                            <h4>Edit Contact</h4>
                            <router-link to="/contacts" class="btn btn-outline-info">View Contact</router-link>
                            <p class="text-danger fw-bold h3 mt-3">{{ errorMessage }}</p>
                        </div>
                        <div class="card-body">
                            <h3 class="text-center text-success"></h3>
                            <form action="" method="" @submit.prevent="updateSumbit()">
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
                                        <input type="text" v-model="contact.picture" class="form-control mb-3"  required>
                                        <img :src="contact.picture" class="contact-img">
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
                                        <select class="form-control" v-model="contact.title" v-if="groups.length > 0">
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
                                        <input type="submit" class="btn btn-outline-success" value="Update">
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
import { ContactService } from '@/services/ContactService';
export default {
    name: 'EditContact',
    data: function(){
        return{
            contactId: this.$route.params.contactId,
            errorMessage: null,
            contact: {
                name: '',
                email: '',
                mobile: '',
                picture: '',
                title: '',
            },
            groups: []
        }
    },

    created: async function(){
        try{
            let response = await ContactService.getContact(this.contactId);
            let gropusResponse = await ContactService.getAllGroups();
            this.groups = gropusResponse.data; 
            this.contact = response.data;
        }
        catch(error){
            this.errorMessage = error;
        }
    },

    methods: {
        updateSumbit: async function(){
            try{
                let response = await ContactService.updateContact(this.contact,this.contactId);
                if(response){
                    return this.$router.push('/');
                }
                else{
                    return this.$route.push(`/contacts/edit/${this.contactId}`);
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