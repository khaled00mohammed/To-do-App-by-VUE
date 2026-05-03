<template>
  <div class="item">
    <i>
      <slot name="icon"></slot>
    </i>
    <div class="details">
      <h3>
        <slot name="heading"></slot>
      </h3>
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;
  color: var(--color-text);
}

h3 {
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 960px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
    border-left: 1px solid var(--divider-color);
    position: relative;
  }

  .details {
    margin-left: calc(var(--section-gap) / 2);
  }

  i {
    top: calc(calc(var(--section-gap) / 2) - (32px / 2));
    left: calc(0px - calc(var(--section-gap) / 2) - (32px / 2));
    position: absolute;
    border: 1px solid var(--divider-color);
    background: var(--color-background);
    border-radius: 8px;
  }
}
</style>
