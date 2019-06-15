<template>
  <div id="app">
    <span v-if="step < 7"><b>step{{step+1}}</b></span>

    <RadioInterview
      key="question1"
      v-if="step === 0"
      v-model="questions.q1"
      @next="handleNext"
      :message="questionsList[0]"
      :choices="choicesList[0]"
    />


    <RadioInterview
      key="question2"
      v-if="step === 1"
      v-model="questions.q2"
      @next="handleNext"
      :message="questionsList[1]"
      :choices="choicesList[1]"
    />

    <RadioInterview
      key="question3"
      v-if="step === 2"
      v-model="questions.q3"
      @next="handleNext"
      :message="questionsList[2]"
      :choices="choicesList[2]"
    />

    <RadioInterview
      key="question4"
      v-if="step === 3"
      v-model="questions.q4"
      @next="handleNext"
      :message="questionsList[3]"
      :choices="choicesList[3]"
    />

    <RadioInterview
      key="question5"
      v-if="step === 4"
      v-model="questions.q5"
      @next="handleNext"
      :message="questionsList[4]"
      :choices="choicesList[4]"
    />
    <InputQuestion
      key="question6"
      v-if="step === 5"
      v-model="questions.q6"
      @next="handleNext"
      :message="questionsList[5]"
      :choices="choicesList[5]"
    />
    <RadioInterview
      key="question7"
      v-if="step === 6"
      v-model="questions.q7"
      @next="handleNext"
      :message="questionsList[6]"
      :choices="choicesList[6]"
    />

    <Result
      v-if="step === 7"
      :questions="questions"
    />


  </div>
</template>

<script>
  import SelectQuestion from './components/SelectQuestion.vue';
  import InputQuestion from './components/InputQuestion.vue';
  import RadioInterview from './components/RadioInterview.vue';
  import Result from './components/Result.vue';

  import {parse} from 'querystring';

  export default {
    name: 'app',
    data() {
      return {
        step: 0,
        questions: {
          q1: 1,
          q2: 1,
          q3: 1,
          q4: 1,
          q5: 1,
          q6: 1,
          q7: 1,
        },
      };
    },
    components: {
      SelectQuestion,
      RadioInterview,
      InputQuestion,
      Result,
    },
    mounted() {
      const params = parse(location.search.replace('?', ''));
      const isValid = ['q1', 'q2', 'q3', 'q4', 'q5', 'q6','q7'].every((val) => {
        if (!params[val]) {
          return false;
        }
        if (val != 'name' && parseInt(params[val]) < 1) {
          return false;
        }
        return true;
      });
      if (isValid) {
        const questions = {
          name: params.name,
          title: parseInt(params.title),
          q1: parseInt(params.q1),

        };
        this.questions = questions;
        this.step = 7;
      }
    },
    methods: {
      handleNext() {
        this.step++;
      },
      handleStart(startData) {
        this.questions.name = startData.name;
        this.questions.title = startData.title;
        this.handleNext();
      },
    },
    computed: {
      questionsList() {
        return [
          '好きな食べ物は？',
          '最近いつ食べましたか？',
          'どこのが好きとかそういうのありますか？',
          'そのメニューは' +
          this.dishChoice.join('と') +
          'に分けられると思うのですが、どっちが大事だと思いますか？',
          '関係ないですが、ご趣味は？',
          '好きな言葉を書いてください',
          'こんどはいつ食べますか？',
        ];
      },
      dishChoice() {
        let commonValue = ['どっちも好きだ','分けられない'];
        const values = [
          null,
          [
            '麺',
            'スープ',
          ],
          [
            'ルー',
            'ごはん',
          ],
          [
            'ネタ',
            '酢飯'
          ],
          [
            '具材',
            '皮'
          ],
          [
            '肉',
            '衣'
          ],

          [
            'ちくわ',
            'チーズ'
          ]
        ]
        return [
          ...values[this.questions.q1], //q1 = 1~6
          ...commonValue
        ]
      },
      choicesList() {
        return [
          [
            'ラーメン',
            'カレー',
            '寿司',
            'ウインナー',
            'からあげ',
            'チーちく',
          ],
          [
            '今日',
            '今週',
            '今月',
            '今年',
            'ずっと前',
            '実は食べたことがない',
          ],
          [
            '自分で作るもの',
            '親が作ってくれたもの',
            '道ばたに落ちてたもの',
            '特定のお店',
            'どこのでもいいんだよ',
            'うまいものにあったことがないんだ',
          ],
          this.dishChoice,
          [
            '音楽',
            '読書',
            '旅行',
            'スポーツ',
            'インターネット',
            'お笑い番組を見る',
          ],
          [
          ],
          [
            '今日',
            'あした',
            '今週中',
            '今月中',
            '今年中',
            'わからない',
          ],

        ];
      },
    },
  };
</script>

<style>
  * {
    box-sizing: border-box;
  }

  #app {
    font-family: sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    max-width: 640px;
    font-size: 12px;
    padding: 0 50px;
  }
</style>
