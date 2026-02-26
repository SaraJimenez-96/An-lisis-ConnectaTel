# Análisis ConnectaTel
Análisis de una empresa de telecomunicaciones con operaciones en México y Colombia.
Se quiere analizar el uso de los servicios moviles. Pero primero se debe explorar, limpiar y analizar estas bases de datos para construir una visión clara y confiable sobre el comportamiento de los usuarios 

- **¿Qué problemas se encontraron inicialmente en los datos? ¿qué tratamiento se les dió?**
  
Se tienen 3 Datasets distintos, para los cuales inicialmente se encontró:
1. DataSet *Plan*:
Representa los planes actuales con sus respectivos precio, minutos incluidos, GB incluidos, costo por extra. No representa valores ausentes y es de facil lectura.
2.  Dataset *users*:
Contiene información de clientes: edad, ciudad, fecha de registro, plan contratado.
 	- El tipo de dato de la columna 'reg_date' se debía de cambiar a tipo fecha, las fechas futuras, superiores a 2024 cambiarlas a NA. Puesto que se analizará hasta 2024.
	- Columna 'city' tiene 11% de nulos, se revisa la relación entre 'city' y 'user_id'.
    - Revisión de Sentinels: La columna 'user_id' no tiene sentinels. La columna 'age' sí tiene 55 veces el sentinel -999, el cual es reemplazado por la mediana. En el caso de las columnas categóricas, se corrige el tipo de sentinels de la columna 'city', que tiene 96 valores del sentinel '?', que se reemplaza por "NA" .
	- La columna churn_date tiene 88% de nulos, lo cual me hace preguntar, ¿esta columna me sirve? ¿Me dice algo? Se tomó la decisión de no tenerla en cuenta en el análisis.

3. DataSet *Usage*
Contiene información de los clientes: edad, ciudad, fecha de registro, plan contratado.
	- El tipo de dato de la columna 'date' se debía cambiar a tipo fecha. Aquí solo se encuentran datos del año 2024. Esta columna tiene un 1% de nulos que dejarlos como nulo.
	- La columna 'duration' tiene 55% de nulos, el cual esta relacionado con a la duración del texto; en ese caso se deja como nulo, puesto que un texto no se mide por duración de tiempo, sino por caracteres, es decir, que son de tipo MAR, que depende del 'type'.
	- La columna length tiene 44% de nulos, que esta relacionado con la elongación de caracteres de las llamadas, en ese caso tiene sentido dejarlo como nulo, puesto que una llamada no se mide por carácter sino por duración. Es decir que, son de tipo MAR que depende del 'type'.
    - Para la revision de sentinels: La columna 'user_id' y 'Id' no tiene sentinels detectados, en cambio las columnas 'duration' y 'length' se encontró 15 y 133 sentinels respectivamente para el tipo 0.0000, puedo dejarlos como error.
   
 - **¿Qué segmentos de clientes identificaste y cómo se comportan según su edad y nivel de uso?**
   
   - Se encuentra que el 64.8% de los usuarios tiene el plan Básico y 35.2% el plan Premium. En ambos planes se encuentra un comportamiento similar, en el cual recorre por todas las edades desde 18 años hasta los 80 años, pero en su mayoria son personas adultas entre 30 y 60 años.  Los usuarios en su mayoria tienen un uso moderado de llamdas y mensajes de texto.
   - En el plan Premium, en promedio no se encuentran mas de 22 personas por rango de edad. En cambio, en el plan Básico, en pueden encontrar hasta 40 personas en promedio. La edad de los 47 años, es donde mas cantidad de personas vemos por plan, aproximadamente 80 personas de 47 años tienen plan Básico y 32 personas tienen plan Premium.
   - En cuanto al uso de llamadas, se encuentra un patron bajo de llamdas que estan en promedio entre 3 y 5 llamadas. Para el plan Basico alrededor de 500 personas realizan 4 llamadas. y para el Plan Premium 250 usuarios realizan entre 3 y 4 llamadas. Estas llamadas se extienen en aproximadamente entre 14 y 18 minutos en ambos planes.
   - Por ultimo, los usuarios envian entre 3 a 7 mensaje de texto. En el caso del Plan Premium aproximadamente 200 usuarios envian 5 mensajes y en el plan Básico 4000 usuarios envian 5 mensajes.

- **¿Qué segmentos parecen más valiosos para ConnectaTel y por qué?**
Dado que el 64% de los usuarios tienen un plan Básico y el 50% de los usuarios son adultos, me enfocaría en este segmento, adultos de plan básico para generar beneficios.

- **¿Qué patrones de uso extremo (outliers) encontraste y qué implican para el negocio?**
  
Se encuentran pocos outliers a revisar entre las distintas categorias. En el caso de Cantidad de mensajes, los outliers van entre 12 a 17 mensajes y cantidad de llamadas de 11 a 15 llamdas, estos son pocos, son valores posibles y no parecen un error, es decir que son manejables y no afectan el analisis para el negocio.  Por el otro lado, la cantidad de minutos si tiene outliers que van desde los 60 a 155minutos de elongacion de la llamda, lo cual es muy poco probable y si afectan el analisis del negocio. Aunque no es imposible tener llamadas tan extensas, podria referirse a problemas o a una minoria que se le podrian dar beneficios por minutos utilizados en las llamadas.

## Análisis ejecutivo
⚠️ Problemas detectados en los datos

* En ambos datasets la columna relacionada a las fechas estaba en formato object, el cual se cambia a tipo fecha.
* Se encuentran varios tipos de Sentinel, de tipo numérico y categórico. A los cuales se les realiza su respectivo tratamiento. Se cambian a su valor promedio, se cambian a NAN.

🔍 Segmentos por Edad

* Ambos planes tienen un patrón similar de edades, se recorre por todos los rangos de edad desde los 18 hasta los 80 años.
* La edad de la mayoría de los usuarios es adulta, es decir, que cuentan entre 30 y 60 años.

📊 Segmentos por Nivel de Uso

* Se encuentra que se envían más mensajes que llamadas realizadas. En promedio se  envían entre 3 y 7 mensajes, mientras que se realizan entre 3 y 5 llamadas que no duran más de 23 minutos.
* La mayoría de usuarios tienen un nivel de uso medio en su plan con llamadas y envío de mensajes.
  
➡️ Esto sugiere que tener un enfoque en llamadas y mensajes no es el más recomendado, dado que en su mayoría tienen un uso medio bajo y de pocos minutos al mes. Vale explorar el nivel de GB incluidos y usados en el plan.

💡 Recomendaciones

* Darle mejores opciones en los planes en cuanto a precios de la mayoría de mis usuarios, es decir, a los adultos de uso medio de plan. 
* Analizaría el segmento de usuarios que le da un alto uso a las llamadas con larga duración, para mejorar sus condiciones de precio por duración de llamada.
