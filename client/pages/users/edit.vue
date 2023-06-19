<template>
    <div class="mx-auto max-w-5xl py-10">
        <h3 class="text-2xl mb-2">Edit User</h3>
        <div v-if="pending">
            Loading...
        </div>
        <div v-else>
            <form class="space-y-3" @submit.prevent="updateUser()">
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
                    <FormLabel label="Status" />
                    <br />
                    <FormCheckbox :value="state.user.is_active ? true : false"
                        @click="state.user.is_active = !state.user.is_active" />
                </div>
                <div class="flex gap-2">
                    <button @click="back()"
                        class="bg-yellow-400 text-black text-sm font-bold hover:bg-yellow-300 order rounded-md px-4 py-1">Back</button>
                    <FormButton type="submit" buttonStyle="primary">Update</FormButton>
                </div>
            </form>
        </div>
    </div>
</template>
  
<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const userId = route.query.user_id;

const { pending, data: user } = await useLazyAsyncData('user', () => $fetch(`http://127.0.0.1:8000/api/users/${userId}`));

const state = reactive({
    user: {
        name: user && user.value && user.value.name,
        course: user && user.value && user.value.course,
        is_active: user && user.value && user.value.is_active,
    }
});

const showNameValidation = ref(false);
const showCourseValidation = ref(false);

watch(user, (newData) => {
    state.user = {
        name: newData && newData.name,
        course: newData && newData.course,
        is_active: newData && newData.is_active,
    };
});

function back() {
    navigateTo('/');
}

async function updateUser() {
    showNameValidation.value = true;
    showCourseValidation.value = true;

    if (!state.user.name || !state.user.course) {
        alert('Please fill in all required fields.');
        return;
    }

    const payload = {
        name: state.user.name,
        course: state.user.course,
        is_active: state.user.is_active,
    };
    await $fetch(`http://127.0.0.1:8000/api/users/${userId}`, {
        method: 'PUT',
        body: payload
    }).then((result) => {
        if (result) {
            alert('Successfully updated.');
        }
    });
}
</script>
