![](https://itq.edu.ec/wp-content/uploads/2023/02/Recurso-6.png)
>Instituto Superior Tecnologico Quito

----


| Integrantes  | Docente | Carrera |
| :------------ | |:------------:| | :------------ :| 
| Nixon Carcelen    | Ing. Sebastian Landazuri | Desarrollo de Software |
| Jonathan Toca     |        **Proyecto**              |  **Nivel**                     |  
| Darlyn Vasquez|       IoT            |      3ro                  |
|Cristhian Recalde| Internet de las cosas||
|Davis Yepez| Casa automatizada|   |

----

- El Instituto Superior Tecnologico Quito junto con sus estudiantes forman proyectos por cada modulo conforme la materia por ende ponemos a disposicion una Base de Datos no Relacionales sobre IoT de una casa automatizada ；
-  IoT describe objetos físicos con sensores, capacidad de procesamiento, software y otras que se conectan e intercambian datos con otros dispositivos y sistemas a través de internet u otras redes de comunicación.；

- MongoDB es un sistema de base de datos NoSQL, orientado a documentos y de código abierto. En lugar de guardar los datos en tablas, tal y como se hace en las bases de datos relacionales；
- Nuestra base de datos consta de mas de 20 colecciones, todas estan fueron creadas conforme nuestro proyecto acerca de una Casa automatizada;
- Algunas coleciciones estan referenciasdas se podria decir que la mayoria；
- Para crear esta base de datos se nececesito de requerimientos decididos por el docente ；




![](https://dynamoiot.com/wp-content/uploads/2020/08/IoT-2.jpg)
> Iot

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/MongoDB_Logo.svg/2560px-MongoDB_Logo.svg.png) 
>MongoDb


----





###Links

----

`<GitHub>` : <https://github.com/cristhian-recalde24/Iot-NoSQL>
>GitHub

`<MongoDB>` : `mongodb+srv://Cristhian:*****@cristhian.ubq462s.mongodb.net/`
>MongoDB

----

####Consultas

----

*1. Recupera todos los dispositivos de iluminación instalados en la sala de estar, ordenados 
por su nivel de brillo de manera descendente*
```javascript
db.Dispositivos.find({tipo:"iluminacion",ubicacion:"sala"}).sort({brillo:-1})
```
*2. Encuentra los últimos 10 registros de temperatura registrados por el sensor en el 
dormitorio principal*
```javascript
db.Temperaturas.find({_id:"65b6e917d8ced9841ae21d64",temperatura:"20°c"}).limit(10)

```
*3.  Filtra los dispositivos de seguridad cuyo estado sea "activado" y están ubicados en el 
exterior de la casa*
```javascript
db.Dispositivos.find({tipo:"Seguridad", ubicacion:"Calle principal", estado:"true"})

```
*4. Obtiene la lista de dispositivos cuya fecha de instalación sea anterior al 01 de enero de 
2023.*
```javascript
db.Dispositivos.find({fecha_instalacion:{$lt:"2023-01-01" }})
```
*5. Encuentra los dispositivos de climatización cuyo consumo de energía esté entre 1000 y 
2000 vatios.*
```javascript
db.Dispositivos.aggregate([
{
			$match: {
					tipo: "Climatización"
			}
		},
			{
				$lookup: {
					from: "ConsumoEnergia",
					localField: "tipo", 
					foreignField: "consumo?vatios", 
					as: "ConsumoTotal"
				}
			},
				{
				$match: {
					"ConsumoTotal": {
								$gte: 1000,
							$lte: 2000
						}
				}
			}
	])
```

*7. Filtra los eventos de detección de movimiento ocurridos en el garaje y que hayan sido 
registrados en los últimos 7 días*
```javascript
db.Eventos.find({
tipo: "Detección de Movimiento",
"dispositivo.nombre": "Alarma de lunes a viernes",
fecha_hora: {
$gte: new Date(new Date() - 7 * 24 * 60 * 60 * 1000)
}
});

```
*8. Encuentra los dispositivos de audio que contengan la palabra "altavoz" en su nombre.*
```javascript
db.Dispositivos.find({
"nombre": {
$regex: /altavoz/i
},
"tipo": "Audio"
});

```
*9. Recupera los 5 dispositivos con el mayor consumo de energía en el último mes.
*
```javascript
db.ConsumoEnergia.aggregate([
{
$match: {
"fecha_hora": {
$gte: new Date(new Date().setMonth(new Date().getMonth() - 1))
}
}
},
{
$sort: {
"consumo?vatios": -1
}
},
{
$project: {
_id: 1,
"consumo?vatios": 1,
fecha_hora: 1
}
}
]);

```
*10. . Filtra los eventos de apertura de puertas que hayan sido registrados en las habitaciones 
y que ocurrieron en las últimas 48 horas*
```javascript
db.Eventos.find({
tipo: "Apertura de Puerta",
"dispositivo.nombre": "Alerta de seguridad",
fecha_hora: {
$gte: new Date(new Date() - 2 * 24 * 60 * 60 * 1000)
}
});

```
*11. Encuentra los dispositivos de seguridad que aún no han sido activados.*
```javascript
db.Dispositivos.find({ tipo: "Seguridad", estado: "false" }, { nombre: 1 })  
```
*12. Recupera los datos de los sensores de luz que han experimentado cambios en su nivel 
de iluminación en los últimos 10 minutos*
```javascript
db.Actualizaciones.aggregate([
  {
    $match: {
      _id: "65b712bfd8ced9841ae21d87",
      fecha_actualizacion: {
        $gte: new Date(Date.now() - 10 * 60 * 1000) // Filtra los documentos actualizados en los últimos 10 minutos
      }
    }
  },
  {
    $group: {
      _id: "$id_sensor",
      ultima_actualizacion: { $last: "$fecha_actualizacion" }, 
      nivel_iluminacion: { $last: "$nivel_iluminacion" } 
    }
  }
])
```
*13. Filtra los eventos de apagado de luces ocurridos en la sala de estar durante la noche.*
```javascript
db.Acciones.aggregate([
  {
    $match: {
      descripcion: "Apagar luz",
      estado: true
    }
  },
  {
    $project: {
      _id: 0,
      "Accion": "$descripcion"
    }
  }
])

```
*14. Encuentra los dispositivos de climatización cuyo estado actual sea "encendido" y la 
temperatura objetivo sea mayor a 25 grados Celsius
```javascript
 db.Temperaturas.find({"tempertura":{"$gt":"25"},"estado":true},{"tempertura":1,"_id":1,"estado":1}) 

```
*15. .Recupera los eventos de detección de humo que han sido registrados en la cocina y que 
contengan la palabra "emergencia".
```javascript
db.Dispositivos.aggregate([
  {
    "$match": {
      "nombre": "alarma de incendio"
    }
  },
  {
    "$lookup": {
      "from": "Actividades",
      "localField": "nombre",
      "foreignField": "nombre",
      "as": "actividad"
    }
  },
  {
    "$project": {
      "_id": 0,
      "actividad": 1
    }
  }
])
```
*16. Filtra los dispositivos de seguridad instalados en el jardín y cuya batería tenga un nivel 
inferior al 20%.*
```javascript
db.dispositivos.find({"ubicacion": "jardín", "tipo": "Seguridad", "estado": true, "datos.bateria": {$lt: 20}})
```
*17. Encuentra los dispositivos de audio que han estado inactivos durante más de 48 horas*
```javascript
db.dispositivos.find({"tipo": "Audio", "estado": true, "ultima_actividad": {$lt: ISODate("2024-01-29T21:52:00.000Z")}})
```
*18. Recupera los registros de temperatura y humedad de la sala de estar en los últimos 7 
días*
```javascript
db.RegistroActividades.find({"lugares.descripcion": "sala de star", "temperatura.fecha": {$gte: ISODate("2024-01-24")}})
```
*19. Filtra los eventos de apertura de ventanas que han ocurrido en las habitaciones y que 
no hayan sido seguidos por eventos de cierre.
*
```javascript
db.RegistroActividades.find({"descripcion": "abrir las cortinas", "condicion": "moviento detectado", "acciones.descripcion": "cerra puerta"})
```
*20. Encuentra los dispositivos de iluminación que tienen un consumo de energía superior al 
promedio en el último mes.*
```javascript
db.dispositivos.find({"tipo": "iluminacion", "estado": true, "consumo_energia.consumo": {$gt: "promedio_del_ultimo_mes"}})
```
*21. Recupera los eventos de detección de movimiento que han sido registrados en el pasillo 
y están asociados a la presencia de mascotas.*
```javascript
db.RegistroActividades.find({"descripcion": "prender luz", "condicion": "moviento detectado", "dispositivos.ubicacion": "pasillo", "dispositivos.usuarios.nombre": "Cristhian"})
```

*22. Filtra los dispositivos de seguridad que han experimentado una alerta de intrusión en las 
últimas 24 horas.*
```javascript
db.RegistroActividades.find({"acciones.descripcion": "alerta de seguridad", "acciones.programaciones.fecha": {$gte: ISODate("2024-01-29T21:52:00.000Z")}})
```
*23. Encuentra los dispositivos de climatización cuya fecha de instalación sea mayor al 01 de 
enero de 2022 y la garantía esté próxima a vencer.*
```javascript
db.dispositivos.find({"tipo": "climatizacion", "estado": true, "fecha_instalacion": {$gt: ISODate("2022-01-01")}, "garantia": {$lt: "proxima_a_vencer"}})
```
*24. Recupera los eventos de apagado de luces que han sido registrados en el comedor 
durante las horas de la tarde.
*
```javascript
db.RegistroActividades.find({"descripcion": "apagar luz", "dispositivos.ubicacion": "comedor", "hora": {$gte: "tarde"}})

```

*25. Filtra los dispositivos de seguridad que han experimentado una alerta de incendio y cuya 
ubicación sea la cocina.*
```javascript
db.Dispositivos.aggregate([
		{
		$match: {
				tipo: "Seguridad"
		}
	},
	{
		$lookup: {
			from: "Lugares",
			localField: "tipo",
			foreignField: "nombre",
			as: "Nombre_Lugar"
			}
		}
])
```

----

***Desarrolo de Software***  
***Base de datos no relaciones  01-febrero-2024***
*** Proyecto final Grupo IoT***

----


###GRACIAS! :D
