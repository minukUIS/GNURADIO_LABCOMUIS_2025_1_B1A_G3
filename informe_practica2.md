# Práctica 2A. Modelo de canal

## Objetivos
- Observar cómo el canal puede afectar la calidad de la señal transmitida y cómo  mitigar sus efectos.
- Evaluar aspectos clave como la relación señal-ruido y la eficiencia en la transmisión de datos.

Este enfoque permitirá no solo verificar la teoría, sino también desarrollar habilidades prácticas en el manejo de equipos de laboratorio, como equipos de medición (USRP 2920, osciloscopio R&S RTB2004 y analizador de espectros R&S FPC1000).

---

## Materiales y Equipos

- **USRP 2920:** Radio definido por software.
- **Osciloscopio R&S RTB2004:** Para visualización de señales en el dominio del tiempo y la frecuencia.
- **Analizador de Espectros R&S FPC1000:** Para mediciones en el dominio de la frecuencia.
- **Computador con GNU Radio:** Para simulación y generación de señales usando el USRP 2920.
- **Cables y conectores:** Para interconexión de equipos.
- **Audífonos y micrófono** (opcional, debe traerlo cada grupo)

### Ajustes preliminares

- En el [flujograma](filters_flowgraph.grc) propuesto para esta práctica, se incluye un bloque "Wav File Source". **Antes** de ejecutar el flujograma, seleccione un archivo WAV para ser usado por este bloque. Algunos archivos WAV de ejemplo los puede encontrar en: [LabComUIS/samples/](../../samples/) 
- Tenga en cuenta que existen instrumentos de visualización en dominio tiempo y frecuencia tanto para la señal ANTES como DESPUÉS del filtro.
---

## Actividad 1: Actividades de simulación de canal en GNU Radio

### Objetivo

Familiarizarse con algunos fenómenos de canal en un ambiente simulado.

### Procedimiento

**Simulación**
   - Verificar equipos y elementos a utilizar (revisar manuales de ser necesario)
   - Cargar el flujograma: [filters_flowgraph.grc](filters_flowgraph.grc).
   - Configurar siempre la frecuencia de muestreo (`samp_rate`) en $25e6/2^n$ Hz`, donde $n$ es un número entero mayor a 2.
   - Genere diferentes señales y observe el efecto de variar las frecuencias de corte del filtro.
   - Analice el efecto del ruido en el dominio del tiempo y la frecuencia para al menos dos formas de onda distintas.
   - Muestre con un ejemplo gráfico el umbral de máximo de ruido ante el cual considera que es posible recuperar cada forma de onda utilizando únicamente filtrado.

### Preguntas Orientadoras

- ¿Cuál es el efecto de filtrar las frecuencias altas de una señal? Puede perder parte la forma de la señal ya que se pierden armónicos.
- ¿Qué sucede al filtrar muy cerca de la frecuencia fundamental de la señal?. Se pierde la forma de la señal, es decir, una señal cuadrada si se le filtra la frecuencia fundamental quedarían pequeñas "vibraciones" en los puntos donde pasa de High a Low o en otras palabras los puntos donde hay transiciones bruscas de señal.
- ¿Cuál es el efecto de filtrar las frecuencias bajas de una señal? Al filtrar las frecuencias bajas de la señal, esta podría distorsionarse dependiendo del tipo de señal.
- ¿Qué ocurre al eliminar armónicos de una señal? Al eliminar armónicos de una señal, se parece cada más a una senoidal pura. Pero para mantener la forma basta con mantener los primeros 3 armónicos de la frecuencia fundamental.
- ¿Qué efecto tiene la desviación de frecuencia en la señal recibida? ¿Qué efecto(s) produce el filtro cuando la señal recibida se ve afectada por desviación de frecuencia? El desviar la frecuencia en una señal, esta comienza a tener ondulaciones indeseados a lo largo de la señal, esto debido a que aparecen frecuencias donde no deberían estar debido al desplazamiento.
- ¿Cómo cuantificar la degradación de la señal al aumentar los niveles de ruido? Utilizando la relación señal a ruido.
- ¿Cómo se puede mejorar la relación señal a ruido en una señal? Utilizando filtros, se pueden eliminar las frecuencias indeseadas donde el ruido es muy prominente y la señal es claramente observable.
- ¿Cómo podría cuantificar la calidad de la señal recibida? Considere el caso de señales analógicas y digitales.

### Evidencia

*(Adjuntar las evidencias de la práctica en el Aula Virtual: capturas de pantalla, observaciones, cálculos o mediciones preliminares)*

---
![fe4057c2-5329-4152-9c3c-f1446af1eab5](https://github.com/user-attachments/assets/743d0f33-eb41-418f-abc7-1158d2b4a2ea)
![13e83b33-f49e-4942-ae4c-167c2faec137](https://github.com/user-attachments/assets/d177fe8a-b0a4-4945-a0ca-476724d25383)
![13e83b33-f49e-4942-ae4c-167c2faec137](https://github.com/user-attachments/assets/9f15f312-57b2-4493-8e70-b2d7f4eb79c1)

## Actividad 2: Fenómenos de canal en el osciloscopio

### Objetivo

Familiarizarse con los fenómenos de un canal alámbrico real en el dominio del tiempo.

### Procedimiento

1. **Configurar el USRP 2920:**
   - Configurar el flujograma [filters_flowgraph.grc](filters_flowgraph.grc) en GNU Radio para transmitir una señal a través del USRP.
   - Habilitar o deshabilitar los bloques correspondientes (`Channel Model`, `Throttle`, `UHD: USRP Sink`, `UHD: USRP Source`, `Virtual Sink`). Para esto, seleccione el bloque deseado y presione **E** (enable) o **D** (disable), según corresponda.
   - Configurar siempre la frecuencia de muestreo (`samp_rate`) en $25e6/2^n$ Hz`, donde $n$ es un número entero mayor a 2. Verifique que la frecuencia de muestreo durante la ejecución, sea la misma que ha configurado en el flujograma.

2. **Configurar el osciloscopio:**
   - Encender, configurar y conectar el osciloscopio a la salida del USRP 2920 usando diferentes cables coaxiales, y ajustando los parámetros necesarios para evidenciar los fenómenos de canal analizados en la Actividad 1.
   - Variar la frecuencia de portadora del USRP entre 50 MHz hasta 500 MHz y anaalizar los resultados.

### Preguntas Orientadoras

- ¿Cuál es el efecto del ruido sobre la amplitud de las señales medidas en el osciloscopio? ¿Conservan las mismas relaciones que se evidencian en la simulación? Si, mantienen las mismas relaciones que se se evidencian en la simulación dando una distorsión a la señal según el nivel de ruido.
- ¿La relación señal a ruido creada intencionalmente en el computador se amplifica o se reduce en la señal observada en el osciloscopio? Aparentemente la SNR se amplificó comparado a la simulación
- Demuestre ¿cómo se puede mejorar la relación señal a ruido en una señal? Se puede mejorar utilizando filtros o aumentar la ganancia o amplitud de la señal que se transmite.
- ¿Cómo se evidencia el fenómeno de desviación de frecuencia en el osciloscopio? Evidenciar al menos con dos formas de onda.
- Determine la afectación de un medio de transmisión coaxial (usar cables largos) sobre una señal periódica operando a las capacidades máximas de muestreo del USRP.
- 
  - **NOTA:** La frecuencia de transmisión no debe superar los 500 MHz para ser observada en el osciloscopio. Para el experimento, considere las relaciones de muestreo correspondientes.
- Usando cables coaxiales de diferentes longitudes, ¿cómo afecta la distancia entre el transmisor y el receptor a la amplitud de la señal medida? La amplitud de la señal en el osciloscopio comparado a la simulación decrementó
- Usando antenas, ¿cómo afecta la distancia entre el transmisor y el receptor a la amplitud de la señal medida? ¿Es posible compensar el fenómeno? Si es posible compensar el fenómeno, por ejemplo usando las manos como reflectores de señal
- ¿Qué modelo de canal básico describe mejor las mediciones obtenidas en la práctica? Canal con Ruido Gaussiano o Canal de Cable Coaxial con atenuación

### Evidencia

*(Adjuntar las evidencias de la práctica en el Aula Virtual: capturas de pantalla, observaciones, cálculos o mediciones preliminares)*

---

![1394cfd6-1f62-4c95-85af-d04c8e4a1e20](https://github.com/user-attachments/assets/8ccbd8de-9ff2-4ff7-9a71-8bd5268c6b47)
![6a77a516-1e98-4f84-8d04-bd14b8399b72](https://github.com/user-attachments/assets/33a59081-7b1a-4dc7-9fc4-66df45f1eb9a)
![7f3ee5b7-ccc1-48ac-987d-af2cad1fa74a](https://github.com/user-attachments/assets/a6061841-98c0-4595-8e04-b6fc94472f2e)
![aa11834c-bad8-4db4-bc93-e723ce285dba](https://github.com/user-attachments/assets/2a8c865d-af88-4b8f-b5b6-cce8af1c7cf6)
![9d832422-8fa9-4215-95dd-002024bc1c79](https://github.com/user-attachments/assets/54f26cb8-7b3b-44be-a0d6-35754762f612)
![dcdb2ca6-cb10-4287-beac-d4c30b451a54](https://github.com/user-attachments/assets/0fee06e5-9176-4993-a1cf-da49b50189aa)
![1394cfd6-1f62-4c95-85af-d04c8e4a1e20](https://github.com/user-attachments/assets/d4b3319f-c5d2-4c70-ac26-02e382e5c613)
![01a32872-b119-4414-9469-bcca0e881e79](https://github.com/user-attachments/assets/040bd5d1-844c-4a91-831f-df909d719538)
![17793de2-1d71-4eb1-9d73-3ec783fca242](https://github.com/user-attachments/assets/f1e98aad-9bc4-4c41-8fb9-2d9707be6392)
![22276c12-2e9e-44f9-b3ac-46d17bf82c75](https://github.com/user-attachments/assets/813dbd3c-9f72-46bc-8e9d-cc6166a4b369)

## Actividad 3: Fenómenos de canal en el analizador de espectro

### Objetivo

Familiarizarse con los fenómenos de un canal alámbrico real en el dominio de la frecuencia.

### Procedimiento

1. **Configurar el USRP 2920:**
   - Configurar el flujograma [filters_flowgraph.grc](filters_flowgraph.grc) en GNU Radio para transmitir una señal a través del USRP.
   - Habilitar o deshabilitar los bloques correspondientes (`Channel Model`, `Throttle`, `UHD: USRP Sink`, `UHD: USRP Source`, `Virtual Sink`). Para esto, seleccione el bloque deseado y presione **E** (enable) o **D** (disable), respectivamente.
   - Configurar siempre la frecuencia de muestreo (`samp_rate`) en $25e6/2^n$ Hz`, donde $n$ es un número entero mayor a 2.  Verifique que la frecuencia de muestreo durante la ejecución, sea la misma que ha configurado en el flujograma.

2. **Configurar el Analizador de Espectros:**
   - Encender, configurar y conectar el analizador de espectros a la salida del USRP 2920 usando diferentes cables coaxiales, y ajustando los parámetros necesarios para evidenciar los fenómenos de canal analizados en la Actividad 1.

### Preguntas Orientadoras

- ¿Cuál es el efecto del ruido sobre la respuesta en frecuencia de las señales medidas en el analizador de espectro? ¿Conservan las mismas relaciones que se evidencian en la simulación? El efecto del ruido es que el piso de ruido sube y provoca que se pierda parte de la señal ya que no se diferencia del ruido, conservando las mismas relaciones que se evidencian en la simulación
- ¿La relación señal a ruido creada intencionalmente desde el computador se amplifica o se reduce en la señal observada en el analizador de espectro? El SNR se amplificó ya que paso de -52dB de la simulación y se midió aproximadamente -30dB en el analizador de espectros.
- Adjunte la evidencia de la medición de la relación señal a ruido de dos formas de onda distintas.
- ¿Cómo se evidencia el fenómeno de desviación de frecuencia en el analizador de espectro? Evidenciar al menos con dos formas de onda. 
- Determine la afectación de un medio de transmisión coaxial (usar cables largos) sobre una señal periódica operando a las capacidades máximas de muestreo del USRP. La potencia de la señal se redujo poco de menos de 3dB respecto al cable corto
  - **NOTA:** La frecuencia de transmisión no debe superar los 1000 MHz para ser observada en el analizador. Para el experimento, considere las relaciones de muestreo correspondientes.
- Usando cables coaxiales de diferentes longitudes, ¿cómo afecta la distancia entre el transmisor y el receptor a la amplitud de la señal medida? La distancia entre transmisor y receptor utilizando cables se reduce considerablemente ya que 3dB en potencia significa que se redujo casi la mitad.
- Usando antenas, ¿cómo afecta la distancia entre el transmisor y el receptor a la amplitud de la señal medida? ¿Es posible compensar el fenómeno? Al igual que con el experimento del osciloscopio, la distancia entre las antenas muestra una leve reducción de la potencia de la señal al estar más alejadas que al estar cercanas. Y es posible compensar el fenómeno usando algo que reflecte las ondas.
- ¿Qué modelo de canal básico describe mejor las mediciones obtenidas en la práctica? Modelo de pérdidas por espacio libre para las antenas, pérdidas por cable coaxial 

### Evidencia

*(Adjuntar las evidencias de la práctica en el Aula Virtual: capturas de pantalla, observaciones, cálculos o mediciones preliminares)*
![b91266a6-4ab1-40e5-9959-5b97c05d3793](https://github.com/user-attachments/assets/6fcff7ef-22c8-4673-baf3-aad62f4560ab)
![37a04a1c-0c9f-417c-8b50-840c88f17f30](https://github.com/user-attachments/assets/8ed21616-c6cd-47da-b0e9-17782f7e756f)
![b8a04ec5-e970-4552-85d4-c747b251b635](https://github.com/user-attachments/assets/4a63e2ed-f072-444c-a0cc-9e76bca9931f)

![b8a04ec5-e970-4552-85d4-c747b251b635](https://github.com/user-attachments/assets/09c4dc9a-ae9a-4446-a6e5-af579ae59826)
![1ed9cf51-6a65-49fd-9aa7-551e5abbd72e](https://github.com/user-attachments/assets/b7c8fa2e-499d-4217-9aba-1e066aeadc61)
![eeecbca1-8255-440b-9349-1d92ee8ca54e](https://github.com/user-attachments/assets/846be3b8-4663-44c0-a591-f847115b1aee)
![6c121068-e8b8-422f-875a-9c8a893a1e82](https://github.com/user-attachments/assets/02ed32fc-da14-40d5-ad85-eb86d5f18c4a)

## Actividad 4: Efectos de los fenómenos de canal en la conversión de frecuencia

### Objetivo

Familiarizarse con los efectos de los fenómenos de un canal alámbrico e inalámbrico real en la conversión de frecuencia.

### Procedimiento

**Configurar el USRP 2920:**
   - Configurar el flujograma [filters_flowgraph.grc](filters_flowgraph.grc) en GNU Radio para **transmitir y recibir ** una señal a través del USRP.
   - Habilitar o deshabilitar los bloques correspondientes (`Channel Model`, `Throttle`, `UHD: USRP Sink`, `UHD: USRP Source`, `Virtual Sink`). Para esto, seleccione el bloque deseado y presione **E** (enable) o **D** (disable), respectivamente.
   - Configurar siempre la frecuencia de muestreo (`samp_rate`) en $25e6/2^n$ Hz`, donde $n$ es un número entero mayor a 2. Verifique que la frecuencia de muestreo durante la ejecución, sea la misma que ha configurado en el flujograma.
   - Compare los resultados al recibir la señal usando diferentes medios (aire o cable coaxial).

### Preguntas Orientadoras

- ¿Cómo se evidencian los diferentes fenómenos de canal en la señal recibida? Los fenómenos que afectan la señal son mayoritariamente indeseados, por muchas razones, pérdida de potencia de una señal, mucho ruido evita que una señal se capte correctamente e incluso la distancia es un factor importante en cuanto a transmisión de señales.
- ¿Cómo se pueden mitigar los efectos del canal en la señal recibida? Dependiendo del tipo de efecto podría poder ser mitigado, por ejemplo en el caso de las antenas se pueden usar reflectores para captar llevar mejor las ondas a la antena. Pero efectos como las pérdidas por cable coaxial son más complicados de arreglar debido a que son fenómenos que son del propio medio.

### Evidencia

*(Adjuntar las evidencias de la práctica en el Aula Virtual: capturas de pantalla, observaciones, cálculos o mediciones preliminares)*
