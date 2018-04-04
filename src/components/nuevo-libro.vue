<template>
<div class="container">
    <div class="row">
        <div class="col-md-12">
        <form-wizard 
        title="Personalizador de libros" 
        subtitle="Crea tu libro personalizado paso a paso"
        back-button-text="Atras"
        next-button-text="Siguiente"
        finish-button-text="Finalizar"
         @on-complete="onComplete">
            <tab-content title="Personaje"
                icon="ti-user"
                :before-change="prota_basico">
                    <h3>Configura tu personaje</h3>
                    <div class="form-group">
                        <label for="prota_nombre">Protagonista:</label>
                        <input type="text" class="form-control" v-model="protagonista.nombre">
                    </div>
                    <div class="form-group">
                        <label for="prota_sexo">Sexo:</label>
                        <select v-model="protagonista.sexo" class="form-control">
                            <option></option>
                            <option value="1">Nene</option>
                            <option value="2">Nena</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="prota_idioma">Idioma:</label>
                        <select v-model="protagonista.idioma" class="form-control">
                            <option value="" disabled selected>Seleccione el idioma</option>
                            <option value="español">Español</option>
                        </select>
                    </div>
            </tab-content>
            <tab-content title="Apariencia"
            icon="ti-face-smile">
                <div class="row">
                   <div class="col-md-6">
                       <label>Piel:</label><select class="form-control" v-on:change="generarPJ" v-model="protagonista.apariencia.color_piel"><option value="1">1</option><option value="2">2</option></select>
                    </div>
                    <div class="col-md-6">
                        <img id="previsualizador"/>
                    </div>
                </div>
            </tab-content>
            <tab-content title="Dedicatoria"
                icon="ti-pencil-alt">

            </tab-content>
            <tab-content title="Datos de registro"
                icon="ti-notepad">
                    Datos de registro
            </tab-content>
            <tab-content title="Completar pago"
                icon="ti-shopping-cart-full"
                :before-change="onConfirmacion">
                     <PayPal
                        amount="10.00"
                        currency="USD"
                        :client="id_clientepp"
                        env="sandbox"
                        v-on:payment-authorized="onAuthOK"
                        v-on:payment-completed="onSaleOK"
                        v-on:payment-cancelled="onPagoCancelado">
                    </PayPal>
            </tab-content>
            <tab-content title="Confirmación"
                icon="ti-check">
                    Confirmación
            </tab-content>
        </form-wizard>
        </div>
    </div>
 </div>
</template>
<script>
import mergeImages from 'merge-images';
import PayPal from 'vue-paypal-checkout'
export default {
    data:  function () {
        return {
            id_clientepp: {
                sandbox: 'AUAY6_aDlrB-MVnUeJo87F9tZlqzfzCa4e_jRJ1e8fH5TkiXqdsqo2DKeJ0Ooo1JpyL7FNkSVoH86Fba',
                production: '<production client id>'
            },
            protagonista:{
                nombre:'',
                sexo:'',
                idioma:'',
                apariencia:{
                    pelo:'1',
                    color_pelo:'1',
                    color_piel:'1'
                }
            },
            pago:false
        }
    },
    components: {
        PayPal
    },
    computed: {
        fondo () {
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/fondo.png')
        },
        ojos_color(){
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/Piel/Ojos1.png')
        },
        piel_contorno(){
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/Piel/PielContorno.png')
        },
        piel_color(){
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/Piel/Piel'+this.protagonista.apariencia.color_piel+'.png')
        },
        pelo_contorno(){
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/Pelo 3/PeloContorno.png')
        },
        pelo_color(){
            return require('../assets/capas/feliz-cumpleanos/Pagina 1/Girl/Pelo 3/Pelo1.png')
        }
    },
	methods: {
        onComplete: function(){
            alert('Yay. Done!');
   },
   onAuthOK: function(resp){
       alert("Autorizado para el pago. Detalles en JSON: "+JSON.stringify(resp));
   },
   onSaleOK: function(resp){
       alert("Pago recibido. Detalles en JSON: "+JSON.stringify(resp));
       this.pago=true;
   },
   onPagoCancelado: function(resp){
       alert("El pago se canceló :(");
   },
   onConfirmacion: function(){
       if(this.pago){
           alert("Pago checkeado, te dejo seguir.");
       } else {
           alert("No recibi tu pago. Intenta de nuevo");
       }
       return this.pago
   },
   generarPJ: function(){
       mergeImages([this.fondo, this.ojos_color, this.piel_color, this.piel_contorno, this.pelo_color, this.pelo_contorno],
       {

       }).then(b64 => document.querySelector('#previsualizador').src = b64);
   },
        prota_basico(){
            if(this.protagonista.nombre && this.protagonista.sexo && this.protagonista.idioma){
                this.generarPJ();
                return true;
            } else {
                return false;
            }
        }
    }
}
</script>
<style>
.wizard-progress-with-circle {
    top: 30px !important;
}
a .wizard-icon {
    font-size: 18px !important;
}
.wizard-icon-circle{
   width: 48px !important;
   height: 48px !important;
}
img {
    max-width: 100%;
}
</style>