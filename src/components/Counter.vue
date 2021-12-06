<template>
  <div class="flex flex-row min-h-screen bg-gray-100 text-gray-800">
    <transition name="fade">
      <aside
        style="z-index: 100"
        id="sidebar"
        class="
          p-2
          content-left
          justify-left
          sidebar
          md:shadow
          transform
          transition-transform
          duration-150
          ease-in
          bg-white
        "
        v-if="isOpen"
      >
        <form action="#">
          <label for="textSize">Text size:</label>
          <input
            type="range"
            name="textSize"
            id="textSize"
            min="0"
            max="10"
            step="1"
            style="width: 100%"
            v-model="textSize"
          />
          <label for="firstLine" class="mt-2">First text line:</label>
          <input
            id="firstLine"
            type="text"
            name="firstLine"
            class="
              mb-2
              text-sm
              sm:text-base
              placeholder-gray-500
              px-2
              rounded-lg
              border border-gray-300
              w-full
              h-10
              focus:outline-none focus:border-gray-500
            "
            v-model="firstLine"
          />
          <label for="secondLine">Second text line:</label>
          <input
            id="secondLine"
            type="text"
            name="secondLine"
            class="
              mb-2
              text-sm
              sm:text-base
              placeholder-gray-500
              px-2
              rounded-lg
              border border-gray-300
              w-full
              h-10
              focus:outline-none focus:border-gray-500
            "
            v-model="secondLine"
          />
          <label for="counterSize">Counter size:</label>
          <input
            type="range"
            name="counterSize"
            id="counterSize"
            min="0"
            max="100"
            step="1"
            style="width: 100%"
            v-model="counterSize"
          />
          <label for="minutes">Minutes:</label>
          <input
            type="range"
            name="minutes"
            id="minutes"
            min="0"
            max="60"
            step="1"
            style="width: 100%"
            v-model="minutes"
          />
          <label for="seconds">Seconds:</label>
          <input
            type="range"
            name="seconds"
            id="seconds"
            min="0"
            max="60"
            step="1"
            style="width: 100%"
            v-model="seconds"
          />
        </form>
        <button
          class="
            m-2
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-bold
            py-2
            px-4
            rounded
          "
          style="width: 90%"
          @click="startCountDown()"
        >
          <span>Start</span>
        </button>
        <button
          class="
            m-2
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-bold
            py-2
            px-4
            rounded
          "
          style="width: 90%"
          @click="pauseCountDown()"
        >
          <span>Pause</span>
        </button>
        <button
          class="
            m-2
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-bold
            py-2
            px-4
            rounded
          "
          style="width: 90%"
          @click="resetCounter()"
        >
          <span>Reset</span>
        </button>
      </aside>
    </transition>
    <button
      id="sidebar-button-close"
      class="ml-1"
      @click="isOpen = !isOpen"
      style="z-index: 75"
      :style="{ backgroundColor: bgColor, color: textColor }"
      v-if="isOpen"
    >
      <span>&lt;&lt;</span>
    </button>
    <button
      id="sidebar-button-open"
      class="ml-1"
      @click="isOpen = !isOpen"
      style="z-index: 75; opacity: 50%"
      :style="{ backgroundColor: bgColor, color: textColor }"
      v-if="!isOpen"
    >
      <span>&gt;&gt;</span>
    </button>
    <main
      class="
        main
        p-2
        flex flex-col flex-grow
        md:ml-0
        transition-all
        duration-150
        ease-in
        text-center
        noselect
      "
      :style="{ backgroundColor: bgColor, color: textColor }"
    >
      <h2
        class="p-0 m-0 text-center"
        :style="{ fontSize: textSize + 'rem', lineHeight: '100%' }"
      >
        {{ firstLine }}
      </h2>
      <h2
        class="p-0 m-0 text-center"
        :style="{ fontSize: textSize + 'rem', lineHeight: '100%' }"
      >
        {{ secondLine }}
      </h2>
      <div id="full-size" class="flex flex-col flex-grow justify-center">
        <h1 :style="{ fontSize: counterSize + 'rem' }">
          <div style="width: 48%; display: inline-flex; justify-content: right">
            <span v-if="minutes < 10">0</span>{{ minutes }}
          </div>
          <div style="width: 4%; display: inline-flex; justify-content: center">
            :
          </div>
          <div style="width: 48%; display: inline-flex; justify-content: left">
            <span v-if="seconds < 10">0</span>{{ seconds }}
          </div>
        </h1>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "Counter",
  data() {
    return {
      firstLine: "Countdown to",
      secondLine: "LIVE BROADCAST",
      textColor: "#fff",
      bgColor: "#0f206c",
      counterSize: 25,
      minutes: 60,
      seconds: 0,
      textSize: 3,
      isStarted: false,
      isOpen: true,
    };
  },
  methods: {
    startCountDown() {
      this.startTimer(parseInt(this.minutes) * 60 + parseInt(this.seconds));
      this.isOpen = false;
    },
    pauseCountDown() {
      clearInterval(this.interval);
    },
    resetCounter() {
      this.seconds = 0;
      this.minutes = 60;
      this.firstLine = "Countdown to";
      this.secondLine = "LIVE BROADCAST";
      this.counterSize = 25;
      this.textSize = 3;
      clearInterval(this.interval);
    },
    startTimer(duration) {
      let start = Date.now(),
        difference;

      const self = this;

      function timer() {
        difference = duration - (((Date.now() - start) / 1000) | 0);
        self.minutes = (difference / 60) | 0;
        self.seconds = difference % 60 | 0;

        if (difference <= 0) {
          clearInterval(self.interval);
        }
      }

      timer();
      this.interval = setInterval(timer, 1000);
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}

#full-size {
  position: absolute;
  top: 10%;
  bottom: 0;
  right: 0;
  left: 0;
  overflow: hidden;
}

#sidebar {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 85%;
  left: 0;
  overflow: hidden;
}

#sidebar-button-close {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 80%;
  left: 15%;
  overflow: hidden;
}

#sidebar-button-open {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 95%;
  left: 0%;
  overflow: hidden;
}

.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                        supported by Chrome, Edge, Opera and Firefox */
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
