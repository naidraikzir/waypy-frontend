<template>
<div class="message">
  <div class="bubble" :class="[{ '-user': message.user }]">
    <div class="content" v-if="message.user">{{ message.message }}</div>
    <div class="content" v-html="message.message" v-else></div>
    <div class="timestamp">{{ format(message.timestamp) }}</div>
  </div>
</div>
</template>

<script>
import { mapGetters } from 'vuex'
import format from 'date-fns/format'
import isThisHour from 'date-fns/is_this_hour'
import isThisMinute from 'date-fns/is_this_minute'
import differenceInMinutes from 'date-fns/difference_in_minutes'

export default {
  props: {
    message: Object
  },

  methods: {
    format (date) {
      if (isThisHour(date)) {
        if (isThisMinute(date)) {
          return 'Baru saja'
        }

        return `${differenceInMinutes(new Date, date)} menit yang lalu`
      }

      return format(date, 'HH:mm')
    },
  },
}
</script>

<style scoped>
.message {
  margin-top: 1em;
}

.message::after {
  content: '';
  clear: both;
  display: table;
}

.bubble {
  background: white;
  border-radius: 6px 6px 6px 0;
  box-shadow: 0 0.5em 1em 0 rgba(0, 0, 0, 0.05);
  display: inline-block;
  max-width: 80%;
  padding: 0.5em 0.75em;
  position: relative;
  align-self: flex-start;
}

.bubble::after {
  content: '';
  position: absolute;
  bottom: 0;
  right: 100%;
  transform: translateX(1px);
  border-width: 4px;
  border-color: transparent white white transparent;
  border-style: solid;
}

.-dark .bubble:not(.-user) {
  background: #2f2f40;
}

.-dark .bubble:not(.-user)::after {
  border-color: transparent #2f2f40 #2f2f40 transparent;
}

.-user {
  background: #394492;
  border-radius: 6px 6px 0 6px;
  color: white;
  float: right;
  align-self: flex-end;
}

.-user::after {
  border-color: transparent transparent #394492 #394492;
  left: 100%;
  right: initial;
  transform: translateX(-1px);
}

.content {
  word-wrap: break-word;
}

.timestamp {
  float: right;
  font-size: 0.6em;
  margin-top: 1em;
}
</style>
