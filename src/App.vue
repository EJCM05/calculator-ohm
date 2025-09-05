<script setup>
import Calculator from '@/components/Calculator.vue';
import Logo from "@/assets/logo.png"
</script>

<template>
    <!-- <link href="cover.css" rel="stylesheet"> -->
    <div class="cover-container d-flex w-100 h-100 p-3 mx-auto flex-column text-center">
      <header class="mx-auto">
        <div>
            <h1 class="float-md-start mb-0 d-flex align-items-center justify-content-center">Calculator<span class="color-ohm px-2"><img :src="Logo" alt=""> </span> ohm</h1>
        </div>
      </header>
      <main>
      <Calculator></Calculator>
      </main>
      <footer class="mt-auto">
        <p><a href="#" class="text-black">From Computing</a>, by <a
            href="https://x.com/mdo" class="text-black">@ECJM</a>.</p>
      </footer>
    </div>
</template>

<style>
      
      .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none
      }

      @media (min-width: 768px) {
        .bd-placeholder-img-lg {
          font-size: 3.5rem
        }
      }

      .b-example-divider {
        width: 100%;
        height: 3rem;
        background-color: #0000001a;
        border: solid rgba(0, 0, 0, .15);
        border-width: 1px 0;
        box-shadow: inset 0 .5em 1.5em #0000001a, inset 0 .125em .5em #00000026
      }

      .b-example-vr {
        flex-shrink: 0;
        width: 1.5rem;
        height: 100vh
      }

      .bi {
        vertical-align: -.125em;
        fill: currentColor
      }

      .nav-scroller {
        position: relative;
        z-index: 2;
        height: 2.75rem;
        overflow-y: hidden
      }

      .nav-scroller .nav {
        display: flex;
        flex-wrap: nowrap;
        padding-bottom: 1rem;
        margin-top: -1px;
        overflow-x: auto;
        text-align: center;
        white-space: nowrap;
        -webkit-overflow-scrolling: touch
      }

      .btn-bd-primary {
        --bd-violet-bg: #712cf9;
        --bd-violet-rgb: 112.520718, 44.062154, 249.437846;
        --bs-btn-font-weight: 600;
        --bs-btn-color: var(--bs-white);
        --bs-btn-bg: var(--bd-violet-bg);
        --bs-btn-border-color: var(--bd-violet-bg);
        --bs-btn-hover-color: var(--bs-white);
        --bs-btn-hover-bg: #6528e0;
        --bs-btn-hover-border-color: #6528e0;
        --bs-btn-focus-shadow-rgb: var(--bd-violet-rgb);
        --bs-btn-active-color: var(--bs-btn-hover-color);
        --bs-btn-active-bg: #5a23c8;
        --bs-btn-active-border-color: #5a23c8
      }

      .bd-mode-toggle {
        z-index: 1500
      }

      .bd-mode-toggle .bi {
        width: 1em;
        height: 1em
      }

      .bd-mode-toggle .dropdown-menu .active .bi {
        display: block !important
      }
      .color-ohm{
        font-size: 2.5rem;
        color: var(--color-accent-ohm) !important;
        img{
          border-radius: 20rem;
          width: 3rem;
          height: auto;
        }
      }
</style>
