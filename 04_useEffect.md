# Báo cáo Toàn diện về useEffect trong React

## 1. Side Effects là gì?

### 1.1 Định nghĩa
Side effects là những thao tác làm thay đổi trạng thái bên ngoài scope hiện tại của component, không liên quan trực tiếp đến việc render JSX. Trong React, side effects là những tác vụ "thoát khỏi" hệ thống React để tương tác với thế giới bên ngoài.

### 1.2 Các loại Side Effects phổ biến

#### **API Calls**
```ts
// Gọi API để fetch dữ liệu
useEffect(() => {
  const fetchUserData = async () => {
    try {
      const response = await fetch('/api/users');
      const userData = await response.json();
      setUser(userData);
    } catch (error) {
      setError(error.message);
    }
  };

  fetchUserData();
}, []);
```

#### **Thao tác DOM trực tiếp**
```ts
// Thay đổi title trang web
useEffect(() => {
  document.title = `Welcome ${userName}`;

  // Thay đổi CSS của element
  const element = document.getElementById('header');
  if (element) {
    element.style.backgroundColor = 'blue';
  }
}, [userName]);
```

#### **Event Listeners**
```ts
// Đăng ký event listener
useEffect(() => {
  const handleScroll = () => {
    console.log('User is scrolling');
  };

  window.addEventListener('scroll', handleScroll);

  return () => {
    window.removeEventListener('scroll', handleScroll);
  };
}, []);
```

#### **Timers và Intervals**
```ts
// Tạo timer
useEffect(() => {
  const timer = setTimeout(() => {
    setShowMessage(false);
  }, 5000);

  return () => clearTimeout(timer);
}, []);
```

#### **Browser Storage**
```ts
// Lưu trữ dữ liệu vào localStorage
useEffect(() => {
  localStorage.setItem('userPreferences', JSON.stringify(preferences));
}, [preferences]);
```

#### **Third-party Libraries**
```ts
// Khởi tạo thư viện bên ngoài
useEffect(() => {
  const map = new GoogleMap(mapContainer.current);
  map.setCenter(coordinates);

  return () => {
    map.destroy();
  };
}, [coordinates]);
```

## 2. Sử dụng useEffect để thực hiện Side Effects sau khi render

### 2.1 Tại sao cần useEffect?

**Vấn đề khi không dùng useEffect:**
```ts
// ❌ KHÔNG bao giờ làm thế này
function BadComponent() {
  const [count, setCount] = useState(0);

  // Side effect trong render - Rất nguy hiểm!
  document.title = `Count: ${count}`;  // Có thể gây infinite loop
  console.log('Rendering...');         // Spam console
  localStorage.setItem('count', count); // Performance issue

  return <div>{count}</div>;
}
```

**Giải pháp với useEffect:**
```ts
// ✅ An toàn và đúng cách
function GoodComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
    console.log('Effect ran after render');
    localStorage.setItem('count', count);
  }, [count]); // Chỉ chạy khi count thay đổi

  return <div>{count}</div>;
}
```

### 2.2 Timing của useEffect

**Thứ tự thực hiện:**
```ts
function TimingDemo() {
  console.log('1. Component function starts');

  const [data, setData] = useState(null);

  console.log('2. During render phase');

  useEffect(() => {
    console.log('4. useEffect runs AFTER DOM commit');
    // DOM đã được cập nhật, an toàn để thao tác
  });

  console.log('3. Render phase continues');

  return <div>{data}</div>; // 5. JSX được return
}
```

### 2.3 useEffect vs useLayoutEffect

**useEffect (Bất đồng bộ):**
- Chạy sau khi browser đã paint
- Không block rendering
- Phù hợp cho hầu hết side effects

**useLayoutEffect (Đồng bộ):**
- Chạy trước khi browser paint
- Block rendering cho đến khi hoàn thành
- Dùng cho DOM measurements, CSS animations

```ts
// useEffect - Có thể thấy flicker
useEffect(() => {
  tooltip.current.style.left = calculatePosition() + 'px';
}, []);

// useLayoutEffect - Không có flicker
useLayoutEffect(() => {
  tooltip.current.style.left = calculatePosition() + 'px';
}, []);
```

## 3. Dependency Array trong useEffect

### 3.1 Ba cách sử dụng Dependency Array

#### **Không có dependency array - Chạy mỗi render**
```ts
useEffect(() => {
  console.log('Runs after EVERY render');
}); // Không có []

// Use case: Debugging, logging mỗi render
```

#### **Empty dependency array - Chỉ chạy một lần**
```ts
useEffect(() => {
  // Tương tự componentDidMount
  fetchInitialData();
  setupEventListeners();
}, []); // Empty array

// Use case: Setup một lần khi component mount
```

#### **Có dependencies - Chạy khi dependencies thay đổi**
```ts
const [userId, setUserId] = useState(1);
const [theme, setTheme] = useState('light');

useEffect(() => {
  fetchUserData(userId);
}, [userId]); // Chỉ chạy khi userId thay đổi

useEffect(() => {
  applyTheme(theme);
}, [theme]); // Chỉ chạy khi theme thay đổi
```

### 3.2 Rules of Dependencies

#### **Reactive Values phải được khai báo**
```ts
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');
  const [serverUrl, setServerUrl] = useState('localhost:3000');

  // ❌ Missing dependencies
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, []); // ESLint sẽ warning!

  // ✅ All dependencies declared
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [serverUrl, roomId]);
}
```

#### **Object và Function Dependencies**
```ts
// ❌ Object được tạo mới mỗi render
const user = { id: userId, name: userName }; // Tạo mới mỗi lần
useEffect(() => {
  fetchUserDetails(user);
}, [user]); // user luôn khác nhau!

// ✅ Chỉ depend on primitive values
useEffect(() => {
  const user = { id: userId, name: userName };
  fetchUserDetails(user);
}, [userId, userName]); // Primitive values
```

#### **Function Dependencies với useCallback**
```ts
// ❌ Function tạo mới mỗi render
const fetchData = () => {
  return api.getData(userId);
};

useEffect(() => {
  fetchData().then(setData);
}, [fetchData]); // fetchData luôn khác nhau

// ✅ Memoized function
const fetchData = useCallback(() => {
  return api.getData(userId);
}, [userId]);

useEffect(() => {
  fetchData().then(setData);
}, [fetchData]); // fetchData chỉ thay đổi khi userId thay đổi
```

### 3.3 Dependency Comparison

React sử dụng `Object.is()` để so sánh dependencies:

```ts
// Primitive values
Object.is(1, 1);           // true
Object.is('hello', 'hello'); // true
Object.is(true, true);      // true

// Objects và Arrays
Object.is({}, {});          // false - khác reference
Object.is([], []);          // false - khác reference

// Functions
const fn1 = () => {};
const fn2 = () => {};
Object.is(fn1, fn2);        // false - khác reference
Object.is(fn1, fn1);        // true - cùng reference
```

## 4. Cleanup Function trong useEffect

### 4.1 Tại sao cần Cleanup?

Cleanup function giúp:
- Tránh memory leaks
- Hủy subscriptions
- Clear timers/intervals
- Remove event listeners
- Cancel network requests

### 4.2 Cú pháp Cleanup Function

```ts
useEffect(() => {
  // Setup phase
  const subscription = api.subscribe(handleData);
  const timer = setInterval(updateClock, 1000);

  // Cleanup phase
  return () => {
    subscription.unsubscribe();
    clearInterval(timer);
  };
}, [dependencies]);
```

### 4.3 Timing của Cleanup

```ts
function CleanupDemo({ userId }) {
  useEffect(() => {
    console.log(`Setup effect for user ${userId}`);

    return () => {
      console.log(`Cleanup effect for user ${userId}`);
    };
  }, [userId]);

  // Khi userId thay đổi từ 1 → 2:
  // 1. "Cleanup effect for user 1"
  // 2. "Setup effect for user 2"
}
```

### 4.4 Các trường hợp Cleanup phổ biến

#### **Event Listeners**
```ts
useEffect(() => {
  const handleResize = () => {
    setWindowWidth(window.innerWidth);
  };

  window.addEventListener('resize', handleResize);

  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```

#### **Timers và Intervals**
```ts
// Timer
useEffect(() => {
  const timerId = setTimeout(() => {
    setShowAlert(false);
  }, 3000);

  return () => clearTimeout(timerId);
}, []);

// Interval
useEffect(() => {
  const intervalId = setInterval(() => {
    setTime(new Date());
  }, 1000);

  return () => clearInterval(intervalId);
}, []);
```

#### **Subscriptions**
```ts
useEffect(() => {
  const subscription = eventBus.subscribe('user-update', handleUserUpdate);

  return () => {
    subscription.unsubscribe();
  };
}, []);
```

#### **Abort Network Requests**
```ts
useEffect(() => {
  const abortController = new AbortController();

  const fetchData = async () => {
    try {
      const response = await fetch('/api/data', {
        signal: abortController.signal
      });
      const data = await response.json();
      setData(data);
    } catch (error) {
      if (error.name !== 'AbortError') {
        console.error('Fetch failed:', error);
      }
    }
  };

  fetchData();

  return () => {
    abortController.abort();
  };
}, []);
```

#### **Third-party Library Cleanup**
```ts
useEffect(() => {
  // Khởi tạo map
  const map = new GoogleMap(mapRef.current);
  map.setCenter(coordinates);

  // Animation library
  const animation = gsap.to(elementRef.current, {
    duration: 2,
    x: 100
  });

  return () => {
    // Cleanup map
    map.destroy();

    // Kill animation
    animation.kill();
  };
}, [coordinates]);
```

#### **WebSocket Connections**
```ts
useEffect(() => {
  const ws = new WebSocket('ws://localhost:8080');

  ws.onopen = () => {
    console.log('WebSocket connected');
  };

  ws.onmessage = (event) => {
    setMessages(prev => [...prev, event.data]);
  };

  ws.onerror = (error) => {
    console.error('WebSocket error:', error);
  };

  return () => {
    ws.close();
  };
}, []);
```

## 5. Best Practices và Common Patterns

### 5.1 Tách riêng Effects
```ts
// ❌ Một effect làm nhiều việc
useEffect(() => {
  fetchUserData();
  setupWebSocket();
  trackPageView();
  startTimer();
}, [userId]);

// ✅ Tách riêng từng effect
useEffect(() => {
  fetchUserData();
}, [userId]);

useEffect(() => {
  setupWebSocket();
  return cleanupWebSocket;
}, []);

useEffect(() => {
  trackPageView();
}, []);

useEffect(() => {
  const timer = startTimer();
  return () => clearTimer(timer);
}, []);
```

### 5.2 Custom Hooks
```ts
// Custom hook cho window size
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return size;
}

// Sử dụng
function MyComponent() {
  const { width, height } = useWindowSize();
  return <div>Screen: {width}x{height}</div>;
}
```

### 5.3 Race Condition Prevention
```ts
useEffect(() => {
  let ignore = false;

  const fetchData = async () => {
    try {
      const result = await api.getData(query);

      // Chỉ update state nếu effect chưa bị cleanup
      if (!ignore) {
        setData(result);
      }
    } catch (error) {
      if (!ignore) {
        setError(error);
      }
    }
  };

  fetchData();

  return () => {
    ignore = true; // Prevent state update after cleanup
  };
}, [query]);
```

### 5.4 Optimizing Dependencies
```ts
// ❌ Unnecessary re-renders
const expensiveConfig = {
  timeout: 5000,
  retries: 3,
  endpoint: '/api/data'
};

useEffect(() => {
  fetchWithConfig(expensiveConfig);
}, [expensiveConfig]); // Object tạo mới mỗi render

// ✅ Move static config outside
const CONFIG = {
  timeout: 5000,
  retries: 3,
  endpoint: '/api/data'
};

useEffect(() => {
  fetchWithConfig(CONFIG);
}, []); // Không có dependencies

// ✅ Hoặc dùng useMemo
const expensiveConfig = useMemo(() => ({
  timeout: 5000,
  retries: 3,
  endpoint: '/api/data'
}), []); // Chỉ tạo một lần
```

## 6. Common Troubleshooting

### 6.1 Effect chạy hai lần trong Development
```ts
// React Strict Mode chạy effect hai lần để test cleanup logic
useEffect(() => {
  console.log('Effect runs'); // In 2 lần trong dev mode

  return () => {
    console.log('Cleanup runs'); // Cũng chạy thêm 1 lần
  };
}, []);
```

### 6.2 Infinite Loop Detection
```ts
// ❌ Gây infinite loop
const [count, setCount] = useState(0);

useEffect(() => {
  setCount(count + 1); // count thay đổi → effect chạy lại
}, [count]);

// ✅ Fix với functional update
useEffect(() => {
  setCount(c => c + 1); // Không depend on count
}, []); // Empty dependency array
```

### 6.3 Missing Dependencies Warning
```ts
// ❌ ESLint warning
function Component({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then(setUser); // Dùng userId nhưng không khai báo
  }, []); // Missing dependency: userId

  // ✅ Fix
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // Khai báo đúng dependency
}
```

## 7. Performance Considerations

### 7.1 Effect Scheduling
- useEffect được schedule để chạy sau khi DOM commit
- Không block browser painting
- Batch multiple effects trong cùng render

### 7.2 Dependency Optimization
```ts
// ❌ Expensive dependency comparison
const config = {
  apiUrl: process.env.API_URL,
  timeout: 5000
}; // Tạo mới mỗi render

useEffect(() => {
  initializeService(config);
}, [config]);

// ✅ Optimize với useMemo
const config = useMemo(() => ({
  apiUrl: process.env.API_URL,
  timeout: 5000
}), []); // Chỉ tạo một lần
```

### 7.3 Cleanup Performance
```ts
// ✅ Efficient cleanup
useEffect(() => {
  const listeners = [];

  // Collect tất cả listeners
  const addListener = (event, handler) => {
    window.addEventListener(event, handler);
    listeners.push([event, handler]);
  };

  addListener('resize', handleResize);
  addListener('scroll', handleScroll);
  addListener('keydown', handleKeydown);

  return () => {
    // Cleanup tất cả cùng lúc
    listeners.forEach(([event, handler]) => {
      window.removeEventListener(event, handler);
    });
  };
}, []);
```

## 8. Kết luận

useEffect là một trong những hooks quan trọng nhất trong React, cho phép developers thực hiện side effects một cách an toàn và hiệu quả. Việc hiểu rõ về:

- **Side effects** và tại sao cần useEffect
- **Dependency array** và cách React compare dependencies
- **Cleanup function** để tránh memory leaks
- **Best practices** và common patterns

sẽ giúp bạn viết code React tốt hơn, tránh bugs phổ biến và tối ưu performance. useEffect không chỉ là công cụ để thay thế lifecycle methods của class components, mà còn là cách để đồng bộ component với external systems một cách declarative và predictable.


# Bộ câu hỏi trắc nghiệm ôn tập useEffect

## Câu 1: Khái niệm cơ bản
**Side effect trong React là gì?**

A) Một bug xảy ra khi component render
B) Những thao tác làm thay đổi trạng thái bên ngoài scope của component
C) Một loại hook đặc biệt
D) Lỗi xảy ra trong useEffect

<details>
<summary>Đáp án</summary>
<strong>B) Những thao tác làm thay đổi trạng thái bên ngoài scope của component</strong>

Side effects bao gồm: API calls, DOM manipulation, event listeners, timers, localStorage, etc.
</details>


## Câu 2: Dependency Array
**Điền vào chỗ trống để effect chỉ chạy một lần khi component mount:**

```ts
const [count, setCount] = useState(0);

useEffect(() => {
  console.log('Component mounted');
}, ______); // Điền vào đây
```

A) `[count]`
B) `[]`
C) `[count, setCount]`
D) Không cần điền gì

<details>
<summary>Đáp án</summary>
<strong>B) []</strong>

Empty dependency array `[]` làm cho effect chỉ chạy một lần sau first render, tương tự componentDidMount.
</details>


## Câu 3: Cleanup Function
**Khi nào cleanup function trong useEffect được chạy?**

A) Chỉ khi component unmount
B) Trước mỗi lần effect chạy lại và khi component unmount
C) Sau mỗi lần effect chạy
D) Khi có lỗi xảy ra

<details>
<summary>Đáp án</summary>
<strong>B) Trước mỗi lần effect chạy lại và khi component unmount</strong>

Cleanup function chạy để "dọn dẹp" trước khi setup mới hoặc khi component bị unmount.
</details>


## Câu 4: Code Completion - Event Listener
**Hoàn thiện đoạn code sau để thêm event listener cho window resize:**

```ts
useEffect(() => {
  const handleResize = () => {
    setWindowWidth(window.innerWidth);
  };

  _________________(_______, _______);

  return () => {
    _________________(_______, _______);
  };
}, []);
```

A) `window.addEventListener('resize', handleResize)` và `window.removeEventListener('resize', handleResize)`
B) `window.addEvent('resize', handleResize)` và `window.removeEvent('resize', handleResize)`
C) `addEventListener('resize', handleResize)` và `removeEventListener('resize', handleResize)`
D) `window.on('resize', handleResize)` và `window.off('resize', handleResize)`

<details>
<summary>Đáp án</summary>
<strong>A) window.addEventListener('resize', handleResize) và window.removeEventListener('resize', handleResize)</strong>

```ts
useEffect(() => {
  const handleResize = () => {
    setWindowWidth(window.innerWidth);
  };

  window.addEventListener('resize', handleResize);

  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```
</details>


## Câu 5: Race Condition
**Tại sao cần sử dụng biến `ignore` trong đoạn code sau?**

```ts
useEffect(() => {
  let ignore = false;

  fetchData(userId).then(result => {
    if (!ignore) {
      setData(result);
    }
  });

  return () => {
    ignore = true;
  };
}, [userId]);
```

A) Để tối ưu performance
B) Để tránh memory leak
C) Để tránh race condition khi API response về sau khi component đã unmount
D) Để cache dữ liệu

<details>
<summary>Đáp án</summary>
<strong>C) Để tránh race condition khi API response về sau khi component đã unmount</strong>

Biến `ignore` ngăn việc setState khi component đã bị unmount hoặc effect đã được cleanup.
</details>


## Câu 6: Dependency Rules
**Đoạn code nào sau đây sẽ gây ra ESLint warning về missing dependencies?**

A)
```ts
useEffect(() => {
  console.log('Effect runs');
}, []);
```

B)
```ts
const [count, setCount] = useState(0);
useEffect(() => {
  console.log(count);
}, [count]);
```

C)
```ts
const [count, setCount] = useState(0);
useEffect(() => {
  console.log(count);
}, []); // Missing count in dependencies
```

D)
```ts
useEffect(() => {
  setInterval(() => console.log('tick'), 1000);
});
```

<details>
<summary>Đáp án</summary>
<strong>C) Missing count in dependencies</strong>

Effect sử dụng `count` nhưng không khai báo trong dependency array, vi phạm Rules of Hooks.
</details>


## Câu 7: Timer Cleanup
**Điền vào chỗ trống để tạo interval và cleanup đúng cách:**

```ts
useEffect(() => {
  const intervalId = ______(() => {
    setTime(new Date());
  }, 1000);

  return () => {
    ______(intervalId);
  };
}, []);
```

A) `setTimeout` và `clearTimeout`
B) `setInterval` và `clearInterval`
C) `requestAnimationFrame` và `cancelAnimationFrame`
D) `Promise.resolve` và `Promise.reject`

<details>
<summary>Đáp án</summary>
<strong>B) setInterval và clearInterval</strong>

```ts
useEffect(() => {
  const intervalId = setInterval(() => {
    setTime(new Date());
  }, 1000);

  return () => {
    clearInterval(intervalId);
  };
}, []);
```
</details>


## Câu 8: Infinite Loop
**Đoạn code nào sau đây sẽ gây ra infinite loop?**

A)
```ts
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(c => c + 1);
}, []);
```

B)
```ts
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

C)
```ts
const [count, setCount] = useState(0);
useEffect(() => {
  console.log(count);
}, [count]);
```

D)
```ts
useEffect(() => {
  fetchData();
}, []);
```

<details>
<summary>Đáp án</summary>
<strong>B) setCount(count + 1) với [count] dependency</strong>

Effect update `count` → component re-render → `count` thay đổi → effect chạy lại → infinite loop.
</details>


## Câu 9: Custom Hook Pattern
**Hoàn thiện custom hook sau để track window size:**

```ts
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      ______({
        width: _______,
        height: _______
      });
    };

    window.addEventListener(_______, _______);

    return () => {
      window._______(_______, _______);
    };
  }, _____);

  return _____;
}
```

A) `setSize`, `window.innerWidth`, `window.innerHeight`, `'resize'`, `handleResize`, `removeEventListener`, `'resize'`, `handleResize`, `[]`, `size`

B) `useState`, `screen.width`, `screen.height`, `'scroll'`, `handleResize`, `off`, `'scroll'`, `handleResize`, `[size]`, `[size]`

C) `setSize`, `document.width`, `document.height`, `'resize'`, `handleResize`, `removeEventListener`, `'resize'`, `handleResize`, `[handleResize]`, `size`

D) `setState`, `window.outerWidth`, `window.outerHeight`, `'resize'`, `handleResize`, `removeEventListener`, `'resize'`, `handleResize`, `[]`, `{size}`

<details>
<summary>Đáp án</summary>
<strong>A) Tất cả các từ theo thứ tự</strong>

```ts
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return size;
}
```
</details>


## Câu 10: useEffect vs useLayoutEffect
**Khi nào nên sử dụng useLayoutEffect thay vì useEffect?**

A) Khi cần fetch data từ API
B) Khi cần đo kích thước DOM element trước khi browser paint
C) Khi cần setup event listeners
D) Khi cần cleanup resources

<details>
<summary>Đáp án</summary>
<strong>B) Khi cần đo kích thước DOM element trước khi browser paint</strong>

useLayoutEffect chạy synchronously trước khi browser paint, phù hợp cho DOM measurements, positioning tooltips, etc.
</details>


## Câu Bonus: Debugging Effect
**Làm thế nào để debug effect dependency changes?**

```ts
useEffect(() => {
  // Effect logic
}, [serverUrl, roomId]);

// Thêm dòng code nào để debug?
```

A) `console.log('Effect ran');`
B) `console.log([serverUrl, roomId]);`
C) `debugger;`
D) `React.debug([serverUrl, roomId]);`

<details>
<summary>Đáp án</summary>
<strong>B) console.log([serverUrl, roomId]);</strong>

Log dependency array để xem giá trị thay đổi như thế nào giữa các renders. Có thể dùng browser console để so sánh với Object.is().
</details>
