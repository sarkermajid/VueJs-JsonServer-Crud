<template>
    <div class="row py-5">
        <div class="col-md-8 mx-auto">
            <div class="card bg-dark text-white">
                <div class="card-body text-white">
                    <h4>Contact Details</h4>
                    <router-link to="/contacts" class="btn btn-outline-info">View All Contacts</router-link>
                    <p class="text-danger fw-bold h3 mt-3">{{ errorMessage }}</p>
                    <table class="table table-bordered text-white mt-3">
                      <tr>
                        <th>Name</th>
                        <td>{{ contact.name }}</td>
                      </tr>
                      <tr>
                        <th>Photo</th>
                        <td><img :src="contact.picture" alt="" class="contact-img"></td>
                      </tr>
                      <tr>
                        <th>Email</th>
                        <td>{{ contact.email }}</td>
                      </tr>
                      <tr>
                        <th>Mobile</th>
                        <td>{{ contact.mobile }}</td>
                      </tr>
                      <tr>
                        <th>Title</th>
                        <td>{{ group.name }}</td>
                      </tr>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ContactService } from '@/services/ContactService';
export default {
    name:'ViewContact',
    data: function(){
      return{
        contactId: this.$route.params.contactId,
        contact: {},
        group: {},
        errorMessage: null,
      }
    },

    created: async function(){
      try{
        console.log(this.contactId);
        let response = await ContactService.getContact(this.contactId);
        let groupResponse = await ContactService.getGroup(response.data);
        this.contact = response.data;
        this.group = groupResponse.data;
      }
      catch(error){
        this.errorMessage = error;
      }
    }
}
</script>

<style>

</style>