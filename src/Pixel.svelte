<script>
  export let on = false;
</script>

<div class={`pixel ${on ? "on" : ""}`}>
</div>

<style>
  .pixel.on {
    background-color: red;
  }

  .pixel {
    display: inline-block;
    box-sizing: border-box;
    border: 1px solid #ccc;
    width: 5px;
    height: 5px;
    min-width: 5px;
    min-height: 5px;
    margin-left: -1px;
    margin-top: -1px;
    background-color: blue;
  }
</style>