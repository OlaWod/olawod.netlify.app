---
title: Make a Hand Animation with SkinnedMesh, three.js
date: "2020-09-20T11:44:00.000Z"
template: "post"
draft: false
slug: "hand-animation-w-skinnedmesh-three.js"
category: "Unarchived"
tags:
  - " "
description: "A simple guide on how to make a hand animation with SkinnedMesh, three.js. (No model loading, built with only basic geometries.)"
socialImage: 
  -"/media/hand.gif"
  -"/media/hand-geometry.png"
  -"/media/hand-geometry-bones.jpg"
  -"/media/hand-geometry-vertexes.png"
---

SkinnedMesh is a mesh that has a Skeleton with bones that can then be used to animate the vertices of the geometry. The material must support skinning and have skinning enabled - citing from the [official website of three.js](https://threejs.org/docs/index.html#api/en/objects/SkinnedMesh) .

In this post I am going to describe how to build a hand animation like this one 👇 with SkinnedMesh, three.js.

![Hand animation.](/media/hand.gif)

<figcaption>Code is available at <a href="https://codepen.io/olawod/pen/poyxMge" target="_blank" rel="noopener noreferrer">https://codepen.io/olawod/pen/poyxMge</a> .</figcaption>

## 1 Before we start

Before you can use three.js, you need somewhere to display it. Save the following HTML to a file on your computer, along with a copy of [three.js](https://threejs.org/build/three.js) in the js/ directory, and open it in your browser.
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>My first three.js app</title>
		<style>
			body { margin: 0; }
			canvas { display: block; }
		</style>
	</head>
	<body>
		<script src="js/three.js"></script>
		<script>
			// Our Javascript will go here.
		</script>
	</body>
</html>
```
That's all. All the code below goes into the empty `<script>` tag.

(Lazy as I was, I copied the above part from the [official website of three.js](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene) .)

(Here's an available source of three.js : <a href="https://threejs.org/build/three.js" target="_blank" rel="noopener noreferrer">https://threejs.org/build/three.js</a> .)

## 2 Creating the scene

```javascript
// scene, camera, renderer
var scene = new THREE.Scene();

var camera = new THREE.PerspectiveCamera(
  60,
  window.innerWidth / window.innerHeight,
  1,
  100
);
camera.position.set(10, 20, -40);
camera.lookAt(new THREE.Vector3(0, 5, 0));

var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

## 3 Build our hand geometry

![Hand geometry.](/media/hand-geometry.png)

```javascript
// build geometry
let palm = new THREE.BoxGeometry(16, 20, 4); // 手掌
let finger1 = new THREE.CylinderGeometry(2, 2, 4 * Math.sqrt(3) * 1.4, 8, 2); // 大拇指
finger1.rotateZ(-Math.PI / 3);
let finger2 = new THREE.CylinderGeometry(2, 2, 12, 8, 3); // 食指
let finger3 = new THREE.CylinderGeometry(2, 2, 15, 8, 3); // 中指
let finger4 = new THREE.CylinderGeometry(2, 2, 12, 8, 3); // 无名指
let finger5 = new THREE.CylinderGeometry(2, 2, 9, 8, 3); // 小拇指

palm.merge(
  finger1,
  new THREE.Matrix4().makeTranslation(11, 2 * Math.sqrt(3) * 1.4, 0)
);
palm.merge(finger2, new THREE.Matrix4().makeTranslation(6, 15, 0));
palm.merge(finger3, new THREE.Matrix4().makeTranslation(2, 16.5, 0));
palm.merge(finger4, new THREE.Matrix4().makeTranslation(-2, 15, 0));
palm.merge(finger5, new THREE.Matrix4().makeTranslation(-6, 13.5, 0));
let hand = new THREE.BufferGeometry().fromGeometry(palm);
```

## 4 Assign bones

Note : joints are called 'bones' here.

![Hand geometry bones.](/media/hand-geometry-bones.jpg)

```javascript
// bones
let bones = [];
let bone0 = new THREE.Bone(); // palm
let bone1 = new THREE.Bone(); // finger1-0
bone1.position.y = Math.sqrt(3) * 1.4;
bone1.position.x = 11 - 3 * 1.4;
let bone2 = new THREE.Bone(); // finger1-1
bone2.position.y = Math.sqrt(3) * 1.4;
bone2.position.x = 3 * 1.4;
let bone3 = new THREE.Bone(); // finger1-2
bone3.position.y = Math.sqrt(3) * 1.4;
bone3.position.x = 3 * 1.4;
let bone4 = new THREE.Bone(); // finger2-0
bone4.position.y = 9;
bone4.position.x = 6;
let bone5 = new THREE.Bone(); // finger2-1
bone5.position.y = 4;
let bone6 = new THREE.Bone(); // finger2-2
bone6.position.y = 4;
let bone7 = new THREE.Bone(); // finger2-3
bone7.position.y = 4;
let bone8 = new THREE.Bone(); // finger3-0
bone8.position.y = 9;
bone8.position.x = 2;
let bone9 = new THREE.Bone(); // finger3-1
bone9.position.y = 5;
let bone10 = new THREE.Bone(); // finger3-2
bone10.position.y = 5;
let bone11 = new THREE.Bone(); // finger3-3
bone11.position.y = 5;
let bone12 = new THREE.Bone(); // finger4-0
bone12.position.y = 9;
bone12.position.x = -2;
let bone13 = new THREE.Bone(); // finger4-1
bone13.position.y = 4;
let bone14 = new THREE.Bone(); // finger4-2
bone14.position.y = 4;
let bone15 = new THREE.Bone(); // finger4-3
bone15.position.y = 4;
let bone16 = new THREE.Bone(); // finger5-0
bone16.position.y = 9;
bone16.position.x = -6;
let bone17 = new THREE.Bone(); // finger5-1
bone17.position.y = 3;
let bone18 = new THREE.Bone(); // finger5-2
bone18.position.y = 3;
let bone19 = new THREE.Bone(); // finger5-3
bone19.position.y = 3;

bone0.add(bone1);
bone0.add(bone4);
bone0.add(bone8);
bone0.add(bone12);
bone0.add(bone16);

bone1.add(bone2);
bone2.add(bone3);

bone4.add(bone5);
bone5.add(bone6);
bone6.add(bone7);

bone8.add(bone9);
bone9.add(bone10);
bone10.add(bone11);

bone12.add(bone13);
bone13.add(bone14);
bone14.add(bone15);

bone16.add(bone17);
bone17.add(bone18);
bone18.add(bone19);

bones.push(bone0);
bones.push(bone1);
bones.push(bone2);
bones.push(bone3);
bones.push(bone4);
bones.push(bone5);
bones.push(bone6);
bones.push(bone7);
bones.push(bone8);
bones.push(bone9);
bones.push(bone10);
bones.push(bone11);
bones.push(bone12);
bones.push(bone13);
bones.push(bone14);
bones.push(bone15);
bones.push(bone16);
bones.push(bone17);
bones.push(bone18);
bones.push(bone19);
```

## 5 Assign skinIndices & skinWeights of vertexes

Assign skinIndices and skinWeights of these vertexes 👇.

![Hand geometry vertexes.](/media/hand-geometry-vertexes.png)

For example, if vertex-a's skinIndice is (8, 12, 0, 0) and skinWeight is (0.5, 0.5, 0, 0), that means vertex-a is influenced 50% by bone-8 and 50% by bone-12.

```javascript
// skinIndices & skinWeights of hand vertexes
var position = hand.attributes.position;
var vertex = new THREE.Vector3();
var skinIndices = [];
var skinWeights = [];

for (let i = 0; i < position.count; i++) {
  vertex.fromBufferAttribute(position, i);

  if (
    vertex.y > 0 &&
    vertex.y < 4.85 &&
    vertex.x < 11 - 2 * 1.4 &&
    vertex.x > 11 - 4 * 1.4
  ) {
    // finger1
    skinIndices.push(1, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (
    vertex.y > 2.4 &&
    vertex.y < 7.28 &&
    vertex.x < 11 + 1.4 &&
    vertex.x > 11 - 1.4
  ) {
    skinIndices.push(2, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (
    vertex.y > 4.8 &&
    vertex.y < 9.7 &&
    vertex.x < 11 + 4 * 1.4 &&
    vertex.x > 11 + 2 * 1.4
  ) {
    skinIndices.push(3, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 9 && vertex.x <= 8 && vertex.x >= 4) {
    // finger2
    skinIndices.push(4, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 13 && vertex.x <= 8 && vertex.x >= 4) {
    skinIndices.push(5, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 17 && vertex.x <= 8 && vertex.x >= 4) {
    skinIndices.push(6, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 21 && vertex.x <= 8 && vertex.x >= 4) {
    skinIndices.push(7, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 9 && vertex.x <= 4 && vertex.x >= 0) {
    // finger3
    skinIndices.push(8, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 14 && vertex.x <= 4 && vertex.x >= 0) {
    skinIndices.push(9, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 19 && vertex.x <= 4 && vertex.x >= 0) {
    skinIndices.push(10, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 24 && vertex.x <= 4 && vertex.x >= 0) {
    skinIndices.push(11, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 9 && vertex.x <= 0 && vertex.x >= -4) {
    // finger4
    skinIndices.push(12, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 13 && vertex.x <= 0 && vertex.x >= -4) {
    skinIndices.push(13, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 17 && vertex.x <= 0 && vertex.x >= -4) {
    skinIndices.push(14, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 21 && vertex.x <= 0 && vertex.x >= -4) {
    skinIndices.push(15, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 9 && vertex.x <= -4 && vertex.x >= -8) {
    // finger5
    skinIndices.push(16, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 12 && vertex.x <= -4 && vertex.x >= -8) {
    skinIndices.push(17, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 15 && vertex.x <= -4 && vertex.x >= -8) {
    skinIndices.push(18, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 18 && vertex.x <= -4 && vertex.x >= -8) {
    skinIndices.push(19, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  } else if (vertex.y == 9 && vertex.x == 4) {
    // finger2,3
    skinIndices.push(4, 8, 0, 0);
    skinWeights.push(0.5, 0.5, 0, 0);
  } else if (vertex.y == 9 && vertex.x == 0) {
    // finger3,4
    skinIndices.push(8, 12, 0, 0);
    skinWeights.push(0.5, 0.5, 0, 0);
  } else if (vertex.y == 9 && vertex.x == -4) {
    // finger4,5
    skinIndices.push(12, 16, 0, 0);
    skinWeights.push(0.5, 0.5, 0, 0);
  } else {
    skinIndices.push(0, 0, 0, 0);
    skinWeights.push(1, 0, 0, 0);
  }
}

hand.setAttribute("skinIndex", new THREE.Uint16BufferAttribute(skinIndices, 4));
hand.setAttribute(
  "skinWeight",
  new THREE.Float32BufferAttribute(skinWeights, 4)
);
```

## 6 Build mesh and add to scene

```javascript
// build SkinnedMesh with hand-geometry and bones
var material = new THREE.MeshNormalMaterial({
  skinning: true, // 不加这个的话，bone的变形就不起作用了
  side: THREE.DoubleSide,
  shading: THREE.FlatShading
  //transparent: true, opacity: 0.5,
});
var mesh = new THREE.SkinnedMesh(hand, material);

var skeleton = new THREE.Skeleton(bones);
var rootBone = skeleton.bones[0];
mesh.add(rootBone);
mesh.bind(skeleton);

scene.add(mesh);

// skeletonHelper
var skeletonHelper = new THREE.SkeletonHelper(mesh);
scene.add(skeletonHelper);
```

## 7 Animation

```javascript
// animate
skeleton.bones[2].rotation.y += 0.5;
skeleton.bones[5].rotation.x -= 0.5;
skeleton.bones[6].rotation.x -= 0.5;
skeleton.bones[9].rotation.x -= 0.5;
skeleton.bones[10].rotation.x -= 0.5;
skeleton.bones[13].rotation.x -= 0.5;
skeleton.bones[14].rotation.x -= 0.5;
skeleton.bones[17].rotation.x -= 0.5;
skeleton.bones[18].rotation.x -= 0.5;

var isClosing = true;

var animate = function () {
  requestAnimationFrame(animate);

  mesh.rotation.y += 0.1 / 2;

  if (isClosing) {
    skeleton.bones[1].rotation.y += 0.1 / 2;
    skeleton.bones[4].rotation.x -= 0.15 / 2;
    skeleton.bones[8].rotation.x -= 0.1 / 2;
    skeleton.bones[12].rotation.x -= 0.15 / 2;
    skeleton.bones[16].rotation.x -= 0.1 / 2;
  } else {
    skeleton.bones[1].rotation.y -= 0.1 / 2;
    skeleton.bones[4].rotation.x += 0.15 / 2;
    skeleton.bones[8].rotation.x += 0.1 / 2;
    skeleton.bones[12].rotation.x += 0.15 / 2;
    skeleton.bones[16].rotation.x += 0.1 / 2;
  }
  if (skeleton.bones[1].rotation.y > 1.5) isClosing = false;
  if (skeleton.bones[1].rotation.y < 0) isClosing = true;

  renderer.render(scene, camera);
};

animate();
```



Once again, the full code is available at <a href="https://codepen.io/olawod/pen/poyxMge" target="_blank" rel="noopener noreferrer">https://codepen.io/olawod/pen/poyxMge</a> .

That's all, thanks for reading!