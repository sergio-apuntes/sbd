Para la ejecución de docker en un contenedor

En primer lugar se debe crear un contenedor por defecto, en la pestaña de `general` tiene que tener marcado tanto *Contenedores sin privilegios* como *Anidado* de forma que al final una vez creado el conenedor tenemos en Opciones `Privilegios: Si` y `Funcionalidades: Nesting=1`

Luego, para ejecutar **Docker** en un contenedor (CT) de Proxmox con una imagen de Ubuntu 24.04, sigue estos pasos:

1. **Actualizar el sistema**:
   Asegúrate de que tu sistema esté actualizado:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Instalar paquetes necesarios**:
   Docker requiere algunos paquetes esenciales. Instálalos con el siguiente comando:
   ```bash
   sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Agregar la clave GPG y el repositorio de Docker**:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. **Instalar Docker**:
   Actualiza la lista de paquetes e instala Docker:
   ```bash
   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io
   ```

5. **Iniciar y habilitar Docker**:
   Asegúrate de que el servicio Docker esté activo y se inicie automáticamente al arrancar el sistema:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

6. **Probar la instalación de Docker**:
   Ejecuta el contenedor de prueba `hello-world` para verificar que Docker esté funcionando correctamente:
   ```bash
   sudo docker run hello-world
   ```

Si todo está configurado correctamente, deberías ver un mensaje de "Hello from Docker!"