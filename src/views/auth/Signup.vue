<template>
  <form @submit.prevent="handleSubmit">
    <h3>Sign up</h3>
    <input type="text" placeholder="Display Name" v-model="displayName" />
    <input type="email" placeholder="Email" v-model="email" />
    <input type="password" placeholder="Password" v-model="password" />
    <div v-if="error" class="error">{{ error }}</div>
    <button v-if="!isPending">Sign up</button>
    <button v-if="isPending" disabled>Loading</button>
  </form>
</template>

<script>
import useSignup from "@/composables/useSignup";
import useDocument from '@/composables/useDocument'
import { ref } from "vue";
import { useRouter } from 'vue-router';

export default {
  setup() {
    const { error, signup, isPending } = useSignup();

    const email = ref("");
    const password = ref("");
    const displayName = ref("");
    const route = useRouter()

    const handleSubmit = async () => {
      await signup(email.value, password.value, displayName.value)
        .then((res) => {const {updateDoc} = useDocument('users', res.user.uid);
        updateDoc({
            displayName: displayName.value
        })});
      if (!error.value) {
        
        console.log("user signed up");
        route.push('./')
      }
    };

    return { email, password, displayName, handleSubmit, error, isPending };
  },
};
</script>