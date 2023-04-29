<script setup>
import { ref, computed } from 'vue'
const jugador = ref('X') //Variable que permite identificar al usuario que está en el turno.
const tablero = ref([ //Tablero inicial, si se inicia el juego parte sin datos//
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])
const saberGanador = (tablero) => {
  const jugadasGanadoras = [ //Cuando se cumplen éstos espacios (simulando un tablero de 3x3), entonces hay un ganador.
    [6, 7, 8],
    [0, 3, 6],
    [2, 5, 8],
    [0, 1, 2],
    [3, 4, 5],
    [0, 4, 8],
    [2, 4, 6]]

  for (let i = 0; i < jugadasGanadoras.length; i++) {
    const [a, b, c] = jugadasGanadoras[i]
    if (tablero[a] && tablero[a] === tablero[b] && tablero[a] === tablero[c]) { //Ésta línea compara la posición a con la b y c, si son iguales entonces hay un ganador.
      return tablero[a]
    }
  }
  return null
}
const ganador = computed(() => saberGanador(tablero.value.flat()))
const jugada = (x, y) => {
	if (ganador.value) return //Si hay un jugador que ganó la partida, entonces se retorna (se reinicia la partida).
	if (tablero.value[x][y]) return 
	tablero.value[x][y] = jugador.value
	jugador.value = jugador.value === 'X' ? 'O' : 'X' //Ésta línea contiene un operador ternario que cambia de turno al jugador que ya hizo su jugada.
}
const reiniciarElJuego = () => {
	tablero.value = [   /* Éste es el tablero pincipal, el que aparecerá sin datos cuando se visualice la aplicación web*/
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	jugador.value = 'X'  //Se referencia con X al usuario, para detectar su jugada una vez que la haga.
}
</script>

<template>
	<main class="text-center py-10">
    <h1 id="title" class="mb-2 text-4xl font-bold">Tres en línea</h1>
		<div class="flex flex-col items-center mb-8">
      	
			<div 
				v-for="(row, x) in tablero" 
				:key="x"
				class="flex">
				<div id="square"
					v-for="(cell, y) in row" 
					:key="y" 
					@click="jugada(x, y)" 
					:class="`border-2 border-blue w-44 h-44 hover:bg-violet-900 duration-300 flex items-center justify-center material-icons-outlined text-7xl font-bold cursor-pointer ${cell === 'X' ? 'text-yellow-500' : 'text-orange-800'}`">
					{{ cell === 'X' ? 'X' : cell === 'O' ? '0' : '' }} <!--Ternario que define las marcas de cada jugador, y deja vacío el tablero en caso de que no hayan jugadas-->
				</div>
			</div>
		</div>

		<div class="text-center">
      <h2 v-if="ganador" class="text-5xl font-bold mb-10">Ganador: '{{ ganador }}'  !</h2>
      <button @click="reiniciarElJuego" class="px-6 py-4 bg-violet-500 rounded font-bold hover:bg-violet-800 duration-300">Reiniciar</button>
				
        </div>
        
	</main>
</template>
<style>
body {
@apply bg-cyan-900 text-white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

@media (max-width: 700px) {
  main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-left: 0;
    margin-bottom: 0;
    margin-right: 0;
  }

  #title{
    margin-top: -25%;
  }
  body {
	@apply bg-cyan-900 text-white;
  }
  #square{
    width: 70px;
    height: 70px;
  }
}
</style>
