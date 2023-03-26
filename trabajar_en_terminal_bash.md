
.: current directory
clear: limpiar el front de la terminal
cd: da igual en el path que esté, que si escribimos cd, volvemos a home
ls: listado de archivos que contiene la ruta donde estemos
exit: salir del terminal
echo $VARIABLE: son variables de entorno. Son muy importantes porque son variables a la que tiene acceso cualquier programa que se esté corriendo en la terminal actual. Por ejemplo, guardar paths importantes
Si ya existe una variable de entorno es porque es importante, como $PATH:

/c/Users/SOPORTE/anaconda3/envs/spaceflights:/c/Users/SOPORTE/anaconda3/envs/spaceflights/Library/mingw-w64/bin:/c/Users/SOPORTE/anaconda3/envs/spaceflights/Library/usr/bin:/c/Users/SOPORTE/anaconda3/envs/spaceflights/Library/bin:/c/Users/SOPORTE/anaconda3/envs/spaceflights/Scripts:/c/Users/SOPORTE/anaconda3/envs/spaceflights/bin:C:/Users/SOPORTE/anaconda3/condabin:/c/Users/SOPORTE/bin:/mingw64/bin:/usr/local/bin:/usr/bin:/usr/bin:/mingw64/bin:/usr/bin:/c/Users/SOPORTE/bin:/c/Windows/system32:/c/Windows:/c/Windows/System32/Wbem:/c/Windows/System32/WindowsPowerShell/v1.0:/c/Windows/System32/OpenSSH:/cmd:/c/Users/SOPORTE/AppData/Local/Programs/Python/Python310/Scripts:/c/Users/SOPORTE/AppData/Local/Programs/Python/Python310:/c/Users/SOPORTE/AppData/Local/Microsoft/WindowsApps:/c/Users/SOPORTE/AppData/Local/Programs/Microsoft VS Code/bin:/usr/bin/vendor_perl:/usr/bin/core_perl

paths diferentes separados por dos puntos. Están las carpetas ordenadas por prioridades
export $VARIABLE="contenido" manera de crear variables de entorno
echo $VARIABLE #modo de chequear el contenido de la variable de entorno
echo $(función): se sabe que es una función y no una variable de entorno por los paréntesis
conda activate environment: también funciona en bash terminal para activar entornos
mv: comando move. Mueve un fichero de un path a otro, y se utiliza también comúnmente para hacer rename de los ficheros.
ll: listado más fácil de entender que ls.
touch name_file: creación de un fichero en la ruta en la que estemos. 
touch .name-file: creación de un fichero oculto.
ls -a: visualizar ficheros ocultos.
rm ./namefile: eliminamos en la ruta en la que estemos el file con el nombre especificado.
mkdir: sirve para crear directorios
mkdir -p one/two/three: -p lo que hace posible es que, si no existe la carpeta TWO (ya que son carpetas encadenadas el resultado), la crea
cp ./file1 file2: créame una copia del fichero file 1 llamada file 2(en la carpeta donde me encuentro)
mv file1 file2: en la carpeta donde estoy, sobreescribe en file2 el contenido de file1, y file1 ya no existe. Se puede verificar este resultado con ls.
mv funciona con carpetas; cp sin embargo no. Para ello, hay que escribir cp -r(r significa recursivo, que copie todo)
rm -rf: CUIDADO CON ESTE MODIFICADOR DE RM. Implica remove recursivo y forced (sin preguntar confirmación)
cd: cd sin nada nos lleva a home
