<script lang="ts" setup>
  import { ref, computed } from 'vue'
  import 'element-plus/theme-chalk/display.css'

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
  <div class="common-layout">
    <el-container>
      <el-header>
        <h1>Доктор Китаева</h1>
      </el-header>
      <el-main>
        <el-row justify="center">
          <el-col :span="24">
            <h3>Прогнозируемый остаток жизни.</h3>
          </el-col>
        </el-row>
        <el-row justify="center">
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
          <el-col class="line-up" :xs="21" :sm="14" :md="12" :lg="10" :xl="6">
            <el-text class="mx-1" size="large">Возраст</el-text>
          </el-col>
          <el-col class="line-up right" :xs="3" :sm="2" :md="2" :lg="2" :xl="2">
            <el-input-number :min="0" :max="150" v-model="age" size="defalut" placeholder="Укажите количество полных лет." />
          </el-col>
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
        </el-row>
        <el-row justify="center">
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
          <el-col :xs="21" :sm="14" :md="12" :lg="10" :xl="6">
            <el-text class="mx-1" size="large">Вес</el-text>
          </el-col>
          <el-col class="right" :xs="3" :sm="2" :md="2" :lg="2" :xl="2">
            <el-input-number :min="1" :max="500" v-model="weight" size="defalut" placeholder="Укажите вес в кг." />
          </el-col>
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
        </el-row>
        <el-row justify="center">
          <el-col :sm="4" :md="5" :lg="6" :xl="8"></el-col>
          <el-col :xs="21" :sm="14" :md="12" :lg="10" :xl="6">
            <el-text class="mx-1" size="large">Пол</el-text>
          </el-col>
          <el-col class="right" :xs="3" :sm="2" :md="2" :lg="2" :xl="2">
            <el-select v-model="gender" placeholder="Укажите ваш пол.">
              <el-option
                v-for="item in [{ value: 0, label: 'Мужской' }, { value: 1, label: 'Женский' }]"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-col>
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
        </el-row>
        <el-row justify="center">
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
          <el-col class="line-up" :xs="21" :sm="14" :md="12" :lg="10" :xl="6">
            <el-text class="mx-1" size="large">Ваш прогнозируемый остаток жизни:</el-text>
          </el-col>
          <el-col class="line-up" :xs="3" :sm="2" :md="2" :lg="2" :xl="2">
            <el-input class="center" v-model="years" size="defalut" readonly/>
          </el-col>
          <el-col class="hidden-xs" :sm="4" :md="5" :lg="6" :xl="8"></el-col>
        </el-row>
      </el-main>
    </el-container>
  </div>
</template>

<style>
  .el-row {
    margin-bottom: 20px;
  }
  .el-row:last-child {
    margin-bottom: 0;
  }
  .el-col.right {
    text-align: right;
  }
  .el-col.line-up {
    border-top: 1mm solid blue;
    padding-top: 3mm;
  }
  .el-input.center input {
    text-align: center;
  }
  el-input-number {
    width: 100%;
  }
  .el-header h1, .el-row h3, .el-footer {
    text-align: center;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>