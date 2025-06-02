Recentemente fiz uma apresentação sobre push notifications no trabalho e isso me deu a ideia de fazer um artigo sobre o assunto. Com isso, escrevi esse artigo com o objetivo de responder as seguintes perguntas:
- [O que são push notifications?](#o-que-são-push-notifications)
- [Porque usar push notifications?](#porque-usar-push-notifications)
- [Como push notifications funcionam?](#como-push-notifications-funcionam)
- [Como implementar push notifications no seu sistema?](#como-implementar-push-notifications-no-seu-sistema)

## O que são push notifications?

Push notifications são notificações as notificações que você recebe no seu dispositivo mobile que geralmente vem de algum aplicativo instalado. Ela é composta por título, ícone, mensagem, *Timestamp* (desde que foi recebida) e *Call to action* (Permitem tomar alguma ação dentro do app sem precisar entrar no mesmo).

![Push notification image](https://raw.githubusercontent.com/pdrzan/articles/refs/heads/main/01_push_notifications/images/push_notification.png)

## Porque usar push notifications

Existem vários motivos para se usar push notifications no seu app. Destaco aqui os três mais importantes, na minha opinião:

- É uma forma instantânea e direta para falar com os usuários, se tornando assim uma ótima maneira de comunicar atualizações importantes, confirmações de ações, emitir alertas, etc. Um exemplo disso são os alertas de novo login que recebemos quando acessamos nossa conta em um novo dispositivo;
- É uma ótima maneira de manter o usuário utilizando o app. Isso fica bem claro quando consideramos a quantidade de notificações que recebemos de redes sociais. Geralmente esse tipo de app utiliza atualizações de curtidas, seguidores, comentários para aumentar o tempo de tela do usuário;
- Ajuda a melhorar a experiência do usuário, tornando-o mais informado em relação ao aplicativo. Um exemplo disso são as notificações de mensagens que nos atualizam assim que obtemos uma resposta;

## Como push notifications funcionam?

Para ser possível o envio de push notifications, alguns passos são necessários:

1. A empresa dona do aplicativo publica seu aplicativo em alguma das lojas (Apple Store, Play Store, etc).
2. O usuário instala o aplicativo em seu dispositivo móvel.
3. Na primeira vez que o usuário entra no aplicativo, um identificador único para o dispositivo/aplicativo é criado.
4. Esse identificador é registrado no *Operating System Push Notification Service* (OSPNS) correspondente e também armazenado pela empresa dona do aplicativo.
6. A partir desse identificador, a empresa dona do aplicativo envia push notifications.

Os OSPNSs são serviços que um aplicativo usa para fazer o envio de notificações aos usuários. Dessa maneira, as empresas podem facilmente fazer o envio de notificações para os OSPNSs, ficando com os OSPNSs a responsabilidade de enviar as push notifications. Os OSPNSs mais comuns são o Apple Push Notification service (APNs) e Firebase Cloud Messaging (FCM).

Um vídeo bom, curto e simples sobre esse assunto é [esse](https://www.youtube.com/watch?v=ATYhOlK11QM).

## Como implementar push notifications no seu sistema?

Há vaŕias alternativas para implementar push notifications no seu aplicativo. O que geralmente é mais fácil é utilizar serviços que suportam o envio de notificações tanto para dispositivos Android quanto para dispositivos IOS. Geralmente eles seguem o mesmo padrão, anteriormente visto, de criar notificações e enviá-las para esses serviços junto com o identificador único do dispositivo de destino. Alguns desses serviços:

- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
- [Amazon Simple Notification](https://aws.amazon.com/sns/)
- [Notification Hubs](https://azure.microsoft.com/en-us/products/notification-hubs)
- [Expo Push Notifications](https://docs.expo.dev/push-notifications/overview/)
- [Pushover](https://pushover.net/)

<hr>

Achou algum erro no artigo? Me mande um email para pedeozanelato19@gmail.com falando sobre ele. 

## Referências
- https://www.airship.com/resources/explainer/push-notifications-explained/
- https://www.ibm.com/think/topics/push-notifications
- https://www.adjust.com/glossary/push-notification/
- https://onesignal.com/what-are-push-notifications
- https://aws.amazon.com/pt/what-is/push-notification-service/
- https://firebase.google.com/products/cloud-messaging
- https://developer.apple.com/notifications/
