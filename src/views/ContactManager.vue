<template>
  <div class="container py-5">
    <div class="row">
      <div class="col">
        <router-link to="/contacts/add" class="btn btn-outline-info"><i class="fa fa-plus-circle"></i> Add New</router-link>
        <!-- <form action="" class="mt-3">
          <div class="row">
            <div class="col-md-6">
              <div class="row">
                <div class="col">
                  <input type="text" class="form-control" placeholder="Search Name">
                </div>
                <div class="col">
                  <input type="submit" class="btn btn-outline-info" value="Search">
                </div>
              </div>
            </div>
          </div>
        </form> -->
      </div>
    </div>
  </div>

  <div v-if="errorMessage">
      <div class="container">
        <div class="row">
          <div class="col">
            <p class="text-danger fw-bold display-4">{{ errorMessage }}</p>
          </div>
        </div>
      </div>
  </div>

  <div class="container py-3" v-if="contacts.length > 0">
    <div class="row">
      <div class="col-md-6" v-for="contact in contacts" :key="contact">
        <div class="card bg-dark shadow-lg mb-3">
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-md-4">
                <img :src="contact.picture" alt="" class="contact-img">
              </div>
              <div class="col-md-7">
                <ul class="list-group">
                  <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span></li>
                  <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span></li>
                  <li class="list-group-item">Mobile: <span class="fw-bold">{{ contact.mobile }}</span></li>
                </ul>
              </div>
              <div class="col-md-1 d-flex flex-column justify-content-center align-items-center">
                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-outline-warning my-1"><i class="fa fa-eye"></i></router-link>
                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-outline-primary my-1"><i class="fa fa-pen"></i></router-link>
                <button class="btn btn-outline-danger my-1" @click="deleteContact(contact.id)">
                  <i class="fa fa-trash"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService';
export default {
    name: 'ContactManager',

    data: function (){
      return {
        contacts: [],
        errorMessage: null
      }
    },

    created: async function (){
      try{
        let response = await ContactService.getAllContacts();
        this.contacts = response.data;
      }
      catch(error){
        this.errorMessage = error;
      }
    },

    methods: {
      deleteContact: async function(contactId){
        try{
          let response = await ContactService.deleteContact(contactId);
          if(response){
            let response = await ContactService.getAllContacts();
            this.contacts = response.data;
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