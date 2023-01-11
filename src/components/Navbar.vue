<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <nav class="navbar navbar-expand-lg navbar-light">
        <div class="container container-fluid">
            <!-- TODO: This should switch between different OS user paths with blinking marker-->
            <RouterLink class="navbar-brand" to="/">C:\Users\Garrido> |</RouterLink>
            <div class="collapse navbar-collapse">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <RouterLink class="nav-link" to="/">Desktop/</RouterLink>
                    </li>
                    <li class="nav-item">
                        <RouterLink class="nav-link" to="/about">About/</RouterLink>
                    </li>
                    <li class="nav-item">
                        <RouterLink class="nav-link" to="/projects">Projects/</RouterLink>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>