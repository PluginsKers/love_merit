<script setup lang="ts">
import { getCurrentInstance, onMounted, reactive, ref } from 'vue';

let merit = reactive({
	data: localStorage.getItem("merit") ? Number(localStorage.getItem("merit")) : 0,
	current: 1,
	list: new Array(),
	scale: 1,
});

function throttle(func: Function, time: number, immediate = false) {
	if (immediate) {
		let prevTime = 0;
		return (...args: any) => {
			let nowTime = Date.now();
			if (nowTime - prevTime >= time) {
				func.apply(throttle, args)
				prevTime = nowTime
			}
		}
	} else {
		let timer: number | null = null;
		return (...args: any) => {
			if (!timer) {
				func.apply(throttle, args)
				timer = window.setTimeout(() => {
					if (timer) clearInterval(timer)
					timer = null
				}, time);
			}
		}
	}
}

const subs = ['善有善报', '恶有恶报', '七级浮屠', '苦海无边', '回头是岸', '立地成佛', '道高一尺', '魔高一丈', '百尺竿头', '更进一步', '顽石点头', '磨砖作镜', '衣钵相传', '戒律清规', '大千世界', '拈花微笑', '老僧人定'];

function hit(probability: number, accuracy = 1000) {
	let _a = Array.from(Array(accuracy), () => 0);
	let excludes = [];
	for (let i = 0; i < probability * 0.01 * accuracy; i++) {
		_a[i] = 1;
	}
	function shuffle(arr: number[]) {
		let result = [],
			random;
		while (arr.length > 0) {
			random = Math.floor(Math.random() * arr.length);
			result.push(arr[random])
			arr.splice(random, 1)
		}
		return result;
	}
	let _r = shuffle(_a);
	if (_r[Math.round(Math.random() * _a.length)] === 1) return true;
	return false;
}

let player = new Audio('merit.mp3');

const togoodevil = throttle((n?: number) => {
	let hited: boolean = hit(8);
	merit.current = n ? n : (hited ? Math.round(Math.random() * 2) + 1 : 1);
	merit.scale = hited ? merit.current + 1 : 1.345;
	merit.data += merit.current;
	merit.list.push({
		left: Math.round(Math.random() * window.innerWidth) - Math.round(Math.random() * window.innerWidth) * (hited ? 0 : 0.3),
		top: Math.round(Math.random() * window.innerHeight) - Math.round(Math.random() * window.innerHeight) * (hited ? 0 : 0.3),
		size: Math.round(Math.random() * 24) + (hited ? 32 : 24),
		color: hited ? '#fbff08' : 'rgba(' + Math.floor(Math.random() * 255) + ',' + Math.floor(Math.random() * 255) + ',' + Math.floor(Math.random() * 255) + ',0.8)',
		weight: hited ? 800 : 300,
		shadow: hited ? '0 5px 4px black' : 'none',
		merit: merit.current,
		perfix: hited ? subs[Math.round(Math.random() * subs.length)] : '功德'
	});
	setTimeout(() => {
		merit.list.shift();
	}, Math.round(Math.random() * 100000) + 5000);

	if (player.played) player = new Audio('merit.mp3');
	if (player.paused) player.play();
	setTimeout(() => {
		merit.scale = 1;
		player.remove();
	}, 60);
}, 300);
</script>

<template>
	<div class="touch-overlay" @click="togoodevil()"></div>
	<img src="./assets/wooden_fish.svg" class="wooden_fish" alt="merit++" @click=""
		:style="{transform: `scale(`+ merit.scale + `)`}" />
	<ul class="merits">
		<li v-for="v,index of merit.list"
			:style="{top: v.top + 'px', left: v.left + 'px', fontSize: v.size + 'px', color: v.color, textShadow: v.shadow, fontWeight: v.weight}">
			{{ v.perfix }}+{{v.merit}}
		</li>
	</ul>
	<h1 class="mymerits">{{merit.data}}</h1>
</template>

<style scoped>
.touch-overlay {
	z-index: 99;
}

img.wooden_fish {
	position: relative;
	pointer-events: none;
	transition: all .1s;
	z-index: 1;
}

.touch-overlay,
.merits {
	position: absolute;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
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
	white-space: nowrap;
	font-family: HarmonyOS_Regular;
	font-size: 56px;
	line-height: 64px;
	color: var(--text-p1);
	opacity: 0.3;
}

* {
	overflow: hidden !important;
}

.mymerits {
	position: fixed;
	bottom: 0;
	right: 20px;
	font-weight: 100;
	font-size: 30px;
}
</style>