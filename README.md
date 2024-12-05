# HTSTSU
~how_to_set_this_shit_up~
Guía para Configurar Claves SSH en GitHub y Clonar Repositorios

## 1. Verificar si ya tienes una clave SSH generada

Antes de generar una nueva clave SSH deberiamos mirar si hay alguna en tu maquina `.ssh` de tu máquina(si la hay ngl no es mi problema, borrala si eso 👀):

```bash
ls -al ~/.ssh
```
Busca archivos como id_rsa o id_ed25519. Si no encuentras ninguno de estos archivos, necesitarás generar una nueva clave SSH.

## 2. Generar una nueva clave SSH
Para generar la clave deberemos ejecutar el siguiente comando
Ejecuta el siguiente comando para generar una clave SSH RSA de 4096 bits:

```
ssh-keygen -t rsa -b 4096 -C "email@dominio.com"
```
  <details>
      <summary>Que significa cada cosa</summary>
    
  - `-t` especifica el tipo de algoritmo que se va a usar para generar la clave.
  - `rsa` es el algoritmo utilizado, que es uno de los más comunes para generar claves SSH. RSA (Rivest-Shamir-Adleman) es un sistema de cifrado de clave pública que permite generar claves SSH robustas. RSA es ampliamente utilizado y compatible con la mayoría de los servidores y servicios.
  - `-b` especifica el número de bits de la clave generada.
  - `4096` indica que la clave será de 4096 bits. Cuantos más bits tenga una clave, más segura será, ya que un mayor número de bits hace más difícil de descifrar la clave mediante ataques de fuerza bruta. Con 4096 bits, la clave es considerablemente más robusta frente a ataques comparada con una clave de 2048 bits
  - `-C` permite agregar un comentario o etiqueta a la clave generada.
  - `"email@dominio.com"` es el comentario que se asocia con la clave. Usualmente se utiliza tu dirección de correo electrónico para identificar la clave, lo que facilita saber a quién pertenece la clave cuando se gestionan múltiples claves en tu sistema o en servicios como GitHub.
  
  
    </details>

## 3. Agregar tu clave pública a GitHub
Una vez que hayas generado tu clave SSH, necesitarás agregarla a tu cuenta de GitHub:

Copia la clave pública. Para ello, ejecuta el siguiente comando en la terminal:
```
cat ~/.ssh/id_rsa.pub
```
Esto mostrará la clave pública en la terminal. Copia todo el contenido de la salida (incluyendo ssh-rsa al inicio y tu correo al final).

⚠️ Añade la clave a GitHub ⚠️ :

Ve a **GitHub** > **Settings** > **SSH and GPG Keys** > **New SSH Key** , y pega la clave pública en el campo correspondiente.

## 4. Iniciar el agente SSH y añadir la clave
El agente SSH es responsable de almacenar las claves SSH que se usarán en las conexiones. Para asegurarte de que tu clave está cargada correctamente, sigue estos pasos:

Iniciar el agente SSH:
```bash
eval "$(ssh-agent -s)"
```
Esto iniciará el agente SSH y mostrará un mensaje como este (el numero puede variar):
```
Agent pid 33030
```

Añadir tu clave SSH al agente:

```bash
ssh-add ~/.ssh/id_rsa
```
Si todo va bien, verás algo como esto:

```bash
Identity added: /Users/tu_usuario/.ssh/id_rsa
```

## 5. Probar la conexión con GitHub
Una vez estos pasos esten podemos comprobar si hay conexion o no, para ello ejecutamos el siguiente codigo
```c
ssh -T git@github.com
```
Deberías obtener una respuesta similar a esta:
```
Hi tu_usuario! You've successfully authenticated, but GitHub does not provide shell access.
```
Esto indica que tenemos conexion 🟢 sino probablemente dira algo como esto otro 🔴

```
goal@github.com: Permission denied (publickey).
```

## 6. Usar SSH para clonar o interactuar con repositorios
A partir de este momento, podrás usar SSH para clonar repositorios/modificarlos. 🔧
```
git clone git@github.com:your_user/repo.git
```
