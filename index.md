## Especies de flores
Entrene su aplicación para distinguir entre diferentes especies de flores.
<div> Especies </div>
<button type = "button" onclick = "init ()"> Iniciar </button>
<div id = "webcam-container"> </div>
<div id = "label-container"> </div>
<script src = "https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@1.3.1/dist/tf.min.js"> </script>
<script src = "https://cdn.jsdelivr.net/npm/@teachablemachine/image@0.8/dist/teachablemachine-image.min.js"> </script>
<script type = "text / javascript">
    // Más funciones de API aquí:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image

    // el enlace a su modelo proporcionado por el panel de exportación de Teachable Machine
    const URL = "https://teachablemachine.withgoogle.com/models/ilcN7gXIs/";

    dejar modelo, webcam, labelContainer, maxPredictions;

    // Cargue el modelo de imagen y configure la cámara web
    función asíncrona init () {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        // carga el modelo y los metadatos
        // Consulte tmImage.loadFromFiles () en la API para admitir archivos de un selector de archivos
        // o archivos de su disco duro local
        // Nota: la biblioteca de pose agrega el objeto "tmImage" a su ventana (window.tmImage)
        modelo = espera tmImage.load (modelURL, metadataURL);
        maxPredictions = model.getTotalClasses ();

        // Función de conveniencia para configurar una cámara web
        const flip = verdadero; // si voltear la webcam
        webcam = nueva tmImage.Webcam (200, 200, flip); // ancho, alto, voltear
        espera webcam.setup (); // solicitar acceso a la webcam
        esperar webcam.play ();
        window.requestAnimationFrame (bucle);

        // agregar elementos al DOM
        document.getElementById ("contenedor de webcam"). appendChild (webcam.canvas);
        labelContainer = document.getElementById ("etiqueta-contenedor");
        for (let i = 0; i <maxPredictions; i ++) {// y etiquetas de clase
            labelContainer.appendChild (document.createElement ("div"));
        }
    }

    bucle de función asíncrona () {
        webcam.update (); // actualiza el marco de la webcam
        esperar predecir ();
        window.requestAnimationFrame (bucle);
    }

    // ejecutar la imagen de la cámara web a través del modelo de imagen
    función asíncrona predecir () {
        // predecir puede tomar una imagen, un video o un elemento html de lienzo
        predicción constante = espera modelo.predicto (webcam.canvas);
        para (sea i = 0; i <maxPredictions; i ++) {
            const classPrediction =
                predicción [i] .className + ":" + predicción [i] .probability.toFixed (2);
            labelContainer.childNodes [i] .innerHTML = classPrediction;
        }
    }
</script>
<script src = "https://www.gstatic.com/dialogflow-console/fast/messenger/bootstrap.js?v=1"> </script>
<df-messenger
  intent = "BIENVENIDO"
  chat-title = "EspeciesdeFlores"
  id-agente = "b548e786-bd4c-4738-b25f-5e6175f7ce80"
  código-idioma = "es"
> </df-messenger>
