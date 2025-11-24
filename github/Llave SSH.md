## SSH
SSH es un protocolo que crea un túnel seguro y cifrado para conectar una computadora local a un servidor remoto, garantizando que la conexión no pueda ser espiada.

**¿Para qué sirve en GitHub?**  
Permite conectar el entorno local con los repositorios en la nube de forma segura. Su principal ventaja es la comodidad y la seguridad: identifica al usuario mediante claves criptográficas, lo que le permite subir cambios, descargar actualizaciones y gestionar proyectos sin tener que escribir la contraseña cada vez.
## Configuración en GitHub
1. **Acceso a Claves SSH**: 
- Ir a: "Settings" > "SSH and GPG keys".
	![[Pasted image 20251124115220.png]]
- Comprobar si hay claves SSH en la PC (Linux) con:
```bash
ls -al ~/.ssh
```
- **Recomendación**: Usar claves con nombres `id_rsa.pub`, `id_ecdsa.pub` o `id_ed25519.pub`.

2. **Generación de Nueva Clave SSH:**
- La clave pública se agregará a GitHub para autenticación.
- Ejecutar en la terminal:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Esto crea una llave SSH utilizando el correo electrónico proporcionado como etiqueta.
- **Instrucciones**:
    - Presionar **Enter** para aceptar la ubicación de archivo predeterminada.
    - Escribir una frase de contraseña segura cuando se solicite.

3. **Agregar la Clave SSH al ssh-agent**:
- Iniciar el agente SSH en segundo plano:
```bash
eval "$(ssh-agent -s)"
```
- Agregar la clave privada SSH al ssh-agent:
```bash
ssh-add ~/.ssh/id_ed25519
```

4. **Agregar Nueva Clave SSH a la Cuenta de GitHub:**
- Copiar la clave pública SSH al portapapeles:
```bash
cat ~/.ssh/id_ed25519.pub
```
- Ir a GitHub: "Settings" > "SSH and GPG keys" > "New SSH Key".
	- Pasos:
		- En el campo "Title", agregar una etiqueta descriptiva (ej. "Portátil personal").
		- Seleccionar el tipo de clave.
		- Pegar la clave pública en el campo "Clave".
		- Hacer clic en "Agregar clave SSH".
