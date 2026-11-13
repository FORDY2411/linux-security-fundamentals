# linux-security-fundamentals

# Laboratorio: Fundamentos de Seguridad, Criptografía Práctica y Control de Accesos en Linux

## 📝 Descripción del Proyecto
Este repositorio documenta las pruebas prácticas realizadas en un entorno controlado para comprender los pilares fundamentales de la ciberseguridad. A través de este laboratorio, se analizan conceptos críticos como la **Tríada CIA**, los mecanismos de **Autenticación y Autorización** a nivel de sistema operativo, el uso de **Funciones Hash (SHA-256)** para garantizar la integridad y la auditoría técnica de **Certificados Digitales SSL/TLS**.

Este proyecto forma parte de mi entrenamiento práctico en seguridad informática y demuestra habilidades de administración en Linux y criptografía aplicada.

---

## 🛠️ Entorno y Herramientas Utilizadas
* **Sistema Operativo:** Kali Linux (vía Oracle VirtualBox).
* **Segmento de Red:** Subred interna `10.0.2.0/24` (Puerta de enlace: `10.0.2.2`).
* **Herramientas de Terminal:** `Bash`, `coreutils` (`chmod`, `sha256sum`), `OpenSSL`, `cURL`.

---

## 🚀 Desarrollo del Laboratorio: Paso a Paso

### Paso 1: Aplicación de la Tríada CIA (Confidencialidad)
Para demostrar el principio de **Confidencialidad**, se creó un archivo con información sensible y se restringieron sus permisos utilizando el sistema POSIX de Linux, garantizando que ningún otro usuario del sistema pueda leer o modificar su contenido.

```bash
echo "mi tarea secreta" > ~/cia.txt
chmod 600 ~/cia.txt
ls -l ~/cia.txt
```
* **Análisis Técnico:** Al aplicar `chmod 600`, los permisos se reducen estrictamente al propietario del archivo (`-rw-------`), protegiendo la confidencialidad de los datos frente a accesos no autorizados en el sistema.

> <img width="1280" height="466" alt="paso1 png" src="https://github.com/user-attachments/assets/9c9bbfa4-551c-4054-96ea-48aeb2b06ce9" /> <img width="1280" height="403" alt="paso1 1" src="https://github.com/user-attachments/assets/814e164e-c9c4-4a17-ab31-f1f9aa5cfc57" />



---

### Paso 2: Auditoría de Control de Accesos (Autenticación vs. Autorización)
Se ejecutaron comandos para auditar la identidad del usuario actual (**Autenticación**) y validar los privilegios de administración asignados (**Autorización**).

```bash
id
sudo -l
```
* **Análisis Técnico:** El comando `id` devuelve el identificador único del usuario (UID) y los grupos a los que pertenece (roles). Por otra parte, `sudo -l` enumera las acciones específicas que el sistema autoriza a ejecutar con privilegios de superusuario (`root`). Esto demuestra que la autenticación (quién soy) precede y difiere de la autorización (qué puedo hacer).

> <img width="1280" height="950" alt="paso2" src="https://github.com/user-attachments/assets/fa6e8326-cc6e-4b8b-a51d-c1027e4bcf7b" />


---

### Paso 3: Integridad de Datos mediante Criptografía (Hashing)
Se utilizó el algoritmo **SHA-256** para generar huellas digitales criptográficas de textos y archivos, simulando cómo se detectan alteraciones en los datos y cómo se protegen las contraseñas en bases de datos.

```bash
echo -n "hola" | sha256sum
echo -n "Hola" | sha256sum
sha256sum ~/cia.txt
echo "mi tarea secreta!" >> ~/cia.txt
sha256sum ~/cia.txt
```
* **Análisis Técnico:** Se comprobó el **efecto avalancha**: cambiar un solo carácter (de "hola" a "Hola") resulta en un hash completamente distinto. Asimismo, al agregar un carácter (`!`) al archivo `cia.txt`, su hash cambió drásticamente, demostrando cómo las funciones hash validan de inmediato la **Integridad** de la información sin necesidad de leer todo el contenido.

> <img width="1280" height="488" alt="paso3" src="https://github.com/user-attachments/assets/b64a3979-9276-4b2e-bc6f-548f7d5b6f5f" /> <img width="1280" height="277" alt="paso3 1" src="https://github.com/user-attachments/assets/2a0f901d-fb80-44d8-be9c-f9a3058afc9c" />



---

### Paso 4: Inspección de Certificados Digitales (Autenticación de Servidores)
Se realizó una conexión segura hacia un servidor público externo para auditar la validez y procedencia de su certificado criptográfico SSL/TLS sobre el puerto 443.

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```
* **Análisis Técnico:** El comando extrae información del handshake criptográfico. Se verificó el **Subject** (entidad dueña del sitio web), el **Issuer** (la Autoridad de Certificación de confianza que lo firmó) y las **Dates** (periodo de vigencia). Esto demuestra cómo nuestro navegador autentica la identidad del servidor web antes de iniciar una sesión segura cifrada.

> <img width="1280" height="277" alt="paso4" src="https://github.com/user-attachments/assets/7be0ac4b-4916-4e83-9f09-edfb702fda70" />


---

## 🧠 Conclusiones y Lecciones Aprendidas

1. **La Tríada CIA en Acción:** Toda defensa o ataque informático interactúa directamente con estos pilares; restringir permisos asegura Confidencialidad, mientras que monitorizar firmas digitales asegura Integridad.
2. **Defensa en Capas:** Un sistema robusto requiere autenticar correctamente la identidad de un usuario y, de forma estricta y posterior, validar sus políticas de autorización basadas en el principio de menor privilegio.
3. **Criptografía Unidireccional:** Las funciones hash son irreversibles; su uso es vital para almacenar credenciales de forma segura (nunca en texto claro) y para detectar malware o modificaciones maliciosas en archivos del sistema.
4. **Confianza Digital:** Los certificados digitales actúan como el mecanismo de identidad de la infraestructura web, previniendo ataques de suplantación de identidad (Man-in-the-Middle) al verificar criptográficamente a los servidores.
5.
