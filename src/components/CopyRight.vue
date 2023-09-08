<template>
  <p class="copyright">
    This game owned by RHP Team in Vue 3 course for begginers -
    <a
      href="https://www.youtube.com/watch?v=CHM75-NqOmk&list=PLU4OBh9yHE94sZ3TPGt0QG_PIwrZ1QF6i"
      >view here</a
    >
  </p>
</template>

<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
