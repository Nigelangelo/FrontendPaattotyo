<!--
  Halusin yhtenevät napit tähän, jota käytetty aikaisemmin, mutta ei näitä tullutkaan kun vain yksi kappale käyttöön.
-->
<script>
  export let disabled = false;
</script>

<button {disabled} on:click><slot /></button>

<style>
  button {
    color: rgb(255, 255, 255);
    background-color: rgba(160, 152, 167, 0.658);
  }
</style> 
