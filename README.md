# React practice breakdown
## 1. Create TodoPage.tsx
frontend/src/pages
-> TodoPage.tsx

### 1-1 Init TodoPage function
frontend/src/pages/TodoPages.tsx
```typescript=
import React from 'react';
export default function TodoPage()
{
    return (
    <>
    </>
    );
}
```

### 1-2 Register route
frontend/src/index.tsx
```typescript=
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import { store } from './app/store';
import { Provider } from 'react-redux';
import * as serviceWorker from './serviceWorker';
import { BrowserRouter, Switch, Route } from 'react-router-dom';
import HomePage from './pages/HomePage';
import HeaderBar from './components/HeaderBar';
// TODO: IMPORT TodoPage
import TodoPage from './pages/TodoPage';
// END


const environment = process.env.NODE_ENV;



ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <BrowserRouter>
        <HeaderBar />
        <div className="p-grid">
          <div className="p-col-3" />
          <div className="p-col-6">
            <Switch>
              <Route exact path="/" component={HomePage}></Route>
// TODO: ADD ROUTE
                <Route exact path="/todo" component={TodoPage}></Route>
// END
            </Switch>
          </div>
          <div className="p-col-3" />
        </div>
      </BrowserRouter>
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```

### 1-3 Add hello message
frontend/src/pages/TodoPage.tsx
```typescript=
import React from 'react';
// TODO: IMPORT REDUX
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
// END
export default function TodoPage()
{
// TODO: ADD REDUX FUNCTION
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
// END
    return (
    <>
// TODO: ADD HELLO MESSAGE
        <h3>hello, {user.name}</h3>
// END
    </>
    );
}
```

### 1-4 Add Header link to Todo
frontend/src/components/HeaderBar.tsx
```typescript=
import React from 'react';
import './../index.css';
import { Menubar } from 'primereact/menubar';
import { useHistory } from 'react-router-dom';

export default function HeaderBar() {
  let history = useHistory();

  const items = [
    {
      label: 'Home',
      command: () => history.push('/'),
    }
// TODO: ADD TODO LINK
    ,{
        label: 'Todo',
        command: () => history.push('/todo'),
    }
// END
  ];
  return (
    <>
      <Menubar model={items} />
    </>
  );
}
```
## 2. TodoPage Layout
frontend/src/pages/TodoPage.tsx

### 2-1 Create Todo Part
```typescript=
// TODO: IMPORT useState
import React, { useState }from 'react';
// END
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
// TODO: IMPORT InputText, Button FROM PRIMEREACT
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
// END

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
// TODO: ADD useState
    const [inputTodo, setInputTodo] = useState<string>('');
// END
    
    return (
    <>
        <h3>hello, {user.name}</h3>
// TODO: ADD CREATE TODO TEMPLATE
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (console.log('create',inputTodo))}></Button>
            </div>
        </div>
// END
    </>
    );
}
```

### 2-2 Add CardTemplate for Todo 
```typescript=
import React, { useState }from 'react';
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
// TODO: IMPORT Card, Tag FROM PRIMEREACT & TYPES 
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
// END

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');

// TODO: ADD MOCK DATA
    const temArr: Array<TodoBody> = [
      {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
      {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
      {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
      {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    ];
// END
// TODO: ADD TODO TEMPLATE
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
              onClick={() => console.log('Start',todoObject)}
            />
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
              onClick={() => console.log('Delete',todoObject)}
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };
// END

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (console.log('create',inputTodo))}></Button>
            </div>
        </div>
// TODO: RENDER TODO ARRAY
        <div className="p-mt-2">{temArr.map((e)=>cardTemplate(e))}</div>
// END
    </>
    );
}
```
### 2-3 Confirm dialog
```typescript=
import React, { useState }from 'react';
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
// TODO: IMPORT confirmDialog FROM PRIMEREACT
import { confirmDialog } from 'primereact/confirmdialog';
// END

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');

    const temArr: Array<TodoBody> = [
      {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
      {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
      {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
      {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    ];
// TODO: ADD CONFIRMDIALOG
    const acceptUpdateStatus = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept update status');
      };
      const acceptDeleteTodo = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept delete todo');
      };
      const rejectUpdateStatus = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel update status');
      };
      const rejectDeleteTodo = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel delete todo');
      };
    const confirmUpdateStatus = (todoObject: TodoBody) => {
        return confirmDialog({
          message: 'Do you want to process this todo item?',
          header: 'Process Todo',
          accept: () => acceptUpdateStatus(todoObject),
          reject: () => rejectUpdateStatus(todoObject),
        });
      };
    const confirmDeleteTodo = (todoObject: TodoBody) => {
        return confirmDialog({
            message: 'Do you want to Delete this todo item?',
            header: 'Delete Todo',
            accept: () => acceptDeleteTodo(todoObject),
            reject: () => rejectDeleteTodo(todoObject),
        });
    };
// END
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
// TODO: ONCLICK TO TRIGGER CONFIRMDIALOG
              onClick={() => confirmUpdateStatus(todoObject)}
// END
            />
            
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
// TODO: ONCLICK TO TRIGGER CONFIRMDIALOG
              onClick={() => confirmDeleteTodo(todoObject)}
// END
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (console.log('create',inputTodo))}></Button>
            </div>
        </div>
        <div className="p-mt-2">{temArr.map((e)=>cardTemplate(e))}</div>
    </>
    );
}
```
## 3. Api Service
frontend/src/services/NodeService.ts
### 3-1 GET
```typescript=
async getTodo(): Promise<AxiosResponse<TodoResponse>> {
        try {
          const res = await axios.get<TodoResponse>('/todos');
          return Promise.resolve(res);
        } catch (err) {
          return Promise.reject(`${err}`);
        }
    }
```
### 3-2 POST
```typescript=
async postTodo(todoObject: TodoBody): Promise<AxiosResponse<TodoResponse>> {
        try {
          const res = await axios.post<TodoResponse>('/todos', todoObject);
          return Promise.resolve(res);
        } catch (err) {
          return Promise.reject(`${err}`);
        }
      }
```
### 3-3 DELETE
```typescript=
async deleteTodo(todoObject: TodoBody): Promise<AxiosResponse<TodoResponse>> {
        try {
          const res = await axios.delete<TodoResponse>(`/todos/${todoObject.id}`);
          return Promise.resolve(res);
        } catch (err) {
          return Promise.reject(`${err}`);
        }
      }
```
### 3-4 PUT
```typescript=
async putTodo(todoObject: TodoBody): Promise<AxiosResponse<TodoResponse>> {
        try {
          const res = await axios.put<TodoResponse>(`/todos/${todoObject.id}`, todoObject);
          return Promise.resolve(res);
        } catch (err) {
          return Promise.reject(`${err}`);
        }
      }
```

## 4. View with API
frontend/src/pages/TodoPage.tsx
### 4-1 Get todo
```typescript=
// TODO: IMPORT useEffect
import React, { useState,useEffect }from 'react';
// END
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
import { confirmDialog } from 'primereact/confirmdialog';
// TODO: IMPORT NodeService
import { NodeService } from '../services/NodeService';
// END

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');
// TODO: ADD useState FOR TODOLIST & use api to get todolist
    const [todolist, setTodolist] = useState<Array<TodoBody>>([]);
    
    const nodeService = new NodeService();


    useEffect(() => {
        getTodos();
      }, []);
      const getTodos = async () => {
        await nodeService.getTodo().then((res) => setTodolist(res.data.todos));
      };
// END

// TODO: COMMENT OUT MOCK DATA
    // const temArr: Array<TodoBody> = [
    //   {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
    //   {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
    //   {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
    //   {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    // ];
// END
    const acceptUpdateStatus = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept update status');
      };
      const acceptDeleteTodo = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept delete todo');
      };
      const rejectUpdateStatus = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel update status');
      };
      const rejectDeleteTodo = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel delete todo');
      };
    const confirmUpdateStatus = (todoObject: TodoBody) => {
        return confirmDialog({
          message: 'Do you want to process this todo item?',
          header: 'Process Todo',
          accept: () => acceptUpdateStatus(todoObject),
          reject: () => rejectUpdateStatus(todoObject),
        });
      };
    const confirmDeleteTodo = (todoObject: TodoBody) => {
        return confirmDialog({
            message: 'Do you want to Delete this todo item?',
            header: 'Delete Todo',
            accept: () => acceptDeleteTodo(todoObject),
            reject: () => rejectDeleteTodo(todoObject),
        });
    };
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
              onClick={() => confirmUpdateStatus(todoObject)}
            />
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
              onClick={() => confirmDeleteTodo(todoObject)}
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (console.log('create',inputTodo))}></Button>
            </div>
        </div>
        <div className="p-mt-2">
// TODO: REPLACE MOCK DATA ARRAY           
            {todolist.map((e)=>cardTemplate(e))}
// END
        </div>
    </>
    );
}
```

### 4-2 Create Todo
```typescript=
import React, { useState,useEffect }from 'react';
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
import { confirmDialog } from 'primereact/confirmdialog';
import { NodeService } from '../services/NodeService';

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');
    const [todolist, setTodolist] = useState<Array<TodoBody>>([]);
    const nodeService = new NodeService();

    useEffect(() => {
        getTodos();
      }, []);
      const getTodos = async () => {
        await nodeService.getTodo().then((res) => setTodolist(res.data.todos));
      };
// TODO: ADD POST API METHOD
      const createTodo = async () => {
        await nodeService
          .postTodo({ name: inputTodo, status: TodoStatus.NotStarted })
          .then((res) => setTodolist(res.data.todos));
      };
// END
    // const temArr: Array<TodoBody> = [
    //   {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
    //   {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
    //   {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
    //   {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    // ];
    const acceptUpdateStatus = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept update status');
      };
      const acceptDeleteTodo = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept delete todo');
      };
      const rejectUpdateStatus = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel update status');
      };
      const rejectDeleteTodo = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel delete todo');
      };
    const confirmUpdateStatus = (todoObject: TodoBody) => {
        return confirmDialog({
          message: 'Do you want to process this todo item?',
          header: 'Process Todo',
          accept: () => acceptUpdateStatus(todoObject),
          reject: () => rejectUpdateStatus(todoObject),
        });
      };
    const confirmDeleteTodo = (todoObject: TodoBody) => {
        return confirmDialog({
            message: 'Do you want to Delete this todo item?',
            header: 'Delete Todo',
            accept: () => acceptDeleteTodo(todoObject),
            reject: () => rejectDeleteTodo(todoObject),
        });
    };
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
              onClick={() => confirmUpdateStatus(todoObject)}
            />
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
              onClick={() => confirmDeleteTodo(todoObject)}
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
// TODO: ADD ONCLICK FUNCTION WITH CREATE
                <Button label="Submit" onClick={() => (createTodo())}></Button>
// END
            </div>
        </div>
        <div className="p-mt-2">{todolist.map((e)=>cardTemplate(e))}</div>
    </>
    );
}
```
### 4-3 Update Todo
```typescript=
import React, { useState,useEffect }from 'react';
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
import { confirmDialog } from 'primereact/confirmdialog';
import { NodeService } from '../services/NodeService';

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');
    const [todolist, setTodolist] = useState<Array<TodoBody>>([]);
    const nodeService = new NodeService();

    useEffect(() => {
        getTodos();
      }, []);
      const getTodos = async () => {
        await nodeService.getTodo().then((res) => setTodolist(res.data.todos));
      };
      const createTodo = async () => {
        await nodeService
          .postTodo({ name: inputTodo, status: TodoStatus.NotStarted })
          .then((res) => setTodolist(res.data.todos));
      };
    // const temArr: Array<TodoBody> = [
    //   {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
    //   {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
    //   {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
    //   {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    // ];
    const acceptUpdateStatus = async (todoObject: TodoBody) => {
// TODO: ADD PUT METHOD
        if (todoObject.id !== undefined) {
          await nodeService
            .putTodo({ id: todoObject.id, name: todoObject.name, status: TodoStatus.Process })
            .then((res) => setTodolist(res.data.todos));
        }
    
        console.log(todoObject);
// END
      };
      const acceptDeleteTodo = async (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Accept delete todo');
      };
      const rejectUpdateStatus = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel update status');
      };
      const rejectDeleteTodo = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel delete todo');
      };
    const confirmUpdateStatus = (todoObject: TodoBody) => {
        return confirmDialog({
          message: 'Do you want to process this todo item?',
          header: 'Process Todo',
          accept: () => acceptUpdateStatus(todoObject),
          reject: () => rejectUpdateStatus(todoObject),
        });
      };
    const confirmDeleteTodo = (todoObject: TodoBody) => {
        return confirmDialog({
            message: 'Do you want to Delete this todo item?',
            header: 'Delete Todo',
            accept: () => acceptDeleteTodo(todoObject),
            reject: () => rejectDeleteTodo(todoObject),
        });
    };
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
              onClick={() => confirmUpdateStatus(todoObject)}
            />
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
              onClick={() => confirmDeleteTodo(todoObject)}
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (createTodo())}></Button>
            </div>
        </div>
        <div className="p-mt-2">{todolist.map((e)=>cardTemplate(e))}</div>
    </>
    );
}
```
### 4-4 Delete Todo
```typescript=
import React, { useState,useEffect }from 'react';
import { useAppDispatch, useAppSelector } from '../app/hooks';
import { selectUser } from '../features/user/userSlice';
import { InputText } from 'primereact/inputtext';
import { Button } from 'primereact/button';
import { Card } from 'primereact/card';
import { Tag } from 'primereact/tag';
import { TodoBody, TodoStatus } from '../types/todo';
import { confirmDialog } from 'primereact/confirmdialog';
import { NodeService } from '../services/NodeService';

export default function TodoPage()
{
    const dispatch = useAppDispatch();
    const user = useAppSelector(selectUser);
    const [inputTodo, setInputTodo] = useState<string>('');
    const [todolist, setTodolist] = useState<Array<TodoBody>>([]);
    const nodeService = new NodeService();

    useEffect(() => {
        getTodos();
      }, []);
      const getTodos = async () => {
        await nodeService.getTodo().then((res) => setTodolist(res.data.todos));
      };
      const createTodo = async () => {
        await nodeService
          .postTodo({ name: inputTodo, status: TodoStatus.NotStarted })
          .then((res) => setTodolist(res.data.todos));
      };
    // const temArr: Array<TodoBody> = [
    //   {id: '1',name:'Mon.',status: TodoStatus.NotStarted},
    //   {id: '2',name:'Tue.',status:TodoStatus.NotStarted},
    //   {id: '3',name:'Wed.',status:TodoStatus.NotStarted},
    //   {id: '4',name:'Thur.',status:TodoStatus.NotStarted}
    // ];
    const acceptUpdateStatus = async (todoObject: TodoBody) => {
        if (todoObject.id !== undefined) {
          await nodeService
            .putTodo({ id: todoObject.id, name: todoObject.name, status: TodoStatus.Process })
            .then((res) => setTodolist(res.data.todos));
        }
    
        console.log(todoObject);
      };
      const acceptDeleteTodo = async (todoObject: TodoBody) => {
// TODO: ADD DELETE METHOD
        await nodeService.deleteTodo(todoObject).then((res) => setTodolist(res.data.todos));
        console.log(todoObject);
// END
      };
      const rejectUpdateStatus = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel update status');
      };
      const rejectDeleteTodo = (todoObject: TodoBody) => {
        console.log(todoObject);
        console.log('Cancel delete todo');
      };
    const confirmUpdateStatus = (todoObject: TodoBody) => {
        return confirmDialog({
          message: 'Do you want to process this todo item?',
          header: 'Process Todo',
          accept: () => acceptUpdateStatus(todoObject),
          reject: () => rejectUpdateStatus(todoObject),
        });
      };
    const confirmDeleteTodo = (todoObject: TodoBody) => {
        return confirmDialog({
            message: 'Do you want to Delete this todo item?',
            header: 'Delete Todo',
            accept: () => acceptDeleteTodo(todoObject),
            reject: () => rejectDeleteTodo(todoObject),
        });
    };
    const notStartTag = () => {
        return <Tag value="Not Started"></Tag>;
      };
      const processTag = () => {
        return <Tag value="Process" severity="warning"></Tag>;
      };
    const cardFooter = (todoObject: TodoBody) => {
        return (
          <span>
            <Button
              className="p-button-success p-m-1"
              label="Start"
              icon="pi pi-play"
              onClick={() => confirmUpdateStatus(todoObject)}
            />
            <Button
              className="p-button-danger p-m-1"
              label="Delete"
              icon="pi pi-trash"
              onClick={() => confirmDeleteTodo(todoObject)}
            />
          </span>
        );
      };
    const cardTemplate = (todoObject: TodoBody) => {
        return (
          <div className="p-d-flex p-m-1 p-col-6" key={todoObject.id}>
            <Card title={todoObject.name} footer={cardFooter(todoObject)}>
              <div className="p-text-right">
                {todoObject.status === TodoStatus.NotStarted ? notStartTag() : processTag()}
              </div>
            </Card>
          </div>
        );
      };

    return (
    <>
        <h3>hello, {user.name}</h3>
        <div className="p-d-inline-flex p-mt-6">
            <div className="p-mb-2 p-m-1">
                <span className="p-float-label">
                    <InputText
                    id="inTodo"
                    value={inputTodo}
                    onChange={(e) => setInputTodo(e.target.value)}
                    />
                    <label htmlFor="inTodo">Create Todo </label>
                </span>
            </div>
            <div className="p-mb-2 p-m-1">
                <Button label="Submit" onClick={() => (createTodo())}></Button>
            </div>
        </div>
        <div className="p-mt-2">{todolist.map((e)=>cardTemplate(e))}</div>
    </>
    );
}
```

## 5. Mock Server with Mirage
### 5-1 Create MockServer.ts
frontend/src/services -> MockServer.ts
### 5-2 Init MockServer.ts
frontend/src/services/MockServer.ts
```typescript=
import { createServer, Factory, Model} from 'miragejs';
import { TodoBody, TodoStatus } from '../types/todo';

export function MockServer({ environment = 'development' }) {
    return createServer({
      environment,
      models: {
      },
      factories: {
      },
      seeds(server) {
      },
      routes() {
      },
    });
  }
```
### 5-3 Setup model
frontend/src/services/MockServer.ts
```typescript=
import { createServer, Factory, Model} from 'miragejs';
import { TodoBody, TodoStatus } from '../types/todo';
export function MockServer({ environment = 'development' }) {
    return createServer({
      environment,
      models: {
// TODO: ADD MODEL
        todo: Model.extend<Partial<TodoBody>>({}),
// END
      },
      factories: {
      },
      seeds(server) {
      },
      routes() {
      },
    });
  }
```

### 5-4 Setup routes
frontend/src/services/MockServer.ts
```typescript=
import { createServer, Factory, Model} from 'miragejs';
import { TodoBody, TodoStatus } from '../types/todo';
export function MockServer({ environment = 'development' }) {
    return createServer({
      environment,
      models: {
        todo: Model.extend<Partial<TodoBody>>({}),
      },
      factories: {
      },
      seeds(server) {
      },
      routes() {
// TODO: ADD ROUTES
        this.get('/todos', (schema, request) => {
            return schema.all('todo');
          });
          this.post('/todos', (schema, request) => {
            let attrs = JSON.parse(request.requestBody);
            schema.create('todo', attrs);
            return schema.all('todo');
          });
          this.put('/todos/:key', (schema, request) => {
            let keyid = request.params.key;
            let attrs = JSON.parse(request.requestBody);
            schema.db.todos.update(keyid, attrs);
    
            //schema.find("todo", keyid).destroy();
            return schema.all('todo');
          });
          this.del('/todos/:key', (schema, request) => {
            let keyid = request.params.key;
            let post = schema.findBy('todo', { id: keyid });
            if (post !== null) {
              post.destroy();
            }
            return schema.all('todo');
          });
// END
      },
    });
  }
```
### 5-5 Add Mock Server to React
frontend/src/index.tsx
```typescript=
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import { store } from './app/store';
import { Provider } from 'react-redux';
import * as serviceWorker from './serviceWorker';
import { BrowserRouter, Switch, Route } from 'react-router-dom';
import HomePage from './pages/HomePage';
import HeaderBar from './components/HeaderBar';
import TodoPage from './pages/TodoPage';
// TODO: IMPORT MockServer
import { MockServer } from './services/MockServer';
// END

const environment = process.env.NODE_ENV;

// TODO: SETUP MockServer
if (environment !== 'production') {
  MockServer({ environment });
}
// END

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <BrowserRouter>
        <HeaderBar />
        <div className="p-grid">
          <div className="p-col-3" />
          <div className="p-col-6">
            <Switch>
              <Route exact path="/" component={HomePage}></Route>
              <Route exact path="/todo" component={TodoPage}></Route>
            </Switch>
          </div>
          <div className="p-col-3" />
        </div>
      </BrowserRouter>
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```

### 5-6 Add faker, factory, seeds to Mock Server
frontend/src/services/MockServer.ts
```typescript=
import { createServer, Factory, Model} from 'miragejs';
import { TodoBody, TodoStatus } from '../types/todo';
// TODO: IMPORT FAKER
import faker from 'faker';
// END

export function MockServer({ environment = 'development' }) {
    return createServer({
      environment,
      models: {
        todo: Model.extend<Partial<TodoBody>>({}),
      },
      factories: {
// TODO: ADD MOCK DATA
        todo: Factory.extend<Partial<TodoBody>>({
            get name() {
              //console.log(this.id)
              //faker.seed(Number(this.id))
              return `Listen to ${faker.music.genre()}`;
            },
            get status() {
              return TodoStatus.NotStarted;
            },
          }),
// END
      },
      seeds(server) {
// TODO: ADD MOCK DATA
          //server.schema.create('todo',{ name: "Go to Market" });
        //server.create("todo", { name: "Buy Cookies" });
        server.createList('todo', 3);
// END
      },
      routes() {
        this.get('/todos', (schema, request) => {
            return schema.all('todo');
          });
          this.post('/todos', (schema, request) => {
            let attrs = JSON.parse(request.requestBody);
            schema.create('todo', attrs);
            return schema.all('todo');
          });
          this.put('/todos/:key', (schema, request) => {
            let keyid = request.params.key;
            let attrs = JSON.parse(request.requestBody);
            schema.db.todos.update(keyid, attrs);
    
            //schema.find("todo", keyid).destroy();
            return schema.all('todo');
          });
          this.del('/todos/:key', (schema, request) => {
            let keyid = request.params.key;
            let post = schema.findBy('todo', { id: keyid });
            if (post !== null) {
              post.destroy();
            }
            return schema.all('todo');
          });
      },
    });
  }
```
