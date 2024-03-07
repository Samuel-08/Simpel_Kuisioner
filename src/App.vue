<script setup>
// Import modul ref dan computed dari Vue untuk membuat referensi reaktif dan nilai yang dihitung
import { ref, computed } from "vue";

// Inisialisasi daftar pertanyaan beserta opsi jawaban dan status jawaban yang dipilih
const questions = ref([
  {
    question: "apa itu vue ? ",
    answer: 0, // Indeks opsi jawaban yang benar
    options: ["Framework", "Library", "Languange"], // Opsi jawaban
    selected: null, // Status jawaban yang dipilih
  },
  {
    question: "apa itu React JS ? ",
    answer: 0,
    options: ["Framework", "Library", "Languange"],
    selected: null,
  },
  {
    question: "apa itu React HTML ? ",
    answer: 2,
    options: ["Framework", "Library", "Languange"],
    selected: null,
  },
]);

// Referensi reaktif untuk menandai apakah kuis telah selesai atau belum
const quizCompleted = ref(false);
// Referensi reaktif untuk menyimpan indeks pertanyaan yang sedang ditampilkan
const currentQuestion = ref(0);

// Hitung jawaban yang benar untuk setiap pertanyaan
const correctAnswers = questions.value.map(
  (question) => question.options[question.answer]
);

// Hitung skor berdasarkan jawaban yang dipilih oleh pengguna
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      value++;
    }
  });
  return value;
});

// Dapatkan pertanyaan saat ini yang akan ditampilkan
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

// Fungsi untuk mengatur jawaban yang dipilih oleh pengguna
const setAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
};

// Fungsi untuk beralih ke pertanyaan berikutnya atau menyelesaikan kuis
const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h2>Pertanyaan dari Code Hero</h2>
    <!-- Bagian kuis yang ditampilkan jika kuis belum selesai -->
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
      </div>
      <div class="options">
        <!-- Elemen HTML untuk setiap opsi jawaban -->
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected !== null &&
            index !== getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <!-- Input radio untuk memilih jawaban -->
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <!-- Tombol untuk beralih ke pertanyaan berikutnya -->
      <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index === questions.length - 1
            ? "selesai"
            : getCurrentQuestion.selected === null
            ? "Pilih sebuah Opsi"
            : "Pertanyaan Selanjutnya"
        }}
      </button>
    </section>
    <!-- Bagian tampilan yang ditampilkan setelah kuis selesai -->
    <section v-else>
      <h2>Kamu telah menyelesaikan kuis</h2>
      <p>Jawaban yang benar adalah:</p>
      <ul>
        <!-- Daftar jawaban yang benar untuk setiap pertanyaan -->
        <li v-for="(question, index) in questions" :key="index">
          {{ question.question }}: {{ correctAnswers[index] }}
        </li>
      </ul>
      <!-- Tampilkan skor pengguna -->
      <p>Skor kamu adalah {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>


<style>
/* Gaya tampilan untuk aplikasi */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
body {
  background-color: blue; /* Warna latar belakang */
  color: black; /* Warna teks */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.app {
  width: 96%;
  max-width: 600px;
  background-color: white;
  padding: 26px;
  border-radius: 8px;
}

.quiz {
  margin-bottom: 20px;
}
h2 {
  margin-bottom: 24px;
}
.quiz-info {
  margin-bottom: 20px;
  font-size: 28px;
  text-align: center;
}

.options {
  display: flex;
  flex-direction: row;
}

.option {
  margin-bottom: 10px;
  padding: 10px;
  display: flex;
  align-items: center;
}
.option:hover {
  background-color: #d8d7d7c4;
  border-radius: 5px;
}
.option input[type="radio"] {
  margin-right: 10px;
}

button {
  padding: 10px 20px;
  background-color: transparent;
  color: darkblue;
  border: 2px solid blue;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

button:hover {
  background-color: blue;
  color: whitesmoke;
}

@media screen and (max-width: 768px) {
  .app {
    width: 100%;
  }
}
</style>

 
 