# Desarollo de Bot para Telegram

## Herramientas
- NPM
- NodeJS
- Vercel
- BotFather

## Configuracion de Entorno

### Instalar Vercel
Se necesita instalar verce para poder hacer deploy de nuestro web
```
npm install -g vercel
```

### Inicializacion de Carpeta
```
mkdir myTelegramBot
cd myTelegramBot
mkdir api
npm init
```

### Instalacion Telegram API
```
npm install node-telegram-bot-api
```

### Solicitar API Key a Telegram

* Ingresamos a [https://telegram.me/BotFather](https://telegram.me/BotFather) desde Telegram
* Dentro del chat tecleamos **/newbot** y completamos lo que nos solicita
* Al finalizar el paso anterior debemos contar con nuestra API Key la cual necesitaremos mas adelante

## Programacion de Bot

Dentro de nuestra carpeta api creamos un archivo llamaado **webhook.js** con el siguiente contenido

```javascript
const TelegramBot = require('node-telegram-bot-api');

module.exports = async (request, response) => {
    try {
        const bot = new TelegramBot(process.env.TELEGRAM_TOKEN);
        const { body } = request;

        if (body.message) {
            const { chat: { id }, text } = body.message;

            const message = `✅ Mensaje recibido: *"${text}"*\n 👋🏻`;

            await bot.sendMessage(id, message, {parse_mode: 'Markdown'});
        }
    }
    catch(error) {
        console.error('Error sending message');
        console.log(error.toString());
    }

    response.send('OK');
};

```

## Deploy con Vercel  

Si es tu primera vez usando Vercel necesitas logearte en la linea de comandos con 

```
vercel login
```

Sobre la carpeta de nuestro proyecto ejecutamos el siguiente comando y aceptamos la configuracion recomendada

```
vercel --prod
```

Luego necesitamos configurar nuestra API Key como variable de entorno de nuestro proyecto  
Para eso nos podemos dirigir a la pagina Web de Vercel loguearnos y entrar a nuestro proyecto

Posteriormente entramos a Settings->Enviroment Variables

Y agregamos una variabla llamada **TELEGRAM_TOKEN** y como valor colocamos la API Key que nos proporciono Telegram  
Es importante decir que **NUNCA** compartas esta API Key

Como ultimo paso necesitamos decirle a Telegram la direccion del WebHook que usara nuestro bot  
Para eso desde nuestro navegador o Postman realizamos una peticion GET con el siguiente formato
```
https://api.telegram.org/bot{APIKEY}/setWebhook?url={DominioVercel}/api/webhook
```

Recuerda cambiar:
- **{APIKEY}** por tu propia API Key
- **{DominioVercel}** por la URL que te brinda Vercel lo puedes encontrar en Settings->Domain

Si la peticion se ejecuta exitosamente deberias obtener una respuesta similar a esta 

```JSON
{
    "ok": true,
    "result": true,
    "description": "Webhook was set"
}
```

**LISTO**  
Ya puedes utilizar tu Bot en Telegram  

Cualquier cambio que hagas cobre el codigo y quieras agregar solo ejecuta el comando
```
vercel --prod
```

