[Наверх](Moleculer-js)

```
import SocketIoService from 'moleculer-io';
import ApiGateway from 'moleculer-web';  

const SocketService = {
	name: 'io',
	mixins: [ApiGateway, SocketIoService],
	settings: {
		port: process.env.PORT != null ? Number(process.env.PORT) : 3000,
		cors: {
			origin: '*',
		},
		io: {
			namespaces: {
				"/": {
					authorization: false,
					events: {
						call: {
							whitelist: ["notification.*"],
						},
					}
				}
			}
		}
	},
	methods: {
	
	}
}

export default SocketService;
```

Это пример описания API gateway для работы с websockets.
Более традиционный пример для REST API можно посмотреть вот здесь:
[[https://github.com/moleculerjs/moleculer-template-project-typescript/blob/master/template/services/api.service.ts]]
Или вот здесь:
[[https://github.com/IgorNK/gallery-backend/blob/0c53f0e28e04d7b7e3c0b755789741a6f8c84a86/services/api.service.ts]]

В случае с REST в API gateway будет описание роутов приложения:
```
routes: [
	{
      	path: "/users",
		authentication: true,
		authorization: true,
		aliases: {
			'GET /': 'users.list',
			'POST /': 'users.create',
			'GET /:id': 'users.get',
			'POST /login': 'users.login',
			'GET /me': 'users.me'
		},
		whitelist: ['**'],
		mappingPolicy: 'restrict',
		bodyParsers: {
			json: true,
			urlencoded: {
				extended: false
			}
		}
	},
	{
	    path: "/galleries",
		authentication: true,
		authorization: true,
		aliases: {
			'GET /': 'galleries.list',
			'POST /': 'galleries.create',
			'GET /:id': 'galleries.get'
		},
		whitelist: ['**'],
		mappingPolicy: 'restrict',
		bodyParsers: {
			json: true,
			urlencoded: {
				extended: false
			}
		},
    },
	{
		path: "/stories",
		authentication: true,
		authorization: true,
		aliases: {
			'GET /': 'stories.list',
			'POST /': 'stories.create',
			'GET /:id': 'stories.get'
		},
		whitelist: ['**'],
		mappingPolicy: 'restrict',
		bodyParsers: {
			json: true,
			urlencoded: {
				extended: false
			}
		},
	},
]
```

