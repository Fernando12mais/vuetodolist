<script setup lang="ts">
type Task = {
  id: number;
  task: string;
  compĺeted: boolean;
  index: number;
};
import Input from "./components/Input.vue";
import Button from "./components/Button.vue";
import { onMounted, ref } from "vue";

const inputValue = ref("");
const submitted = ref(false);

const tasks = ref<Task[]>([]);

function onInput(e: Event) {
  inputValue.value = (e.target as HTMLInputElement).value;
}

function onSubmit(e: Event) {
  e.preventDefault();
  submitted.value = true;

  if (inputValue.value.trim().length < 1) return;

  tasks.value.push({
    compĺeted: false,
    id: Math.random(),
    task: inputValue.value,
    index: tasks.value.length,
  });
  inputValue.value = "";
  submitted.value = false;

  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function removeTask(task: Task) {
  tasks.value = tasks.value.filter((item) => item.id !== task.id);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function completeTask(index: Task["index"]) {
  const task = tasks.value[index];
  tasks.value[index].compĺeted = !task.compĺeted;
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

onMounted(() => {
  const storagedTasks = localStorage.getItem("tasks");

  if (storagedTasks) {
    tasks.value = JSON.parse(storagedTasks);
  }
});
</script>

<template>
  <main>
    <section class="w-full">
      <form
        @submit="onSubmit"
        class="bg-slate-900/50 rounded p-3 gap-4 flex flex-col w-fit mx-auto"
      >
        <Input label="Nova tarefa:" :oninput="onInput" :value="inputValue" />
        <span
          class="text-rose-600"
          v-if="inputValue.trim().length < 1 && submitted"
          >Preencha este campo para criar uma tarefa</span
        >
        <Button>Criar tarefa</Button>
      </form>
    </section>

    <section class="w-full">
      <h1>{{ tasks.length ? "Tarefas:" : "Crie uma tarefa" }}</h1>

      <div
        v-if="tasks.length"
        class="bg-slate-800/50 p-3 rounded w-full max-w-lg mx-auto animate-fadeIn"
      >
        <ol class="flex flex-col gap-4">
          <li
            v-for="(task, index) in tasks"
            :key="task.id"
            :class="`flex items-center justify-between bg-slate-400/20 p-2 rounded uppercase animate-fadeIn ${
              task.compĺeted ? 'bg-cyan-900' : ''
            }`"
          >
            <span
              :class="
                task.compĺeted && 'line-through text-emerald-300 font-bold'
              "
            >
              {{ task.task }}
            </span>

            <div class="flex items-center gap-4">
              <Button variant="done" :onclick="() => completeTask(index)">{{
                task.compĺeted ? "Desfazer" : "Feito"
              }}</Button>
              <Button variant="delete" :onclick="() => removeTask(task)"
                >Deletar tarefa</Button
              >
            </div>
          </li>
        </ol>
      </div>
    </section>
  </main>
</template>
