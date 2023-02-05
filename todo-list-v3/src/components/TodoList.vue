<template lang="">
  <div class="todo-list">
      <h1>To-Do List</h1>
    
      <!-- um input do tipo text, emque toda vez que apertar 'enter' (@change="") vai add algo, é  este algo é o model que vai ser do tipo texto -->
      <input class="text-fiel" type="text" @change="addToList" v-model="text" />
      <ul>
        <!-- v-for é usado quando queremos que algo repita, neste caso colocamos no li, para ele se repetir toda vez que for add algo na lista -->
        <li v-for="(item, index) in list" :key="index">
          <span @click="toggleCheckbox(item)">
            <!-- este :checked é uma propriedade que check se é true ou false. -->
            <input type="checkbox" :checked="item.done">    
            <!-- check se foi ativado ou não, se sim, ele risca, se não ele deixa como está.. OBS: tem algumas config no css -->
            <!-- é criando uma class, onde o nome 'done' é a referencia e o : item.done é a condição de true ou false -->
            <span :class="{'done' : item.done}">{{ item.label }}</span>
        </span>
          <span @click="deleteFromList(index)">delete</span>
        </li>
      </ul>
  </div>
  
</template>

<script>
export default {
  data() {
    return {
      // é um array vazio e um texto vazio
      list: [],
      text: "",
    };
  },
  created() {
    // para pegar o conteúdo no inicio, eu desconstuo o JSON, voltando para o que era antes, usando o JSON.parse(), dentro eu passo o localstorage pegando o item, que neste caso é o array que se encontra no data. :
    this.list = JSON.parse(localStorage.getItem("list")) || [];
    // a logica é que, se tiver algo dentro, add, caso não, é um array vasio, por isso ao dinal é importante coloca || []
  },
  methods: {
    // este metodo é chamado toda que apertar o enter, que se encontra la no @change="addToList"
    addToList() {
      // add um novo item no inicio da lista
      //Neste caso para que os checkbox sejam gravados, foi necessario tranformar o um objeto
      this.list.unshift({label: this.text, done: false}); 
      this.updateLocalStorage();

      // limpa o campo de input
      this.text = "";
    },

    deleteFromList(index) {
      this.list.splice(index, 1);
      this.updateLocalStorage();
    },

    // este metodo, alem de adicionar o elemento ao local storage, ele atualiza (update), e com isso fica o codigo mais limpo, necessitando apenas a chamada nos outros metodos
    updateLocalStorage() {
      // add a lista no localstorage para que quando o browser atualizar, não perde a lista
      // como a minha lista é um array, para transformar em string, que é o formato usado no local storage, basta usar o JSON.stringify() e passa a lista dentro dos parenteses
      localStorage.setItem("list", JSON.stringify(this.list));
    },
    //Este metodo é para que quando o checkbox for acionado 
    toggleCheckbox(item) {
        // Desta forma estou negando o que tem.
        item.done = !item.done; 
        this.updateLocalStorage()
    }
  },
};

// este metodo não é default (padrão) então pode ser qualquer metodo dentro do @change, desde que chame ele dentro do methods, tipo uma função.
</script>
