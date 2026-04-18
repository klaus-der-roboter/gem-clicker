<script>
  import { T } from '@threlte/core';
  import { createEventDispatcher, onMount } from 'svelte';
  import { Vector3, MeshPhongMaterial, BoxGeometry } from 'three';

  const dispatch = createEventDispatcher();

  let gemRef;
  let rotation = new Vector3(0, 0, 0);
  let scale = 1;
  let isHovered = false;

  onMount(() => {
    const animate = () => {
      if (gemRef) {
        rotation.y += 0.005;
        rotation.z = Math.sin(Date.now() * 0.001) * 0.3;
        if (gemRef.object) {
          gemRef.object.rotation.order = 'YXZ';
          gemRef.object.rotation.y = rotation.y;
          gemRef.object.rotation.z = rotation.z;
        }
        scale = isHovered ? 1.2 : 1;
        if (gemRef.object) {
          gemRef.object.scale.set(scale, scale, scale);
        }
      }
      requestAnimationFrame(animate);
    };
    animate();
  });

  const handleClick = () => {
    dispatch('click');
  };

  const handlePointerOver = () => {
    isHovered = true;
  };

  const handlePointerOut = () => {
    isHovered = false;
  };
</script>

<T.Group>
  <!-- Ambient Light -->
  <T.AmbientLight intensity={0.6} />

  <!-- Directional Light -->
  <T.DirectionalLight position={[5, 10, 7]} intensity={0.8} />

  <!-- Point Light for gem glow -->
  <T.PointLight position={[0, 0, 5]} intensity={1} color="#00ff88" />

  <!-- The Clickable Gem -->
  <T.Mesh
    bind:ref={gemRef}
    on:click={handleClick}
    on:pointerover={handlePointerOver}
    on:pointerout={handlePointerOut}
    interactive
  >
    <T.IcosahedronGeometry args={[2, 6]} />
    <T.MeshPhongMaterial
      color="#00ff88"
      emissive="#00ff88"
      emissiveIntensity={0.5}
      wireframe={false}
      shininess={100}
    />
  </T.Mesh>

  <!-- Floating particles around gem -->
  {#each Array(8) as _, i}
    <T.Mesh position={[Math.cos((i / 8) * Math.PI * 2) * 5, Math.sin((i / 8) * Math.PI * 2) * 3, 0]}>
      <T.SphereGeometry args={[0.15, 8, 8]} />
      <T.MeshPhongMaterial color="#00ffff" emissive="#0088ff" />
    </T.Mesh>
  {/each}
</T.Group>
