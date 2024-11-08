## 요약

## 내용
### Components, Props, State이 설명되는 이름을 작성한다.
> Use Descriptive Names for Components, Props, and State Variables

```javascript
export function Count({ count }) {
  return <div className="count">{count}</div>;
}
```
무엇을 위한 `Count`인지 알 수 없다.

```javascript
export function PagesReadDisplay({ pagesReadCount }) {
  return <div className="pagesRead">{pagesReadCount}</div>;
}
```
이는 조회수를 표현하는 카운트를 보여주는 컴포넌트로 이전의 코드와 다르게 설명이 되기 때문에 가독성이 좋아진다.

### 명확하고 간결한 코드를 쓴다.
> Write Clear and Concise Code

코드의 의도를 이해하기 어렵게 만드는 똑똑한 코드나 복잡한 로직은 피하고 명확하고 단순해야한다.
```javascript
type ApiTodo = {
  userId: number;
  id: number;
  title?: string;
  completed?: boolean;
};

type Todo = {
  userId: string;
  id: string;
  title: string;
  completed: boolean;
};

type JSONResponse = {
  data?: Array<ApiTodo>;
  errors?: Array<{ message: string }>;
};

export async function fetchData(): Promise<Todo[]> {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/todos"
  );
  
  const { data: apiTodos, errors }: JSONResponse = await response.json();

  if (response.ok && apiTodos) {
    return apiTodos
      .map((todo: ApiTodo) => ({
        userId: todo.userId.toString(),
        id: todo.id.toString(),
        title: todo.title || "",
        completed: todo.completed === true ? true : false,
      }))
      .sort((a, b) => a.title.localeCompare(b.title))
      .sort((a, b) => {
        if (a.completed && !b.completed) {
          return 1;
        } else if (!a.completed && b.completed) {
          return -1;
        } else {
         return 0;
        }
      });
  } else {
    throw new Error(errors?.[0].message || "Something went wrong");
  }
}
```
위 코드는 어렵다.
```javascript
type ApiTodo = {
  userId: number;
  id: number;
  title?: string;
  completed?: boolean;
};

type Todo = {
  userId: string;
  id: string;
  title: string;
  completed: boolean;
};

type JSONResponse = {
  data?: Array<ApiTodo>;
  errors?: Array<{ message: string }>;
};

export async function fetchTodos(): Promise<Todo[]> {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/todos"
  );
  const { data: apiTodos, errors }: JSONResponse = await response.json();
  if (response.ok && apiTodos) {
    const todos = apiTodos.map(transformApiTodo);
    return sortTodosByIncomplete(sortTodosByAlphabetical(todos));
  } else {
    throw new Error(errors?.[0].message || "Something went wrong");
  }
}

// The following can move to a 'utils' directory and can be reused elsewhere

function transformApiTodo(apiTodo: ApiTodo): Todo {
  return {
    userId: apiTodo.userId.toString(),
    id: apiTodo.id.toString(),
    title: apiTodo.title || "",
    completed: apiTodo.completed === true ? true : false,
  };
}

function sortTodosByAlphabetical(todos: Todo[]): Todo[] {
  return todos.sort((a, b) => a.title.localeCompare(b.title));
}

function sortTodosByIncomplete(todos: Todo[]): Todo[] {
  return todos.sort((a, b) => {
    if (a.completed && !b.completed) {
      return 1;
    } else if (!a.completed && b.completed) {
      return -1;
    } else {
      return 0;
    }
  });
}
```
48줄에서 58줄로 늘어났지만, 구체적이고 이름도 잘 지어진 함수가 생겨서 감수가 가능하고 재사용이 가능하다. 만약 코드가 어렵다면 함수가 너무 많은 것을 처리하고 있기 때문이다.

### 컴포넌트를 작고 집중적으로 유지한다.
> Keep Components Small and Focused

컴포넌트는 다른 형태의 함수일 뿐이다. 더 작고 구체적인 컴포넌트로 세분화하는 것이 필요하다. 가이드라인은 다음과 같습니다.
- 컴포넌트 파일이 100줄보다 긴가?
- 컴포넌트가 3개 이상의 상태 값을 추적하는가?
- 컴포넌트에 여러개의 useEffect가 필요한가?
위 가이드라인에 걸린다면 컴포넌트를 분할하는 것을 추천한다.

### 일관된 코드 포맷을 유지한다.
> Format Your Code Consistently

Prettier를 사용하는 것을 추천한다.

### 코드에 주석을 달아라.
> Comment Your Code

코드에 주석을 달면 가독성을 높이는 효과적인 방법이 될 수 있다.
코드가 수행하는 작업, 특정 결정을 내린 이유, 오류 케이스를 설명해라.

## 참고
- [5 Quick Tips for Writing More Readable React Code [ChatGPT Experiment]](https://www.bitovi.com/blog/5-quick-tips-for-writing-more-readable-react-code-chatgpt-experiment)