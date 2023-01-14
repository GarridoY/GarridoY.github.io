<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <nav class="navbar navbar-expand-lg navbar-dark">
        <div class="container container-fluid">
            <!-- TODO: This should switch between different OS user paths with blinking marker-->
            <RouterLink class="navbar-brand" to="/">C:\Users\Garrido> |</RouterLink>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse"  id="navbarSupportedContent">
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