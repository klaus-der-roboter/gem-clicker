<script>
  import { Canvas } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';
  import { MeshPhongMaterial, BoxGeometry, SphereGeometry } from 'three';
  import Scene from './Scene.svelte';
  import HUD from './HUD.svelte';

  let clicks = 0;
  let gems = 0;
  let cps = 0; // gems per second
  let upgrades = {
    autoClick: 0,
    clickPower: 0,
    clickSpeed: 0
  };
  let upgradeCosts = {
    autoClick: 50,
    clickPower: 25,
    clickSpeed: 40
  };

  const clickGem = () => {
    clicks++;
    const power = 1 + upgrades.clickPower * 0.5;
    gems += power;
    cps = upgrades.autoClick * 0.5;
  };

  const buyUpgrade = (type) => {
    if (gems >= upgradeCosts[type]) {
      gems -= upgradeCosts[type];
      upgrades[type]++;
      upgradeCosts[type] = Math.floor(upgradeCosts[type] * 1.15);
      cps = upgrades.autoClick * 0.5;
    }
  };

  const reset = () => {
    clicks = 0;
    gems = 0;
    cps = 0;
    upgrades = { autoClick: 0, clickPower: 0, clickSpeed: 0 };
    upgradeCosts = { autoClick: 50, clickPower: 25, clickSpeed: 40 };
  };
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
