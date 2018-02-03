<template>
	<div id="training">
		<h3>Математический тренажёр. Уровень {{ level + 1 }}</h3>
		<hr>
		<div class="progress">
			<div class="progress-bar" :style="progressStyles"></div>
		</div>
		<div class="box">
			<transition name='flip' mode="out-in">		
				<AppStartScreen
					v-if="state == 'start'"
					@onStart="onStart"
				>
				</AppStartScreen>

				<AppResultScreen v-else-if="state == 'result'"
								 :stats="stats"
								 @repeat="onStart"
								 @nextLevel="onNextLevel"
				>	
				</AppResultScreen>
				<AppQuestion v-else-if="state == 'question'"
							 @success="onQuestSuccess"
							 @error="onQuestError"
							 :settings="levels[level]"
				>
					
				</AppQuestion>
				<AppMessage v-else-if="state == 'message'"
							:type="message.type"
							:text="message.text"
							@onNext="onNext"
				>
					
				</AppMessage>
				<div v-else>Unknow state</div>
			</transition>
		</div>
	</div>
</template>

<script>
export default {
  data() {
    return {
      state: 'start',
      stats: {
      	success: 0,
      	error: 0
      },
      message: {
      	type: '',
      	text: ''
      },
      questMax: 3,
      level: 0,
      levels: [
      	{
      		variants: 3,
      		from: 10,
      		to: 40,
      		range: 5 
      	},
      	{
      		variants: 4,
      		from: 100,
      		to: 200,
      		range: 20
      	},
      	{
      		variants: 6,
      		from: 500,
      		to: 2000,
      		range: 40
      	}
      ]
    }
  },
  computed: {
  	questDone() {
  		return this.stats.success + this.stats.error
  	},
  	progressStyles() {
  		return {
  			width: (this.questDone / this.questMax * 100) + '%'
  		}
  	}
  },
  methods: {
  	onStart() {
  		this.state = 'question'
  		this.stats.success = 0
  		this.stats.error = 0
  	},
  	onQuestSuccess() {
  		this.state = 'message'
  		this.message.text = 'Верно!'
  		this.message.type = 'success'
  		this.stats.success++
  	},
  	onQuestError(msg) {
  		this.state = 'message'
  		this.message.text = `Неверно! ${msg}`
  		this.message.type = 'warning'
  		this.stats.error++
  	},
  	onNext() {
  		this.questDone < this.questMax ? this.state = 'question' : this.state = 'result'
  	},
  	onNextLevel() {
  		if (this.level < this.levels.length - 1) {
  			this.level++
  			this.onStart()
  		} else {
  			this.level = 0
  			this.state = 'start'
  		}
  	}
  }
}
</script>

<style scoped>
	#training {
		max-width: 700px;
		margin: 0 20px;
	}
	.box {
		margin: 10px 0;
	}
	.flip-enter {
	
	}
	.flip-enter-active {
		animation: flipInX .1s linear
	}
	.flip-leave {

	}
	.flip-leave-active {
		animation: flipOutX .1s linear;
	}
	@keyframes flipInX {
		from {transform: rotateX(90deg);}
		to {transform: rotateX(0deg);}
	}
	@keyframes flipOutX {
		from {transform: rotateX(0deg);}
		to {transform: rotateX(90deg);}
	}
</style>
