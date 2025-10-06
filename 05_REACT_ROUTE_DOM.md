# React Router DOM (Điều hướng giữa các trang)

## Lý thuyết:
- Cài đặt React Router DOM.
- Thiết lập BrowserRouter, Routes, Route.
- Sử dụng Link component để điều hướng.
- Dynamic routes (`/post/:id`) và `useParams`.

## Built-in React DOM Hooks
Gói `react-dom` chứa các Hooks chỉ được hỗ trợ cho các ứng dụng web (chạy trong môi trường DOM của trình duyệt). Các Hooks này không được hỗ trợ trong các môi trường không phải trình duyệt như ứng dụng iOS, Android hoặc Windows.

### Form Hooks
`Forms` cho phép bạn tạo các điều khiển tương tác để gửi thông tin. Để quản lý biểu mẫu trong các component của bạn, hãy sử dụng một trong các Hooks sau:
- `useFormStatus` cho phép bạn cập nhật UI dựa trên trạng thái của một biểu mẫu.

```javascript
function Form({ action }) {
  async function increment(n) {
    return n + 1;
  }
  const [count, incrementFormAction] = useActionState(increment, 0);
  return (
    <form action={action}>
      <button formAction={incrementFormAction}>Count: {count}</button>
      <Button />
    </form>
  );
}

function Button() {
  const { pending } = useFormStatus();
  return (
    <button disabled={pending} type="submit">
      Submit
    </button>
  );
}
```

## React DOM APIs
Gói `react-dom` chứa các phương thức chỉ được hỗ trợ cho các ứng dụng web (chạy trong môi trường DOM của trình duyệt). Chúng không được hỗ trợ cho React Native.

### APIs
Các API này có thể được import từ các component của bạn. Chúng hiếm khi được sử dụng:
- `createPortal` cho phép bạn render các component con vào một phần khác của cây DOM.
- `flushSync` cho phép bạn buộc React flush một bản cập nhật trạng thái và cập nhật DOM đồng bộ.

### Resource Preloading APIs
Các API này có thể được sử dụng để làm cho các ứng dụng nhanh hơn bằng cách tải trước các tài nguyên như script, stylesheet và font ngay khi bạn biết mình cần chúng, ví dụ trước khi điều hướng đến một trang khác nơi các tài nguyên sẽ được sử dụng.
Các framework dựa trên React thường xử lý việc tải tài nguyên cho bạn, vì vậy bạn có thể không cần gọi các API này. Tham khảo tài liệu của framework của bạn để biết chi tiết.
- `prefetchDNS` cho phép bạn prefetch địa chỉ IP của một tên miền DNS mà bạn mong đợi sẽ kết nối.
- `preconnect` cho phép bạn kết nối với một máy chủ mà bạn mong đợi sẽ yêu cầu tài nguyên, ngay cả khi bạn chưa biết bạn sẽ cần tài nguyên nào.
- `preload` cho phép bạn fetch một stylesheet, font, hình ảnh hoặc script bên ngoài mà bạn mong đợi sẽ sử dụng.
- `preloadModule` cho phép bạn fetch một module ESM mà bạn mong đợi sẽ sử dụng.
- `preinit` cho phép bạn fetch và đánh giá một script bên ngoài hoặc fetch và chèn một stylesheet.
- `preinitModule` cho phép bạn fetch và đánh giá một module ESM.

### Entry points
Gói `react-dom` cung cấp hai điểm vào bổ sung:
- `react-dom/client` chứa các API để render các component React trên client (trong trình duyệt).
- `react-dom/server` chứa các API để render các component React trên server.

### Removed APIs
Các API này đã bị xóa trong React 19:
- `findDOMNode`: xem các lựa chọn thay thế.
- `hydrate`: sử dụng `hydrateRoot` thay thế.
- `render`: sử dụng `createRoot` thay thế.
- `unmountComponentAtNode`: sử dụng `root.unmount()` thay thế.
- `renderToNodeStream`: sử dụng các API `react-dom/server` thay thế.
- `renderToStaticNodeStream`: sử dụng các API `react-dom/server` thay thế.

## Client React DOM APIs
Các API `react-dom/client` cho phép bạn render các component React trên client (trong trình duyệt). Các API này thường được sử dụng ở cấp cao nhất của ứng dụng để khởi tạo cây React của bạn. Một framework có thể gọi chúng cho bạn. Hầu hết các component của bạn không cần import hoặc sử dụng chúng.

### Client APIs
- `createRoot` cho phép bạn tạo một root để hiển thị các component React bên trong một node DOM của trình duyệt.
- `hydrateRoot` cho phép bạn hiển thị các component React bên trong một node DOM của trình duyệt mà nội dung HTML của nó đã được tạo trước đó bởi `react-dom/server`.

### Browser support
React hỗ trợ tất cả các trình duyệt phổ biến, bao gồm Internet Explorer 9 trở lên. Một số polyfill được yêu cầu cho các trình duyệt cũ hơn như IE 9 và IE 10.

## Server React DOM APIs
Các API `react-dom/server` cho phép bạn server-side render các component React sang HTML. Các API này chỉ được sử dụng trên server ở cấp cao nhất của ứng dụng để tạo HTML ban đầu. Một framework có thể gọi chúng cho bạn. Hầu hết các component của bạn không cần import hoặc sử dụng chúng.

### Server APIs for Node.js Streams
Các phương thức này chỉ có sẵn trong các môi trường có Node.js Streams:
- `renderToPipeableStream` render một cây React sang một Node.js Stream có thể pipe được.

### Server APIs for Web Streams
Các phương thức này chỉ có sẵn trong các môi trường có Web Streams, bao gồm trình duyệt, Deno và một số runtime edge hiện đại:
- `renderToReadableStream` render một cây React sang một Readable Web Stream.

### Legacy Server APIs for non-streaming environments
Các phương thức này có thể được sử dụng trong các môi trường không hỗ trợ stream:
- `renderToString` render một cây React sang một chuỗi.
- `renderToStaticMarkup` render một cây React không tương tác sang một chuỗi.

## Static React DOM APIs
Các API `react-dom/static` cho phép bạn tạo HTML tĩnh cho các component React. Chúng có chức năng hạn chế so với các API streaming. Một framework có thể gọi chúng cho bạn. Hầu hết các component của bạn không cần import hoặc sử dụng chúng.

### Static APIs for Web Streams
Các phương thức này chỉ có sẵn trong các môi trường có Web Streams, bao gồm trình duyệt, Deno và một số runtime edge hiện đại:
- `prerender` render một cây React sang HTML tĩnh với một Readable Web Stream.

### Static APIs for Node.js Streams
Các phương thức này chỉ có sẵn trong các môi trường có Node.js Streams:
- `prerenderToNodeStream` render một cây React sang HTML tĩnh với một Node.js Stream.

---

# 1. React Router DOM – Điều hướng giữa các trang

## Lý thuyết cơ bản
React Router DOM là thư viện giúp SPA (Single Page Application) có thể điều hướng giữa các trang mà không cần reload toàn bộ trình duyệt.

### Cài đặt
```bash
npm install react-router-dom
```

### Thiết lập cấu trúc cơ bản
- `BrowserRouter`: Bọc toàn bộ app, để React Router quản lý history của trình duyệt.
- `Routes`: Nơi khai báo tập hợp các route.
- `Route`: Khai báo từng đường dẫn.
- `Link`: Điều hướng mà không reload trang.
- `useParams`: Lấy tham số động từ URL.

## Ví dụ minh họa
```javascript
import { BrowserRouter, Routes, Route, Link, useParams } from "react-router-dom";

function Home() {
  return <h2>Trang chủ</h2>;
}

function About() {
  return <h2>Giới thiệu</h2>;
}

function PostDetail() {
  const { id } = useParams(); // lấy id từ URL
  return <h2>Bài viết số {id}</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Trang chủ</Link> |
        <Link to="/about">Giới thiệu</Link> |
        <Link to="/post/101">Bài viết 101</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/post/:id" element={<PostDetail />} />
      </Routes>
    </BrowserRouter>
  );
}
```
**Điểm hay:** Khi bạn click link → React Router chỉ render lại component tương ứng, không tải lại toàn bộ trang như HTML truyền thống.

## Case thực tế
- Blog: `/post/:id` để hiển thị chi tiết bài viết.
- E-commerce: `/product/:id` để hiển thị chi tiết sản phẩm.
- Dashboard: `/user/:id/profile` để hiển thị hồ sơ từng người.

## Bài tập điền code (Practice)
Hoàn thiện đoạn code sau để tạo ứng dụng có 3 trang: Home, Contact, UserDetail (hiển thị userId từ URL).

```javascript
import { BrowserRouter, Routes, Route, Link, useParams } from "react-router-dom";

function Home() {
  return <h2>Home Page</h2>;
}

function Contact() {
  return <h2>Contact Page</h2>;
}

// TODO: Tạo component UserDetail dùng useParams để lấy userId từ URL
function UserDetail() {
  // ❓ điền vào đây
  return <h2>User ID: { /* ❓ in userId ra */ }</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        {/* ❓ thêm link để điều hướng đến Home, Contact, và UserDetail với userId=99 */}
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/contact" element={<Contact />} />
        {/* ❓ thêm route cho UserDetail */}
      </Routes>
    </BrowserRouter>
  );
}
```

---

# Mini Project Blog với React Router DOM v6

## 6.1 Cài đặt Router
Cài thư viện `react-router-dom` phiên bản 6:

```bash
npm install react-router-dom@6
```

## 6.2 Thiết lập các Route
Giả sử chúng ta có cấu trúc:

```
src/
 ├── App.js
 ├── components/
 │    ├── Header.js
 │    ├── PostCard.js
 │    └── PostDetail.js
 ├── pages/
 │    ├── Home.js
 │    └── About.js
 └── data.js
```

### App.js
```javascript
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Header from "./components/Header";
import Home from "./pages/Home";
import About from "./pages/About";
import PostDetail from "./components/PostDetail";

function App() {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="/" element={<Home />} />           {/* Trang chủ */}
        <Route path="/about" element={<About />} />     {/* Giới thiệu */}
        <Route path="/post/:id" element={<PostDetail />} /> {/* Chi tiết bài viết */}
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

## 6.3 Tạo component PostDetail

### data.js (danh sách bài viết demo)
```javascript
export const posts = [
  {
    id: 1,
    title: "Giới thiệu React Router DOM",
    author: "IT 日本語先生",
    date: "2025-09-26",
    content: "React Router DOM giúp điều hướng SPA mà không cần reload trang."
  },
  {
    id: 2,
    title: "Hook useParams trong React",
    author: "Frontend Mentor",
    date: "2025-09-20",
    content: "useParams cho phép bạn lấy giá trị từ URL, rất hữu ích với route động."
  }
];
```

### PostDetail.js
```javascript
import { useParams } from "react-router-dom";
import { posts } from "../data";

function PostDetail() {
  const { id } = useParams();
  const post = posts.find((p) => p.id === parseInt(id));

  if (!post) {
    return <h2>Không tìm thấy bài viết</h2>;
  }

  return (
    <div>
      <h2>{post.title}</h2>
      <p><strong>Tác giả:</strong> {post.author}</p>
      <p><strong>Ngày:</strong> {post.date}</p>
      <p>{post.content}</p>
    </div>
  );
}

export default PostDetail;
```

## 6.4 Tích hợp Link

### Header.js
```javascript
import { Link } from "react-router-dom";

function Header() {
  return (
    <nav>
      <Link to="/">Trang chủ</Link> |{" "}
      <Link to="/about">Giới thiệu</Link>
    </nav>
  );
}

export default Header;
```

### PostCard.js
```javascript
import { Link } from "react-router-dom";

function PostCard({ post }) {
  return (
    <div>
      <h3>{post.title}</h3>
      <p>Tác giả: {post.author}</p>
      <p>Ngày: {post.date}</p>
      {/* Dùng Link thay vì <a> */}
      <Link to={`/post/${post.id}`}>Đọc thêm</Link>
    </div>
  );
}

export default PostCard;
```

### Home.js
```javascript
import { posts } from "../data";
import PostCard from "../components/PostCard";

function Home() {
  return (
    <div>
      <h2>Danh sách bài viết</h2>
      {posts.map((p) => (
        <PostCard key={p.id} post={p} />
      ))}
    </div>
  );
}

export default Home;
```

### About.js
```javascript
function About() {
  return (
    <div>
      <h2>Về Blog này</h2>
      <p>Đây là mini project demo sử dụng React Router DOM v6.</p>
    </div>
  );
}

export default About;
```

**Kết quả:**
- `/` → Danh sách bài viết.
- `/about` → Trang giới thiệu.
- `/post/:id` → Hiển thị chi tiết bài viết theo id.
- Điều hướng mượt mà với Link mà không reload trang.

---

# Trắc nghiệm ôn tập React Router DOM

## Câu hỏi cơ bản
1. Thư viện `react-router-dom` được dùng để làm gì trong ứng dụng React?
   a) Quản lý state toàn cục
   b) Quản lý điều hướng giữa các trang trong SPA
   c) Kết nối với API backend
   d) Tối ưu hóa performance

2. Trong `react-router-dom@6`, component nào cần bọc toàn bộ ứng dụng để sử dụng router?
   a) `<Routes>`
   b) `<RouterProvider>`
   c) `<BrowserRouter>`
   d) `<Switch>`

3. Đâu là cú pháp đúng để định nghĩa một route trong phiên bản 6?
   a) `<Route path="/" component={Home} />`
   b) `<Route path="/" element={<Home />} />`
   c) `<Route exact path="/" element={Home} />`
   d) `<Route to="/" element={<Home />} />`

4. Hook `useParams()` trong React Router có tác dụng gì?
   a) Lấy tham số từ query string (vd: `?page=2`)
   b) Lấy tham số động từ URL (vd: `/post/:id`)
   c) Lấy trạng thái hiện tại của form
   d) Lấy trạng thái login của user

5. Trong Mini Project Blog, khi muốn điều hướng đến `/post/1` mà không reload trang, bạn nên dùng gì?
   a) `<a href="/post/1">`
   b) `<Link to="/post/1">`
   c) `window.location.href="/post/1"`
   d) `<Redirect to="/post/1">`

6. Trong React Router v6, component nào thay thế cho `<Switch>` ở phiên bản cũ?
   a) `<Routes>`
   b) `<Navigate>`
   c) `<Router>`
   d) `<Outlet>`

7. Khi bạn muốn hiển thị nội dung chi tiết bài viết theo id, bạn cần làm gì trong `PostDetail`?
   a) Dùng `useLocation()` để lấy `pathname`
   b) Dùng `useParams()` để lấy `id` từ URL
   c) Dùng `useNavigate()` để redirect
   d) Dùng `useHistory()` để lấy state

8. Đoạn code nào đúng để tạo navigation bar trong `Header.js`?
   a)
   ```html
   <a href="/">Home</a> | <a href="/about">About</a>
   ```
   b)
   ```javascript
   <Link to="/">Home</Link> | <Link to="/about">About</Link>
   ```
   c)
   ```javascript
   <NavLink path="/">Home</NavLink> | <NavLink path="/about">About</NavLink>
   ```
   d)
   ```javascript
   <Button to="/">Home</Button>
   ```

9. Trong React Router v6, khi không tìm thấy route phù hợp, bạn nên làm gì?
   a) Không cần xử lý, React Router tự hiển thị lỗi
   b) Tạo một `<Route path="*">` để hiển thị trang 404
   c) Dùng `useNavigate` để redirect về `/`
   d) Tạo file `404.html` riêng biệt

10. Giả sử bạn có `Route path="/post/:id"`, khi người dùng truy cập `/post/10`, giá trị trả về của `useParams()` sẽ là gì?
    a) `{ post: "10" }`
    b) `{ id: 10 }`
    c) `{ id: "10" }`
    d) `["10"]`

---

# SPA (Single Page Application) và MPA (Multi Page Application)

## SPA (Single Page Application)
### Đặc điểm chính
- Chỉ có một file HTML chính (thường là `index.html`).
  - Toàn bộ UI được render động bằng JavaScript (React, Vue, Angular…).
- Điều hướng không reload trang:
  - Khi chuyển từ `/home` sang `/about`, browser không tải lại toàn bộ HTML/CSS/JS, chỉ thay đổi phần nội dung cần thiết.
- Trải nghiệm người dùng mượt mà:
  - Vì không reload, nên cảm giác giống như app native.
- Phụ thuộc nhiều vào JS:
  - Nếu tắt JavaScript, ứng dụng gần như không hoạt động.
- SEO khó hơn (cần server-side rendering hoặc prerendering).

## MPA (Multi Page Application)
### Đặc điểm chính
- Mỗi trang là một file HTML riêng (ví dụ: `home.html`, `about.html`).
- Khi người dùng click link → Browser sẽ gửi request mới lên server → Server trả về trang HTML hoàn chỉnh.
- Mỗi lần chuyển trang là một lần reload:
  - Trình duyệt tải lại HTML, CSS, JS mới.
- SEO tốt hơn vì nội dung tĩnh đã có sẵn trên mỗi trang.
- Trải nghiệm chậm hơn (mỗi lần load trang tốn thời gian).
- Ít phụ thuộc JS, ứng dụng vẫn chạy được nếu tắt JS.

## So sánh SPA vs MPA

| Tiêu chí      | SPA (Single Page App)                 | MPA (Multi Page App)                  |
|---------------|---------------------------------------|---------------------------------------|
| Reload trang  | Không reload, chỉ thay đổi phần nội dung cần | Reload toàn bộ trang                  |
| Tốc độ UX     | Mượt, nhanh (giống app mobile)        | Chậm hơn, do load lại nhiều tài nguyên |
| SEO           | Khó SEO (cần SSR hoặc Next.js)        | SEO tốt hơn, nội dung có sẵn          |
| Cấu trúc      | Một `index.html`, JS điều khiển routing | Nhiều file HTML độc lập               |
| Phụ thuộc JS  | Cao                                   | Thấp                                  |
| Ví dụ         | Facebook, Gmail, Twitter (modern)     | Wikipedia, các website truyền thống    |

**Kết luận:**
- Nếu bạn làm ứng dụng web động (dashboard, blog, social network, e-commerce frontend) → SPA hợp lý.
- Nếu bạn cần website nội dung tĩnh + SEO mạnh (báo, tin tức, giới thiệu công ty) → MPA hoặc kết hợp (hybrid) hợp lý.

---

# Quy trình định nghĩa và sử dụng Route (React Router v6)

## 1. Cài đặt thư viện
```bash
npm install react-router-dom@6
```

## 2. Bọc ứng dụng với `<BrowserRouter>`
Toàn bộ ứng dụng cần đặt trong `BrowserRouter` để React Router quản lý history.

```javascript
import { BrowserRouter } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      {/* các Route ở đây */}
    </BrowserRouter>
  );
}
```

## 3. Khai báo `<Routes>` và `<Route>`
- `Routes`: nơi chứa nhiều route.
- `Route`: định nghĩa đường dẫn và component tương ứng.

**Cú pháp đúng ở React Router v6 là:**
```javascript
<Route path="/duong-dan" element={<TenComponent />} />
```
**Lưu ý:** Không dùng `component={}` như v5 nữa, mà phải dùng `element={<Component />}`.

## 4. Điều hướng bằng `<Link>`
Thay vì dùng `<a>`, ta dùng `<Link>` để không reload toàn trang.

```javascript
import { Link } from "react-router-dom";

<Link to="/about">Giới thiệu</Link>
```

## 5. Dynamic Route + useParams
Để tạo route động như `/post/:id`, ta dùng `useParams()` để lấy giá trị từ URL.

```javascript
import { useParams } from "react-router-dom";

function PostDetail() {
  const { id } = useParams();
  return <h2>Bạn đang xem bài viết số {id}</h2>;
}
```

## Ví dụ hoàn chỉnh
```javascript
import { BrowserRouter, Routes, Route, Link, useParams } from "react-router-dom";

function Home() {
  return <h2>Trang chủ</h2>;
}

function About() {
  return <h2>Giới thiệu</h2>;
}

function PostDetail() {
  const { id } = useParams();
  return <h2>Bài viết số {id}</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Trang chủ</Link> |{" "}
        <Link to="/about">Giới thiệu</Link> |{" "}
        <Link to="/post/123">Bài viết 123</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/post/:id" element={<PostDetail />} />
      </Routes>
    </BrowserRouter>
  );
}
```
**Tóm lại:**
Câu hỏi trắc nghiệm bạn đưa ra:
Đâu là cú pháp đúng để định nghĩa một route trong phiên bản 6?
**Đáp án đúng:** b) `<Route path="/" element={<Home />} />`

---

# React Router DOM – Keywords chính

- `BrowserRouter`: Bọc toàn bộ ứng dụng, cho phép quản lý history bằng HTML5 history API.
- `Routes`: Container để nhóm nhiều `<Route>`. (v6 thay thế `<Switch>` của v5).
- `Route`: Định nghĩa đường dẫn (`path`) và component hiển thị (`element`).
- `Link`: Dùng để điều hướng không reload trang (thay cho `<a>`).
- `NavLink`: Giống `Link` nhưng cho phép thêm style/class active khi route khớp.
- `useParams`: Hook lấy tham số động từ URL (ví dụ `/post/:id`).
- `useNavigate`: Hook để điều hướng bằng code (giống `history.push` ở v5).
- `Outlet`: Nơi hiển thị các route con (nested routes).
- `Navigate`: Component dùng để redirect từ một route này sang route khác.
- `useLocation`: Hook lấy thông tin URL hiện tại (`pathname`, `search`, `hash`).

**Ví dụ nhanh minh họa:**
```javascript
<Route path="/post/:id" element={<PostDetail />} />
// -> dùng useParams() để lấy id
```

---

# Vì sao cần nhóm Route?

Trong React Router v6:
- `<Routes>` đóng vai trò “nhóm quản lý” các route.
- Bên trong `<Routes>` ta có thể định nghĩa nhiều `<Route>`.

## Lý do cần nhóm:
- **Rõ ràng, có tổ chức:** Tách biệt tập hợp route với các thành phần khác.
- **Điều khiển logic khớp route:** Trong v6, chỉ `<Routes>` mới biết cách chọn ra 1 Route phù hợp nhất để render.
- **Hỗ trợ nested routes:** Giúp ta tổ chức route cha – con gọn gàng. Ví dụ: `/users` (danh sách) và `/users/:id` (chi tiết).
- **Quản lý redirect/404:** Ta có thể thêm một route `path="*"` cuối cùng trong `<Routes>` để xử lý trang không tồn tại.

## Use Case thực tế

### 1. Trang web nhiều mục (multi-section)
Ví dụ blog:
```javascript
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/post/:id" element={<PostDetail />} />
  <Route path="*" element={<NotFound />} />
</Routes>
```
Nếu không có nhóm `<Routes>`, ta sẽ không thể quản lý đồng bộ và xử lý “chỉ một route khớp” như trên.

### 2. Nested Routes (Route lồng nhau)
Ví dụ: Dashboard có layout chung (menu, header) và các trang con bên trong.
```javascript
<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route path="home" element={<DashboardHome />} />
    <Route path="profile" element={<UserProfile />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```
- `DashboardLayout` hiển thị khung layout.
- Các route con (home, profile, settings) sẽ render vào `<Outlet />`.

### 3. Redirect / Điều hướng đặc biệt
Ví dụ: Người chưa login thì chuyển về `/login`.
```javascript
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/login" element={<Login />} />
  <Route path="/dashboard" element={<RequireAuth><Dashboard /></RequireAuth>} />
</Routes>
```
`<Routes>` giúp gom logic điều hướng phức tạp thành 1 khối.

**Kết luận:**
Ta cần nhóm Route trong `<Routes>` để React Router v6 hoạt động đúng và gọn gàng.
Các use case phổ biến: trang blog, e-commerce, dashboard, nested routes, auth redirect, trang 404.

---

# Link vs NavLink

## 1. Link
- Dùng để điều hướng không reload trang (SPA navigation).
- Không tự động thêm class/style khi đang ở route đó.

### Ví dụ:
```javascript
import { Link } from "react-router-dom";

function Menu() {
  return (
    <nav>
      <Link to="/">Trang chủ</Link> |
      <Link to="/about">Giới thiệu</Link>
    </nav>
  );
}
```
Nếu bạn đang ở `/about`, thì chữ "Giới thiệu" cũng không được highlight.

## 2. NavLink
- Giống Link, nhưng có thêm khả năng tự động áp dụng style hoặc class khi route khớp.
- Dùng nhiều trong menu, navbar, sidebar.

### Ví dụ:
```javascript
import { NavLink } from "react-router-dom";

function Menu() {
  return (
    <nav>
      <NavLink
        to="/"
        className={({ isActive }) => (isActive ? "active" : "")}
      >
        Trang chủ
      </NavLink> |

      <NavLink
        to="/about"
        className={({ isActive }) => (isActive ? "active" : "")}
      >
        Giới thiệu
      </NavLink>
    </nav>
  );
}
```
### CSS:
```css
.active {
  font-weight: bold;
  color: red;
}
```
Khi bạn ở `/about`, link "Giới thiệu" sẽ có màu đỏ và in đậm.

## So sánh nhanh

| Tiêu chí        | Link                               | NavLink                                   |
|-----------------|------------------------------------|-------------------------------------------|
| Điều hướng      | ✅ Có                              | ✅ Có                                     |
| Reload trang    | ❌                                 | ❌                                        |
| Highlight active| ❌                                 | ✅ Tự động thêm class/style khi route khớp |
| Use case chính  | Nút điều hướng bình thường         | Navbar, menu, sidebar                     |

## Demo thực tế
- Khi bạn muốn làm menu navigation thì nên dùng `NavLink`:
```javascript
<NavLink to="/products" className={({ isActive }) => isActive ? "active" : ""}>
  Sản phẩm
</NavLink>
```
- Khi bạn chỉ muốn tạo nút link đơn giản (ví dụ trong một card "Đọc thêm") thì dùng `Link`:
```javascript
<Link to={`/post/${id}`}>Đọc thêm</Link>
```

---

# NavLink giúp giao diện đẹp hơn

## Vì sao NavLink giúp giao diện đẹp hơn?
- `Link` chỉ điều hướng, không biết "mình đang ở đâu".
- `NavLink` thì biết route hiện tại có khớp hay không → và tự động thêm style/class active.
- Điều này cực kỳ hữu ích cho menu, navbar, sidebar vì người dùng cần thấy mình đang ở trang nào.

## Ví dụ minh họa
```javascript
import { NavLink } from "react-router-dom";
import "./App.css"; // file css

function Navbar() {
  return (
    <nav>
      <NavLink to="/" className={({ isActive }) => (isActive ? "active" : "")}>
        Home
      </NavLink>
      {" | "}
      <NavLink to="/about" className={({ isActive }) => (isActive ? "active" : "")}>
        About
      </NavLink>
      {" | "}
      <NavLink to="/contact" className={({ isActive }) => (isActive ? "active" : "")}>
        Contact
      </NavLink>
    </nav>
  );
}

export default Navbar;
```
### CSS (App.css):
```css
nav a {
  text-decoration: none;
  color: gray;
  margin: 0 10px;
}

nav a.active {
  font-weight: bold;
  color: red;
  border-bottom: 2px solid red;
}
```
**Kết quả:**
- Khi bạn ở `/` → link Home được highlight (màu đỏ, in đậm, gạch chân).
- Khi bạn ở `/about` → link About tự động đổi style.
- Người dùng luôn biết mình đang ở đâu → giao diện thân thiện, đẹp và chuyên nghiệp hơn.

---

# Outlet: Nơi hiển thị các route con (nested routes)

## Khái niệm
- `Outlet` là placeholder để hiển thị các route con.
- Khi bạn định nghĩa route cha và lồng route con bên trong, `Outlet` sẽ render component của route con tương ứng.
- Nếu không có `Outlet`, các route con sẽ không xuất hiện.

## Ví dụ: Dashboard có các trang con

### 1. Định nghĩa route cha – con
```javascript
import { BrowserRouter, Routes, Route, Link, Outlet } from "react-router-dom";

function DashboardLayout() {
  return (
    <div>
      <h2>Dashboard</h2>
      <nav>
        <Link to="home">Home</Link> |{" "}
        <Link to="profile">Profile</Link> |{" "}
        <Link to="settings">Settings</Link>
      </nav>

      {/* Route con sẽ được render ở đây */}
      <Outlet />
    </div>
  );
}

function DashboardHome() {
  return <h3>🏠 Đây là trang Home trong Dashboard</h3>;
}

function UserProfile() {
  return <h3>👤 Đây là trang Profile</h3>;
}

function Settings() {
  return <h3>⚙️ Đây là trang Settings</h3>;
}

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Route cha */}
        <Route path="/dashboard" element={<DashboardLayout />}>
          {/* Route con */}
          <Route path="home" element={<DashboardHome />} />
          <Route path="profile" element={<UserProfile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

### 2. Giải thích
- Truy cập `/dashboard` → hiển thị `DashboardLayout`, nhưng vì chưa chọn route con → chỉ thấy layout + menu.
- Truy cập `/dashboard/home` → `DashboardHome` xuất hiện ở chỗ `Outlet`.
- Truy cập `/dashboard/profile` → `UserProfile` xuất hiện.
- Truy cập `/dashboard/settings` → `Settings` xuất hiện.

## Lợi ích khi dùng Outlet
- **Tái sử dụng layout:** Giữ nguyên header, sidebar, menu trong `DashboardLayout`.
- **Dễ quản lý:** Route con hiển thị đúng vị trí mà không phải lặp lại cấu trúc layout.
- **Thực tế:** Dùng cho dashboard, profile page, admin page, hay các app nhiều tầng menu.

---

# Navigate và useLocation

## 1. Navigate – Redirect giữa các route

### Khái niệm
- Là một component dùng để chuyển hướng (redirect) từ một route sang route khác.
- Thường dùng khi: user chưa login, hoặc cần chuyển hướng sau khi submit form.

### Ví dụ
```javascript
import { BrowserRouter, Routes, Route, Navigate } from "react-router-dom";

function Dashboard({ isLoggedIn }) {
  if (!isLoggedIn) {
    // Nếu chưa login → redirect về /login
    return <Navigate to="/login" replace />;
  }
  return <h2>Welcome to Dashboard</h2>;
}

function Login() {
  return <h2>Trang Login</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route path="/dashboard" element={<Dashboard isLoggedIn={false} />} />
      </Routes>
    </BrowserRouter>
  );
}
```
Truy cập `/dashboard` khi chưa login → tự động chuyển sang `/login`.

## 2. useLocation – Lấy thông tin URL hiện tại

### Khái niệm
- Là một hook trả về thông tin về đường dẫn hiện tại.
- Trả về object có dạng:
```javascript
{
  pathname: "/about",    // đường dẫn hiện tại
  search: "?page=2",     // query string
  hash: "#section1",     // hash fragment
  state: {},             // dữ liệu gửi kèm khi navigate
  key: "abc123"          // key nội bộ của router
}
```

### Ví dụ
```javascript
import { BrowserRouter, Routes, Route, useLocation } from "react-router-dom";

function ShowLocation() {
  const location = useLocation();
  return (
    <div>
      <p>📍 Pathname: {location.pathname}</p>
      <p>🔎 Search: {location.search}</p>
      <p>🔗 Hash: {location.hash}</p>
    </div>
  );
}

function About() {
  return <h2>Trang Giới thiệu</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/about" element={<About />} />
        <Route path="/location" element={<ShowLocation />} />
      </Routes>
    </BrowserRouter>
  );
}
```
Khi bạn truy cập: `/location?page=3#section1`
**Output:**
```
📍 Pathname: /location
🔎 Search: ?page=3
🔗 Hash: #section1
```

## So sánh nhanh

| Keyword    | Dùng khi nào?                                     | Trả về / Làm gì?                       |
|------------|---------------------------------------------------|----------------------------------------|
| `Navigate` | Khi muốn chuyển hướng tự động (redirect)          | Render ra một route khác               |
| `useLocation`| Khi muốn biết URL hiện tại (pathname, query, hash)| Trả về object thông tin về route       |

---

# Câu hỏi phỏng vấn về React Router DOM

## Câu hỏi cơ bản
- React Router DOM là gì? Tại sao cần dùng thay vì thẻ `<a>` thông thường?
- Phân biệt `BrowserRouter`, `HashRouter` và `MemoryRouter`. Bạn sẽ chọn cái nào trong môi trường production?
- Vai trò của `Route` và `Routes` trong React Router v6 là gì?
- Sự khác biệt giữa `Link` và `NavLink`? Khi nào thì dùng `NavLink` thay vì `Link`?
- `Outlet` dùng để làm gì? Hãy đưa ra ví dụ về nested routes.

## Câu hỏi trung cấp
- Giải thích cách hoạt động của `Navigate`. Tại sao có `replace` và `state`?
- `useParams` hoạt động như thế nào? Hãy lấy ví dụ cho route `/post/:id`.
- `useLocation` khác gì với `useParams`? Khi nào nên dùng `useLocation`?
- Giải thích sự khác biệt giữa SPA (Single Page Application) và MPA (Multi Page Application) trong ngữ cảnh React Router.
- Làm thế nào để bảo vệ route (Protected Route) trong React Router DOM?

## Câu hỏi nâng cao
- React Router xử lý lazy loading route như thế nào? Bạn đã dùng `React.lazy` và `Suspense` để tối ưu chưa?
- Giải thích cách bạn sẽ thiết kế một Dashboard với nhiều menu con (ví dụ: `/dashboard/home`, `/dashboard/profile`, `/dashboard/settings`) bằng React Router.
- Nếu bạn muốn redirect người dùng sau khi login thành công về trang mà họ định vào trước đó (ví dụ `/dashboard`), bạn sẽ kết hợp `Navigate` + `useLocation` như thế nào?
- Bạn sẽ preload dữ liệu (data fetching) cho một route như thế nào trước khi render component?
- Khi nào bạn sẽ chọn Nested Routes và khi nào chọn Flat Routes?

## Câu hỏi tình huống thực tế
- Bạn có route `/post/:id`. Làm thế nào để xử lý khi user nhập một id không tồn tại (ví dụ: `/post/9999`)?
- Khi click vào `<Link>` thì không reload trang, nhưng SEO có ảnh hưởng không? Làm sao để cải thiện SEO cho SPA?
- Nếu app của bạn cần chạy cả trên web và mobile (React Native), bạn có dùng `react-router-dom` không? Giải thích.
- Khi user nhấn Back trên trình duyệt, React Router hoạt động ra sao? Có cách nào can thiệp để chặn điều hướng không?
- Trong dự án lớn, bạn sẽ tổ chức file route thế nào để dễ bảo trì (ví dụ: tách `routes.js` riêng, lazy load, grouping routes)?
