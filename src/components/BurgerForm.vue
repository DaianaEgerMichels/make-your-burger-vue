<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form id="burger-form" method="POST" @submit="createBurger">
                <div class="input-container">
                    <label for="nameClient">Client Name:</label>
                    <input type="text" id="nameClient" name="nameClient" v-model="nameClient" placeholder="Enter your name here">
                </div>
                <div class="input-container">
                    <label for="bread">Choose the Type of Bread:</label>
                    <select id="bread" name="bread" v-model="bread">
                        <option value="">Select the Bread</option>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{bread.tipo}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Choose the Type of Meat:</label>
                    <select id="meat" name="meat" v-model="meat">
                        <option value="">Select the Meat</option>
                        <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{meat.tipo}}</option>
                    </select>
                </div>
                <div id="options-container" class="input-container">
                    <label id="options-title" for="options">Select the Options:</label>
                    <div class="checkbox-container" v-for="optional in optionalsdata" :key="optional.id">
                        <input type="checkbox" name="options" v-model="options" :value="optional.tipo">
                        <span>{{optional.tipo}}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Create My Burger">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'BurgerForm',
    data() {
      return {
        breads: null,
        meats: null,
        optionalsdata: null,
        nameClient: null,
        bread: null,
        meat: null,
        options: [],
        msg: null
      }
    },
    methods: {
      async getIngredients() {

        const req = await fetch("http://localhost:3000/ingredients");
        const data = await req.json();

        this.breads = data.breads;
        this.meats = data.meats;
        this.optionalsdata = data.optionals;
      },
      async createBurger(e) {
        e.preventDefault();

        const data = {
          nameClient: this.nameClient,
          meat: this.meat,
          bread: this.bread,
          optionals: this.optionals,
          status: "Requested"
        }

        const dataJson = JSON.stringify(data);

        const req = await fetch("http://localhost:3000/burgers", {
          method: "POST",
          headers: {"Content-Type": "application/json"},
          body: dataJson
        });

        const res = await req.json();

        //clean fields
        this.nameClient = "";
        this.meat = "";
        this.bread = "";
        this.options = "";
      }
    },
    mounted(){
      this.getIngredients();
    }
}
</script>

<style scoped>
  #burger-form {
    max-width: 500px;
    margin: 0 auto;
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 25px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 6px solid #FCBA03;
  }

  label, input, select {
    font-size: 20px;
  }

  input, select {
    padding: 5px 10px;
    width: 400px;
  }

  #options-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #options-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }

  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }

  .checkbox-container span {
    margin-left: 8px;
    font-weight: bold;
    font-size: 18px;
  }

  .submit-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    /* margin: 0 auto; */
    cursor: pointer;
    transition: .5s;
  }

  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>