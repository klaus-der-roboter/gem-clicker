<script>
  import { Canvas } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';
  import { MeshPhongMaterial, BoxGeometry, SphereGeometry } from 'three';
  import Scene from './Scene.svelte';
  import HUD from './HUD.svelte';
  import { onMount } from 'svelte';

  let clicks = 0;
  let gems = 0;
  let cps = 0; // gems per second
  let upgrades = {
    autoClick: 0,
    clickPower: 0
  };
  let upgradeCosts = {
    autoClick: 50,
    clickPower: 25
  };

  onMount(() => {
    const interval = setInterval(() => {
      if (upgrades.autoClick > 0) {
        const gemsPerSecond = upgrades.autoClick * 0.5;
        gems += gemsPerSecond / 10; // Divide by 10 since this runs 10 times per second
      }
    }, 100); // Update 10 times per second

    return () => clearInterval(interval);
  });

  const clickGem = () => {
    clicks++;
    const power = 1 + upgrades.clickPower * 0.5;
    gems += power;
  };

  const buyUpgrade = (type) => {
    if (gems >= upgradeCosts[type]) {
      gems -= upgradeCosts[type];
      upgrades[type]++;
      upgradeCosts[type] = Math.floor(upgradeCosts[type] * 1.15);
    }
  };

  const reset = () => {
    clicks = 0;
    gems = 0;
    cps = 0;
    upgrades = { autoClick: 0, clickPower: 0 };
    upgradeCosts = { autoClick: 50, clickPower: 25 };
  };

  $: cps = upgrades.autoClick * 0.5;
</script>

<div class="container">
  <Canvas>
    <Scene on:click={clickGem} />
    <OrbitControls autoRotate />
  </Canvas>

  <HUD
    {gems}
    {cps}
    {upgrades}
    {upgradeCosts}
    on:click={clickGem}
    on:upgrade={(e) => buyUpgrade(e.detail)}
    on:reset={reset}
  />
</div>

<style>
  .container {
    position: relative;
    width: 100%;
    height: 100%;
  }
</style>
