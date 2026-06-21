<img width="666" height="279" alt="Captura de pantalla 2026-06-21 001257" src="https://github.com/user-attachments/assets/fd21c9f1-65a5-4ea7-9af9-53425b99a56f" />

# Writeup-Maquina-Matrix

Maquina de la plataforma www.whoami-labs.com

Iniciamos la máquina vulnerable

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231710" src="https://github.com/user-attachments/assets/e40f9ece-ef56-4f75-8f72-50e92642f00c" />

# RECONOCIMIENTO

Una vez iniciada la maquina procedimos a realizar un escaneo de puertos utilizando la herramienta nmap

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231753" src="https://github.com/user-attachments/assets/92b19ff8-ffcb-413c-86f0-c7161afc644f" />

Realizamos un fuzzing utilizando la herramienta ferobuster para ver si nos arrojaba algunos directorios mas

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 005223" src="https://github.com/user-attachments/assets/5d323ff5-c924-4da3-a12e-2342c94dfe4f" />

Dentro de los puertos encontrados tenemos
Puerto #22 corriendo un servicio ssh en versión OpenSSH 8.4p1 Debian 5+deb11u7 (protocol 2.0)
Puerto #3000 corriendo un servicio http en su versión Node.js Express framework

Ya sabiendo los puertos que tiene esta maquina ejecutando procedi a ir a un navegador a ver si encontraba información relevante

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231820" src="https://github.com/user-attachments/assets/4affc1b0-e0ff-4868-a2df-f5a9f4e88199" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231929" src="https://github.com/user-attachments/assets/08a138f6-f538-4c35-8ef0-7ac2b2243d50" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231936" src="https://github.com/user-attachments/assets/4cfe63d9-795c-4c52-9d4b-b47caba3eb00" />

Y por ultimo encontramos un login de inicio de sesión el cual tratamos de ingresar con credenciales comunes y obtuvimos acceso al mismo como un usuario "guest"

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 231826" src="https://github.com/user-attachments/assets/fae9384d-e19a-49b6-970b-ed77295ac683" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 232027" src="https://github.com/user-attachments/assets/9c841c54-513e-49c0-ae98-c298186abc3b" />

En vista que pude acceder al sistema utilizando este usuario procedi a verificar el codigo de la pagina a ver como funcionaba, como vimos anteriormente la maquina corre en el puerto 3000 un servicio hhtp en el cual se ejecuta un JSON por lo cual dentro del codigo encontramos un llamado a una api dentro de la cual habia una herramienta que nos permitia hacer un ping a la maquina objetivo 

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 234125" src="https://github.com/user-attachments/assets/091ee2fb-d775-46c6-acc7-0f754fdfdeed" />

Ya sabiendo que la pagina contenia esta api volvimos a realizar otro fuzzing en busca de poder encontrar el directorio que la contenia

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 234337" src="https://github.com/user-attachments/assets/71ba8bcf-c016-4a0e-a26e-f669629845d5" />

La abrimos y encontramos una posible credencial

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 234357" src="https://github.com/user-attachments/assets/41d571cd-b837-493e-941a-8dd3ab79a27a" />

Luego inspeccionamos y dentro del apartado storage pudimos encontrar la cookie de sesión, por lo que procedimos a tratar de descifrarla a ver si encontrabamos algo importante

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-20 234943" src="https://github.com/user-attachments/assets/68ebba5d-4b1d-46a1-a3e9-2fd00aeeff08" />

Utilizamos una herramiena para descifrarla

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 000602" src="https://github.com/user-attachments/assets/06d02584-24e0-4169-a1c5-7a0d6b6be2d2" />


Y ya teniendo estructurado toda la información al igual que como encontramos anterior mente el secreto para realizar los cambios de los tokens ya teniamos todo para proceder a lo que es la explotación de la vulnerabilidad, procedimos a realizar los cambios de los valores, cambiamos el role del usuario por el de operator para poder que nos diera una sesiómn como un usuario que tuviera privilegios administrativos y nos genero un token nuevo

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 000827" src="https://github.com/user-attachments/assets/52fb60b6-17ca-4198-a765-034dc6fa0707" />

# EXPLOTACION

Hicimos el cambio de la cookie de sesión y refrescamos el navegador en la pagina del dashboard y obtuvimos la sesión como operador

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 000906" src="https://github.com/user-attachments/assets/df0ee6bb-7539-4ec1-aa10-515a6e1aa7f5" />

Realizamos prueba

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 000932" src="https://github.com/user-attachments/assets/16ee0ee1-82dd-4c36-9ac5-2b4066c582df" />

Luego procedimos a inyectar un codigo de bash el cual nos permitira una reverse shell, y pusimos nuestro kali en escucha

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 001043" src="https://github.com/user-attachments/assets/405e122f-335e-47c7-9d6e-ce828a8b4abe" />

Obtuvimos acceso como root

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 001036" src="https://github.com/user-attachments/assets/c777eaf6-b389-4c95-8037-34e5b5084121" />

Buscamos la flag

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 001122" src="https://github.com/user-attachments/assets/37f1235e-5b4c-42f7-8a6d-7d5ba24b8e0c" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-06-21 001136" src="https://github.com/user-attachments/assets/d3fca031-b414-47c9-b85d-11723a0f42d3" />

<img width="2688" height="1596" alt="Gemini_Generated_Image_8i02238i02238i02" src="https://github.com/user-attachments/assets/4c9bfe2d-b722-4ea0-9285-e0904ab712a5" />

