<template>
  <div id="app" class="flex flex-column vh-100">
    <app-header></app-header>
    <router-view/>
    <app-footer></app-footer>
  </div>
</template>

<script>
import AppHeader from '@/components/header';
import AppFooter from '@/components/footer';
export default {
  name: 'app',
  components: { AppHeader, AppFooter }
};
</script>
