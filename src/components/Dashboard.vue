<template>
  <div id="burger-table">
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
  </div>
  <div id="burger-table-rows">
    <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
      <div class="order-number">{{ burger.id }}</div>
      <div>{{ burger.nome }}</div>
      <div>{{ burger.pao }}</div>
      <div>{{ burger.carne }}</div>
      <div>
        <ul>
          <li v-for="(opcional, index) in burger.opcionais" :key="index">
            {{ opcional }}
          </li>
        </ul>
      </div>
      <div>
        <select name="status" id="status" @change="getUpdate($event,burger.id)">
          <!--selected compara o valor do objeto com o status, para indicar qual status deve apresentar no objeto-->
          <option v-for="estado in status" :key="estado.id" :value="estado.tipo" :selected="burger.status == estado.tipo">
            {{ estado.tipo }}
          </option>
        </select>
        <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
      </div>
    </div><!--burger-table-row-->
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: []
    }
  },
  methods: {
    async getRequests(){
      const req = await fetch("http://localhost:3000/burgers");

      const data = await req.json();

      this.burgers = data;

      
      // console.log(this.burgers)

      //resgatar status
      this.getStatus()

    },
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");

      const data = await req.json()

      this.status = data;

      // console.log(this.status)
    }, async deleteBurger(id){
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE"
      });

      const res = await req.json()


      //msg

      this.getRequests()
    },
    //atualizar status
    async getUpdate(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option })

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: {
          "Content-Type":"application/json"
        },
        body: dataJson
      });

      const res = await req.json()

      console.log(res)
    }
  },
  mounted() {
    this.getRequests()
  }
}
</script>

<style scoped>
  #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }
  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #ccc;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }


  select {
    padding: 12px 6px;
    margin: 12px;
  }
  .delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: solid 2px #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>