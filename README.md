# socket.io-subscriber-middleware
Redux middleware to subscribe events from socket.io server

## API
### Apply middleware
```javascript
import io from 'socket.io-client';
import { createStore, applyMiddleware } from 'redux';

import socketIOSubscriberMiddleware from 'socket.io-subscriber-middleware';

import reducer from './reducer';

const socket = io.connect(process.env.SOCKET_URL);

const store = createStore(reducer, applyMiddleware(
  socketIOSubscriberMiddleware(socket)
));
```
* `socketIOSubscriberMiddleware` receive a `socket` instance created by `io.connect(<url>)`.

### Example action
```javascript
const action = {
  type: 'BIND_USER_JOINED',
  socket: {
    subscribe: {
      event: 'onlogin',
      handle: (socketResponse) => {
        // Your callback here
      }
    }
  }
};

const action = {
  type: 'BIND_USER_JOINED',
  socket: {
    subscribe: {
      event: 'onlogin',
      responseType: 'NEW_USER_JOINED'
    }
  }
};

const action = {
  type: 'UNBIND_USER_JOINED',
  socket: {
    unsubscribe: {
      event: 'onlogin'
    }
  }
};
```
* `socket.subscribe.event` define the socket.io event to bind.
* `socket.subscribe.handle` (optional) function callback called when the binded event is fired from server. The data is passed as first parameter.
* `socket.subscribe.responseType` (optional) name of action type raised in redux with server data as payload.
