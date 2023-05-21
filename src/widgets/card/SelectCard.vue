<script lang="ts">
import type { PropType } from 'vue'
type Option = {
  title: string
  serial: number
}

type Props = {
  title: string
  description: string
  options: Option[]
  onSelect: (serial: number) => void
  onSkip: (serial: number) => void
  onNext: (vaseriallue: number) => void
}

export default {
  name: 'card',
  props: {
    title: Object as PropType<Props['title']>,
    description: Object as PropType<Props['description']>,
    options: Object as PropType<Props['options']>,
    onSelect: Object as PropType<Props['onSelect']>,
    onSkip: Object as PropType<Props['onSkip']>,
    onNext: Object as PropType<Props['onNext']>
  },
  data() {
    return {
      value: 0
    }
  },
  methods: {
    onChange(serial: number) {
      this.value = serial
      this.onSelect && this.onSelect(serial)
    },
    onLocalSkip() {
      this.onSkip && this.onSkip(this.value)
    },
    onLocalNext() {
      this.onNext && this.onNext(this.value)
    }
  }
}
</script>

<template>
  <div class="card">
    <h1 class="title">{{ title }}</h1>
    <div class="desc">
      {{ description }}
    </div>
    <div class="cardInput">
      <ul>
        <li v-for="question in options" :key="question.serial">
          <input type="radio" name="ans" @change="() => onChange(question.serial)" />{{
            question.title
          }}
        </li>
      </ul>
    </div>
    <div class="footer">
      <div class="skip">
        <button class="btn btn-transparent button-skip" @click="onLocalSkip">пропустить</button>
      </div>
      <div class="next"><button class="btn button-next" @click="onLocalNext">далее</button></div>
    </div>
  </div>
</template>

<style scoped>
.card {
  border-radius: var(--border-radius-l);
  box-shadow: 0 0 30px 10px #33333323;
  padding: 24px 32px;
  max-width: 800px;
  min-width: 500px;
}

.card .footer {
  display: flex;
  width: 100%;
  justify-content: end;
}

.card .title {
  font-family: MontserratBold;
  font-size: 24px;
  color: var(--text-main-color);
  margin-bottom: 16px;
}

.card .desc {
  font-family: Montserrat;
  letter-spacing: 0.5px;
}

.cardInput {
  margin: 24px 0;
}
</style>