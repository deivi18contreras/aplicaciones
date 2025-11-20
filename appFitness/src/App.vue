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
              <q-tab-panel name="inicio">
                <h4>HOME</h4>
                <div v-if="mensaje" class="mensaje-epico">
                  <div class="card">
                  {{ mensaje }}
                </div>
                 </div>
              </q-tab-panel>


              <q-tab-panel name="comidas">
                <h4>COMIDAS</h4>
                <div class="parteUno">
                  <div class="card">
                    <cards title="Desayuno" :image="desayuno" colorFondo=""></cards>
                    <botones @click="abrirMenu('desayuno')" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Almuerzo" :image="almuerzo" colorFondo=""></cards>
                    <botones @click="abrirMenu('almuerzo')" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Cena" :image="cena" colorFondo=""></cards>
                    <botones @click="abrirMenu('cena')" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
                    </botones>
                  </div>
                  <div class="card">
                    <cards title="Bebidas" :image="bebidas" colorFondo=""></cards>
                    <botones @click="abrirMenu('bebidas')" colorFondo="red" textoColor="black" nombreBoton="Agregar Alimento">
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
import { useQuasar } from 'quasar'
const $q = useQuasar()
const tab = ref("comidas");
const seleccionFinal = ref([]) 
const mensaje = ref("");

import { GoogleGenAI } from "@google/genai";

const IA = new GoogleGenAI({
  apiKey: "AIzaSyAYxosLe9ts62wxwRESgaSxrLcL8CuOs78"
});

async function LecturaGeneradaIA(prompt) {
  try {
    const response = await IA.models.generateContent({
      model: "gemini-2.5-flash",
      contents: [
        {
          role: "user",
          parts: [{ text: prompt }]
        }
      ]
    });

    const texto = response.candidates?.[0]?.content?.parts?.[0]?.text || 
                  "No pude generar un mensaje 😭";

    return texto;

  } catch (error) {
    console.error("Error IA:", error);
    return "Error generando el mensaje épico 💀";
  }
}



const menus = {
  desayuno: ["pan","manzana","huevos","chocolate","tinto"],
  almuerzo: ["arroz con pollo","caldo de carne","pastas con carne","mojarra frita","aire"],
  cena: ["changua","huevos revueltos","empanadas","hamburguesa","salchipapa"],
  bebidas: ["agua","leche","jugo de fresa","gaseosa", "gatorade" ]
}

function abrirMenu(tipo) {
  const opciones = menus[tipo].map(item => ({
    label: item,
    value: item,
    color: 'primary'
  }))

  $q.dialog({
    title: `Menú de ${tipo}`,
    message: 'Selecciona tus opciones:',
    options: {
      type: 'toggle',
      model: [],
      items: opciones
    },
    cancel: true,
    persistent: true
  })
  .onOk(async seleccion => {
  seleccionFinal.value = seleccion;

  const prompt = `
Quiero que actúes como un entrenador fitness exagerado, motivador y extremadamente dramático sabiendo que el usuario selecciono estos items ${seleccion.join(", ")}. Cada vez que el usuario seleccione un alimento o un ejercicio, debes responder con un mensaje corto, divertido y muy épico.
Las respuestas deben sonar como si el usuario hubiera hecho algo absolutamente legendario, incluso si la acción es mínima o el alimento es común.
Usa humor, exageración extrema y un estilo épico-motivacional.
Si el usuario selecciona un alimento, exagera lo saludable, poderoso o transformador que es comerlo.
Si el usuario selecciona un ejercicio, exagera lo heroico, épico o salvaje que es haberlo realizado.
Las respuestas deben ser siempre positivas, motivadoras y emocionantes.
Formato de respuesta: 1 o 2 líneas, muy intensas y emocionantes.
Ejemplos:
• Si selecciona “pan”: ‘¡DIOS MÍO! Acabas de elegir pan… el alimento de los guerreros ancestrales. ¡Tu cuerpo te lo agradece con +999 de energía vital!’
• Si selecciona “arepa”: ‘¡INCREÍBLE! Una arepa… la rueda dorada que impulsa a los campeones. ¡Estás imparable!’
• Si hace “mover un brazo”: ‘¡¿QUÉEE?! ¡Ese movimiento de brazo equivalió a correr una maratón cuesta arriba en el Everest! Eres pura leyenda.’
• Si hace “sentadilla”: ‘¡¡BRUTAL!! Esa sentadilla liberó más poder que una tormenta eléctrica. ¡Tus piernas ya son armas secretas!’

Siempre responde con ese estilo.
  `;

  mensaje.value = await LecturaGeneradaIA(prompt);
})


}

 

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