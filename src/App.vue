<script setup>
import { ref, computed, onMounted } from 'vue'

const win_count = ref(0)
const draw_count = ref(0)
const loss_count = ref(0)

const choose_list = ref(null)
const AI_player = ref(null)
const text_notify = ref(null)

const reset_round = () => {
  choose_list.value = null
  AI_player.value = null
  text_notify.value = null
  win_count.value = 0
  draw_count.value = 0
  loss_count.value = 0
  save_value_game()
}

const control_game = {
	'ค้อน': {
		'ค้อน': 'เสมอ',
		'กระดาษ': 'แพ้',
		'กรรไกร': 'ชนะ'
	},
	'กระดาษ': {
		'ค้อน': 'ชนะ',
		'กระดาษ': 'เสมอ',
		'กรรไกร': 'แพ้'
	},
	'กรรไกร': {
		'ค้อน': 'แพ้',
		'กระดาษ': 'ชนะ',
		'กรรไกร': 'เสมอ'
	}
}

const play_game = (i) => {
	choose_list.value = i
	
	const choose_list_box = ['ค้อน', 'กระดาษ', 'กรรไกร']
	const random_value = Math.floor(Math.random() * choose_list_box.length)
	AI_player.value = choose_list_box[random_value]

	const control_game_edit = control_game [ choose_list.value][AI_player.value]
	
	if (control_game_edit === 'ชนะ') {
		win_count.value++
		text_notify.value = 'คุณชนะ'
	} else if (control_game_edit === 'แพ้') {
		loss_count.value++
		text_notify.value = 'คุณแพ้'
	} else {
		draw_count.value++
		text_notify.value = 'คุณเสมอ'
	}

	save_value_game()
}

const win_rate_value = computed(() => {
	const total_value = win_count.value + draw_count.value + loss_count.value
	return total_value ? (win_count.value / total_value) * 100 : 0
})

const save_value_game = () => {
	localStorage.setItem('win_count', win_count.value)
	localStorage.setItem('draw_count', draw_count.value)
	localStorage.setItem('loss_count', loss_count.value)
}

const load_value = () => {
	win_count.value = parseInt(localStorage.getItem('win_count')) || 0
	draw_count.value = parseInt(localStorage.getItem('draw_count')) || 0
	loss_count.value = parseInt(localStorage.getItem('loss_count')) || 0
}

const count_round = () => {
	choose_list.value = null
	AI_player.value = null
	text_notify.value = null
}

onMounted(() => {
	load_value()

	window.addEventListener('keypress', (c) => {
		if (c.key === 'r') {
			count_round()
		}
	})
})
</script>





<template>

	<div class="contrainer_all">
      <div class="box_title">
			<div class="text_title">เกมเป่ายิ๊งฉุบ แสนสนุก </div>
      </div>

		<main class="container_box">
			<div v-if="choose_list === null" class="contrain_box_list">
        <div>
				<button @click="play_game('ค้อน')" class="img_box">
					<img src="/img/3.png" />
				</button>
        </div>

        <div>
				<button @click="play_game('กระดาษ')" class="img_box">
					<img src="/img/1.png" />
				</button>
        </div>

        <div>
				<button @click="play_game('กรรไกร')" class="img_box">
					<img src="/img/2.png"  />
				</button>
        </div>

			</div>

			<div v-else>

				<div class="text_box">
					ผู้เล่น ได้ออก : <span class="text-pink-500">{{ choose_list }}</span>
				</div>

				<div class="text_box">
					AI player ได้ออก :  <span class="text-green-500">{{ AI_player }}</span>
				</div>
				<div class="text_box">{{ text_notify }}</div>

        <div class="box_restart">
				<button @click="count_round" class="buttom_restart"> อีกครั้ง </button>
        </div>
			</div>

			<div class="contrainer_score">
        <div class="box_score"> แต้มผู้เล่นชนะ <br> {{ win_count }}  </div>  
        <div class="box_score"> แต้มผู้เล่นแพ้ <br>{{ loss_count }} </div>
        <div class="box_score"> แต้มผู้เล่นเสมอ <br>{{ draw_count }} </div>

      </div>
			<div class="win_rate_box"> 
        อัตราการชนะ : {{ Math.round(win_rate_value) }}%
      </div>

      <div class="box_newgame">
        <button @click="reset_round" class="buttom_new"> เริ่มเกมใหม่ </button>
      </div>
      

		</main>
	</div>



</template>


<style>

</style>

