<script lang="ts" setup>
import { ref, computed } from 'vue'

const age = ref(50)
const height = ref(175)
const weight = ref(80)
const gender = ref(2)
const smoking = ref(1)
const periods = ref(1)
const broken_bones = ref(1)
const parent_bb = ref(1)
const activity = ref(1)
const groups = ref([])
const steroids = ref(1)
const transplant = ref(1)
const necrose = ref(1)

const options = [
	{ key: '0', value: 3, label: 'Алкилирующие препараты' },
	{ key: '1', value: 1, label: 'Атрациклины' },
	{ key: '2', value: 2, label: 'алкалоиды барвинка (винбластин,винкристин)' },
	{ key: '3', value: 1, label: 'гликопептидный антибиотик (блеомицин)' },
	{ key: '4', value: 2, label: 'ингибитор топоизомеразыII (этопозид)' },
	{ key: '5', value: 2, label: 'иммунотерапия (ниволумаб, брентуксимаба ведотин)' },
	{ key: '6', value: 2, label: 'антиметаболит группы аналогов пиримидина (гемзар)' }
]

function getAgeWeight(age : number) : number
{
	if(age < 18) return 0
	if(age <= 40) return 1
	return 2
}

function getHeightWeight(height : number, weight : number) : number
{
	if(height == 0) return 0

	const k = weight / height * 100
	if(k < 18.5) return 3
	if(k < 25) return 1
	if(k < 30) return 2
	return 3
}

const risk1 = computed(() => {
	return getAgeWeight(age.value) + getHeightWeight(height.value, weight.value) + gender.value
		+ (gender.value == 3 ? periods.value : 0) + smoking.value + broken_bones.value
		+ parent_bb.value + activity.value
})

const risk2 = computed(() => {
	let mul = 1
	if(groups.value.length) mul *= groups.value.reduce((total, value) => { return total * options[value].value }, 1) 
	return mul * steroids.value * transplant.value * necrose.value
})

const result = computed(() => risk1.value * risk2.value)

</script>

<template>
<main>
	<header>
		<h1>Доктор Китаева</h1>
	</header>
	<section>
		<header>
			<h3>Расчёт риска остеопороза.</h3>
		</header>
		<article class="part-h3">
			<h3>I. Часть общие факторы риска остеопороза.</h3>
		</article>
		<article>
			<p>Возраст, полных лет:</p>
			<input type="number" min="18" max="150" v-model="age" placeholder="Укажите количество полных лет." />
		</article>
		<article>
			<p>Рост, см:</p>
			<input type="number" min="50" max="2500" v-model="height" placeholder="Укажите ваш рост в см." />
		</article>
		<article>
			<p>Вес, кг:</p>
			<input type="number" min="15" max="500" v-model="weight" placeholder="Укажите вес в кг." />
		</article>
		<article>
			<p>Пол:</p>
			<select v-model="gender" placeholder="Укажите ваш пол.">
				<option
					v-for="item in [{ value: 2, label: 'Мужской' }, { value: 3, label: 'Женский' }]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article v-if="gender == 3">
			<p>Аменорея:</p>
			<p class="remark"><b>Да</b> - в настоящий момент отсутсвует менструация более 6 месяцев.</p>
			<p class="remark"><b>Была в прошлом</b> - менструации отсутствовали более 6 месяцев, но восстановились.</p>
			<p class="remark"><b>Нет</b> - менструальный цикл не нарушался.</p>
			<select v-model="periods" placeholder="Укажите состояние менструального цикла.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да' },
						{ value: 2, label: 'Была в прошлом' },
						{ value: 1, label: 'Нет' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Курение:</p>
			<select v-model="smoking" placeholder="Укажите курите вы или нет.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да, в настоящий момент' },
						{ value: 2, label: 'Курил, но в настоящий момент нет' },
						{ value: 1, label: 'Нет, никогда' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Предшествующие переломы:</p>
			<select v-model="broken_bones" placeholder="Укажите были ли переломы.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да, свершившиеся после 40 лет' },
						{ value: 2, label: 'Да, свершившиеся переломы до 39 лет' },
						{ value: 1, label: 'Нет' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Переломы трубчатых костей у родителей:</p>
			<select v-model="parent_bb" placeholder="Укажите были ли переломы трубчатых костей у родителей.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да' },
						{ value: 2, label: 'Неизвестно' },
						{ value: 1, label: 'Нет' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Физическая нагрузка:</p>
			<p class="remark"><b>Незначительная</b> - прогулки менее 30 минут в день 2-3 раза в неделю, преимущественно сидячий образ жизни.</p>
			<p class="remark"><b>Умеренная</b> - прогулки до 2-х часов в день 3-5 дней в неделю.</p>
			<p class="remark"><b>Активная</b> - прогулки более 2-х часов в день 6-7 дней в неделю.</p>
			<select v-model="activity" placeholder="Укажите степень физической нагрузки.">
				<option
					v-for="item in [
						{ value: 3, label: 'Незначительная' },
						{ value: 2, label: 'Умеренная' },
						{ value: 1, label: 'Активная' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article class="part-h3">
			<h3>II. Специфические факторы риска</h3>
		</article>
		<article>
			<p>Применение групп цитостатических препаратов:</p>
			<div class="options" v-for="item in options">
				<input type="checkbox" :id="item.key" :value="item.key" v-model="groups"/>
				<label :for="item.key">{{item.label}}</label>
			</div>
		</article>
		<article>
			<p>Глюкокортикостероиды:</p>
			<select v-model="steroids" placeholder="Укажите uлюкокортикостероидные препараты.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да' },
						{ value: 1, label: 'Нет' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Трансплантация ТГСК:</p>
			<select v-model="transplant" placeholder="Укажите былф ли трансплантация ТГСК.">
				<option
					v-for="item in [
						{ key: 0, value: 3, label: 'Ауто' },
						{ key: 1, value: 3, label: 'Алло' },
						{ key: 2, value: 1, label: 'Нет' }
					]"
					:key="item.key"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article>
			<p>Асептический некроз головки бедренной кости:</p>
			<select v-model="necrose" placeholder="Укажите был ли асептический некроз головки бедренной кости.">
				<option
					v-for="item in [
						{ value: 3, label: 'Да' },
						{ value: 1, label: 'Нет' }
					]"
					:key="item.value"
					:label="item.label"
					:value="item.value"
				/>
			</select>
		</article>
		<article class="part-h3">
			<h3>III заключение </h3>
		</article>
		<article>
			<p>Общий фактор риска: {{risk1}}</p>
			<p>Специфические фактор риска: {{risk2}}</p>
			<p>Суммарный фактор риска: {{result}}</p>
			<p v-if="result <= 16">Риск минимальный.</p>
			<p v-if="result > 16 && result <= 23">Средний риск развития остеопороза.</p>
			<p v-if="result > 23">Высокий риск остеопороза.</p>
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

header {
	font-weight: 600;
	margin: 1em;
	text-align: center;
}

header h1 {
	font-size: 2em;
	margin-top: 2em;
}

header h3 {
	font-size: 1.5em;
}

article {
	display: flex;
	flex-flow: row wrap;
	padding: 0.5em;
	text-align: center;
}

article.part-h3 {
	border-bottom: solid 1mm blue;
	margin-top: 1em;
}

article.part-h3 h3 {
	font-size: 1.25em;
	font-weight: 600;
}

article p {
	flex-grow: 8;
	flex-shrink: 1;
	flex-basis: 70%;
	font-size: 1.125em;
	padding: 0.2em 0;
	text-align: left;
}

article p.remark {
	color: #444444;
	font-size: 1em;
}

article p.remark b {
	font-weight: 600;
}

article div.options {
	margin-top: 0.25em;
	text-align: left;
	width: 100%;
}

article div.options input {
	display: inline;
	width: auto;
	margin-right: 1em;
}

article input,
article select {
	border: 1px solid #0070ff;
	border-radius: 0.2em;
	flex-grow: 1;
	flex-shrink: 1;
	flex-basis: 28%;
	font-size: 1.125em;
	max-height: 2em;
	max-width: 8em;
	padding: 0.2em;
	width: 28%;
}

article input[type="number"] {
	text-align: right;
}

article select {
	text-align: left;
}

@media (min-width: 10em) {
	main {
		width: 100%;
	}

	.show-on-mobile {
		display: block;
	}

	.hide-on-mobile {
		display: none;
	}

	section {
		padding: 1em 1em;
	}

	article input,
	article select {
		flex-basis: 100%;
		max-width: none;
		width: 100%;
	}
}

@media (min-width: 20em) {
	section {
		padding: 1em 2em;
	}
}

@media (min-width: 30em) {
	main {
		width: 90%;
	}
}

@media (min-width: 40em) {
	main {
		width: 80%;
	}
}

@media (min-width: 60em) {
	main {
		width: 70%;
	}

	.show-on-mobile {
		display: none;
	}

	.hide-on-mobile {
		display: block;
	}

	article input,
	article select {
		flex-basis: 28%;
		max-width: 28%;
		width: 28%;
	}
}

@media (min-width: 80em) {
	main {
		width: 60%;
	}
}

</style>