<template>
  <div>
    <form @submit.prevent="loginUser">
      <div>
        <input id="email" required name="email" type="email" v-model="email" autoComplete="email" />
        <input
          id="password"
          name="password"
          type="password"
          v-model="password"
          autoComplete="current-password"
        />
      </div>
      <button type="submit">Sign in with Email</button>
    </form>
    <button @click="signInWithPasskey">Sign in with Passkey</button>
  </div>
</template>

<script>
import { get } from '@github/webauthn-json';


export default {
  data() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    async loginUser() {
      try {
        const response = await fetch('http://localhost:5001/api/login', {
          method: 'POST',
          credentials: 'include',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ email: this.email, password: this.password })
        })
        if (!response.ok) {
          throw new Error('Login failed')
        }
        const data = await response.json()
        // Handle success - you might want to redirect the user or store the login state
        console.log('Login successful', data);
        this.$router.push({ name: 'dashboard' });
      } catch (error) {
        // Handle error - show user feedback or log it
        console.error('Login error:', error)
      }
    },
    async signInWithPasskey() {
      try {
        const createOptionsResponse = await fetch("http://localhost:5001/api/passkeys/login", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: 'include',
          body: JSON.stringify({ start: true, finish: false, credential: null }),
        });

        const { loginOptions } = await createOptionsResponse.json();

        // Open "sign in with passkey" dialog
        const options = await get(loginOptions);

        const response = await fetch("http://localhost:5001/api/passkeys/login", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: 'include',
          body: JSON.stringify({ start: false, finish: true, options }),
        });

        if (response.ok) {
          console.log("User logged in with passkey");
          this.$router.push({ name: 'dashboard' });
        } else {
          throw new Error("Login with passkey failed");
        }
      } catch (error) {
        console.error("Login with passkey error:", error);
        // Optionally, handle error (e.g., show an error message to the user)
      }
    }
  }
}
</script>
