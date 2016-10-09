#Ionic 2 - Push Notifications with Firebase Cloud Messaging

	ionic io init
###crear proyecto en firebase google

###cambiar SENDER_ID=12341234 por el del proyecto firebase mensajeria en la nube (ID del remitente)
	cordova plugin add phonegap-plugin-push --variable SENDER_ID=12341234 --save

###cambiar datos en src/app/app.module.ts
	const cloudSettings: CloudSettings = {
	  	'core': {
	    	'app_id': 'APP_ID' // carmbiar por ionic.config.json 'app_id'
	  	},
	    'push': {
	        'sender_id': 'SENDER_ID', // cambiar por el del proyecto firebase mensajeria en la nube (ID del remitente)
	        'pluginConfig': {
	            'ios': {
	                'badge': true,
	                'sound': true
	            },
	            'android': {
	                'iconColor': '#343434'
	            }
	        }
	    }
	};

ir al proyecto de ionic.io setttings/cretificates crear un new segurity profile

dar a edit y en android añadir el FCM API Key del proyecto firebase google mensajeria en la nube (clave del servidor)

	ionic platform add android
	adb devices
	ionic run android

###enviar notificacion
ir al proyecto ionic.io API Keys y crear un nuevo API Token

crear archivo envio_push.sh y pegar

	curl --request POST \
    --url https://api.ionic.io/push/notifications \
    -d '{
        "tokens": [
        	"token_del_dispositivo"
        ],
        "profile": "nombre_segurity_profile_ionic_io", // cambiar por el nombre del segurity profile ionic.io/certificates (eliminar comentario para poder ejecutar)
        "notification": {
            "title": "Demo Push",
            "message": "Notificacion push demo",
            "android": {
                "payload": {
                    "idEvento": "28"
                }
            },
            "ios": {
              "title": "Push Demo",
              "message": "Demo Notificacion push"
            }
        }
    }' \
    --header 'Authorization: Bearer API_Token_ionic_io' \ // cambiar (API_Token_ionic_io) por API Token ionic.io API keys (eliminar comentario para poder ejecutar)
    --header 'Content-Type: application/json' \

ejecutar

	bash envio_push.sh