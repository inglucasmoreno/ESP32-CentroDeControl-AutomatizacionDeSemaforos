# ESP32 - Centro de control
Ejemplo de aplicacion real usando ESP32. Es un centro de control conformado por un MT8870 (Decodificador de tonos multifrecuentes)
conectado a un telefono celular por medio de su jack de audio por el cual recibe los tonos a decodificar. El MT8870 genera una salida
binaria que representa el numero recibido y que sera enviado al ESP32 para su posterior procesamiento y comunicacion via MQTT con un
servidor web que manejara las alertas del sistema.

(CELULAR) ---> (MT8870) ---> (ESP32) ---> [MQTT] ---> (BROKER MQTT) ---> [MQTT] ---> (SERVIDOR WEB)

### Requisitos
- Placa ESP32
- Libreria WiFi.h
- Libreria BluetoothSerial.h
- Libreria EEPROM.h
- Libreria PubSubClient.h
- Celular o PC para generar los tonos multifrecuentes
- Circuito con MT8870 para decodificar los tonos y enviar el binario a la ESP32
- Broker MQTT (Puede desarrollarse facilmente con NodeJS) - [ Topico utilizado -> "intermitencia" ]
- Celular con programa Bluetooth Terminal (Para enviar los comandos de configuracion)


### Comandos via Bluetooth Terminal (Config WiFi y MQTT)
- Setear SSID de red WiFi -> ssid-[ssid de red a conectar]
- Setear Password de red WiFi -> password-[password de red a conectar]
- Setear Direccion IP de Broker MQTT -> mqttserver-[Direccion IP del Broker MQTT]
- Setear Puerto para comunicacion MQTT -> mqttport-[Puerto para comunicacion MQTT]
- Conectar a Red WiFi + Habilitar reconexion -> connect-wifi
- Reconectar WiFi + Habilitar reconexion WiFi -> reconnect-wifi
- Desconectar de Red WiFi + Deshabilitar reconexion -> disconnect-wifi
- Conectar a Broker MQTT + Habilitar reconexion MQTT -> connect-mqtt
- Reconectar MQTT + Habilitar reconexion MQTT -> reconnect-mqtt
- Desconectar de Broker MQTT + Deshabilitar reconexion MQTT -> disconnect-mqtt
- Conectar WiFi y MQTT + Habilitar sus reconexiones -> connect-all
- Desconectar WiFi y MQTT + Deshabilitar sus reconexiones -> disconnect-all
- Reconexion de WiFi y MQTT + Habilitar sus reconexiones -> reconnect-all

Nota: No colocar los corchetes [ ]

### Memoria EEPROM
- Se aprovecha la memoria EEPROM de la ESP32 para almacenar SSID y Password de Red WiFi, ademas del Servidor MQTT y el Puerto de conexion MQTT
- Cuando el sistema inicia, levanta automaticamente SSID y Password de Red WiFi a conectar, ademas de la IP del servidor MQTT y puerto de conexion MQTT desde la memoria

### Builtin LED
- Se aprovecha el Builtin LED de la ESP 32 para indicar estado de conexion
- BUILTIN LED Apagado -> Desconectado de red WiFi y/o MQTT | Ademas esta deshabilitada la reconexion
- BUILTIN LED Parpadeando ( rapido [200ms] ) -> Intentando de conectar a Red WiFi
- BUILTIN LED Parpadeando muy lento -> Intentando de conectar al Broker MQTT
- BUILTIN LED Encendido -> Conexion a red WiFi y MQTT Broker establecida

### Caracteristicas del programa
- Conexion a WiFi
- Conexion a Broker MQTT
- Configuracion de conexion WiFi y MQTT via Bluetooth Serial
- Almacenamiento en EEPROM de las credenciales WiFi y MQTT 
- Recibe un binario de entrada proveniente desde un MT8870 y obtiene su valor entero (Representa la tecla presionada en el celular)
- Espera a recibir 3 digitos que conforman el codigo de alerta y lo envia al broker MQTT por el topico "intermitencia"
- Cada 5 segundos limpia el codigo basura si no recibe nuevo digito
- El codigo de 3 digitos esta asociado a un semaforo que se encuentra intermitente por una falla
- El envio del codigo se realiza desde un celular que se encuentra en el semaforo al celular que se encuentra en el centro de control


