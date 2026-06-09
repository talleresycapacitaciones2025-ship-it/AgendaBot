🤖 AgendaBot – Asistente de agenda personal con IA
AgendaBot es un asistente virtual inteligente que permite gestionar eventos, recordatorios y citas mediante lenguaje natural.
Está construido con Cloudflare Workers (backend), Cloudflare KV (almacenamiento persistente) y Groq (IA para interpretar comandos), con una interfaz web amigable usando FullCalendar y HTML/CSS/JS puro.

🚀 Características principales
🗣️ Comandos en lenguaje natural:
"Agenda una reunión con Juan el viernes a las 3pm"
"¿Qué tengo esta semana?"
"Cancela el webinar del martes"
"Cambia la reunión con Ana a las 4pm"

📅 Calendario visual interactivo

Muestra eventos por mes, semana o día.

Hacer clic en un evento lo cancela automáticamente.

Formato de hora con dos dígitos (ej. 11:00).

🔔 Recordatorios automáticos (opcional, vía email usando Resend).

🌍 Zona horaria local (Ecuador / America/Guayaquil) para fechas y horas.

📌 Respuestas claras con íconos (📌, ✅, 🗑️, 📅) y formato amigable.

💾 Almacenamiento persistente en Cloudflare KV (los eventos no se pierden al reiniciar el Worker).

🛠️ Tecnologías utilizadas
Tecnología	Propósito
Cloudflare Workers	Backend serverless (API REST)
Cloudflare KV	Base de datos clave-valor para eventos
Groq Cloud	Modelo llama-3.1-8b-instant para entender lenguaje natural
FullCalendar	Calendario interactivo en el frontend
HTML/CSS/JS	Interfaz de usuario responsive
GitHub Pages	(Opcional) para alojar el frontend estático

📁 Estructura del proyecto
text
agendabot/
├── worker.js          # Código del Cloudflare Worker (backend)
├── index.html         # Interfaz web del chat + calendario
└── README.md          # Este archivo

⚙️ Requisitos previos
Una cuenta en Cloudflare (plan gratuito).

Una cuenta en Groq Cloud para obtener una API Key.

(Opcional) Una cuenta en Resend para recordatorios por email.

3. Configurar KV (almacenamiento)
Crea un namespace KV:
npx wrangler kv namespace create AGENDA_KV
(o desde el panel: Workers & Pages → KV → Create namespace con nombre AGENDA_KV).

Vincula el namespace al Worker:

En el Worker, ve a Settings → Bindings → Add binding.

Variable name: AGENDA_KV

KV namespace: selecciona el que creaste.

4. Configurar la API Key de Groq
En el Worker, ve a Settings → Variables → Environment Variables.

Añade GROQ_API_KEY con el valor de tu clave de Groq.

5. (Opcional) Configurar recordatorios por email
En el Worker, añade las variables:

RESEND_API_KEY (tu clave de Resend)

En el panel, activa un Cron Trigger (por ejemplo 0 * * * * para cada hora).

6. Desplegar el frontend
Publica el archivo index.html en GitHub Pages, Cloudflare Pages o simplemente ábrelo localmente.

Asegúrate de que las constantes WORKER_URL y EVENTOS_URL dentro del HTML apunten a tu Worker:

javascript
const WORKER_URL = "https://agenda-bot.tu-usuario.workers.dev/chat";
const EVENTOS_URL = "https://agenda-bot.tu-usuario.workers.dev/eventos";
🧪 Uso básico
Una vez desplegado, interactúa con el asistente a través del chat o el calendario.

Comandos soportados (ejemplos)
Tarea	Comando
Crear evento	"Agenda reunión con Ana el jueves a las 10am"
Listar eventos de hoy	"¿Qué tengo hoy?"
Listar eventos de una semana	"¿Qué tengo esta semana?"
Listar eventos de fecha específica	"¿Qué tengo el viernes?" o "¿Qué tengo el 15 de junio?"
Cancelar evento	"Cancela reunión con Ana" (cancelará el más reciente con ese título)
Cancelar evento con fecha	"Cancela reunión con Ana del 2026-06-12"
Cancelar el último evento	"Cancela evento"
Editar evento (hora o título)	"Cambia la reunión con Ana a las 4pm"
Editar título	"Cambia el título de 'reunión' a 'clase'"
Cancelación desde el calendario
Haz clic en cualquier evento del calendario y confirma la cancelación. El evento se eliminará al instante.

📝 Personalización
Colores: En el HTML, modifica la paleta de colores en la sección <style>.

Formato de hora: En FullCalendar, cambia la opción eventTimeFormat en el JavaScript.

Palabras clave: Ajusta la función extraerPalabraClave en worker.js para añadir o eliminar stopwords.

Zona horaria: Si estás en otra región, cambia America/Guayaquil por tu zona en la línea:

javascript
const localDateStr = new Date().toLocaleString('en-US', { timeZone: 'America/Guayaquil' });
🧰 Solución de problemas comunes
Problema	Posible solución
El Worker no responde	Verifica que la variable GROQ_API_KEY esté configurada y sea válida.
Los eventos no se guardan	Comprueba el binding KV (debe llamarse AGENDA_KV).
El calendario muestra la hora incorrecta	Asegúrate de que en el HTML se use eventTimeFormat: { hour: '2-digit', minute: '2-digit', hour12: false }.
La cancelación no funciona	Revisa que el evento enviado desde el calendario sea Cancela id:${id}. Si usas el chat, intenta con el título exacto.
El bot no entiende fechas relativas	Mejora el prompt en la llamada a Groq o implementa más lógica de fechas en parsearFecha.

🙏 Agradecimientos
Cloudflare por su excelente plataforma serverless.

Groq por la API de inferencia rápida y generosa.

FullCalendar por el componente de calendario.

A la comunidad de código abierto por las herramientas que hacen esto posible.

Desarrollado con ❤️ para gestión personal de tiempo.


Nota: Este asistente no almacena datos biométricos ni información sensible fuera de los eventos que el usuario introduce. Es seguro para uso personal.
