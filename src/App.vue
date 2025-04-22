<template>
  <div class="contenedor">
    <h1 class="title title--01">Tic Tac Toe</h1>
    <div class="contenedor__turno">
      <h2 class="title title--02">Tu turno:</h2>
      <button 
        class="btn"
        :class="activarBoton(FIGURA.X)"
        @click="cambiarTurno(FIGURA.X)">{{ FIGURA.X }}</button>
      <button  
        class="btn"
        :class="activarBoton(FIGURA.O)"
        @click="cambiarTurno(FIGURA.O)">{{ FIGURA.O }}</button>
      </div> 
      <div class="fila" v-for="(fila, i) in tablero">
        <Casilla 
          v-for="(columna, j) in fila"
          :figura="columna"
          @click="cambiarEstadoCasilla(i, j)"
          />
      </div>
      <div class="fila fila-rst" style="margin-top: 2rem;">
        <h3 class="title title--03"
        v-show="resultado"
        style="color: #fff;">{{ resultado }}</h3>
        <button 
          class="btn btn-reset"
          @click="resetar()">Reset</button>
      </div>
  </div>
</template>

<script setup lang="ts">
import {computed, ref} from "vue";
import Casilla from "./components/Casilla.vue";

enum FIGURA{
  X= '✖️',
  O= '⭕'
}
const resultado = ref<string | null>(null)
const turno = ref(FIGURA.X);
const tablero = ref([
    ['','',''],
    ['','',''],
    ['','','']
])

const resetar = ()=>{
  tablero.value = [
    ['','',''],
    ['','',''],
    ['','','']
  ]
  resultado.value = null;
}

const verificarVertical = (columna: number, figura:string)=>{
  for (let i = 0; i < tablero.value.length; i++) {
    if(tablero.value[i][columna] !== figura){
      return false
    }    
  }
  return true;
}

const verificarHorizontalmente = (fila: number, figura:string)=>{
  for (let i = 0; i < tablero.value.length; i++) {
    if(tablero.value[fila][i] !== figura){
      return false
    }    
  }
  return true;
}

const verificarDiagonalmente = (figura:string)=>{
  if(tablero.value[0][0] === figura && tablero.value[2][2] === figura && tablero.value[1][1] === figura){
    return true
  }
  if(tablero.value[2][0] === figura && tablero.value[0][2] === figura && tablero.value[1][1] === figura){
    return true;
  }
  return false;
}

const tableroVacio = computed(()=>{
  for(let fila of tablero.value){
    for(let columna of fila){
      if(columna !== ''){
        return false
      }
    } 
  }
  return true;
})

const tableroLleno = computed(()=>{
  for(let fila of tablero.value){
    for(let columna of fila){
      if(columna === ''){
        return false
      }
    } 
  }
  return true;
})

const cambiarEstadoCasilla = (fila:number, columna:number)=>{
  if(!tablero.value[fila][columna] && !resultado.value){
    tablero.value[fila][columna] = turno.value;
    turno.value = FIGURA.X === turno.value ? FIGURA.O : FIGURA.X;
    
    //Si Gano
    if(
        verificarVertical(columna, tablero.value[fila][columna]) || 
        verificarHorizontalmente(fila, tablero.value[fila][columna]) ||
        verificarDiagonalmente(tablero.value[fila][columna])){
      resultado.value = `GANÓ ${tablero.value[fila][columna]}`
    }else if(tableroLleno.value){
      resultado.value = 'EMPATE'
    }
  }  
}

const cambiarTurno = (figura:FIGURA)=>{
  if(tableroVacio.value){
    turno.value = figura;
  }
}

const activarBoton = (figura:string)=>{
  return figura === turno.value ? 'btn-activo':'';
} 

</script>

<style scoped>
.title {
  color: var(--color--white);
}

.title--01 {
  font-size: 3rem;
}

.title--02 {
  font-size: 1.8rem;
}

.title--03 {
  font-size: 2.2rem;
  margin-right: 1rem;
}

.contenedor {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.contenedor__turno {
  display: flex;
  margin-bottom: 2rem;
}

.fila button {
  border-radius: .5rem;
}

.fila {
  display: flex;
  align-items: center;
}

.fila.fila-rst {
  display: flex;
  flex-direction: column;
}

.btn {
  font-size: 2rem;
  background-color: transparent;
  border-color: var(--color--green);
  border-radius: .5rem;
  margin: .5rem;
  color: var(--color--white);
  padding: 1rem;
}

.btn.btn-reset {
  background-color: var(--color--red);
  width: 14rem;
  border-radius: 1.8rem;
  border: none;

}

.btn-activo {
  background-color: var(--color--green);
}
</style>