<template>
  <div class="card">
   <div>
     <slot name="header"></slot>
   </div>
    <div class="card-body">
      <slot name="body" />
    </div>

    <div class="card-footer bg-white mt-">
      <slot name="footer"/>
    </div>
  </div>
</template>

<script>

export default {
  name: "Card"
}
</script>

