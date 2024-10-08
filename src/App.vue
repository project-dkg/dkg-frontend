<script setup>
// Copyright (C) 2023-2024 Maxim [maxirmx] Samsonov  (www.sw.consulting)
// All rights reserved.
// This file is a part of Dkg Frontend applcation
//
// Redistribution and use in source and binary forms, with or without
// modification, are permitted provided that the following conditions
// are met:
// 1. Redistributions of source code must retain the above copyright
// notice, this list of conditions and the following disclaimer.
// 2. Redistributions in binary form must reproduce the above copyright
// notice, this list of conditions and the following disclaimer in the
// documentation and/or other materials provided with the distribution.
//
// THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
// ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED
// TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
// PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDERS OR CONTRIBUTORS
// BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
// CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
// SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
// INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
// CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
// ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
// POSSIBILITY OF SUCH DAMAGE.

import { RouterLink, RouterView } from 'vue-router'
import router from '@/router'

import { storeToRefs } from 'pinia'
import { version } from '@/../package'

import { useDisplay } from 'vuetify'
const { height } = useDisplay()

import { useAuthStore } from '@/stores/auth.store.js'
const authStore = useAuthStore()

import { useVersionsStore } from '@/stores/versions.store.js'

const versionsStore = useVersionsStore()
const { currentVersion } = storeToRefs(versionsStore)
versionsStore.getCurrent()
  import { drawer, toggleDrawer } from '@/helpers/drawer.js'

function deauth() {
  authStore.logout()
  router.push('')
}

function getUserName() {
  return authStore.user
    ? ' | ' +
        authStore.user.name
    : ''
}

</script>

<template>
  <v-app class="rounded rounded-md">
    <v-app-bar>
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click.stop="toggleDrawer()" color="blue-darken-2"></v-app-bar-nav-icon>
      </template>
      <v-app-bar-title class="orange">Dkg Frontend {{ getUserName() }} </v-app-bar-title>
      <v-spacer />
    </v-app-bar>
    <v-navigation-drawer v-model="drawer" elevation="4">
      <template v-slot:prepend>
        <div class="pa-2" v-if="height > 480">
          <img alt="Dkg Frontend" class="logo" src="@/assets/logo.png" />
        </div>
      </template>
      <v-list v-if="authStore.user">
        <v-list-item>
          <RouterLink to="/rounds" class="link">Rounds</RouterLink>
        </v-list-item>
        <v-list-item>
          <RouterLink to="/nodes" class="link">Nodes</RouterLink>
        </v-list-item>
        <v-list-item v-if="authStore.user?.isAdmin">
          <RouterLink :to="'/statistics'" class="link">Statistics</RouterLink>
        </v-list-item>
        <v-list-item v-if="!authStore.user?.isAdmin">
          <RouterLink :to="'/user/edit/' + authStore.user.id" class="link">Settings</RouterLink>
        </v-list-item>
        <v-list-item v-if="authStore.user?.isAdmin">
          <RouterLink to="/users" class="link">Users</RouterLink>
        </v-list-item>
        <v-list-item>
          <RouterLink to="/login" custom v-slot="{ href }">
            <a :href="href" @click="deauth()" class="link">Logout</a>
          </RouterLink>
        </v-list-item>
      </v-list>
      <v-list v-if="!authStore.user">
        <v-list-item>
          <RouterLink to="/login" class="link">Login</RouterLink>
        </v-list-item>
        <v-list-item>
          <RouterLink to="/register" class="link">Register</RouterLink>
        </v-list-item>
        <v-list-item>
          <RouterLink to="/recover" class="link">Recover password</RouterLink>
        </v-list-item>
      </v-list>
      <template v-slot:append>
        <div class="pa-2 orange version">
            Client {{ version }}<br/>Server {{ currentVersion.versionNumber }}
        </div>
      </template>
    </v-navigation-drawer>

    <v-main class="d-flex align-center justify-center vvv">
      <RouterView />
    </v-main>
  </v-app>
</template>

<style scoped>
.vvv {
  width: 90vw;
  margin: 1rem;
  min-width: 480px;
}

.logo {
  margin: 1rem;
  display: block;
  width: 90%;
}

.version {
  margin: 2rem;
  font-size: 1rem;
}

nav {
  width: 100%;
  margin-top: 2rem;
  text-align: left;
  margin-left: -1rem;
  font-size: 1rem;
  padding: 1rem 0;
  margin-top: 1rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}
</style>
