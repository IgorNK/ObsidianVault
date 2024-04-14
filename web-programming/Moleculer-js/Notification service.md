[Наверх](Moleculer-js)

Для начала, опишем необходимые интерфейсы typescript для сервиса:

```
import type { ServiceSchema, ServiceSettingSchema, Service, Context } from 'moleculer';

export interface NotificationParams {
	message: string,
}

interface NotificationLocalVars {
	
}

interface NotificationMethods {
	
}

type NotificationThis = Service<ServiceSettingSchema> & NotificationMethods & NotificationLocalVars;
```

NotificationParams - это параметры запроса, который мы будем отправлять со стороны клиента. Для примера, запрос будет содержать одну строчку с сообщением.

NotificationLocalVars - описание локальных переменных сервиса. Пока что их нет.

NotificationMethods - описание служебных методов сервиса. Пока что их тоже нет.

```
type Meta = {
	'$socketId': string,
	user: string,
	'$rooms': string[]
}
```

Тип Meta нужен для описания того, что содержится в контексте запроса. В частности, нам нужен $socketId - айдишник клиента, обращающегося к сервису.

Создаём сам сервис:
```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	name: "notification",
	actions: {},
	methods: {},
};

export default NotificationService;
```

Для начала, попробуем сделать так, чтобы пользователь мог отправить сообщение через сокеты, и тут же получить его обратным ответом от сервера.

```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	name: "notification",
	actions: {
		notify: {
			params: {
				message: { type: "string" }
			},
			handler(ctx: Context<NotificationParams, Meta>) {
				this.logger.info(ctx.meta.$socketId);
				this.broker.call('io.broadcast', {
					namespace: '/',
					event: 'event',
					args: [ctx.params.message],
					local: true,
					rooms: [ctx.meta.$socketId]
				});
				return ctx.params.message;
			}
		}
	},
	...
}
```

Со стороны фронта это будет выглядеть так:
```
useEffect(() => {
	socket.on('connect', onConnect);
	socket.on('disconnect', onDisconnect);
	socket.on('event', onEvent);

	return () => {
		socket.off('connect', onConnect);
		socket.off('disconnect', onDisconnect);
		socket.off('event', onEvent);
	};
}, []);

const onConnect = () => {
	
}

const onDisconnect = () => {
	
}

const onEvent = (data: any) => {
	console.log(data);
}

const onButtonClick = () => {
	socket.emit('call', 'notification.notify', { message: "my message" });
}
```

Теперь попробуем сделать оповещения со стороны сервиса. Для примера, организуем оповещения каждую секунду.
Можно воспользоваться пакетом cron, чтобы не заморачиваться с SetTimeout:
```
npm install cron
```

```
import { CronJob } from 'cron';
```

```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	...
	async started(this) {
		const job = CronJob.from({
			cronTime: '* * * * * *',
			onTick: () => {
				this.notifyAll(`Current time: ${new Date().toString()}`)
			},
			start: true,
			timeZone: 'Asia/Tokyo'
		});
	}
}
```

Нам понадобится локальная переменная, в которой мы будем хранить всех подписчиков:

```
interface NotificationLocalVars {
	subscribers: string[],
}
```

Переменную нужно инициализировать при старте сервиса, и по желанию - почистить при остановке:

```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	...
	created() {
		this.subscribers = [];
	},
	stopped() {
		this.subscribers = [];
	},
	...
}
```

Действия в сервисе для подписки и отписки:
```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	...
	actions: {
		...
		subscribe: {
			handler(ctx: Context<null, Meta>) {
			this.logger.info(`${ctx.meta.$socketId} subscribed!`);
			this.subscribers.push(ctx.meta.$socketId);
		},
		unsubscribe: {
			handler(ctx: Context<null, Meta>) {
			this.logger.info(`${ctx.meta.$socketId} unsubscribed!`);
			this.subscribers = this.subscribers.filter((id: string) => id !== ctx.meta.$socketId);
		}
	}
}

```

И сам метод для оповещения:

```
interface NotificationMethods {
	notifyAll: (message: string) => void;
}
```

```
const NotificationService: ServiceSchema<ServiceSettingSchema> & { methods: NotificationMethods } = {
	...
	methods: {
		notifyAll(message: string) {
			this.broker.call('io.broadcast', {
				namespace: '/',
				event: 'event',
				args: [message],
				local: true,
				rooms: this.subscribers
			});
		}
	},
	...
}
```

Со стороны фронта добавим подписку и отписку к сервису оповещений:

```
const onConnect = () => {
	socket.emit('call', 'notification.subscribe');
}

const onDisconnect = () => {
	socket.emit('call', 'notification.unsubscribe');
}
```

Теперь каждую секунду по ws мы будем получать точное время в Токио.

[Наверх](Moleculer-js)