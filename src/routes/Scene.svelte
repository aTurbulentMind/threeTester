<script>
	import { T, useTask, useLoader } from '@threlte/core';
	import { interactivity } from '@threlte/extras';
	import { Spring } from 'svelte/motion';
	import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

	interactivity();

	const gltf = useLoader(GLTFLoader).load('/src/lib/assets/models/testObject.glb');

	const scale = new Spring(1);

	let rotation = 0;
	useTask((delta) => {
		rotation += delta / 2;
	});
</script>

<T.PerspectiveCamera
	makeDefault
	position={[10, 10, 10]}
	oncreate={(ref) => {
		ref.lookAt(0, 1, 0);
	}}
/>

<T.DirectionalLight position={[0, 10, 10]} castShadow />
<!-- 
<T.Mesh
	rotation.y={rotation}
	position.y={1}
	scale={scale.current}
	onpointerenter={() => {
		scale.target = 1.5;
	}}
	onpointerleave={() => {
		scale.target = 1;
	}}
	castShadow
>
	<T.BoxGeometry args={[1, 2, 1]} />
	<T.MeshStandardMaterial color="hotpink" />
</T.Mesh>

<T.Mesh rotation.x={-Math.PI / 2} receiveShadow>
	<T.CircleGeometry args={[4, 40]} />
	<T.MeshStandardMaterial color="white" />
</T.Mesh> -->

{#if $gltf}
	<T
		is={$gltf.scene}
		position={[0, 0, 0]}
		rotation.z={rotation}
		rotation.x={rotation}
		scale={scale.current}
		onpointerenter={() => {
			scale.target = 1.5;
		}}
		onpointerleave={() => {
			scale.target = 1;
		}}
		castShadow
	/>
{/if}
