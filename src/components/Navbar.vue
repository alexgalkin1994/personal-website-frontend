<template>
  <header class="bg-primary">
    <div class="flex flex-row container justify-between py-4 items-baseline">
      <g-link to="/" class="text-xl font-medium"></g-link>
      <nav class="flex flex-row items-baseline justify-end">
        <g-link class="font-medium mr-8" to="/projects/">Projects</g-link>
        <g-link class="font-medium mr-8" to="/music/">Music</g-link>
        <g-link class="font-medium mr-8" to="/about/">About</g-link>
        <g-link class="font-medium mr-8" to="/contact/">Contact</g-link>
      </nav>
    </div>
  </header>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>