1. Клонируем репозиторий и переходим в ветку /develop:
```
git clone git@github.com:ProCharity-Practicum/core.git
cd ./core
git checkout develop
```

2. Заходим в директорию нужного сервиса и начинаем работу. В качестве примера, работаем над NotificationService:
```
cd ./Core/packages/notification/service
```

3. Для начала нужно установить необходимые зависимости: moleculer, moleculer-io:
```
npm install moleculer moleculer-io dotenv
npm install -D moleculer-repl moleculer-web typescript ts-node
```
moleculer-web и moleculer-repl нужны будут для запуска сервиса.

4. Создаём файл конфига moleculer. Его образец можно подсмотреть здесь: [https://github.com/moleculerjs/moleculer-template-project-typescript](https://github.com/moleculerjs/moleculer-template-project-typescript)
```
touch moleculer.config.ts
```

```
import type { BrokerOptions, MetricRegistry, ServiceBroker } from "moleculer";
import { Errors } from "moleculer";

const brokerConfig: BrokerOptions = {
	namespace: "",
	nodeID: null,
	metadata: {},
	logger: {
		type: "Console",
		options: {
			colors: true,
			moduleColors: false,
			formatter: "full",
			objectPrinter: null,
			autoPadding: false,
		},
	},
	logLevel: "info",
	// In production you can set it via `TRANSPORTER=nats://localhost:4222` environment variable.
	transporter: null,
	cacher: null,
	serializer: "JSON",
	requestTimeout: 10 * 1000,
	retryPolicy: {
		enabled: false,
		retries: 5,
		delay: 100,
		maxDelay: 1000,
		factor: 2,
		check: (err: Error) =>
			err && err instanceof Errors.MoleculerRetryableError && !!err.retryable,
	},
	maxCallLevel: 100,
	heartbeatInterval: 10,
	heartbeatTimeout: 30,
	contextParamsCloning: false,
	tracking: {
		enabled: false,
		shutdownTimeout: 5000,
	},
	disableBalancer: false,
	registry: {
		strategy: "RoundRobin",
		preferLocal: true,
	},
	circuitBreaker: {
		enabled: false,
		threshold: 0.5,
		minRequestCount: 20,
		windowTime: 60,
		halfOpenTime: 10 * 1000,
		check: (err: Error) => err && err instanceof Errors.MoleculerError && err.code >= 500,
	},
	bulkhead: {
		enabled: false,
		concurrency: 10,
		maxQueueSize: 100,
	},
	validator: true,
	errorHandler: null,
	metrics: {
		enabled: false,
	},
	tracing: {
		enabled: false,
	},
	middlewares: [],
	replCommands: null,
};

export default brokerConfig;
```

Немного по конфигу:

Transporter отвечает за способ обмена данными между сервисами. 
![[Pasted image 20240413124035.png]]
Рекомендуемый разработчиками transporter - это NATS, но можно использовать и, например, redis.

На этапе разработки его указывать в конфиге, в проде его можно будет указать через .env, не меняя конфиг.

Поскольку в Procharity уже используется redis в качестве базы данных, подозреваю, его же и будут использовать в качестве транспортера.
В целом описание конфига можно почитать на сайте:
[https://moleculer.services/docs/0.14/configuration](https://moleculer.services/docs/0.14/configuration)

5. Создаем конфиг typescript:
```
touch tsconfig.json
```

```
{
	"compilerOptions": {
		"target": "ES2019",
		"module": "commonjs",
		"outDir": "./dist",
		"strict": true,
		"strictNullChecks": true,
		"esModuleInterop": true,
		"forceConsistentCasingInFileNames": true
	}
}
```

6. Нужно не забыть добавить в .gitignore все нужное:
```
.yarn/*
.pnp.*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/sdks
!.yarn/versions
.node_modules/
.env
.vscode/*
```

7. Добавляем скрипты в package.json:
```
"scripts": {
	"dev": "ts-node ./node_modules/moleculer/bin/moleculer-runner.js --env --config moleculer.config.ts --hot --repl services/**/*.service.ts",
	"start": "moleculer-runner --env --config ./dist/moleculer.config.js --instances 1 ./dist/services",
	"build": "tsc --project tsconfig.build.json"
}
```
dev-скрпит запускает сервисы с hot-reload

8. Создаем api gateway и сервис:
```
mkdir services
cd ./services
touch api.service.ts notification.service.ts
```

Далее - в главах про api.service и notification.service:
[[Api service]]
[[Notification service]]
[[Frontend example]]