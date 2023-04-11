<script setup>
  import { RouterView } from 'vue-router'
</script>

<template>
  <RouterView />
</template>

<style scoped lang="scss">
    @for $i from 0 through 200 {
    .bor-#{$i} { border-radius: 1px * $i; }
    .Bor-#{$i} { border-radius: 1% * $i; }

    .t#{$i} {top: 1px * $i}
    .b#{$i} {bottom: 1px * $i}
    .l#{$i} {left: 1px * $i}
    .r#{$i} {right: 1px * $i}
    
    .T#{$i} {top: 1% * $i}
    .B#{$i} {bottom: 1% * $i}
    .L#{$i} {left: 1% * $i}
    .R#{$i} {right: 1% * $i}

    .t-#{$i} {top: - (1px * $i)}
    .b-#{$i} {bottom: - (1px * $i)}
    .l-#{$i} {left: - (1px * $i)}
    .r-#{$i} {right: - (1px * $i)}
  }

  .pc-only {
    display: block;
  }
  .mobile-only{
    display: none;
  }
  @media only screen and (max-width: 767px) {
    .pc-only {
      display: none;
    }
    .mobile-only{
      display: block;
    }
  }
</style>
