<script lang="ts" setup>
import { ref, computed } from 'vue'

const age = ref(50)
const weight = ref(80)
const gender = ref(0)
const years = computed(() => {
	const gender_bonus = gender.value * 0.15088757396
	const max_age = 67.6
	const rest = ((1 + gender_bonus) * max_age - age.value) - Math.abs(weight.value - 75) * 1.1
	return (rest > 0 ? Math.round(rest) : 0)
})
</script>

<template>
<main>
	<header>
		<h1>Доктор Китаева</h1>
	</header>
	<section>
		<header>
			<h3>Прогнозируемый остаток жизни.</h3>
		</header>
		<article>
			<p>Возраст</p>
			<input type="number" min="0" max="150" v-model="age" placeholder="Укажите количество полных лет." />
		</article>
		<article>
			<p>Вес</p>
			<input type="number" min="1" max="500" v-model="weight" placeholder="Укажите вес в кг." />
		</article>
		<article>
			<p>Пол</p>
			<select v-model="gender" placeholder="Укажите ваш пол.">
				<option
					v-for="item in [{ value: 0, label: 'Мужской' }, { value: 1, label: 'Женский' }]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Ваш прогнозируемый остаток жизни:</p>
			<input type="text" v-model="years" readonly/>
		</article>
	</section>
</main>
</template>

<style>
*{
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	background-color: white;
	box-sizing: border-box;
	color: black;
	font-family: 'Arial';
	font-weight: 400;
	margin: 0;
}

main {
	margin: 0 auto;
	min-width: 300px;
	width: 50%;
	overflow: scroll;
}

@media (min-width: 20em) {
	main {
		width: 100%;
	}
}

@media (min-width: 30em) {
	main {
		width: 80%;
	}
}

@media (min-width: 40em) {
	main {
		width: 70%;
	}
}

@media (min-width: 60em) {
	main {
		width: 60%;
	}
}

@media (min-width: 80em) {
	main {
		width: 50%;
	}
}

header {
	font-weight: 600;
	padding: 1em;
	text-align: center;
}

header h1 {
	font-size: 2em;
}

header h3 {
	font-size: 1.5em;
}

section {
	padding: 1em 2em;
}

section article {
	display: flex;
	flex-flow: row wrap;
	padding: 0.5em;
	text-align: center;
}

section article p {
	flex-grow: 8;
	flex-shrink: 1;
	flex-basis: 70%;
	font-size: 1.125em;
	padding: 0.2em 0;
	text-align: left;
}

section article input, section article select {
	border: 1px solid #0070ff;
	border-radius: 0.2em;
	flex-grow: 1;
	flex-shrink: 1;
	flex-basis: 10%;
	font-size: 1.125em;
	max-height: 2em;
	max-width: 8em;
	padding: 0.2em;
	text-align: right;
}
</style>