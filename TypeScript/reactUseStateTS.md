# React useState hook with TypeScript

아래의 예제를 보자
```tsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState<number>(0);

  const onIncrease = () => setCount(count + 1);
  const onDecrease = () => setCount(count - 1);

  return (
    <div>
      <h2>{count}</h2>
      <div>
        <button onClick={onIncrease}>+</button>
        <button onClick={onDecrease}>-</button>
      </div>
    </div>
  );
}
```

`useState<number>()` 같이 Generics를 사용해서 해당 상태가 무슨 타입인지 설정을 해주어도 된다. **그런데 `useState`를 사용할때 굳이 `Generics`를 사용하지 않아도 알아서 타입을 유추하기 때문에 굳이 사용 안해도 된다** =)
그럼 도대체 언제 `Generics`를 쓰는게 좋을까? - **바로 상태가 null 일 수도 있고 아닐 수도 있을때 사용하는게 best practice**
```tsx
type Information = { name: string; description: string };
const [info, setInformation] = useState<Information | null>(null);
```

#### 그럼 이번엔 input form 을 작성해보자.

##### [ MyForm.tsx ]
```tsx
import { useState } from "react";

type MyFormProps = {
  onSubmitHandler: (form: { name: string; description: string }) => void;
};

export default function MyForm({ onSubmitHandler }: MyFormProps) {
  const [form, setForm] = useState({
    name: "",
    description: "",
  });

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setForm({
      ...form,
      [name]: value,
    });
  };

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    onSubmitHandler(form); // call props callback
    setForm({
      name: "",
      description: "",
    }); // reset form
  };

  const { name, description } = form;
  return (
    <form onSubmit={handleSubmit}>
      <input name='name' value={name} onChange={onChange} />
      <input name='description' value={description} onChange={onChange} />
      <button type='submit'>register</button>
    </form>
  );
}
```

##### [ App.tsx ]
```tsx
import MyForm from "./MyForm";

const App: React.FC = () => {
  const handleSubmit = (form: { name: string; description: string }) => {
    console.log(form);
  };

  return (
    <>
      <MyForm onSubmitHandler={handleSubmit} />
    </>
  );
};

export default App;
```