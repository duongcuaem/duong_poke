<template>
    <div class="copyright">
        <p>Trò chơi này thuộc bản quyền của Dương -
            <a href="https://www.facebook.com/DuongCuaEm/">xem tại đây</a> 
        </p>
    </div>
</template>
<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1rem;
}

.copyright a {
  color: var(--primary);
}
</style>