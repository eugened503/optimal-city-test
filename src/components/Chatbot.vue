<template>
  <section class="chatbot d-flex align-items-center justify-content-center">
    <div class="container">
      <div class="row d-flex align-items-center justify-content-center">
        <div class="chatbot-body col-md-8 col-lg-6 col-xl-4">
          <div class="card">
            <div
              class="card-header d-flex justify-content-between align-items-center p-3 bg-info text-white border-bottom-0">
              <i class="fas fa-angle-left"></i>
              <p class="mb-0 fw-bold">Live chat</p>
              <i class="fas fa-times"></i>
            </div>
            <div class="card-body" ref="chatbox">
              <div class="wrapper d-flex flex-column">
                <transition-group tag="ul" name="list" class="mb-0 mb-5 p-0">
                  <li v-for="replica in isData?.replicas" :key="replica.id">
                    <div v-if="!replica.option" class="d-flex flex-row justify-content-start mb-4"
                      :class="[replica.author === 'human' ? 'user justify-content-end' : '']">
                      <img :src=imageUrl(replica.author) :alt="replica.author">
                      <div class="p-3 ms-3 message">
                        <p class="small mb-0">{{ replica.content }}</p>
                      </div>
                    </div>
                    <button v-else @click="selectOption(replica)"
                      class="btn small p-2 me-3 mb-3 text-white rounded-3 bg-primary ">
                      {{ replica.content }}
                    </button>
                  </li>
                </transition-group>
              </div>
            </div>
            <div class="card-footer text-muted d-flex justify-content-start align-items-center">
              <div class="input-group mb-0">
                <input type="text" v-model="message" @keyup.enter="sendMessage" class="form-control"
                  placeholder="Введите сообщение" aria-label="Recipient's username" aria-describedby="button-addon2" />
                <button :disabled="message === ''" @click="sendMessage" class="btn bg-info fw-bold text-white"
                  type="button" id="button-addon2">
                  Send
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { v4 as uuidv4 } from 'uuid';
import data from '@/data/data.json';

const dataCopy = JSON.parse(JSON.stringify(data));
const isData = ref({ "replicas": [] });
const chatbox = ref(null);
const message = ref("");

const imageUrl = (name) => new URL(`../assets/images/${name}.svg`, import.meta.url).href;

const scrollHeightBehavior = () => {
  chatbox.value.scrollTo({ top: chatbox.value.scrollHeight, behavior: 'smooth' });
};

const clearArr = () => {
  const options = dataCopy.replicas.slice(1, 4);
  const idArr = options.map(option => Object.values(option)[0]);
  const newArr = isData.value.replicas.filter((replica) => {
    return !idArr.includes(replica.id);
  });

  return { options, newArr };
};

const updateList = (bot, options) => {
  isData.value.replicas.push(...[bot, ...options]);
};

const selectOption = (replica) => {
  const human = {
    "id": uuidv4(),
    "content": replica.content,
    "author": "human",
    "option": false
  };

  const bot = {
    "id": uuidv4(),
    "content": replica.answer,
    "author": "bot",
    "option": false
  };

  const { options, newArr } = clearArr();
  isData.value.replicas = newArr;
  isData.value.replicas.push(human);

  setTimeout(() => updateList(bot, options), 500);
  setTimeout(scrollHeightBehavior, 510);
}

const sendMessage = () => {
  const messageValue = message.value.trim();

  if (messageValue !== '') {
    let bot;
    const human = {
      "id": uuidv4(),
      "content": messageValue,
      "author": "human",
      "option": false
    };

    isData.value.replicas.push(human);

    const { options, newArr } = clearArr();
    const optionArr = isData.value.replicas.filter(replica => replica.option);
    const humanReplica = optionArr.find(item => item.content.toLowerCase() === messageValue.toLowerCase());

    isData.value.replicas = newArr;

    if (!humanReplica) {
      bot = {
        "id": uuidv4(),
        "content": "Не понял Вас. Что я могу для Вас сделать?",
        "author": "bot",
        "option": false
      };
    } else {
      bot = {
        "id": uuidv4(),
        "content": humanReplica.answer,
        "author": "bot",
        "option": false
      };
    }

    setTimeout(() => updateList(bot, options), 500);
    setTimeout(scrollHeightBehavior, 510);

    message.value = '';
  }
}

onMounted(() => {
  const salute = dataCopy.replicas.slice(0, 1);
  const options = dataCopy.replicas.slice(1, 4);

  setTimeout(() => updateList(...salute, []), 500);

  setTimeout(function () {
    for (let i = 0; i < options.length; i++) {
      setTimeout((i) => updateList(options[i], []), i * 500, i);
    }
  }, 1500);
})
</script>

<style lang="scss" scoped>
.chatbot {
  background-color: #eee;
  min-height: 100vh;

  .card {
    border-radius: 15px;
  }

  .card .card-header {
    border-top-left-radius: 15px;
    border-top-right-radius: 15px;
  }
}

.chatbot .card .card-body {
  height: 80vh;
  overflow-y: auto;

  .wrapper {
    min-height: 100%;

    ul {
      flex: 1;

      li {
        list-style-type: none;
      }
    }
  }

  img {
    width: 45px;
    height: 100%;
    flex: 0 0 auto;
  }

  .message {
    border-radius: 15px;
    background-color: rgba(57, 192, 237, .2);

    &.user {
      background-color: #fbfbfb;

      &:first-child {
        margin-top: 10px;
      }
    }
  }

  .btn {
    padding-top: .55rem;
  }

  .user {
    .message {
      background-color: #fbfbfb;
    }
  }
}

.list-enter-active,
.list-leave-active {
  transition: all 1s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
}
</style>
