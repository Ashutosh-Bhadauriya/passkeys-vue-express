<template>
    <div class="p-4">
      <h1>Dashboard</h1>
      <button @click="handleLogout">Logout</button>
      <div>
        <button
          @click="registerPasskey"
          class="flex justify-center items-center space-x-2 mt-8"
        >
          Register a new passkey
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { useRouter } from 'vue-router';
  import { create, type CredentialCreationOptionsJSON } from '@github/webauthn-json';

  
  const router = useRouter();
  
  const handleLogout = async () => {
    try {
      const response = await fetch('http://localhost:5001/api/logout', {
        method: 'POST',
        credentials: 'include',
      });
      const data = await response.json();
      if (response.ok) {
        console.log('Logout successful:', data);
        router.push('/login');
      } else {
        console.error('Logout failed:', data.message);
      }
    } catch (error) {
      console.error('Logout error:', error);
    }
  };
  
  const registerPasskey = async () => {
    const createOptionsResponse = await fetch("http://localhost:5001/api/passkeys/register", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      credentials: 'include',
      body: JSON.stringify({ start: true, finish: false, credential: null }),
    });
  
    const { createOptions } = await createOptionsResponse.json();
    console.log("createOptions", createOptions);
  
    const credential = await create(createOptions as CredentialCreationOptionsJSON);
    console.log(credential);
  
    const response = await fetch("http://localhost:5001/api/passkeys/register", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      credentials: "include",
      body: JSON.stringify({ start: false, finish: true, credential }),
    });
    console.log(response);
  
    if (response.ok) {
        console.log("Passkey registered successfully");
      return;
    }
  };
  </script>