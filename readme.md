# Practica 6 Buses de comunicación

### Código
````
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>


File myFile;

void setup()
{

  Serial.begin(9600);
  Serial.print("Iniciando SD ...");
  SPI.begin(18,19,23,5);

  
  if (!SD.begin(5)) {
    Serial.println("No se pudo inicializar");
    return;
  }
  Serial.println("inicializacion exitosa");
 
 //Comrpovamos si hay un fichero en la tarjeta
 if (SD.exists("/archivo.txt")) {
    Serial.println("example.txt exists.");
  } 

  else {
    Serial.println("archivo.txt doesn't exist.");
  }
  // Abrimos el fichero para escribir en el, si existe no habrá problema, si no existe lo creará.
  myFile = SD.open("/archivo.txt", FILE_WRITE);
  myFile.close();

  if (SD.exists("/archivo.txt")) {

    
    Serial.println("archivo.txt exists.");
  } 
  else {
    Serial.println("archivo.txt doesn't exist.");
  }

  //Escribimos en el fichero
  myFile=SD.open("/archivo.txt",FILE_WRITE);
  myFile.println("Hola mundo");
  myFile.close();
  //Volvemos a abrir el fichero para mostrar lo que hay dentro
  myFile=SD.open("/archivo.txt");
  if (myFile) {
    Serial.println("archivo.txt:");
    while (myFile.available()) {
    	Serial.write(myFile.read());
    }
    myFile.close(); //cerramos el archivo
  }
  
  else {
    Serial.println("Error al abrir el archivo");
  }
}

void loop()
{
  
}
````
### Explicacion

Iniciamos el programa importando dos libreía para facilitarnos la comunicación que deseamos establecer, estas son la SPI.h y la SD.h. seguidamente creamos un objeto de la clase File llamado myFile con el que vamos a poder abrir y cerrar el fichero de la SD para poder leerlo o escribir en él.

Por lo que hace al void setup() establecemos el baud rate a 9600 e imprimimos por pantalla que se esta iniciando la SD. Una vez hecho iniciamos el us SPI con la función SPI.begin(), y le colocaremos los puertos que utilizaremos.

Si todos los pines pasados por parámetro fueran -1 les asignaría él mismo su valor. Si no es así al ser un constructor le pondría el valor pasado a las valiables de la clase.

Una vez inicializada miramos si se encuentra algún fichero en la tarjeta con la función SD.exists('/nombre_archivo'). Ahora que ya todo funciona y se ha podido localizar el archivo, procedemos a abrir y leerlo.
Primero lo abrimos y ponemos que vamos a escribir en él. Escribimos de una forma muy conocida, una función llamada prinln("message"). Y finalmente lo cerramos.

Ahora nos interesa leer lo que hay dentro de él: Primero lo abrimos, comprovamos que todo va bien y mientras esté disponible, vamos a escribir en el monitor serial lo que haya en el fichero con Serial.write(myFile.read()). Vemos que son funciones muy cómodas ya que se asemejan mucho a otras usadas para leer, escribir y mirar si hay información para leer mediante el puerto serial. Finalmente una vez leído, myFile.available() se vuelve false por lo que salimos del bucle y lo cerramos.