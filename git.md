
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
