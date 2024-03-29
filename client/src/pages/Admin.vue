<script setup lang="ts">
import { IndexKind } from 'typescript';
import { type User, getUsers, editUser, createUser, checkUserProfileImage } from '../model/users';
import { ref } from 'vue';
const users = ref([] as User[]);
users.value = getUsers();

const editedUser = ref(null as User | null);

function startEditing(user: User) {
    editedUser.value = { ...user };
}

function saveChanges() {
    if (editedUser.value) {
        editUser(editedUser.value);
        closeModal();
    }
}

function closeModal() {
    editedUser.value = null
}

function deleteUser(index: number) {
    users.value.splice(index, 1);
}

const newUser = ref({} as User);
const addUserModal = ref(false);
function openAddUserModal() {
    addUserModal.value = true;
    console.log('addUserModal opened');
}

function closeAddUserModal() {
    addUserModal.value = false;
    console.log('addUserModal closed');
}

function createNewUser(user: User) {
    createUser(user);
    checkUserProfileImage(user);
    newUser.value= ({} as User);
    closeAddUserModal();
}

</script>

<template>
    <div class="container" style="display: flex; justify-content: center; align-items: center; margin-bottom: 30px;">
        <p class="title is-1">Admin Page</p>
    </div>
    <button @click="openAddUserModal()" class="button is-primary" style="display: block; margin: auto;">
        Add User
    </button>
    <div v-for="(user, index) in users" :key="user.id" class="card">
        <div class="card-content">
            <div class="media">
                <div class="media-left">
                    <figure class="image is-48x48">
                        <img :src="user.profileImage" alt="Placeholder image">
                    </figure>
                </div>
                <div class="media-content">
                    <p class="title is-4">{{ user.firstName + " " + user.lastName }}</p>
                    <p class="subtitle is-6">UserID: {{ user.id }}</p>
                </div>
                <div class="button-container">
                    <button @click="startEditing(user)" class="button is-warning is-focused">Edit</button>
                    <button @click="deleteUser(index)" class="button is-danger is-focused">Delete</button>
                </div>
            </div>
        </div>
    </div>
    <div class="modal" :class="{ 'is-active': editedUser }" v-if="editedUser">
        <div class="modal-background"></div>
        <div class="modal-content">
            <div class="box">
                <h1 class="title">Edit User</h1>
                <div class="field">
                    <label class="label">First Name</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="First Name" v-model="editedUser.firstName">
                    </div>
                </div>
                <div class="field">
                    <label class="label">Last Name</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="Last Name" v-model="editedUser.lastName">
                    </div>
                </div>
                <div class="field">
                    <div class="control">
                        <button @click="saveChanges()" class="button is-primary">Save Changes</button>
                    </div>
                </div>
            </div>
        </div>
        <button @click="closeModal()" class="modal-close is-large" aria-label="close"></button>
    </div>
    <div class="modal" :class="{ 'is-active': addUserModal }">
        <div class="modal-background"></div>
        <div class="modal-content">
            <div class="box">
                <h1 class="title">Add User</h1>
                <div class="field">
                    <label class="label">First Name</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="First Name" v-model="newUser.firstName">
                    </div>
                </div>
                <div class="field">
                    <label class="label">Last Name</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="Last Name" v-model="newUser.lastName">
                    </div>
                </div>
                <div class="field">
                    <label class="label">Profile Image</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="Profile Image" v-model="newUser.profileImage">
                    </div>
                </div>
                <div class="field">
                    <div class="control">
                        <button @click="closeAddUserModal(); createNewUser(newUser)" class="button is-primary">Add User</button>
                    </div>
                </div>
            </div>
        </div>
        <button @click="closeAddUserModal()" class="modal-close is-large" aria-label="close"></button>
    </div>
</template>