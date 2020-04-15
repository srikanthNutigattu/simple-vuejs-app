<style scoped>

.addmargin {
    margin-top: 10px;
    margin-bottom: 10px;
}

.vue-logo-back {
    background-color: black;
}

</style>

<template>

<div class="home">
    <div class="vue-logo-back">
        <img src="../assets/logo.png" width="100px" height="100px">
    </div>
    <div class="col-md-6 centeralign">
        <div class="card centeralign" style="width: 18rem;">
            <div class="card-body">
                <form>
                    <p>Create a customer</p>
                    <input v-model="name" placeholder="name">
                    <input v-model="location" placeholder="location">
                    <input v-model="address" placeholder="address">
                    <div class=" addmargin">
                        <a class="btn btn-primary "  v-on:click="createCustomer(true)">Create</a>
                    </div>
                </form>
            </div>
        </div>

        <p>list of customers</p>
        <div class="card centeralign addmargin" style="width: 18rem;" v-for="customer in customerlist" :key="customer.id">
            <div class="card-body" v-on:click="setSelectedCustomer(customer.name)">
                <h5 class="card-title">{{customer.name}}</h5>
                <p class="card-text">{{customer.location}}</p>

                <a class="btn btn-primary" v-on:click="goToDetailsPage(customer.id)"><span style="color:white">Click for more details</span></a>
            </div>
        </div>
    </div>
    <Display v-if="selectedCustomer!=''" :selectedCustomer="selectedCustomer" />
</div>

</template>

<script>

// @ is an alias to /src
import Display from '@/components/Display.vue'
import axios from 'axios'

export default {
    name: 'customers',
    mounted() {
        this.loadCustomers();
    },
    data() {
        return {
            customerlist: [],
            selectedCustomer: "",
            name: "",
            location: "",
            address: ""
        }
    },
    components: {
        Display
    },
    methods: {
        setSelectedCustomer: function(name) {
            this.selectedCustomer = name;
        },
        goToDetailsPage: function(id) {
            this.$router.push("/customerdetails/"+id);
        },
        loadCustomers: function() {
            axios({
                method: "GET",
                "url": "http://localhost:8080/customer"
            }).then(response => {
                this.customerlist = response.data;
            }, error => {
                console.error(error);
            });
        },
        createCustomer: function() {
            axios.post('http://localhost:8080/customer', {
                name: this.name,
                location: this.location,
                address: this.address,
                })
                .then(response => {
                    this.loadCustomers();
                    console.log("reloading");
                }, error => {
                    console.error(error);
                });
        }

    }
}

</script>
