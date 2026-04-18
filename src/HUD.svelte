<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let gems = 0;
  export let cps = 0;
  export let upgrades = {};
  export let upgradeCosts = {};

  const handleUpgrade = (type) => {
    dispatch('upgrade', type);
  };

  const handleReset = () => {
    dispatch('reset');
  };

  const handleClick = () => {
    dispatch('click');
  };
</script>

<div class="hud">
  <div class="top-left">
    <div class="stat">
      <span class="label">Gems:</span>
      <span class="value">{Math.floor(gems)}</span>
    </div>
    <div class="stat">
      <span class="label">Gems/sec:</span>
      <span class="value">{cps.toFixed(1)}</span>
    </div>
  </div>

  <div class="center">
    <button class="big-button" on:click={handleClick}>
      CLICK!
    </button>
  </div>

  <div class="bottom-right">
    <div class="upgrades">
      <h3>Upgrades</h3>
      <button
        class="upgrade-btn"
        on:click={() => handleUpgrade('autoClick')}
        disabled={gems < upgradeCosts.autoClick}
      >
        <span>Auto-Click</span>
        <span class="level">{upgrades.autoClick}</span>
        <span class="cost">${upgradeCosts.autoClick}</span>
      </button>

      <button
        class="upgrade-btn"
        on:click={() => handleUpgrade('clickPower')}
        disabled={gems < upgradeCosts.clickPower}
      >
        <span>Click Power</span>
        <span class="level">{upgrades.clickPower}</span>
        <span class="cost">${upgradeCosts.clickPower}</span>
      </button>

      <button
        class="upgrade-btn"
        on:click={() => handleUpgrade('clickSpeed')}
        disabled={gems < upgradeCosts.clickSpeed}
      >
        <span>Click Speed</span>
        <span class="level">{upgrades.clickSpeed}</span>
        <span class="cost">${upgradeCosts.clickSpeed}</span>
      </button>

      <button class="reset-btn" on:click={handleReset}>Reset</button>
    </div>
  </div>
</div>

<style>
  .hud {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
  }

  .top-left,
  .center,
  .bottom-right {
    pointer-events: auto;
  }

  .top-left {
    position: absolute;
    top: 20px;
    left: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    background: rgba(0, 0, 0, 0.7);
    padding: 20px;
    border-radius: 8px;
    border: 2px solid #00ff88;
  }

  .stat {
    display: flex;
    justify-content: space-between;
    gap: 15px;
    font-size: 18px;
    font-weight: bold;
  }

  .label {
    color: #00ffff;
  }

  .value {
    color: #00ff88;
    font-size: 24px;
  }

  .center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .big-button {
    padding: 20px 40px;
    font-size: 24px;
    font-weight: bold;
    color: #000;
    background: linear-gradient(135deg, #00ff88, #00ffff);
    border: none;
    border-radius: 12px;
    cursor: pointer;
    box-shadow: 0 0 20px rgba(0, 255, 136, 0.6);
    transition: all 0.2s;
  }

  .big-button:hover {
    transform: scale(1.1);
    box-shadow: 0 0 30px rgba(0, 255, 136, 0.8);
  }

  .big-button:active {
    transform: scale(0.95);
  }

  .bottom-right {
    position: absolute;
    bottom: 20px;
    right: 20px;
    background: rgba(0, 0, 0, 0.8);
    padding: 20px;
    border-radius: 8px;
    border: 2px solid #00ff88;
  }

  .upgrades h3 {
    margin: 0 0 10px 0;
    color: #00ffff;
    font-size: 16px;
  }

  .upgrades {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .upgrade-btn,
  .reset-btn {
    padding: 10px 15px;
    background: rgba(0, 255, 136, 0.1);
    border: 1px solid #00ff88;
    color: #00ff88;
    border-radius: 6px;
    cursor: pointer;
    font-weight: bold;
    font-size: 12px;
    transition: all 0.2s;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
  }

  .upgrade-btn:hover:not(:disabled) {
    background: rgba(0, 255, 136, 0.3);
    box-shadow: 0 0 10px rgba(0, 255, 136, 0.4);
  }

  .upgrade-btn:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }

  .level {
    color: #00ffff;
    font-size: 10px;
  }

  .cost {
    color: #ff6600;
    font-size: 10px;
  }

  .reset-btn {
    margin-top: 10px;
    background: rgba(255, 100, 100, 0.1);
    border-color: #ff6464;
    color: #ff6464;
  }

  .reset-btn:hover {
    background: rgba(255, 100, 100, 0.3);
    box-shadow: 0 0 10px rgba(255, 100, 100, 0.4);
  }
</style>
