<!-- docs/components/PasswordProtect.vue -->
<template>
  <div class="password-protect">
    <div v-if="!unlocked" class="password-form">
      <h3 v-if="title">{{ title }}</h3>
      <input
        v-model="inputPassword"
        type="password"
        placeholder="请输入密码"
        @keyup.enter="checkPassword"
        :class="{ 'shake': isShaking }"
      />
      <button @click="checkPassword">验证</button>
      <p v-if="error" class="error">{{ error }}</p>
      <!-- <p v-if="attempts > 0" class="attempts">
        剩余尝试次数: {{ maxAttempts - attempts }}
      </p> -->
    </div>
    <div v-else class="protected-content">
      <slot />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  password: {
    type: String,
    required: true
  },
  title: {
    type: String,
    default: '此内容受密码保护'
  },
  // maxAttempts: {
  //   type: Number,
  //   default: 3
  // }
})

const inputPassword = ref('')
const unlocked = ref(false)
const error = ref('')
// const attempts = ref(0)
const isShaking = ref(false)

const checkPassword = () => {
  if (inputPassword.value === props.password) {
    unlocked.value = true
    error.value = ''
  } else {
    // attempts.value++
    error.value = '密码错误'
    triggerShakeAnimation()
    
    // if (attempts.value >= props.maxAttempts) {
    //   error.value = '尝试次数过多，请稍后再试'
    // }
  }
}

const triggerShakeAnimation = () => {
  isShaking.value = true
  setTimeout(() => {
    isShaking.value = false
  }, 500) // 动画持续时间
}
</script>

<style scoped>
.password-protect {
  margin: 1rem 0;
}
.password-form {
  padding: 1rem;
  border: 1px solid #eee;
  border-radius: 4px;
  background-color: #f9f9f9;
}
input {
  padding: 0.5rem;
  margin-right: 0.5rem;
  transition: transform 0.1s ease;
}
button {
  padding: 0.5rem 1rem;
  background-color: var(--vp-c-brand);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.error {
  color: red;
  margin-top: 0.5rem;
}
.attempts {
  color: #666;
  font-size: 0.8rem;
}
.protected-content {
  margin-top: 1rem;
}

/* 抖动动画效果 */
.shake {
  animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
  transform: translate3d(0, 0, 0);
}

@keyframes shake {
  10%, 90% {
    transform: translate3d(-1px, 0, 0);
  }
  
  20%, 80% {
    transform: translate3d(2px, 0, 0);
  }
  
  30%, 50%, 70% {
    transform: translate3d(-4px, 0, 0);
  }
  
  40%, 60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>