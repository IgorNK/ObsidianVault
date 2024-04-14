[Наверх](Moleculer-js)

socket.ts:
```
import { io } from 'socket.io-client';

const URL = "http://localhost:3000";

export const socket = io(URL);
```

App.tsx:

```
import { useEffect } from 'react';
import { socket } from './socket';

import './App.css'

function App() {
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
		console.log('on connect');
		socket.emit('call', 'notification.subscribe');
	}
	
	const onDisconnect = () => {
		console.log('on disconnect');
		socket.emit('call', 'notification.unsubscribe');
	}
	
	const onEvent = (data: any) => {
		console.log(data);
	}
	
	const onButtonClick = () => {
		socket.emit('call', 'notification.notify', { message: "my message" });
	}
	
	return (
		<>
			<h1>Vite + React</h1>
			<div className="card">
				<button onClick={onButtonClick}>Notify</button>
				<p>
				Edit <code>src/App.tsx</code> and save to test HMR
				</p>
			</div>
			<p className="read-the-docs">
				Click on the Vite and React logos to learn more
			</p>
		</>
	)
}
export default App
```
