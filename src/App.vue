<script setup lang="ts">
type Task = {
  id: number;
  task: string;
  compĺeted: boolean;
  index: number;
};
import Input from "./components/Input.vue";
import Button from "./components/Button.vue";
import { ref } from "vue";

let inputValue = ref("");
const tasks = ref<Task[]>([]);

function onInput(e: Event) {
  inputValue.value = (e.target as HTMLInputElement).value;
}

function onSubmit(e: Event) {
  e.preventDefault();

  tasks.value.push({
    compĺeted: false,
    id: Math.random(),
    task: inputValue.value,
    index: tasks.value.length,
  });
  inputValue.value = "";
}

function removeTask(task: Task) {
  tasks.value = tasks.value.filter((item) => item.id !== task.id);
}

function completeTask(index: Task["index"]) {
  const task = tasks.value[index];
  tasks.value[index].compĺeted = !task.compĺeted;
}
</script>

<template>
  <main>
    <section class="w-full">
      <form
        @submit="onSubmit"
        class="bg-slate-900/50 rounded p-3 gap-4 flex flex-col w-fit mx-auto"
      >
        <Input label="Nova tarefa:" :oninput="onInput" :value="inputValue" />

        <Button>Criar tarefa</Button>
      </form>
    </section>

    <section class="h-[80vh] w-full">
      <h1>Tarefas:</h1>

      <div class="bg-slate-800/50 p-3 rounded w-full max-w-lg mx-auto">
        <ol class="flex flex-col gap-4">
          <li
            v-for="(task, index) in tasks"
            :key="task.id"
            :class="`flex items-center justify-between bg-slate-400/20 p-2 rounded uppercase ${
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
