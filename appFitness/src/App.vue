<template>
  <div class="principal">
    <h1>{{'SUDOR Y RISAS'}}</h1>

    <div class="q-pa-md" style="max-width: 300px">
      <q-input label="digite su nombre" v-model="nombre" filled autogrow />
    </div>
    <div class="q-pa-md" style="max-width: 300px">
      <q-input label="digite la edad" v-model="edad" filled autogrow />
    </div>
      <footer>
        <div class="q-pa-md ">
          <div class="q-gutter-y-md" style="max-width: auto">
            <q-tabs v-model="tab" class="text-teal">
              <q-tab name="inicio" icon="food_bank" label="Inicio" />
              <q-tab name="comidas" icon="food_bank" label="comidas" />
              <q-tab name="entrenamientos" icon="fitness_center" label="entrenamientos" />
              <q-tab name="perfil" icon="food_bank" label="perfil" />
              <q-tab name="more" icon="fitness_center" label="more" />
            </q-tabs>
            <!-- contenido   -->
            <q-tab-panels v-model="tab" animated>
              <q-tab-panel name="comidas">
                <h4>COMIDAS</h4>
                <div class="parteUno">
                  <div class="card">
                    <cards title="Desayuno" :image="desayuno" colorFondo=""></cards>
                    <botones @accion="agregar" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Almuerzo" :image="almuerzo" colorFondo=""></cards>
                    <botones @accion="agregar" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Cena" :image="cena" colorFondo=""></cards>
                    <botones @accion="agregar" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Bebidas" :image="bebidas" colorFondo=""></cards>
                    <botones @accion="agregar" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                </div>
              </q-tab-panel>

              <q-tab-panel name="entrenamientos">
                <h4>ENTRENAMIENTOS</h4>
              </q-tab-panel>
            </q-tab-panels>
          </div>
        </div>
      </footer>
    </div>
  
</template>

<script setup>
import { ref } from "vue";
import cards from "./components/cards.vue";
import botones from "./components/botones.vue";
import desayuno from "./assets/desayuno.png";
import almuerzo from "./assets/almuerzo.png";
import cena from "./assets/cena.png";
import bebidas from "./assets/bebidas.png";
import { evaFileAdd } from "@quasar/extras/eva-icons";

const tab = ref("inicio");

function agregar() {
  console.log("");
}
let desayunos = ref(["pan","manzana","huevos","chocolate","tinto"]);
let almuerzos= ref(["arroz con pollo","caldo de carne","pastas con carne","mojarra frita","aire"]);
let comidas = ref(["changua","huevos revueltos","empanadas","hamburguesa","salchipapa"]);
let nombre = ref("");
let edad = ref("");
</script>

<style>
* {
  margin: 0;
}

.principal {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
  font-size: 2.5rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

footer {
  text-align: center;
  align-content: center;
}

/*  tarjetas */
.parteUno {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 25px;
  margin-bottom: 40px;
}

.card {
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 25px;
  border: none;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
}

.card h2 {
  font-size: 1.8rem;
  color: #34495e;
  margin-bottom: 20px;
  text-align: center;
}

.card img {
  width: 180px;
  height: 180px;
  object-fit: cover;
  border-radius: 15px;
  border: 3px solid #ecf0f1;
  margin-bottom: 20px;
  transition: transform 0.3s ease;
}

.card:hover img {
  transform: scale(1.05);
}

/* Botones */
.card button {
  margin-top: 15px;
  padding: 12px 25px;
  border-radius: 30px;
  font-weight: 600;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
}

/* Footer */
footer {
  background: white;
  border-radius: 15px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
  padding: 20px;
  margin-top: 30px;
}

.q-tabs {
  background: #f8f9fa;
  border-radius: 10px;
  padding: 5px;
}

.q-tab {
  color: #7f8c8d !important;
  font-weight: 500;
  transition: all 0.3s ease;
}

.q-tab--active {
  color: #009688 !important;
  background: rgba(0, 150, 136, 0.1);
  border-radius: 8px;
}

.q-tab-panel {
  padding: 20px;
  min-height: 200px;
}

.q-tab-panel h4 {
  color: #2c3e50;
  margin-top: 0;
}
</style>