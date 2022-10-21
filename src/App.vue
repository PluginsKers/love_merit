<script setup lang="ts">
import { getCurrentInstance, onMounted, reactive, ref } from 'vue';

let merit = reactive({
	data: localStorage.getItem("merit") ? Number(localStorage.getItem("merit")) : 0,
	current: 1,
	list: new Array(),
	scale: 1,
	raw: "",
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

const subs = ['善有善报', '恶有恶报', '七级浮屠', '苦海无边', '回头是岸', '立地成佛', '百尺竿头', '更进一步', '顽石点头', '磨砖作镜', '衣钵相传', '戒律清规', '大千世界', '拈花微笑', '老僧人定'];

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

let players = new Array(new Audio('merit.mp3'));

function shift() {
	merit.list.shift();
}

const togoodevil = throttle((n?: number) => {
	let hited: boolean = hit(3);
	merit.current = n ? n : (hited ? Math.round(Math.random() * 2) + 2 : 1);
	merit.scale = hited ? merit.current * 0.5 + 1 : 1.345;
	merit.data += merit.current;
	let _d = {
		size: Math.round(Math.random() * 24) + (hited ? 32 : 24) + window.innerWidth * 0.02,
		left: Math.round(Math.random() * window.innerWidth) - Math.round(Math.random() * window.innerWidth) * (hited ? 0 : 0.3),
		top: Math.round(Math.random() * window.innerHeight) - Math.round(Math.random() * window.innerHeight) * (hited ? 0 : 0.3),
		color: hited ? 'rgb(251, 255, 8, 0.9)' : 'rgba(' + Math.floor(Math.random() * 255) + ',' + Math.floor(Math.random() * 255) + ',' + Math.floor(Math.random() * 255) + ', 0.3)',
		weight: hited ? 800 : 300,
		shadow: hited ? '0 2px 4px rgb(0, 0, 0, 0.6)' : '0 2px 4px rgba(0, 0, 0, 0.6)',
		merit: merit.current,
		perfix: hited ? subs[Math.round(Math.random() * subs.length)] : '功德'
	}

	_d.left -= _d.size;

	if (merit.list.length > 50) shift();

	merit.list.push(_d);

	setTimeout(shift, Math.round(Math.random() * 100000) + 5000);

	for (let v = 0; v < players.length; v++) {
		if (players[v] && players[v].paused) {
			players[v].play();
			break;
		} else if (v === players.length - 1) {
			players[v + 1] = new Audio('merit.mp3');
			players[v + 1].play();
			break;
		} else continue;
	}
	setTimeout(() => {
		merit.scale = 1;
	}, 60);
}, 60);
</script>

<template>
	<div class="touch-overlay" @click="togoodevil()"></div>
	<svg version="1.0" xmlns="http://www.w3.org/2000/svg" width="260.000000pt" height="260.000000pt"
		viewBox="0 0 260.000000 260.000000" preserveAspectRatio="xMidYMid meet" class="wooden_fish"
		:style="{transform: `scale(`+ merit.scale + `)`}">
		<g transform="translate(0.000000,260.000000) scale(0.100000,-0.100000)" fill="#000000" stroke="none">
			<path
				d="M944 1891 c-108 -30 -164 -60 -230 -126 -69 -68 -118 -157 -135 -246 -6 -32 -13 -59 -14 -59 -2 0 -52 16 -111 36 -127 43 -199 50 -199 19 0 -16 25 -29 139 -71 76 -29 145 -59 154 -68 8 -8 23 -46 32 -83 17 -67 17 -67 -6 -103 -30 -45 -48 -130 -40 -185 9 -56 63 -118 158 -180 265 -175 582 -193 871 -48 90 45 86 51 -5 8 -107 -51 -215 -76 -350 -82 -133 -6 -204 5 -310 48 -131 54 -254 131 -242 151 4 7 2 8 -4 4 -28 -17 -102 96 -102 157 0 37 23 106 42 126 l19 22 47 -46 c57 -54 151 -105 252 -136 109 -34 275 -33 380 1 104 33 192 33 245 0 36 -22 51 -24 158 -25 67 -1 122 -6 128 -12 10 -10 -40 -65 -106 -114 -49 -36 -53 -44 -10 -19 66 39 119 76 139 99 28 30 44 26 55 -15 16 -57 59 -111 107 -135 55 -26 181 -36 243 -19 56 15 101 59 101 97 0 59 -84 158 -173 205 -56 29 -159 31 -200 4 -37 -25 -29 -30 14 -9 135 69 392 -118 328 -238 -10 -19 -14 -17 -72 38 -112 106 -152 134 -232 158 -42 13 -82 22 -89 19 -10 -4 -14 21 -19 99 -11 180 -72 345 -165 448 -99 111 -288 228 -434 269 -108 31 -273 36 -364 11z m106 -21 c-25 -20 -81 -40 -111 -40 -8 0 -24 6 -34 14 -18 13 -16 15 25 26 25 6 50 13 55 15 6 2 28 4 50 4 l40 1 -25 -20z m117 -19 c59 -53 131 -71 187 -47 52 22 65 20 72 -9 11 -46 132 -135 183 -135 16 0 31 6 34 13 5 15 37 -9 37 -29 0 -32 -42 -44 -150 -44 -197 0 -376 44 -551 136 -98 52 -139 88 -122 105 8 8 19 5 41 -11 25 -18 38 -21 68 -15 45 8 79 25 113 54 13 12 28 21 33 21 6 0 30 -18 55 -39z m152 -22 c40 0 43 -2 23 -11 -29 -14 -121 5 -156 33 -44 35 -28 39 30 8 38 -19 71 -29 103 -30z m-29 31 l25 -8 -25 -1 c-14 0 -32 4 -40 9 -13 9 -13 10 0 9 8 -1 26 -5 40 -9z m-455 -40 c14 -23 128 -92 221 -133 109 -49 289 -96 400 -104 103 -8 201 4 225 28 18 18 39 18 39 0 0 -4 -20 -17 -45 -29 -49 -24 -54 -35 -28 -64 25 -28 77 -24 102 9 12 14 24 24 27 20 9 -9 -25 -66 -47 -78 -26 -14 -80 -4 -109 19 -22 17 -23 17 -55 -15 -48 -48 -117 -43 -177 14 l-28 27 -26 -25 c-50 -47 -159 -20 -195 49 -9 15 -18 20 -28 16 -9 -4 -39 -9 -68 -11 -42 -4 -59 -1 -87 17 -49 29 -76 71 -76 114 l0 36 -50 0 c-42 0 -55 5 -75 26 -33 35 -20 53 14 20 30 -29 49 -32 75 -13 16 12 15 14 -8 36 -26 24 -34 51 -17 51 6 0 13 -5 16 -10z m-30 -40 c18 -19 17 -20 -3 -20 -12 0 -22 2 -22 4 0 2 -3 11 -6 20 -9 22 9 20 31 -4z m722 -17 c91 -56 109 -70 101 -78 -15 -16 -81 8 -130 48 -90 73 -72 92 29 30z m-784 -37 c17 -25 78 -51 100 -43 8 3 19 -13 30 -43 11 -29 33 -62 58 -85 40 -36 42 -37 115 -34 71 3 75 2 106 -28 58 -56 134 -70 181 -33 26 21 47 26 47 12 0 -17 79 -52 118 -52 30 0 47 7 71 30 l32 29 32 -19 c50 -31 90 -26 130 15 34 35 34 35 45 12 46 -90 82 -234 82 -325 0 -74 -9 -87 -46 -75 -24 8 -30 17 -35 49 -11 79 -56 117 -136 116 -35 -1 -62 -13 -140 -63 -148 -93 -153 -96 -228 -125 -55 -21 -93 -27 -180 -31 -136 -6 -219 11 -325 68 -92 50 -155 108 -186 174 -42 88 -29 263 27 362 29 51 72 113 80 113 4 0 14 -11 22 -24z m1001 -173 c-17 -27 -39 -36 -67 -29 -37 9 -34 19 16 43 45 22 70 16 51 -14z m-1286 -73 c91 -29 102 -35 102 -56 0 -13 -3 -24 -7 -24 -4 0 -67 23 -140 51 -197 77 -167 96 45 29z m1283 -265 c31 -17 49 -49 49 -90 0 -22 -2 -23 -34 -13 -29 9 -33 14 -27 32 9 24 4 44 -17 69 -19 21 -10 22 29 2z m-68 -4 c31 -12 44 -48 26 -66 -12 -11 -25 -10 -77 6 -65 21 -67 28 -17 57 25 14 37 15 68 3z m-9 -95 c40 -14 110 -34 155 -46 78 -21 90 -31 73 -57 -9 -14 -85 2 -289 59 -111 3 -126 41 -100 67 15 16 62 9 161 -23z m-49 -73 c76 -22 77 -23 30 -19 -27 2 -75 16 -105 30 -30 14 -44 23 -30 19 14 -4 61 -17 105 -30z" />
			<path
				d="M1080 1800 c-20 -12 -1 -45 26 -45 22 0 32 34 12 46 -17 11 -20 11 -38 -1z m40 -20 c0 -5 -6 -10 -14 -10 -8 0 -18 5 -21 10 -3 6 3 10 14 10 12 0 21 -4 21 -10z" />
			<path
				d="M1214 1772 c-20 -14 -44 -57 -44 -79 0 -24 26 -43 59 -43 37 0 63 66 45 114 -6 17 -41 21 -60 8z m44 -31 c9 -57 -30 -99 -63 -71 -22 18 -18 41 12 72 31 32 46 32 51 -1z" />
			<path
				d="M1364 1705 c-8 -19 13 -45 36 -45 23 0 34 28 19 46 -15 19 -48 18 -55 -1z m46 -10 c10 -13 9 -15 -9 -15 -12 0 -21 6 -21 15 0 8 4 15 9 15 5 0 14 -7 21 -15z" />
			<path
				d="M920 1667 c0 -55 74 -107 121 -83 43 21 33 39 -38 71 -36 17 -70 32 -75 33 -4 2 -8 -7 -8 -21z m75 -29 c51 -25 57 -38 18 -38 -25 0 -73 34 -73 52 0 12 4 10 55 -14z" />
			<path
				d="M1160 1575 c0 -28 55 -75 87 -75 27 0 73 31 73 48 0 4 -26 13 -57 19 -32 7 -68 15 -80 19 -18 5 -23 2 -23 -11z m114 -48 c-21 -12 -73 2 -84 22 -9 17 -7 17 45 2 39 -11 50 -18 39 -24z" />
			<path
				d="M1400 1535 c0 -25 38 -55 69 -55 32 0 81 34 81 56 0 11 -17 14 -75 14 -60 0 -75 -3 -75 -15z m120 -14 c0 -23 -58 -27 -86 -6 -17 13 -15 14 34 15 28 0 52 -4 52 -9z" />
		</g>
	</svg>
	<ul class="merits" ref="merits">
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

.wooden_fish {
	position: relative;
	pointer-events: none;
	transition: all .1s;
	z-index: 8;
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
	z-index: 1;
}

.merits li {
	font-family: STXinwei;
	position: absolute;
	white-space: nowrap;
	font-size: 56px;
	line-height: 64px;
	overflow: visible;
	color: var(--text-p1);
	opacity: 0.8;
	z-index: 1;
}

.mymerits {
	font-family: STXinwei;
	position: fixed;
	bottom: 0;
	right: 20px;
	font-weight: 100;
	font-size: 30px;
}
</style>