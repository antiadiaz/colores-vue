<script setup>
  import {ref, onMounted} from 'vue'
  import Formulario from "./Formulario.vue" // lo importamos
  import Item from "./Item.vue"


  let colores = ref([])

  onMounted(()=>{
    fetch("https://backend-colores.onrender.com/colores")
    .then(respuesta => respuesta.json())
    .then(respuestaProcesada => colores.value = respuestaProcesada)
  })


  function crearColor(color){
        fetch("https://backend-colores.onrender.com/colores/nuevo",{
       method : "POST",
       body : JSON.stringify(color),
       headers : {
        "Content-type" : "application/json"
       }
    })
    .then(respuesta => respuesta.json())
    .then(({id,error}) => {
      if (!error){
        return colores.value.push({id, ...color});
      }
      console.log("error del usuario")
    });
  }

  function borrarColor(id){
    fetch(`https://backend-colores.onrender.com/colores/borrar/${id}`,{
    method : "DELETE"
    })
    .then(respuesta => respuesta.json())
    .then(({error}) => {
    if(!error){
      return colores.value = colores.value.filter(color => color.id != id);
    }
    console.log("error al usuario");
    });
  }

  
</script>

<template>
  <Formulario @crear="crearColor"/>
  <ul>
    <Item v-for="({id,r,g,b}) in colores" :id="id" :r="r" :g="g" :b="b" @borrar="borrarColor"/>
  </ul>
</template>

