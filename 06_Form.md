# Form trong React

## Introduction
HTML form elements work a little bit differently from other DOM elements in React, because form elements naturally keep some internal state.

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with `setState()`.

## 1. Giới thiệu: Form trong React khác gì HTML thuần?

### Trong HTML thuần:
- `<input>`, `<textarea>`, `<select>` tự quản lý state của chính nó.
- Khi bạn gõ vào input, giá trị (value) thay đổi trực tiếp trong DOM.

**Ví dụ HTML thuần:**
```html
<input type="text" />
```
Nếu bạn gõ chữ, trình duyệt sẽ tự động thay đổi giá trị trong input.

### Trong React:
- React khuyến khích quản lý state trong component, không để DOM tự quản lý.
- Vì vậy, bạn sẽ gắn giá trị (value) của form element vào state và cập nhật nó bằng `setState` (hoặc `useState` hook trong function component).
- Các input mà React quản lý gọi là **Controlled Components**.

**Ví dụ React controlled component:**
```jsx
import { useState } from "react";

function MyForm() {
  const [name, setName] = useState(""); // quản lý state trong component

  return (
    <div>
      <label>
        Your name:
        <input
          type="text"
          value={name}
          onChange={(e) => setName(e.target.value)} // cập nhật state
        />
      </label>
      <p>Hello, {name}!</p>
    </div>
  );
}
```
Ở đây:
- `value={name}` → React kiểm soát giá trị của input.
- `onChange={(e) => setName(e.target.value)}` → khi user gõ, state trong React thay đổi, React render lại input với giá trị mới.

## 2. Các thuật ngữ quan trọng
- **Controlled Component**: Form element mà `value` được điều khiển bởi React state.
- **Uncontrolled Component**: Form element để DOM tự quản lý state (dùng `defaultValue` thay vì `value`).
- **Event Handling**: Dùng `onChange`, `onSubmit`, v.v… để xử lý hành động user.
- **Two-way binding**: User thay đổi input → cập nhật state → React render lại input → hiển thị giá trị mới.

## 3. Case thực tế
**Ví dụ: Form đăng nhập**
```jsx
import { useState } from "react";

function LoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault(); // ngăn form reload trang
    console.log("Login with", { email, password });
    // Thực tế: gọi API login ở đây
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Email:
          <input
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
        </label>
      </div>
      <div>
        <label>Password:
          <input
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
        </label>
      </div>
      <button type="submit">Login</button>
    </form>
  );
}
```
- Khi user nhập email & password: state thay đổi theo từng ký tự → React cập nhật UI.
- Khi nhấn nút Login: React ngăn reload và bạn có thể xử lý logic (gọi API).

## 4. Bài tập điền code (fill in the blanks)
**Hãy hoàn thành component dưới đây để tạo một form nhập username và hiển thị "Hello, {username}":**
```jsx
import { useState } from "react";

function GreetingForm() {
  const [username, setUsername] = useState(""); // điền giá trị khởi tạo

  return (
    <div>
      <input
        type="text"
        value={username} // điền vào đây
        onChange={(e) => setUsername(e.target.value)} // điền vào đây
      />
      <p>Hello, {username}!</p> {/* điền vào đây */}
    </div>
  );
}

export default GreetingForm;
```

## 1. `<textarea>` trong React
### HTML thuần:
```html
<textarea>Xin chào</textarea>
```
Nội dung nằm giữa thẻ mở/đóng (children) chính là giá trị ban đầu.

### React:
Trong React, textarea hoạt động giống input → bạn dùng `value` + `onChange`.
```jsx
import { useState } from "react";

function MyTextarea() {
  const [message, setMessage] = useState("Xin chào");

  return (
    <div>
      <textarea
        value={message}
        onChange={(e) => setMessage(e.target.value)}
      />
      <p>Nội dung: {message}</p>
    </div>
  );
}
```
Lợi ích: Cách quản lý giống hệt input → dễ hiểu, dễ maintain.

## 2. `<select>` trong React
### HTML thuần:
```html
<select>
  <option value="chocolate">Chocolate</option>
  <option value="vanilla" selected>Vanilla</option>
  <option value="strawberry">Strawberry</option>
</select>
```
Ở đây, `selected` quyết định option nào được chọn.

### React:
Trong React, bạn quản lý bằng `value` ngay trên `select`:
```jsx
import { useState } from "react";

function MySelect() {
  const [flavor, setFlavor] = useState("vanilla");

  return (
    <div>
      <label>
        Chọn hương vị:
        <select value={flavor} onChange={(e) => setFlavor(e.target.value)}>
          <option value="chocolate">Chocolate</option>
          <option value="vanilla">Vanilla</option>
          <option value="strawberry">Strawberry</option>
        </select>
      </label>
      <p>Bạn chọn: {flavor}</p>
    </div>
  );
}
```
Ưu điểm: một chỗ duy nhất (`value`) quản lý trạng thái.

## 3. `<input type="file">` trong React
### HTML thuần:
```html
<input type="file" />
```
User chọn file, trình duyệt lưu file trong input (giá trị read-only).

### React:
Với file input, bạn không thể controlled hoàn toàn (không set được `value`).
Đây là **Uncontrolled component**.
Bạn lấy file từ `event.target.files`.
```jsx
function FileUpload() {
  const handleChange = (e) => {
    const file = e.target.files[0];
    console.log("Selected file:", file);
  };

  return (
    <div>
      <input type="file" onChange={handleChange} />
    </div>
  );
}
```
Thực tế: bạn sẽ dùng `FormData` để gửi file lên server.

## 4. Tổng kết
- `<input type="text">`, `<textarea>`, `<select>` → Controlled với `value` + `onChange`.
- `<input type="file">` → Uncontrolled (dùng `event.target.files`).

## 5. Case thực tế
**Ví dụ: Form đăng ký sản phẩm**
- Tên sản phẩm (input text)
- Mô tả (textarea)
- Danh mục (select)
- Hình ảnh (file input)

Với form như vậy:
- 3 field đầu controlled → dễ quản lý.
- Hình ảnh uncontrolled → xử lý riêng khi submit.

## 6. Bài tập điền code
**Hãy hoàn thành component này:**
```jsx
import { useState } from "react";

function ProductForm() {
  const [name, setName] = useState("");
  const [desc, setDesc] = useState("");
  const [category, setCategory] = useState("book");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Tên:", name);
    console.log("Mô tả:", desc);
    console.log("Danh mục:", category);
    // File sẽ được lấy ở fileInputRef.current.files[0]
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Tên sản phẩm:
          <input
            type="text"
            value={name} // điền vào
            onChange={(e) => setName(e.target.value)} // điền vào
          />
        </label>
      </div>

      <div>
        <label>
          Mô tả:
          <textarea
            value={desc} // điền vào
            onChange={(e) => setDesc(e.target.value)} // điền vào
          />
        </label>
      </div>

      <div>
        <label>
          Danh mục:
          <select
            value={category} // điền vào
            onChange={(e) => setCategory(e.target.value)} // điền vào
          >
            <option value="book">Book</option>
            <option value="toy">Toy</option>
            <option value="clothes">Clothes</option>
          </select>
        </label>
      </div>

      <div>
        <label>
          Hình ảnh:
          <input type="file" /> {/* không controlled */}
        </label>
      </div>

      <button type="submit">Đăng ký</button>
    </form>
  );
}

export default ProductForm;
```

## Controlled Input Null Value
Specifying the `value` prop on a controlled component prevents the user from changing the input unless you desire so. If you’ve specified a `value` but the input is still editable, you may have accidentally set `value` to `undefined` or `null`. The following code demonstrates this. (The input is locked at first but becomes editable after a short delay.)

## Form action
The built-in browser `<form>` component lets you create interactive controls for submitting information.
```html
<form action={search}>
  <input name="query" />
  <button type="submit">Search</button>
</form>
```

## Handling multiple submission types
When a user taps a specific button, the form is submitted, and a corresponding action, defined by that button’s attributes and action, is executed. For instance, a form might submit an article for review by default but have a separate button with `formAction` set to save the article as a draft.

## Other Alternatives to Controlled Components
It can sometimes be tedious to use controlled components, because you need to write an event handler for every way your data can change and pipe all of the input state through a React component. This can become particularly annoying when you are converting a pre-existing codebase to React, or integrating a React application with a non-React library. In these situations, you might want to check out uncontrolled components, an alternative technique for implementing input forms.

## Fully-Fledged Solutions
If you’re looking for a complete solution including validation, keeping track of the visited fields, and handling form submission, Formik is one of the popular choices. However, it is built on the same principles of controlled components and managing state — so don’t neglect to learn them.

## 1. Controlled Input với null hoặc undefined
Trong React, nếu bạn viết:
```jsx
<input value={null} />
```
hoặc
```jsx
<input value={undefined} />
```
Input sẽ bị khóa, vì React coi đây là controlled component nhưng lại không có giá trị hợp lệ để hiển thị.
Để input hoạt động, bạn cần chắc chắn rằng `value` luôn là một string (thường là `""` khi rỗng).

**Ví dụ lỗi thường gặp:**
```jsx
function BuggyInput() {
  const [text, setText] = React.useState(null);

  return (
    <input
      value={text}
      onChange={(e) => setText(e.target.value)}
    />
  );
}
```
Lúc đầu `value = null` → input khóa. Sau khi user gõ thì `setText("...")` mới hoạt động.

**Cách sửa:**
```jsx
const [text, setText] = useState(""); // khởi tạo luôn string rỗng
```

## 2. Form action (truyền thống trong HTML)
Trong HTML, bạn có thể viết:
```html
<form action="/search">
  <input name="query" />
  <button type="submit">Search</button>
</form>
```
Khi nhấn submit, trình duyệt gửi request GET tới `/search?query=....`.

### Trong React:
Bạn vẫn có thể dùng `action`, nhưng thường ta dùng `onSubmit` để ngăn reload trang và tự xử lý:
```jsx
function SearchForm() {
  const handleSubmit = (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    console.log("Query:", formData.get("query"));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="query" />
      <button type="submit">Search</button>
    </form>
  );
}
```

## 3. Handling multiple submission types
Một form có thể có nhiều nút với hành động khác nhau.

**Ví dụ:**
```html
<form action="/article">
  <input name="title" />
  <button type="submit">Submit for Review</button>
  <button type="submit" formAction="/draft">Save as Draft</button>
</form>
```
Nút đầu gửi đến `/article`, nút sau gửi đến `/draft`.
Trong React SPA, bạn thường quản lý việc này bằng `onClick` hoặc `onSubmit` có logic phân nhánh.

## 4. Alternatives to Controlled Components
Việc quản lý state cho mỗi input đôi khi hơi mệt (nhiều event handler, nhiều `useState`).

Khi:
- Convert codebase cũ sang React
- Dùng thư viện ngoài không thuần React

Bạn có thể dùng **Uncontrolled components** (dùng `defaultValue`, `ref` để đọc giá trị khi cần).

**Ví dụ Uncontrolled:**
```jsx
import { useRef } from "react";

function UncontrolledForm() {
  const inputRef = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Value:", inputRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input defaultValue="Hello" ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 5. Fully-Fledged Solutions (Formik, React Hook Form)
Trong dự án thực tế:
- **Formik**: phổ biến, hỗ trợ validation, quản lý touched fields, error messages.
- **React Hook Form**: nhẹ hơn, dùng ref + register inputs, tối ưu performance.

Dù sao, vẫn nên hiểu controlled component cơ bản, vì các thư viện cũng dựa trên nguyên lý đó.

## 6. Bài tập thực hành 🎯
**Hoàn thành component sau để xử lý nhiều loại submit khác nhau:**
- Nếu user nhấn "Search" → log "Searching for {query}".
- Nếu user nhấn "Lucky" → log "Feeling lucky with {query}".

```jsx
import { useState } from "react";

function MultiSubmitForm() {
  const [query, setQuery] = useState("");

  const handleSubmit = (e, type) => {
    e.preventDefault();
    if (type === "search") {
      console.log("Searching for", query); // điền vào
    } else if (type === "lucky") {
      console.log("Feeling lucky with", query); // điền vào
    }
  };

  return (
    <form>
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)} // điền vào
      />
      <button onClick={(e) => handleSubmit(e, "search")}>
        Search
      </button>
      <button onClick={(e) => handleSubmit(e, "lucky")}>
        I'm Feeling Lucky
      </button>
    </form>
  );
}

export default MultiSubmitForm;
```

## Controlled Component (có React quản lý state)
Input bị “điều khiển” bởi React state.

Bạn gõ gì → React nhận event → `setState` → React render lại input với `value` mới.

Tức là React luôn là single source of truth (nguồn dữ liệu duy nhất).

**Ví dụ:**
```jsx
import { useState } from "react";

function ControlledInput() {
  const [name, setName] = useState("");

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <p>Bạn đã nhập: {name}</p>
    </div>
  );
}
```
Ở đây:
- `value` gắn với state `name`.
- Gõ chữ nào → `onChange` → `setName` → React render lại input.
- Input hoàn toàn phụ thuộc vào React.

## Uncontrolled Component (DOM tự quản lý state)
Input tự quản lý giá trị bên trong nó (giống như trong HTML thuần).

Bạn không cần `useState`.

Khi cần lấy giá trị → bạn dùng `ref` để đọc từ DOM.

**Ví dụ:**
```jsx
import { useRef } from "react";

function UncontrolledInput() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    console.log("Giá trị nhập:", inputRef.current.value);
  };

  return (
    <div>
      <input defaultValue="Hello" ref={inputRef} />
      <button onClick={handleSubmit}>Lấy giá trị</button>
    </div>
  );
}
```
Ở đây:
- Input tự giữ giá trị.
- React không biết input đang có gì cho đến khi bạn query DOM qua `ref`.
- Cách này đơn giản hơn khi form lớn mà bạn không cần validate nhiều.

## Khi nào dùng cái nào?
### Controlled → Khi bạn muốn:
- Validate khi user nhập
- Hiển thị lỗi realtime
- Sync state với UI khác

### Uncontrolled → Khi:
- Form lớn, ít validate
- Convert code HTML cũ sang React
- Dùng với thư viện ngoài (jQuery plugin, Google Maps input, …)

Nói nôm na:
- Controlled = React cầm lái → bạn kiểm soát chặt chẽ.
- Uncontrolled = DOM tự chạy → bạn chỉ lấy giá trị khi cần.

## 1. Nguyên lý cốt lõi của React: One-way Data Flow
Trong React:
- State (hoặc props) chính là nguồn dữ liệu duy nhất (single source of truth).
- Khi state thay đổi → React render lại UI để đồng bộ với state.

Điều này có nghĩa: UI trong React không tự thay đổi → nó chỉ là kết quả render của state.

**Ví dụ nhỏ:**
```jsx
function App() {
  const [count, setCount] = React.useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
Ở đây:
- `count` nằm trong state.
- UI chỉ hiển thị lại khi state thay đổi.
- React không cho phép DOM tự thay đổi độc lập.

## 2. Vấn đề với form element
Trong HTML thuần, `<input>` hay `<textarea>` có state riêng trong DOM:
- Bạn gõ chữ → trình duyệt tự thay đổi giá trị trong input.
- React không biết gì cả, vì React chỉ render ra ban đầu.

Nếu ta để mặc cho DOM quản lý state của input → React và DOM có hai nguồn dữ liệu khác nhau (React state vs DOM state).
Điều này phá vỡ nguyên tắc "single source of truth".

## 3. Controlled Component ra đời
Để giữ cho dữ liệu luôn đi theo React state:
- React bắt input phải lấy `value` từ state.
- Khi user gõ → trigger `onChange` → gọi `setState` → React render lại → input hiển thị giá trị mới từ state.

Như vậy:
- React luôn biết input đang có gì.
- Không có chuyện DOM tự thay đổi độc lập.

**Ví dụ controlled:**
```jsx
function ControlledInput() {
  const [text, setText] = React.useState("");
  return (
    <input
      value={text}
      onChange={(e) => setText(e.target.value)}
    />
  );
}
```
**Cơ chế:**
- User gõ → input phát `onChange` event.
- React handler cập nhật state.
- State thay đổi → React render lại.
- `value={state}` → input hiển thị theo state.

Tức là: input chỉ là "cái màn hình hiển thị" của state, không tự quản lý giá trị.

## 4. Uncontrolled Component là gì?
Đôi khi, bạn không cần React quản lý từng ký tự → bạn để DOM quản lý (giống HTML thuần).
- Dùng `defaultValue` thay vì `value`.
- Dùng `ref` để đọc giá trị khi cần.

```jsx
function UncontrolledInput() {
  const inputRef = React.useRef(null);
  return (
    <>
      <input defaultValue="hello" ref={inputRef} />
      <button onClick={() => alert(inputRef.current.value)}>Lấy giá trị</button>
    </>
  );
}
```
Lúc này, React không can thiệp, chỉ mượn `ref` để đọc khi cần.

## 5. Tại sao thư viện như Formik hay React Hook Form vẫn dựa trên Controlled?
Bởi vì:
- Controlled đảm bảo React biết chính xác trạng thái form.
- Thư viện cần quản lý nhiều thứ:
  - Validation realtime (ngay khi gõ sai hiện lỗi).
  - “Touched” state (field nào đã điền vào).
  - Submit / Reset form chính xác.

Nếu không controlled → React sẽ “mù tịt” không biết field đang có gì → validation và sync dữ liệu sẽ rất khó.

**Ví dụ: Formik khi bạn khai báo:**
```jsx
<Field name="email" type="email" />
```
thì thực chất nó render ra một input controlled được nối với state trong Formik.

## 6. Tóm gọn tận gốc
- React có nguyên tắc: UI phải phản ánh state.
- Form elements trong HTML vốn tự có state riêng.
- Để thống nhất, React biến chúng thành Controlled Component: input chỉ là “cái gương” phản chiếu state trong React.
- Khi cần linh hoạt, bạn có thể dùng Uncontrolled, nhưng sẽ mất lợi thế validation và sync.
- Các thư viện form mạnh mẽ (Formik, React Hook Form,…) cũng chỉ là cách quản lý state và event của controlled components một cách tối ưu hơn.

## Ôn tập React Form
1. **Controlled Component**
- Input/textarea/select được React kiểm soát giá trị thông qua `value` và `onChange`.
- User gõ gì → React cập nhật state → React render lại UI.
- Single source of truth: state trong React là dữ liệu gốc, UI chỉ phản chiếu lại.

**Ví dụ:**
```jsx
<input value={name} onChange={(e) => setName(e.target.value)} />
```
2. **Uncontrolled Component**
- Input tự quản lý state trong DOM.
- React không kiểm soát → bạn chỉ lấy giá trị khi cần thông qua `ref`.

**Ví dụ:**
```jsx
<input defaultValue="Hello" ref={inputRef} />
```
3. **Các trường hợp đặc biệt**
- `textarea` trong React dùng `value` thay vì `children`.
- `select` trong React dùng `value` thay vì `selected`.
- `input type="file"` là uncontrolled (dùng `ref` hoặc `event.target.files`).
- Nếu `value={null}` hoặc `value={undefined}` → input bị khóa.

4. **Form Submission**
- Có thể dùng `onSubmit` để xử lý submit trong React.
- Có thể có nhiều nút với hành động khác nhau (`formAction`).

5. **Thư viện hỗ trợ**
- Formik, React Hook Form → giải quyết validation, quản lý nhiều field phức tạp.
- Nhưng chúng cũng dựa trên nguyên lý controlled component.

## 🍳 Ví von bằng góc nhìn bếp núc
**Controlled Component** giống như một nhà bếp hiện đại có bếp trưởng 👨‍🍳.
- Mọi nguyên liệu (state) được lưu trong kho lạnh trung tâm (React state).
- Khi khách gọi món (user gõ input), bếp trưởng ghi chú → cập nhật kho lạnh → sau đó lấy nguyên liệu chuẩn từ kho để nấu → món ăn đưa ra luôn chuẩn xác và đồng bộ.
- **Ưu điểm**: mọi thứ được quản lý chặt chẽ, dễ kiểm soát chất lượng.
- **Nhược điểm**: nhiều thủ tục, hơi tốn công khi chỉ nấu món nhỏ.

**Uncontrolled Component** giống như quán ăn vỉa hè 🍜.
- Mỗi đầu bếp giữ nguyên liệu riêng (DOM tự giữ value).
- Khi khách gọi món, bếp lấy nguyên liệu ngay tại chỗ và nấu liền.
- **Ưu điểm**: nhanh, ít thủ tục, dễ mở quán.
- **Nhược điểm**: khó kiểm soát đồng bộ (mỗi bếp có thể làm khác nhau, khó quản lý khi đông khách).

**Formik / React Hook Form** giống như nhà hàng lớn có hệ thống quản lý bếp 🍽️.
- Có phần mềm quản lý nguyên liệu, ai lấy gì, khách đã gọi món nào.
- Bếp trưởng vẫn dựa vào nguyên lý kho lạnh trung tâm (controlled), nhưng có thêm quy trình & công cụ để quản lý nhiều món cùng lúc mà không rối.

**Tóm lại:**
- Controlled = bếp trưởng trung tâm quản lý.
- Uncontrolled = bếp tự do, ai nấu nấy.
- Formik / React Hook Form = nhà hàng cao cấp có ERP quản lý toàn bộ.

## 1. React Controlled Form (cơ bản)
```jsx
import { useState } from "react";

export default function ControlledLoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!email.includes("@")) {
      alert("Email không hợp lệ");
      return;
    }
    if (password.length < 6) {
      alert("Mật khẩu ít nhất 6 ký tự");
      return;
    }
    console.log({ email, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Email:</label>
        <input
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </div>
      <div>
        <label>Password:</label>
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
      </div>
      <button type="submit">Login</button>
    </form>
  );
}
```
**Nhược điểm:**
- Bạn phải viết `useState` cho từng field.
- Validation phải code thủ công.

## 2. Formik (dựa trên controlled)
```jsx
import { Formik, Form, Field, ErrorMessage } from "formik";

export default function FormikLoginForm() {
  return (
    <Formik
      initialValues={{ email: "", password: "" }}
      validate={(values) => {
        const errors = {};
        if (!values.email.includes("@")) {
          errors.email = "Email không hợp lệ";
        }
        if (values.password.length < 6) {
          errors.password = "Mật khẩu ít nhất 6 ký tự";
        }
        return errors;
      }}
      onSubmit={(values) => {
        console.log(values);
      }}
    >
      <Form>
        <div>
          <label>Email:</label>
          <Field name="email" type="email" />
          <ErrorMessage name="email" component="div" style={{ color: "red" }} />
        </div>
        <div>
          <label>Password:</label>
          <Field name="password" type="password" />
          <ErrorMessage name="password" component="div" style={{ color: "red" }} />
        </div>
        <button type="submit">Login</button>
      </Form>
    </Formik>
  );
}
```
**Ưu điểm:**
- Quản lý nhiều field dễ dàng.
- Validation tập trung.
- Tự render lỗi (`<ErrorMessage>`).

## 3. React Hook Form (dựa trên uncontrolled + ref)
```jsx
import { useForm } from "react-hook-form";

export default function RHFLoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>Email:</label>
        <input
          {...register("email", {
            required: "Bắt buộc nhập email",
            pattern: { value: /.+@.+\..+/, message: "Email không hợp lệ" }
          })}
        />
        {errors.email && <div style={{ color: "red" }}>{errors.email.message}</div>}
      </div>

      <div>
        <label>Password:</label>
        <input
          type="password"
          {...register("password", {
            required: "Bắt buộc nhập password",
            minLength: { value: 6, message: "Ít nhất 6 ký tự" }
          })}
        />
        {errors.password && <div style={{ color: "red" }}>{errors.password.message}</div>}
      </div>

      <button type="submit">Login</button>
    </form>
  );
}
```
**Ưu điểm:**
- Tối ưu hiệu năng (vì dựa vào uncontrolled + ref, ít re-render).
- Validation khai báo trực tiếp trong `register`.
- Code gọn hơn so với controlled thuần.

**So sánh nhanh**
| Cách | Quản lý state | Validation | Ưu điểm | Nhược điểm |
|---|---|---|---|---|
| Controlled | `useState` cho từng field | Tự code trong `onSubmit` hoặc `onChange` | Dễ hiểu, thuần React | Nhiều field thì rườm rà |
| Formik | Controlled (state trong Formik) | `validate` hoặc schema (Yup) | Tổ chức tốt, dễ maintain | Hơi nặng |
| React Hook Form | Uncontrolled + `ref` | Khai báo ngay khi `register` | Nhẹ, nhanh, dễ dùng | API mới hơn, cần làm quen |

## Hướng dẫn cách xử lý khi form được submit
### Bước 1: Quản lý danh sách bài viết trong `App.js`
```jsx
import { useState } from "react";
import { BrowserRouter as Router, Routes, Route, useNavigate } from "react-router-dom";
import Home from "./Home";
import PostForm from "./PostForm";
import PostDetail from "./PostDetail";

export default function App() {
  const [posts, setPosts] = useState([]);

  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home posts={posts} />} />
        <Route path="/new" element={<PostForm setPosts={setPosts} />} />
        <Route path="/post/:id" element={<PostDetail posts={posts} />} />
      </Routes>
    </Router>
  );
}
```
**Giải thích:**
- `posts` là mảng chứa tất cả bài viết.
- `setPosts` truyền xuống `PostForm` để thêm bài viết mới.
- `PostDetail` hiển thị chi tiết theo `id`.

### Bước 2: Xử lý form trong `PostForm.js`
```jsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";

export default function PostForm({ setPosts }) {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();

    // Tạo object bài viết mới
    const newPost = {
      id: Date.now(), // id duy nhất (tạm thời dùng timestamp)
      title,
      content,
      date: new Date().toLocaleString(),
    };

    // Cập nhật vào mảng posts
    setPosts((prevPosts) => [...prevPosts, newPost]);

    // Điều hướng về trang chi tiết bài viết vừa tạo
    navigate(`/post/${newPost.id}`);
    // hoặc nếu muốn về trang chủ:
    // navigate("/");
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Tiêu đề:</label>
        <input value={title} onChange={(e) => setTitle(e.target.value)} />
      </div>
      <div>
        <label>Nội dung:</label>
        <textarea value={content} onChange={(e) => setContent(e.target.value)} />
      </div>
      <button type="submit">Đăng bài</button>
    </form>
  );
}
```
Ở đây:
- Khi nhấn submit:
  - Tạo object `newPost` có `id` và `date`.
  - Thêm vào state `posts` trong `App.js`.
  - Điều hướng sang trang chi tiết.

### Bước 3: Trang chi tiết `PostDetail.js`
```jsx
import { useParams } from "react-router-dom";

export default function PostDetail({ posts }) {
  const { id } = useParams();
  const post = posts.find((p) => p.id.toString() === id);

  if (!post) return <p>Bài viết không tồn tại</p>;

  return (
    <div>
      <h2>{post.title}</h2>
      <p><em>{post.date}</em></p>
      <p>{post.content}</p>
    </div>
  );
}
```

### Bước 4: Trang chủ hiển thị danh sách bài viết `Home.js`
```jsx
import { Link } from "react-router-dom";

export default function Home({ posts }) {
  return (
    <div>
      <h1>Danh sách bài viết</h1>
      <Link to="/new">+ Viết bài mới</Link>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <Link to={`/post/${post.id}`}>{post.title}</Link> - {post.date}
          </li>
        ))}
      </ul>
    </div>
  );
}
```
**Tổng kết luồng xử lý**
- User vào `/new` → điền form.
- Nhấn submit → tạo object `newPost`.
- Gọi `setPosts` để thêm vào state trong `App.js`.
- Điều hướng sang `/post/:id` để xem bài viết mới.

## Câu hỏi trắc nghiệm ôn tập
### 1. Nhận biết (Hiểu khái niệm cơ bản)
**Câu 1:** Trong React, điểm khác biệt chính giữa `<textarea>` trong HTML và React là gì?
A. React không hỗ trợ thẻ `<textarea>`
B. Trong React, `<textarea>` sử dụng `value` prop thay vì `children`
C. Trong HTML, `<textarea>` không thể thay đổi nội dung
D. React chỉ dùng `<input>` thay cho `<textarea>`

### 2. Thông hiểu (Hiểu ý nghĩa, giải thích được)
**Câu 2:** Tại sao việc sử dụng `value` prop trong Controlled Component giúp dễ quản lý form hơn?
A. Vì cho phép đồng bộ state của React với giá trị input
B. Vì tự động lưu dữ liệu vào database
C. Vì React tự thêm validation mặc định
D. Vì loại bỏ hoàn toàn sự kiện `onChange`

### 3. Vận dụng (Áp dụng trong tình huống cụ thể)
**Câu 3:** Cho đoạn code:
```jsx
<input value={name} />
```
Khi người dùng gõ vào ô input, nhưng không thấy nội dung hiển thị. Nguyên nhân phổ biến nhất là gì?
A. Input trong React không hiển thị giá trị
B. `value` đang được set thành `null` hoặc không có `onChange` cập nhật state
C. React mặc định chặn người dùng nhập dữ liệu
D. Do cần phải dùng `<textarea>` thay vì `<input>`

### 4. Vận dụng cao (Giải quyết tình huống thực tế, so sánh)
**Câu 4:** Một dự án cũ viết bằng jQuery có nhiều input form. Khi tích hợp vào React, để tránh viết lại quá nhiều handler `onChange`, bạn sẽ chọn giải pháp nào phù hợp nhất?
A. Controlled Component cho tất cả input
B. Chuyển toàn bộ code sang Formik ngay lập tức
C. Dùng Uncontrolled Component với `ref` để truy xuất giá trị khi cần
D. Không dùng form trong React nữa, giữ nguyên code jQuery

### 5. Vận dụng cao (Tích hợp giải pháp thư viện)
**Câu 5:** Khi cần một form với nhiều field, validation phức tạp (ví dụ: đăng ký tài khoản), giải pháp nào sau đây thường được khuyên dùng để giảm boilerplate code?
A. Controlled Component với `useState` cho từng input
B. Uncontrolled Component kết hợp `localStorage`
C. Thư viện Formik hoặc React Hook Form
D. Viết custom hook riêng nhưng không tái sử dụng

**Đáp án gợi ý:**
1. B
2. A
3. B
4. C
5. C

## Các keyword quan trọng trong Form React
### 1. Controlled Component
- `value` → Thuộc tính để gắn state vào input/textarea/select.
- `onChange` → Sự kiện bắt thay đổi và cập nhật state.
- `useState` → Hook lưu state cho từng input.
- "Single Source of Truth" → Input được điều khiển bởi React state, không tự giữ state riêng.

**Ví dụ:**
```jsx
const [name, setName] = useState("");
<input value={name} onChange={(e) => setName(e.target.value)} />
```
### 2. Uncontrolled Component
- `ref` → Dùng để truy cập trực tiếp giá trị input DOM.
- `defaultValue` → Dùng cho giá trị khởi tạo thay vì `value`.
- "DOM-driven" → Input tự quản lý state, React chỉ đọc khi cần.

**Ví dụ:**
```jsx
const inputRef = useRef();
<input ref={inputRef} defaultValue="Hello" />
<button onClick={() => console.log(inputRef.current.value)}>Log</button>
```
### 3. Các thẻ form đặc biệt trong React
- `<textarea>` → Dùng `value` thay vì `children`.
- `<select>` → Dùng `value` ở thẻ `<select>`, không dùng `selected` ở `<option>`.
- `<input type="file">` → Luôn là uncontrolled (giá trị read-only).

### 4. Form behavior
- `onSubmit` → Sự kiện submit của form.
- `e.preventDefault()` → Ngăn reload trang khi submit.
- `formAction` → Gán action riêng cho từng button trong form.
- `name` → Dùng để phân biệt field trong form submission.

### 5. Controlled Input Pitfalls
- `null` / `undefined` `value` → Làm input bị "khóa".
- "Controlled vs Uncontrolled warning" → Xảy ra khi input vừa có `value` vừa có `defaultValue`.

### 6. Giải pháp nâng cao
- **Formik** → Thư viện form dựa trên controlled component.
- **React Hook Form** → Thư viện form dựa trên uncontrolled + `ref` (tối ưu re-render).
- **Yup** → Thư viện schema validation hay dùng chung với Formik.

**Tóm tắt siêu ngắn (flashcard nhớ nhanh):**
- Controlled → `value`, `onChange`, `useState`.
- Uncontrolled → `ref`, `defaultValue`, File input.
- Special tags → `textarea(value)`, `select(value)`.
- Form behavior → `onSubmit`, `preventDefault`, `formAction`.
- Thư viện → Formik, React Hook Form, Yup.

## Bài văn: Form trong React và những khái niệm quan trọng
Trong lập trình web, form là cầu nối giữa người dùng và ứng dụng. Với HTML truyền thống, các phần tử như `<input>`, `<textarea>`, hay `<select>` thường tự quản lý trạng thái của chúng. Tuy nhiên, khi bước vào thế giới React, cách tiếp cận này thay đổi. React coi form như một phần trong “Single Source of Truth”, tức là tất cả dữ liệu phải được quản lý thông qua state. Từ đó, xuất hiện khái niệm Controlled Component.

### Controlled Component
Trong React, một input được gọi là Controlled khi giá trị hiển thị của nó được gắn trực tiếp từ state thông qua prop `value`. Khi người dùng thay đổi dữ liệu, sự kiện `onChange` được gọi để cập nhật lại state bằng `setState` hoặc `useState`. Nhờ vậy, giá trị hiển thị trên giao diện luôn phản ánh chính xác state hiện tại trong component. Ví dụ, một ô nhập tên sẽ được khai báo như sau:
```jsx
const [name, setName] = useState("");
<input value={name} onChange={(e) => setName(e.target.value)} />
```
Cách làm này giúp React kiểm soát hoàn toàn dữ liệu của form, dễ dàng xử lý validation, và đảm bảo tính đồng bộ. Tuy nhiên, nếu form quá lớn, việc viết handler cho từng field có thể trở nên rườm rà.

### Uncontrolled Component
Đối nghịch với Controlled là Uncontrolled Component. Ở đây, input vẫn tự giữ state bên trong DOM, còn React chỉ truy xuất khi cần thông qua `ref`. Ngoài ra, giá trị khởi tạo có thể đặt bằng `defaultValue`. Đặc biệt, `<input type="file">` trong React luôn là uncontrolled, bởi vì giá trị file được bảo mật và chỉ có thể lấy qua `ref`. Ví dụ:
```jsx
const inputRef = useRef();
<input ref={inputRef} defaultValue="Hello" />
<button onClick={() => console.log(inputRef.current.value)}>Log</button>
```
Cách này thuận lợi khi tích hợp với code cũ hoặc thư viện ngoài React, nhưng lại khó kiểm soát và validate theo thời gian thực.

### Các phần tử form đặc biệt
Trong React, một số phần tử form có cách xử lý riêng:
- Với `<textarea>`, thay vì viết nội dung giữa thẻ, bạn phải dùng prop `value`.
- Với `<select>`, thuộc tính `value` gắn ở thẻ `<select>` thay cho `selected` trong từng `<option>`.
- Với `<input type="file">`, luôn uncontrolled vì giá trị là read-only.

Những khác biệt này giúp tất cả các input trong React hoạt động theo một mô hình thống nhất.

### Hành vi của form
Một form trong React vẫn tuân theo hành vi cơ bản của HTML. Khi submit, ta thường gọi `onSubmit` và dùng `e.preventDefault()` để ngăn reload trang. Ngoài ra, mỗi nút trong form có thể định nghĩa hành động riêng thông qua `formAction`, ví dụ: một nút để submit bài viết, nút khác để lưu nháp.

### Những lưu ý và lỗi thường gặp
Một lỗi phổ biến trong Controlled Component là khi giá trị `value` bị gán `null` hoặc `undefined`, khiến input bị “khóa” và không nhập được dữ liệu. Ngoài ra, React sẽ cảnh báo nếu một input vừa có `value` (controlled) vừa có `defaultValue` (uncontrolled), vì điều này làm input “nửa nạc nửa mỡ”.

### Các giải pháp nâng cao
Để giảm sự phức tạp khi quản lý form lớn, cộng đồng React đã phát triển các thư viện chuyên dụng. Formik là một thư viện dựa trên Controlled Component, giúp tổ chức validation và quản lý nhiều field dễ dàng hơn. Trong khi đó, React Hook Form lại dựa trên Uncontrolled Component và `ref`, nhờ vậy tối ưu hiệu năng và giảm số lần re-render. Ngoài ra, Yup thường được dùng kèm để định nghĩa schema validation rõ ràng và mạnh mẽ.

### Kết luận
Tóm lại, việc hiểu rõ Controlled Component, Uncontrolled Component, cùng các đặc điểm đặc biệt của form trong React là nền tảng để bạn xây dựng ứng dụng tương tác mạnh mẽ và dễ bảo trì. Controlled giúp kiểm soát và đồng bộ dữ liệu, Uncontrolled giúp tích hợp nhanh và đơn giản, còn các thư viện hỗ trợ như Formik và React Hook Form mở rộng khả năng quản lý form trong những tình huống phức tạp.

## Ví dụ về `<textarea>` và `<select>` trong React
### 1. `<textarea>`
**HTML thuần**
```html
<textarea>
  Đây là nội dung mặc định
</textarea>
```
Nội dung ban đầu được đặt giữa cặp thẻ.

**React (Controlled Component)**
```jsx
import { useState } from "react";

export default function MyTextarea() {
  const [text, setText] = useState("Đây là nội dung mặc định");

  return (
    <textarea
      value={text}
      onChange={(e) => setText(e.target.value)}
    />
  );
}
```
Trong React, nội dung ban đầu được truyền qua prop `value`.

### 2. `<select>`
**HTML thuần**
```html
<select>
  <option value="apple">Táo</option>
  <option value="banana" selected>Chuối</option>
  <option value="orange">Cam</option>
</select>
```
Ở đây ta dùng `selected` trong `<option>` để chọn mặc định.

**React (Controlled Component)**
```jsx
import { useState } from "react";

export default function MySelect() {
  const [fruit, setFruit] = useState("banana");

  return (
    <select value={fruit} onChange={(e) => setFruit(e.target.value)}>
      <option value="apple">Táo</option>
      <option value="banana">Chuối</option>
      <option value="orange">Cam</option>
    </select>
  );
}
```
Trong React, chỉ định giá trị mặc định ở prop `value` của `<select>`, không cần dùng `selected` ở từng `<option>`.

**Tóm gọn:**
- HTML: nội dung `<textarea>` = `children`, `<option selected>` để chọn mặc định.
- React: `<textarea value="...">`, `<select value="...">`.

## Cách tạo form trong React (controlled components)
### Controlled Components là gì?
Trong React, Controlled Component là một input (như `<input>`, `<textarea>`, `<select>`) mà giá trị của nó được điều khiển bởi React state.
- Người dùng nhập → gọi `onChange` → cập nhật state
- State thay đổi → React render lại input với giá trị mới qua `value`

Input không còn “tự quản lý dữ liệu” như HTML thuần, mà React giữ toàn quyền.

### Các bước tạo Form Controlled Components trong React
1. **Tạo state để lưu dữ liệu form**
```jsx
const [username, setUsername] = useState("");
```
2. **Gắn state vào input qua `value`**
```jsx
<input value={username} />
```
3. **Cập nhật state khi người dùng nhập qua `onChange`**
```jsx
<input value={username} onChange={(e) => setUsername(e.target.value)} />
```
4. **Xử lý khi submit form qua `onSubmit`**
```jsx
const handleSubmit = (e) => {
  e.preventDefault(); // ngăn reload trang
  console.log("Username:", username);
};
```
### Ví dụ Form đơn giản (Controlled)
```jsx
import { useState } from "react";

export default function LoginForm() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Login với:", { username, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Username: </label>
        <input
          type="text"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
      </div>

      <div>
        <label>Password: </label>
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
      </div>

      <button type="submit">Đăng nhập</button>
    </form>
  );
}
```
### Một số điểm lưu ý
- Mọi input cần có state riêng (hoặc quản lý chung bằng object).
- Luôn dùng `value` + `onChange` để đảm bảo input controlled.
- Tránh để `value={null}` hoặc `undefined` → input sẽ bị khóa.
- Dễ dàng thêm validation trong `handleSubmit`.
