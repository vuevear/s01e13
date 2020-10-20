<template>
  <section>
    <h3>Counter</h3>
    <p>{{ count }}</p>
    <button @click="inc(10)">Increment</button>
  </section>

  <section>
    <h3>Online</h3>
    <p>{{ online }}</p>
  </section>

  <section>
    <h3>Clipboard</h3>
    <button @click="copy('Hola VueUse!')">Copy</button>
    <p>{{ text }}</p>
  </section>

  <section>
    <h3>Debounce</h3>
    <button @click="print('Hola de nuevo!')">Print</button>
  </section>

  <section>
    <h3>Now</h3>
    <p>{{ new Date(now) }}</p>
  </section>

  <section>
    <h3>Idle</h3>
    <p>{{ idle }}</p>
    <p>{{ lastActive }}</p>
  </section>

  <section>
    <h3>Battery</h3>
    <p>{{ level }}</p>
    <p>{{ charging }}</p>
  </section>

  <section>
    <h3>Title</h3>
    <input type="text" v-model="title">
  </section>

  <section>
    <h3>Geolocation</h3>
    <p>{{ locatedAt }}</p>
    <p>{{ error }}</p>
  </section>

  <section>
    <h3>Dark</h3>
    <p> {{ isDark }}</p>
  </section>
</template>

<script>
import {
  useTitle,
  useBattery,
  useIdle,
  useWebSocket,
  useDebounceFn,
  useOnline,
  useClipboard,
  useCounter,
  useNow,
  useGeolocation,
  usePreferredDark
} from '@vueuse/core'

import { watchEffect } from 'vue'

export default {
  name: "App",

  setup () {
    const { count, inc } = useCounter()
    const online = useOnline()
    const { text, copy } = useClipboard()

    const print = useDebounceFn((val) => {
      console.log(val)
    }, 1000)

    const { state, data, send } = useWebSocket('wss://echo.websocket.org')

    watchEffect(() => {
      console.log('Estado: ', state.value)
      console.log('Data: ', data.value)
      if (state.value === 'OPEN') {
        send('Hola VueUse!')
      }
    })

    const now = useNow()
    const { idle, lastActive } = useIdle(1000)
    const { charging, level } = useBattery()
    const title = useTitle()
    const { locatedAt, error } = useGeolocation()
    const isDark = usePreferredDark()

    return {
      count, inc, online, text, copy, print, now,
      idle, lastActive, charging, level, title,
      locatedAt, error, isDark
    }
  }
};
</script>

<style>

section {
  border-bottom: 2px solid #3d3d3d;
  padding-bottom: 10px;
}

</style>