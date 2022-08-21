<p align="center"><img src="https://raw.githubusercontent.com/carjavi/marlin-v1.1.9/master/img/marlin-v119.png" height="300" alt=" " /></p>
<br>
<h1 align="center">Marlin-v1.1.9 setting for PRUSA i3</h1> 
<h4 align="right">Aug 22</h4>
<img src="https://img.shields.io/badge/Hardware-Arduino__mega-red">

Ramp 1.4 / Arduino Mega 2650 / (sin sensor inductivo en Z)

#  Configuration.h 
Default velocidad de comunicación:
```
#define BAUDRATE 250000
```

## LCD & SD CARD

## Enable LCD (aprox línea 1378) 

Cambiado el valor desde el valor por defecto “en” 
```
#define LCD_LANGUAGE es 
```
Nota: define el idioma de la pantalla, aunque algunos mensajes como Load/Unload habrá que cambiarlos ya que no se traducen automaticamente. 

## Habilitada SD Card (aprox línea 1427)
viene desabilitado.
``` 
#define SDSUPPORT 
```
## Habilita controlador LCD (aprox línea 1530)
No viene habilitado, la LCD no funciona por default
``` 

#define REPRAP_DISCOUNT_SMART_CONTROLLER
``` 
## Nombre de la impresora en la LCD (aprox línea 139)
``` 
#define CUSTOM_MACHINE_NAME "nombre-impresora"
``` 

## electronics board (aprox línea 130)
Ramp 1.4
``` 
#define MOTHERBOARD BOARD_RAMPS_14_EFB
``` 

## filament diameter (aprox línea 150)
``` 
#define DEFAULT_NOMINAL_FILAMENT_DIA 1.75
``` 

## Thermal Settings (aprox línea 261)
valor del sensor de temperatura / activa la cama caliente:
``` 
#define TEMP_SENSOR_0 1
#define TEMP_SENSOR_BED 1
``` 

## Activa el PID-Autotune-menu (aprox línea 371)
```
#define PID_AUTOTUNE_MENU
```

## Endstop Settings (aprox línea 502)
verificar:
``` 
#define USE_XMIN_PLUG
#define USE_YMIN_PLUG
#define USE_ZMIN_PLUG
``` 
si los endstop son por negativo hay que configurar los *pull up*:
``` 
#define ENDSTOPPULLUP_XMIN
#define ENDSTOPPULLUP_YMIN
#define ENDSTOPPULLUP_ZMIN

#define X_MIN_ENDSTOP_INVERTING false // set to true to invert the logic of the endstop.
#define Y_MIN_ENDSTOP_INVERTING false // set to true to invert the logic of the endstop.
#define Z_MIN_ENDSTOP_INVERTING false // set to true to invert the logic of the endstop.
``` 

si los endstop son por Positivo:
``` 
//#define ENDSTOPPULLUP_XMIN
//#define ENDSTOPPULLUP_YMIN
//#define ENDSTOPPULLUP_ZMIN

#define X_MIN_ENDSTOP_INVERTING true // set to true to invert the logic of the endstop.
#define Y_MIN_ENDSTOP_INVERTING true // set to true to invert the logic of the endstop.
#define Z_MIN_ENDSTOP_INVERTING true // set to true to invert the logic of the endstop.
``` 
## Get Endstop Status desde Pronterface
```
M119
```
<p align="center"><img src="https://raw.githubusercontent.com/carjavi/marlin-v1.1.9/master/img/endstop.png"  alt=" " /></p>

## PID settings (aprox línea 361)
1. Conectar la impresora 3d por cable serial a una computadora, y usar un terminal serial
2. Es necesario que la temperatura de hot-end este a temperatura ambiente. Si tienes un ventilador de capa se envia el comando **M106**. Esto prendera el ventilador de capa a toda velocidad.
3. En la consola se ingresa el comando **M303 E0 S200 C8**, donde:
   * M303, es el comando para ejecutar el PID autotune.
   * E0, se refiere al hot end, si tuvieramos dos , derberiamos repetir este proceso para cada uno, pero el siguiente seria E1.
   * S200, se refiere a la temperatura objetivo para las muestras, en este caso 200°C.
   * C8, cantidad de pruebas que va a hacer para encontrar los parametros, P, I , D, en este caso 8.
4. Al final de la prueba, en consola se nos presentara los valores e inclusive nos indica donde debemos cambiar estos archivos en el firmware.
   ```
   PID Autotune finished! Put the last Kp, Ki and Kd constants from below Configueration.h
   #define DEFAULT_kp 19.92
   #define DEFAULT_ki 1.34
   #define DEFAULT_kd 73.83
   ```
5. Si la impresora no usa EEPROM se debe cargar los valores manualmente
6. Si usa EEPROM  **M301 P19.92 I134.26 D73.83** y  M500 para grabar los valores en la EEPROM.
   
Para mayor información: https://reprap.org/wiki/PID_Tuning

# Calibration AXIS X,Y,Z and E0 (aprox línea 611)
Default Axis Steps Per Unit (steps/mm)
```
    #define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 4000, 500 }
```
Calibracion extrusor (aplica para los demás ejes)
1. Marcamos el filamento que entra al extrusor y lo medimos a la entrada del mismo. 
2. En Pronterface extruimos material a una longitud de 10 mm. 
3. medimos que cantidad de material entro al extrusor. ejemplo 11mm
4. Revisamos el valor en el archivo Configuration.h ejemplo:
```
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 2560, 115 }
```
5. Se debe cambiar este valor, para esto hacemos una simple regla de tres, en este ejemplo: "Si con 115 tuve 11mm, cual es el valor para 10mm", seria: (10mm*115)/11mm = 104.54. Cambiamos este valor, y procedemos a subir el nuevo firmware a nuestra placa.
   
nuevo valor:
```
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 2560, 104.54 }
```
Para mayor información: https://minkafab.com/calibracion-impresora-3d-con-marlin-1-1-9/

## Valocidad y aceleración (aprox línea 618)
Default configuration:
```
#define DEFAULT_MAX_FEEDRATE          { 300, 300, 5, 25 }
#define DEFAULT_MAX_ACCELERATION      { 3000, 3000, 100, 10000 }

#define DEFAULT_ACCELERATION          3000    // X, Y, Z and E acceleration for printing moves
#define DEFAULT_RETRACT_ACCELERATION  3000    // E acceleration for retracts
#define DEFAULT_TRAVEL_ACCELERATION   3000    // X, Y, Z acceleration for travel (non printing) moves

#define DEFAULT_XJERK                 10.0
#define DEFAULT_YJERK                 10.0
#define DEFAULT_ZJERK                  0.3
#define DEFAULT_EJERK                  5.0
```

## Prusa i3 setting
```
#define DEFAULT_MAX_FEEDRATE          { 200, 200, 5, 25 }
#define DEFAULT_MAX_ACCELERATION      { 800, 800, 12, 10000 }

#define DEFAULT_ACCELERATION          800    // X, Y, Z and E acceleration for printing moves
#define DEFAULT_RETRACT_ACCELERATION  800    // E acceleration for retracts
#define DEFAULT_TRAVEL_ACCELERATION   800    // X, Y, Z acceleration for travel (non printing) moves

#define DEFAULT_XJERK                 8.0
#define DEFAULT_YJERK                 8.0
#define DEFAULT_ZJERK                  0.3
#define DEFAULT_EJERK                  5.0
```


# Comandos G-code
```
M114  #permite saber la posición actual de la impresora, en X,Y y Z 

M211 S0 #deshabilita la seguridad de los finales de carrera, lo que nos va a permitir seguir bajando a pesar de que el sensor inductivo este detectando la superficie de impresión.

M119 #ver el status de los endstop
```

# Links
- Tutorial Cura https://of3lia.com/ultimaker-cura/
- wiki https://www.instructables.com/Motion-Configuration-on-Ramps-14-with-Marlin-firmw/
-  Pronterface http://www.pronterface.com/
-  wiki https://minkafab.com/calibracion-impresora-3d-con-marlin-1-1-9/
  




---
Copyright &copy; 2022 [carjavi](https://github.com/carjavi). <br>
```www.instintodigital.net``` <br>
carjavi@hotmail.com <br>
<p align="center">
    <a href="https://instintodigital.net/" target="_blank"><img src="https://raw.githubusercontent.com/carjavi/marlin-v1.1.9/master/img/developer.png" height="100" alt="www.instintodigital.net"></a>
</p>
