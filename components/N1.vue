<template>
  <div class="hello">
  <div class="tiempo_bar">
        <div class="progress progresotime" id="bt">
        <div class="progress-bar" role="progressbar" v-bind:style="'width:'+llenado_bar+'%'" aria-valuemin="0" aria-valuemax="100"></div>
    </div>
  </div>
  <div class="contenedor">
  <svg class="figura" width="800px" height="300px" viewBox="0 0 800 300">
    <!-- <path d="m96 250 C150 41 350 102 390 250" stroke="#858F98" stroke-width="3" stroke-linecap="round" stroke-dasharray="12 12" fill="transparent"/> -->
    <line v-for="lineas in objCoordenadas.lineas" :key="lineas.length" v-on:id="lineas.p" v-bind:x1="lineas.x1" v-bind:y1="lineas.y1" v-bind:x2="lineas.x2" v-bind:y2="lineas.y2" stroke="#858F98" stroke-width="3" fill="transparent" stroke-dasharray="12 12"></line>
    <circle v-for="ptsA in objCoordenadas.puntos" :key="ptsA.length" v-bind:id="ptsA.p+'1'" v-bind:cx="ptsA.x" v-bind:cy="ptsA.y" r="13" fill="black" stroke="black"/>
    <image display="none" xlink:href="@/assets/azul.png" id="azul" width="50" height="60" :x="this.startX+5" :y="this.startY-60" />
    <image xlink:href="@/assets/azul_B.png" id="azulB" width="50" height="60" :x="this.startX+5" :y="this.startY-60" />
    <image  :xlink:href="require('@/assets/'+imguno)" width="50" height="60" :x="this.figinix-60" :y="this.figiniy-40" />
    <!-- <use :xlink:href="require('@/assets/sprite.svg')+info.icon"></use> -->
    <image  :xlink:href="require('@/assets/'+imgdos)" width="50" height="60" :x="this.figfinx+25" :y="this.figfiny-25" />
  </svg>
  <canvas ref="pizarra" id="pizarra" class="cva" width="800px" height="300px" v-hammer:panstart="captureOn" v-hammer:pan="mo" v-hammer:panend="captureOff"></canvas>
  </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data:function(){
    return{
      mostrar_pasos_T:null,
      paso_mostrar:0,
      circle:null,
      ptxxx:null,
      imguno:"mouse.png",
      imgdos:"quesito.png",
      ordenvisitado:[],
      // informacion
      objCoordenadas:{},
      //tiempo 
      valorparar:null,
      tiempo_mm: 1000/180000,
      llenado_bar:0,
      milisegundos:0,
      segundos:0,
      //dibujo
      figinix:0,
      figiniy:0,
      figfinx:0,
      figfiny:0,
      captureToggle: false,
      isDrawing:false,
      canvas:null,
      context:null,
      rect:null,
      startX:0,
      startY:0,
      puntoinicioA:0,
      puntoinicioB:0,
      //LLAVES
      llave_finalizar:false,
      llave_desvio:false,
      llave_stop:false,
      llave_dibujar:false,
      llave_inicioOK:false,
      punto_siguiente:0,
      llave_punto:[],//pensar si se elimina
      llave_medesvie:false,
    }
  },
  props: {
    plantilla_nombre: String
  }
   ,mounted(){
    console.log(this.plantilla_nombre)
    
    if(this.plantilla_nombre=="preTrazosUnirPatron1"){
      this.objCoordenadas={
          alineacion:"circ",
          cantidad:7,
          imagenA:"jirafa.png", 
          imagenB:"planta.png", 
          puntos:[{x:96,y:90,p:"A"},
          // {x:96,y:46,p:"B"},
          // {x:285,y:46,p:"C"},
          {x:387,y:90,p:"B"},
          // {x:474,y:250,p:"E"},
          // {x:474,y:46,p:"F"},
          // {x:663,y:51,p:"G"},
          {x:663,y:250,p:"C"}],
          lineas:[
                  {x1:96,y1:90,x2:387,y2:90,p:"LA"},
                  {x1:387,y1:90,x2:663,y2:250,p:"LB"},
                  // {x1:285,y1:46,x2:285,y2:250,p:"LC"},
                  // {x1:285,y1:250,x2:474,y2:250,p:"LD"},
                  // {x1:474,y1:250,x2:474,y2:46,p:"LE"},
                  // {x1:474,y1:46,x2:663,y2:46,p:"LF"},
                  //  {x1:663,y1:46,x2:663,y2:250,p:"LG"}
                  ],
          pathlimite:"m 79,75 l 320,0 l 320,190 l -70,0 l -280,-160 l -280,0 z"
      } 
      
    }
    if(this.plantilla_nombre=="preTrazosUnirPatron2"){
      this.objCoordenadas={
          alineacion:"TRIANG",
          cantidad:7,
          imagenA:"astronauta.png", 
          imagenB:"tierra.png", 
          puntos:[{x:96,y:250,p:"A"},
          // {x:194,y:46,p:"B"},
          // {x:285,y:250,p:"C"},
          {x:385,y:51,p:"B"},
          // {x:474,y:250,p:"E"},
          // {x:580,y:51,p:"F"},
          {x:663,y:250,p:"C"}],
          lineas:[{x1:96,y1:250,x2:385,y2:51,p:"LA"},
                  {x1:385,y1:51,x2:663,y2:250,p:"LB"}
                  // {x1:285,y1:250,x2:385,y2:51,p:"LC"},
                  // {x1:385,y1:51,x2:474,y2:250,p:"LD"},
                  // {x1:474,y1:250,x2:580,y2:51,p:"LE"},
                  // {x1:580,y1:51,x2:663,y2:250,p:"LF"}
                  ],
          pathlimite:"m 75,272 l 0,-40 l 300,-200 l 10,0 l 300,220 l -30,20 l -280,-200 l -280,200 z"
      } 
      
    }
    if(this.plantilla_nombre=="preTrazosUnirPatron3"){
      this.objCoordenadas={
          alineacion:"SIXSAG",
          cantidad:8,
          imagenA:"marcian.png", 
          imagenB:"navemarcian.png", 
          puntos:[{x:96,y:47,p:"A"},
          {x:96,y:250,p:"B"},
          {x:285,y:51,p:"C"},
          {x:285,y:250,p:"D"},
          {x:474,y:51,p:"E"},
          {x:474,y:250,p:"F"},
          {x:663,y:51,p:"G"}
          // {x:663,y:250,p:"H"}
          ],
          lineas:[{x1:96,y1:47,x2:96,y2:250,p:"LA"},
                  {x1:96,y1:250,x2:285,y2:47,p:"LB"},
                  {x1:285,y1:47,x2:285,y2:250,p:"LC"},
                  {x1:285,y1:250,x2:474,y2:47,p:"LD"},//
                  {x1:474,y1:47,x2:474,y2:250,p:"LE"},
                  {x1:474,y1:250,x2:663,y2:47,p:"LF"}
                  // {x1:663,y1:51,x2:663,y2:250,p:"LG"}
                  ],
          pathlimite:"m 70,30 l 50,0 l 0,160 l 160,-160 l 25,0 l 0,160 l 160,-160 l 25,0 l 0,160 l 170,-160 l 25,0 l 0,240 l -38,0 l 0,-170 l -170,170 l -20,0 l 0,-170 l -170,170 l -20,0 l 0,-170 l -170,170 l -25,0 Z"
      } 
      
    }
    if(this.plantilla_nombre=="preTrazosUnirPatron4"){
      this.objCoordenadas={
          alineacion:"SIRCIN",
          cantidad:8,
          imagenA:"sapito.png", 
          imagenB:"charquito.png", 
          puntos:[{x:96,y:250,p:"A"},//
          {x:96,y:47,p:"B"},//
          {x:285,y:250,p:"C"},//
          {x:285,y:51,p:"D"},//
          {x:474,y:250,p:"E"},//
          {x:474,y:51,p:"F"},//
          {x:663,y:250,p:"G"}//
          // {x:663,y:51,p:"H"}
          ],//
          lineas:[{x1:96,y1:250,x2:96,y2:47,p:"LA"},//
                  {x1:96,y1:47,x2:285,y2:250,p:"LB"},//
                  {x1:285,y1:250,x2:285,y2:47,p:"LC"},//
                  {x1:285,y1:47,x2:474,y2:250,p:"LD"},//
                  {x1:474,y1:250,x2:474,y2:47,p:"LE"},//
                  {x1:474,y1:47,x2:663,y2:250,p:"LF"}//
                  // {x1:663,y1:250,x2:663,y2:47,p:"LG"}
                  ],//
          pathlimite:"m 75,32 l 33,0 l 160,175 l 0,-170 33,0 l 160,175 l 0,-175 29,0 l 160,175 l 0,-175 l 30,0 l 0,230 l -30,0 l -160,-175 l 0,180 l -10,0 l -10,0 l -160,-175 l 0,180 l -10,0 l -20,0 l -170,-185 l 0,180 l -30,0 Z"
          // pathlimite:"m 70,30 l 50,0 l 0,160 l 160,-160 l 25,0 l 0,160 l 160,-160 l 25,0 l 0,160 l 170,-160 l 25,0 l 0,240 l -38,0 l 0,-170 l -170,170 l -20,0 l 0,-170 l -170,170 l -20,0 l 0,-170 l -170,170 l -25,0 Z"
} 
      
    }
     if(this.plantilla_nombre=="preTrazosUnirPatron5"){
      this.objCoordenadas={
          alineacion:"TRIANG",
          cantidad:7,
          imagenA:"spin.png", 
          imagenB:"bosque.png",
          puntos:[{x:96,y:250,p:"A"},
          {x:194,y:46,p:"B"},
          {x:285,y:250,p:"C"},
          {x:385,y:51,p:"D"},
          {x:474,y:250,p:"E"},
          {x:580,y:51,p:"F"}
          // {x:663,y:250,p:"G"}
          ],
          lineas:[{x1:96,y1:250,x2:194,y2:46,p:"LA"},
                  {x1:194,y1:46,x2:285,y2:250,p:"LB"},
                  {x1:285,y1:250,x2:385,y2:51,p:"LC"},
                  {x1:385,y1:51,x2:474,y2:250,p:"LD"},
                  {x1:474,y1:250,x2:580,y2:51,p:"LE"}
                  // {x1:580,y1:51,x2:663,y2:250,p:"LF"}
                  ],
          pathlimite:"m 176,28 l 32,0 l 80,170 l 78,-172 l 32,0 l 80,170 l 83,-172 l 32,0 l 100,250 l -45,0 l -70,-172 l -90,172 l -30,0 l -70,-172 l -90,172 l -30,0 l -80,-180 l -90,172 l -35,0 Z"
      } 
      
    }
    if(this.plantilla_nombre=="preTrazosUnirPatron6"){
      this.objCoordenadas={
          alineacion:"CUADRA",
          cantidad:7,
          imagenA:"palita.png", 
          imagenB:"florecita.png", 
          puntos:[{x:96,y:250,p:"A"},
          {x:96,y:46,p:"B"},
          {x:285,y:46,p:"C"},
          {x:285,y:250,p:"D"},
          {x:474,y:250,p:"E"},
          {x:474,y:46,p:"F"},
          {x:663,y:51,p:"G"}
          // {x:663,y:250,p:"H"}
          ],
          lineas:[{x1:96,y1:250,x2:96,y2:46,p:"LA"},
                  {x1:96,y1:46,x2:285,y2:46,p:"LB"},
                  {x1:285,y1:46,x2:285,y2:250,p:"LC"},
                  {x1:285,y1:250,x2:474,y2:250,p:"LD"},
                  {x1:474,y1:250,x2:474,y2:46,p:"LE"},
                  {x1:474,y1:46,x2:663,y2:46,p:"LF"}
                  // {x1:663,y1:46,x2:663,y2:250,p:"LG"}
                  ],
          pathlimite:"m 79,33 l 220,0 l 0,205 l 160,0 l 0,-205 l 220,0 l 0,235 l -30,0 l 0,-210 l -160,0 l 0,210 l -220,0 l 0,-210 l -160,0 l 0,210 l -30,0 Z"
      } 
      
    }
   
    this.dibujar_forma(this.objCoordenadas.pathlimite);
    this.imguno=this.objCoordenadas.imagenA;
    this.imgdos=this.objCoordenadas.imagenB;
    this.startX=this.objCoordenadas.puntos[0].x
    this.canvas =this.$refs.pizarra
    this.context=this.canvas.getContext("2d")
    this.rect = this.canvas.getBoundingClientRect();
    this.iniciando();
    this.figinix= this.objCoordenadas.puntos[0].x;
    this.figiniy=this.objCoordenadas.puntos[0].y;
    this.figfinx= this.objCoordenadas.puntos[this.objCoordenadas.puntos.length-1].x;
    this.figfiny=this.objCoordenadas.puntos[this.objCoordenadas.puntos.length-1].y;
     
  },
  methods:{
    //metodos de dibujo
    mo: function(e) {
      if (this.captureToggle==true & this.llave_finalizar==false){
      var x1=e.center.x-this.rect.left;
      var y1=e.center.y-this.rect.top;
      if(!this.llave_desvio){
      this.trazar(x1,y1) 
      }
      }

    },
    captureOn: function(e) {
      this.captureToggle = true;
      this.isDrawing=true;
      document.getElementById('pizarra').style.cursor = 'none';
      if(!this.llave_desvio){
      document.getElementById('azul').style.display='block';
      document.getElementById('azulB').style.display='none';
      this.startX=e.center.x-this.rect.left;
      this.startY=e.center.y-this.rect.top;
      this.llave_stop=false;
      }
    },
    captureOff: function() {
      document.getElementById('azul').style.display='none';
      document.getElementById('azulB').style.display='block';
      document.getElementById('pizarra').style.cursor = 'auto';
      if(!this.llave_desvio){
        this.captureToggle = false;
        this.isDrawing=false; 
      }   
    },
    responder(){
      var aciertos=0;
      var porcentage=parseInt(0.60*(this.objCoordenadas.cantidad-1));
      if(this.plantilla_nombre=='preTrazosUnirPatron1' | this.plantilla_nombre=='preTrazosUnirPatron2'){
        porcentage=2;
      }
      if(this.ordenvisitado.length==this.objCoordenadas.puntos.length){
        for (let i = 1; i < this.ordenvisitado.length; i++) {
          if(this.objCoordenadas.puntos[i].p===this.ordenvisitado[i]){
            aciertos=aciertos+1;
          }
         }
      }      
         console.log(aciertos,">=",porcentage)  
         if(!this.llave_medesvie & aciertos>=porcentage){
             this.$emit("respuestasalumno",true);
         }else{
            document.getElementById("pantalla_r").style.display='block';
            this.fedback();
         }
    },
    trazar(x,y){
    // this.dibujar_forma(this.objCoordenadas.pathlimite);
     this.canvas =this.$refs.pizarra
     this.context=this.canvas.getContext("2d")
     this.rect = this.canvas.getBoundingClientRect();
    //  console.log(parseInt(x),parseInt(y))
     for (let i = 0; i < this.objCoordenadas.lineas.length; i++) {
       if(this.llave_stop==false){
       this.puntos_valida_inicio(x,y,this.objCoordenadas.lineas[0].x1,this.objCoordenadas.lineas[0].y1,this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].x2,this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].y2)
       }
     }
   
    // console.log(x,y)
    if(this.isDrawing==true & this.llave_dibujar==true){
      for (var k = 0; k < this.objCoordenadas.puntos.length; k++) {
          this.xcir=this.objCoordenadas.puntos[k].x;
          this.ycir=this.objCoordenadas.puntos[k].y;
          this.indice=this.objCoordenadas.puntos[k].p;
          this.marcarPos(this.xcir,x,this.ycir,y,this.indice)
       }
      this.llavedraw(x,y)
        this.context.beginPath();
        this.context.moveTo(this.startX,this.startY);
        this.context.lineTo(x,y);
        this.context.lineWidth =10;
        this.context.lineCap ='round';
        this.context.strokeStyle ="#4978D6";
        this.context.stroke();  
        this.startX=x;
        this.startY=y;
      }
    },
    marcarPos( px,x,py,y,indice) {
      
      if(x<px+20 & px-20<x & y<py+20 & py-20<y){
         
         if(this.ordenvisitado[this.ordenvisitado.length-1]!=this.indice){
          // if(this.ordenvisitado.length<this.objCoordenadas.puntos){

            this.ordenvisitado.push(indice)
            console.log(indice,"=",this.objCoordenadas.puntos[this.objCoordenadas.puntos.length-1].p)
          // }
           
          //  if( this.ordenvisitado[this.ordenvisitado.length-1]==this.objCoordenadas.puntos[this.objCoordenadas.puntos.length-1].p){
          //    this.llave_finalizar=true
          //    this.parar_tiempo();
          //    this.responder();
          // }
         }
         
      }
    },
    llavedraw(x,y){

      this.context=this.canvas.getContext("2d");
      if(this.context.isPointInPath(this.ptxxx,x,y)){
        // this.startX=x
        // this.startY=y
         console.log("true")
      }else{
        this.llave_medesvie=true
        console.log("false")
      }
    },
    dibujar_forma(mipath){
      const canvas = document.getElementById('pizarra');
      const ctx = canvas.getContext('2d');
      // this.circle = new Path2D();
      // this.circle.arc(92, 50, 20, 0, 2 * Math.PI);
      // ctx.stroke(this.circle);
       ctx.fillStyle = 'transparent';
       ctx.strokeStyle = "transparent";
      // ctx.fill(this.circle);
      // ctx.stroke(this.circle);
      this.ptxxx = new Path2D(mipath);
      ctx.stroke(this.ptxxx);
      ctx.fill(this.ptxxx);
    },
        //verificacion 
    puntos_valida_inicio(x,y,x1,y1){ 
      // this.puntos_dibuja();
     if(x<x1+15 & x1-15<x & y<y1+15 & y1-15<y){
        if(this.puntoinicioA===0){
        this.puntoinicioA=1
        this.llave_dibujar=true
        console.log("esto iniciando",this.objCoordenadas.lineas[0].p) 
        }
        
     }
      if(x<this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].x2+15 & this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].x2-15<x & y<this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].y2+15 & this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].y2-15<y){
      
       if(this.puntoinicioB===0 & this.puntoinicioA===1){
         this.puntoinicioB=1
         this.llave_dibujar=false
         this.llave_finalizar=true
         this.parar_tiempo();
         this.responder();
        //  console.log("finalice ",this.objCoordenadas.lineas[this.objCoordenadas.lineas.length-1].p) 
       }if(this.puntoinicioB===0 & this.puntoinicioA===0){
         document.getElementById("pantalla_r").style.display='block';
         this.llave_desvio=true
         document.getElementById('azul').style.display='none';
         document.getElementById('azulB').style.display='block';
         setTimeout(() => {
          this.llave_desvio=false
          document.getElementById("pantalla_r").style.display='none';
         },1000);
       }
      
      //  document.getElementById('azul').style.display='none';
        //  document.getElementById('azulB').style.display='block';
    //    this.llave_dibujar=true
      }
  
    },
    limpiar_trazo(){
        this.canvas =this.$refs.pizarra
        var ctx = this.canvas.getContext("2d");
        ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    },
    fedback(){
      this.mostrar_pasos_T = setInterval(
      this.rutacorrecta,1500);
    },
    // metodos para el tiempo 
    iniciando(){ 
      
     this.valorparar = setInterval(
     this.iniciar_tiempo,10);
    },
    iniciar_tiempo(){
    
    this.milisegundos++;

  //  console.log(this.llenado_bar);
    if(this.milisegundos==100){
      this.milisegundos=0;
      this.segundos++;
      // console.log(this.segundos)
    }
    if(this.segundos===181){
        this.parar_tiempo();
        this.fedback();
        // this.$emit("respuestasalumno",false);
        // document.getElementById("pantalla_r").style.display='block';
    } 
    this.llenado_bar=this.llenado_bar+ this.tiempo_mm;    
    // console.log("hola")    
    },
    parar_tiempo(){ 
    clearInterval(this.valorparar);
    },
    rutacorrecta(){    
        if(this.objCoordenadas.puntos.length-1<=this.paso_mostrar){
          clearInterval(this.mostrar_pasos_T)
         this.$emit("respuestasalumno",false);
         document.getElementById("pantalla_r").style.display='block';
        }
        this.context.beginPath();
        this.context.lineWidth=3;
        this.context.strokeStyle ="#01E19E";
        this.context.fillStyle = "#01E19E";
        this.context.arc(this.objCoordenadas.puntos[this.paso_mostrar].x,
        this.objCoordenadas.puntos[this.paso_mostrar].y, 12, 0, 9 * Math.PI);
        this.context.fill();
        this.context.stroke();
        if( this.objCoordenadas.lineas.length>this.paso_mostrar){
        this.context.beginPath();
        this.context.lineWidth=8;
        this.context.strokeStyle = "#01E19E";
        this.context.moveTo(this.objCoordenadas.lineas[this.paso_mostrar].x1, this.objCoordenadas.lineas[this.paso_mostrar].y1);
				this.context.lineTo(this.objCoordenadas.lineas[this.paso_mostrar].x2,this.objCoordenadas.lineas[this.paso_mostrar].y2);
        this.context.stroke();
        }        
      // console.log(this.paso_mostrar);
      this.paso_mostrar+=1;
    },
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.tiempo_bar{
    display: flex;
    justify-content: center;
    margin-bottom: 50px;
}
.progresotime{
    // margin-left: calc(50% - 25%);
    margin: 5px auto;
    margin-top:10px ;
    width: 78%;
    height: 14px;
     border-radius: 20px;
     background: #ffd6ad;; 
}
.progress-bar{
  background-color:orange;
}
.contenedor{
  // outline: 2px solid blue;
  display: flex ;
  justify-content: center;
  width: 90%;
  margin: 0 auto;
  border-top: 3px solid #FFCC0D ;
  border-bottom: 3px solid #FFCC0D;
}
.cva{
  // outline: 2px solid magenta;
  margin: 0 auto;
  position: relative;
}
.figura{
//  outline: 2px solid green;
  position: absolute;
  margin: 0 auto;
}
</style>
