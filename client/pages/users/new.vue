<template>
    <div class="mx-auto max-w-5xl py-10">
        <h3 class="text-2xl">Add User</h3>

        <div>
            <form class="space-y-3" @submit.prevent="saveUser()">
                <div>
                    <FormLabel label="Name" />
                    <FormTextField name="name" placeholder="Name" v-model="state.user.name" />
                    <div v-if="showNameValidation && !state.user.name" class="text-red-600">The Name field is required.
                    </div>
                </div>
                <div>
                    <FormLabel label="Course" />
                    <FormTextField name="course" placeholder="Course" v-model="state.user.course" />
                    <div v-if="showCourseValidation && !state.user.course" class="text-red-600">The Course field is
                        required.</div>
                </div>
                <div>
                    <FormLabel label="Email" />
                    <FormTextField name="email" placeholder="Email" v-model="state.user.email" />
                    <div v-if="showEmailValidation && !state.user.email" class="text-red-600">The Email field is required.
                    </div>
                </div>
                <div>
                    <FormLabel label="Password" />
                    <FormTextField type="password" name="password" placeholder="Password" v-model="state.user.password" />
                    <div v-if="showPasswordValidation && !state.user.password" class="text-red-600">The Password field is
                        required.</div>
                </div>
                <div>
                    <FormLabel label="Status" />
                    <br />
                    <FormCheckbox :value="state.user.is_active ? true : false"
                        @click="state.user.is_active = !state.user.is_active" />
                </div>
                <div class="flex gap-2">
                    <button @click="back()"
                        class="bg-yellow-400 text-black text-sm font-bold hover:bg-yellow-300 order rounded-md px-4 py-1">Back</button>
                    <FormButton type="submit" buttonStyle="primary">Save</FormButton>
                </div>
            </form>
        </div>
    </div>
</template>
  
<script setup>
import { ref } from "vue";

const state = reactive({
    user: {
        name: null,
        course: null,
        password: null,
        email: null,
        is_active: false,
    }
});

const showNameValidation = ref(false);
const showCourseValidation = ref(false);
const showEmailValidation = ref(false);
const showPasswordValidation = ref(false);

function back() {
    navigateTo('/');
}

async function saveUser() {
    showNameValidation.value = true;
    showCourseValidation.value = true;
    showEmailValidation.value = true;
    showPasswordValidation.value = true;

    if (!state.user.name || !state.user.course || !state.user.email || !state.user.password) {
        alert('Please fill in all required fields.');
        return;
    }

    const payload = {
        name: state.user.name,
        email: state.user.email,
        password: state.user.password,
        course: state.user.course,
        is_active: state.user.is_active,
    };
    await $fetch(`http://127.0.0.1:8000/api/users`, {
        method: 'POST',
        body: payload
    }).then((result) => {
        if (result) {
            alert('Successfully saved.');
        }
    }).catch((error) => {
        alert('Something went wrong.');
    });
}
</script>
