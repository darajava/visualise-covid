<script>
  import Loading from "./Loading.svelte";
  import { fade } from "svelte/transition";
  export let percent = 0;
  let show = false;
  setTimeout(() => { show = true; }, 200);
</script>

{#if show}
  <div class="loading" transition:fade>
    <div class="loadingio-spinner-dual-ring-sdhf">
      <div class="load-aaa">
      <div></div>
      <div>
        <div></div>
      </div>
      </div>
    </div>
    {#if percent !== 0}
      {percent}%
    {/if}
  </div>
{/if}

<style type="text/css">
.loading {
  width: 100vw;
  height: 100vh;
  flex-direction: column;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;
  z-index: 10000000;
  background-color: rgba(255, 255, 255, 0.5);
}
@keyframes load-aaa {
  0% { transform: rotate(0) }
  100% { transform: rotate(360deg) }
}
.load-aaa div { box-sizing: border-box!important }
.load-aaa > div {
  position: absolute;
  width: 146.97px;
  height: 146.97px;
  top: 30.014999999999997px;
  left: 30.014999999999997px;
  border-radius: 50%;
  border: 14.489999999999998px solid #000;
  border-color: #71a5fe transparent #71a5fe transparent;
  animation: load-aaa 2.0408163265306123s linear infinite;
}
.load-aaa > div:nth-child(2) { border-color: transparent }
.load-aaa > div:nth-child(2) div {
  position: absolute;
  width: 100%;
  height: 100%;
  transform: rotate(45deg);
}
.load-aaa > div:nth-child(2) div:before, .load-aaa > div:nth-child(2) div:after { 
  content: "";
  display: block;
  position: absolute;
  width: 14.489999999999998px;
  height: 14.489999999999998px;
  top: -14.489999999999998px;
  left: 51.74999999999999px;
  background: #71a5fe;
  border-radius: 50%;
  box-shadow: 0 132.48px 0 0 #71a5fe;
}
.load-aaa > div:nth-child(2) div:after { 
  left: -14.489999999999998px;
  top: 51.74999999999999px;
  box-shadow: 132.48px 0 0 0 #71a5fe;
}
.loadingio-spinner-dual-ring-sdhf {
  width: 207px;
  height: 207px;
  display: inline-block;
  overflow: hidden;
  background: none;
}
.load-aaa {
  width: 100%;
  height: 100%;
  position: relative;
  transform: translateZ(0) scale(1);
  backface-visibility: hidden;
  transform-origin: 0 0;
}
.load-aaa div { box-sizing: content-box; }
</style>