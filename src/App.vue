<script setup lang="ts">
import { getCurrentInstance, onMounted, reactive, ref } from 'vue';

let merit = reactive({
	data: localStorage.getItem("merit") ? Number(localStorage.getItem("merit")) : 0,
	current: 1,
	list: new Array(),
});

let todogood = (n?: number) => {
	merit.current = n ? n : Math.round((Math.random() * 0) + 1);
	merit.data += merit.current;
	merit.list.push({
		x: Math.round(Math.random() * window.innerWidth * 0.8) + 10,
		y: Math.round(Math.random() * window.innerHeight),
		merit: merit.current,
	});
	setTimeout(() => {
		merit.list.shift();
	}, Math.round(Math.random() * 10000) + 1000);

	let a = new Audio('do.mp3');
	a.play();
}
</script>

<template>
	<div class="overlay" @click="todogood()"></div>
	<img src="./assets/wooden_fish.svg" class="wooden_fish" alt="merit++" @click="" />
	<ul class="merits">
		<li v-for="v of merit.list" :style="{top: v.y + 'px', left: v.x + 'px'}">
			功德+{{v.merit}}
		</li>
	</ul>
	<h1 class="mymerits">我的功德：{{merit.data}}</h1>
</template>

<style scoped>
:root {
	--site-bg: #f8f8f8;
	--block: #f2f2f2;
	--block-border: #e5e5e5;
	--block-hover: #ededed;
	--text-p0: #000;
	--text-p1: #333;
	--text-p2: #5a5a5a;
	--text-p3: #818181;
	--text-p4: #b3b3b3;
	--text-meta: #d0d0d0;
	--text-code: #111;
	--card: #fff;
	--theme-highlight: #03c7fa;
	--theme-bg: #e8fafe;
	--swiper-theme-color: #1bcdfc !important;
	--blur-px: 12px;
	--blur-bg: rgba(255, 255, 255, 0.5);
	--width-left: 256px;
	--width-main: 720px;
	--gap-l: 16px;
	--gap-p: 0.75rem;
}

body {
	background: var(--site-bg);
	margin: 0;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-rendering: optimizelegibility;
	-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

html {
	font-family: HarmonyOS_Regular, system-ui, "Microsoft Yahei", "Segoe UI", -apple-system, Roboto, Ubuntu, "Helvetica Neue", Arial, "WenQuanYi Micro Hei", sans-serif;
	font-size: 16px;
	-webkit-text-size-adjust: 100%;
	-ms-text-size-adjust: 100%;
	scroll-behavior: smooth;
}

.overlay {
	z-index: 99;
}

img.wooden_fish {
	position: relative;
	cursor: pointer;
	z-index: 1;
}

.overlay:active+img.wooden_fish {
	transform: scale(1.1);
}

.overlay,
.merits {
	position: absolute;
	left: 0;
	top: 0;
	width: 100%;
	height: 100%;
}

.merits {
	margin: 0;
	list-style-type: none;
	padding: 0;
	z-index: 2;
}

.merits li {
	position: absolute;
	font-family: HarmonyOS_Regular;
	font-size: 32px;
	line-height: 64px;
	color: var(--text-p1);
	opacity: 0.7;
}

* {
	overflow: hidden;
}

.mymerits {
	position: fixed;
	top: 0;
	left: 10px;
	font-size: 24px;
}
</style>