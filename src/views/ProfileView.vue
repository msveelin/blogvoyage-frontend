<template>
    <NavBar />
    <div class="p-5 w-full">
        <h1 class="font-bold text-2xl">My Profile</h1>
        <div v-if="userData" class="p-3">
            <h2 class="text-xl font-bold mb-2">Personal Information</h2>
            <div class="flex justify-between">
                <div class="flex w-1/2 mr-2 flex-col">
                    <label for="firstName">First Name</label>
                    <input class="bg-slate-100 p-2 rounded-md my-2" type="text" v-model="userData.firstName" id="firstName">
                </div>
                <div class="flex w-1/2 ml-2 flex-col">
                    <label for="lastName">Last Name</label>
                    <input class="bg-slate-100  p-2 rounded-md my-2" type="text" v-model="userData.lastName"
                        id="lastName">
                </div>
            </div>
            <div class="flex justify-between">
                <div class="flex w-1/2 mr-2 flex-col">
                    <label for="userName">Username</label>
                    <input class="bg-slate-100 p-2 rounded-md my-2" type="text" v-model="userData.username" id="userName">
                </div>
                <div class="flex w-1/2 ml-2 flex-col">
                    <label for="emailAddress">Email Address</label>
                    <input class="bg-slate-100 p-2 rounded-md my-2" type="text" v-model="userData.email"
                        id="emailAddress">
                </div>
            </div>

            <h2 class="text-xl font-bold mb-2">Additional Information</h2>
            <div class="flex flex-col">
                <label for="personalBio">Bio</label>
                <textarea rows="5" class="bg-slate-100 w-full p-2 rounded-md my-2" type="text" v-model="userData.bio"
                    id="personalBio"></textarea>
            </div>
            <div class="flex w-full justify-between">
                <div class="flex w-1/2 mr-2 flex-col">
                    <label for="location">Location</label>
                    <input class="bg-slate-100 p-2 rounded-md my-2" type="text" v-model="userData.location" id="location">
                </div>
                <div class="flex w-1/2 ml-2 flex-col">
                    <label for="website">Website (optional):</label>
                    <input class="bg-slate-100 p-2 rounded-md my-2" type="text" v-model="userData.website" id="website">
                </div>
            </div>
            <button @click="updateProfile" class="p-3 bg-sky-400 w-full mt-3 text-white rounded-md font-bold text-lg">SAVE</button>
        </div>

    </div>
    <FooterBar />
</template>
<script setup>
import NavBar from '@/components/NavBar.vue';
import FooterBar from '../components/FooterBar.vue';
import { useUserStore } from '@/stores/user'
import { ref, onMounted } from 'vue'

import apolloClient from "@/apollo-config";
import { USER_BY_ID } from '@/queries'

const userStore = useUserStore()
// const userId = userStore.getUser.id
const user = userStore.getUser
const userData = ref({})
const userDetails = ref({
    firstName: ""
})
const userId = user.id

const fetchUserInfo = async () => {
    try {
        console.log('Fetching user information...');
        const data = await apolloClient.query({
            query: USER_BY_ID,
            variables: { id: userId },
        });
        userData.value = { ...data.data.userById[0] }

        console.log(userData.value)
    } catch (error) {
        console.error('Error fetching site information:', error);
    }
}

const updateProfile = async() => {
    try {
        console.log(userData.value)
    } catch (error) {
        console.error('Error updating information:', error);
    }
}

onMounted(() => {
    fetchUserInfo()
})
</script>