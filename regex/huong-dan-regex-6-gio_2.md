# Hướng Dẫn Regex Từ Cơ Bản Đến Nâng Cao (6 Giờ)

> Áp dụng cho JavaScript (dùng được trong React/React Native, Node.js). Mỗi giờ có lý thuyết ngắn + ví dụ chạy được ngay.

---

## GIỜ 1 — Nhập môn: Ký tự, Class, Anchor (60 phút)

### 1.1 Regex là gì?
Regex (Regular Expression) là một chuỗi mẫu (pattern) dùng để **tìm kiếm, kiểm tra, thay thế** văn bản.

Trong JS có 2 cách tạo regex:
```js
const re1 = /abc/;           // literal
const re2 = new RegExp("abc"); // constructor (dùng khi pattern là biến động)
```

Kiểm tra khớp:
```js
/abc/.test("xabcx");     // true
"Học lập trình".match(/lập trình/); // ["lập trình", index: ...]
```

### 1.2 Ký tự thường & Metacharacter
Ký tự thường (`a`, `b`, `1`...) khớp chính nó. Các ký tự đặc biệt cần escape bằng `\`:
`. * + ? ^ $ ( ) [ ] { } | \`

Ví dụ: tìm dấu chấm thật trong "3.14"
```js
/3\.14/.test("3.14"); // true
/3.14/.test("3x14");  // true (vì . nghĩa là "bất kỳ ký tự nào" nếu không escape!)
```

### 1.3 Character Class `[...]`
```js
/[aeiou]/.test("hello");      // true — có nguyên âm
/[0-9]/.test("SV2024");       // true — có số
/[^0-9]/.test("2024");        // false — không có ký tự ngoài số

// Ví dụ thực tế: kiểm tra mã sinh viên chỉ gồm chữ hoa + số
/^[A-Z0-9]+$/.test("SV2024");  // true
/^[A-Z0-9]+$/.test("sv-2024"); // false
```

### 1.4 Shorthand class
| Ký hiệu | Ý nghĩa | Tương đương |
|---|---|---|
| `\d` | chữ số | `[0-9]` |
| `\D` | không phải chữ số | `[^0-9]` |
| `\w` | chữ/số/gạch dưới | `[A-Za-z0-9_]` |
| `\W` | ngược lại `\w` | |
| `\s` | khoảng trắng | space, tab, newline |
| `\S` | không phải khoảng trắng | |

Ví dụ: tách số điện thoại khỏi chuỗi
```js
"Liên hệ: 0912345678 nhé".match(/\d+/)[0]; // "0912345678"
```

### 1.5 Anchor — vị trí
- `^` : đầu chuỗi
- `$` : cuối chuỗi

```js
/^Hello/.test("Hello world");  // true
/^Hello/.test("Say Hello");    // false
/world$/.test("Hello world");  // true
```

**Bài tập Giờ 1:** Viết regex kiểm tra một chuỗi là mã môn học dạng `IT101` (2 chữ hoa + 3 số).
```js
/^[A-Z]{2}\d{3}$/.test("IT101"); // true (giải ở Giờ 2 khi học {n})
```

---

## GIỜ 2 — Quantifier (Lượng từ) (60 phút)

### 2.1 Các lượng từ cơ bản
| Ký hiệu | Ý nghĩa |
|---|---|
| `*` | 0 lần trở lên |
| `+` | 1 lần trở lên |
| `?` | 0 hoặc 1 lần |
| `{n}` | đúng n lần |
| `{n,}` | tối thiểu n lần |
| `{n,m}` | từ n đến m lần |

```js
/colou?r/.test("color");   // true
/colou?r/.test("colour");  // true

/\d{3}/.test("12");        // false
/\d{3}/.test("123");       // true

/^[A-Z]{2}\d{3}$/.test("IT101"); // true — giải bài tập Giờ 1
```

### 2.2 Greedy vs Lazy
Mặc định quantifier là **greedy** (tham lam — khớp càng nhiều càng tốt). Thêm `?` để thành **lazy**.

```js
const html = "<b>Đậm</b><i>Nghiêng</i>";
html.match(/<.+>/)[0];   // "<b>Đậm</b><i>Nghiêng</i>"  (greedy — lấy hết)
html.match(/<.+?>/)[0];  // "<b>"                       (lazy — dừng sớm)
```

**Mẹo thực tế:** Khi parse HTML/JSON đơn giản bằng regex, luôn cân nhắc dùng lazy `?` để tránh "ăn" quá nhiều ký tự.

### 2.3 Ví dụ thực tế: kiểm tra mật khẩu tối thiểu 8 ký tự
```js
/^.{8,}$/.test("abc123");     // false (6 ký tự)
/^.{8,}$/.test("abc12345");   // true
```

**Bài tập Giờ 2:** Regex kiểm tra hashtag dạng `#reactjs`, `#webdev123` (bắt đầu bằng #, sau đó 1+ ký tự chữ/số).
```js
/^#\w+$/.test("#reactjs");  // true
```

---

## GIỜ 3 — Group, Alternation, Backreference (60 phút)

### 3.1 Group `( )`
Dùng để nhóm và **capture** (bắt lại) một phần kết quả.

```js
const m = "2024-06-15".match(/(\d{4})-(\d{2})-(\d{2})/);
m[0]; // "2024-06-15" (toàn bộ)
m[1]; // "2024" (năm)
m[2]; // "06" (tháng)
m[3]; // "15" (ngày)
```

### 3.2 Non-capturing group `(?:...)`
Khi chỉ muốn nhóm để áp dụng quantifier, không cần lấy giá trị:
```js
/(?:https?:\/\/)?(\w+)\.com/.exec("https://openai.com")[1]; // "openai"
```

### 3.3 Named group `(?<name>...)` — rất hữu ích khi code
```js
const re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const { groups } = re.exec("2024-06-15");
groups.year;  // "2024"
groups.month; // "06"
```
→ Dùng trong React khi parse ngày tháng từ input, dữ liệu API, v.v.

### 3.4 Alternation `|`
```js
/mèo|chó|chim/.test("Tôi nuôi mèo"); // true
/^(GET|POST|PUT|DELETE) /.test("GET /api/users"); // true — kiểm tra HTTP method
```

### 3.5 Backreference `\1`
Tìm từ bị lặp liên tiếp (ví dụ lỗi chính tả "là là"):
```js
/\b(\w+)\s+\1\b/.test("đây là là ví dụ"); // true — bắt được "là là"
```

**Bài tập Giờ 3:** Viết regex bắt tên biến React kiểu `useSomething` và lấy phần "Something".
```js
"useFetch".match(/^use([A-Z]\w*)$/)[1]; // "Fetch"
```

---

## GIỜ 4 — Lookahead / Lookbehind (Nâng cao) (60 phút)

Lookaround không "ăn" ký tự, chỉ kiểm tra điều kiện xung quanh.

| Ký hiệu | Ý nghĩa |
|---|---|
| `(?=...)` | positive lookahead — phía sau phải có |
| `(?!...)` | negative lookahead — phía sau không được có |
| `(?<=...)` | positive lookbehind — phía trước phải có |
| `(?<!...)` | negative lookbehind — phía trước không được có |

### 4.1 Ví dụ: định dạng số tiền có dấu phẩy ngăn cách
```js
"1000000".replace(/\B(?=(\d{3})+(?!\d))/g, ",");
// "1,000,000"
```
Giải thích: `\B` = không phải ranh giới từ; `(?=(\d{3})+(?!\d))` = phía sau còn đúng bội số 3 chữ số nữa.

### 4.2 Ví dụ: kiểm tra mật khẩu mạnh (thực tế hay dùng)
Yêu cầu: tối thiểu 8 ký tự, có chữ hoa, chữ thường, số, ký tự đặc biệt.
```js
const strongPassword =
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).{8,}$/;

strongPassword.test("Abc123!@");  // true
strongPassword.test("abc12345");  // false — thiếu chữ hoa & ký tự đặc biệt
```

### 4.3 Ví dụ: lấy giá trị đứng sau dấu `:` nhưng không lấy dấu `:`
```js
"price: 250000".match(/(?<=price:\s).+/)[0]; // "250000"
```

**Bài tập Giờ 4:** Viết regex khớp từ "React" nhưng KHÔNG khớp nếu ngay sau nó là "Native" (dùng negative lookahead).
```js
/React(?! Native)/.test("React JS");     // true
/React(?! Native)/.test("React Native"); // false
```

---

## GIỜ 5 — Flags & Ứng dụng thực tế (60 phút)

### 5.1 Các flag phổ biến
| Flag | Ý nghĩa |
|---|---|
| `g` | global — tìm tất cả, không dừng ở kết quả đầu |
| `i` | ignore case — không phân biệt hoa thường |
| `m` | multiline — `^ $` áp dụng cho từng dòng |
| `s` | dotAll — `.` khớp cả xuống dòng |

```js
"Hello hello HELLO".match(/hello/gi); // ["Hello", "hello", "HELLO"]
```

### 5.2 Validate Email (mức thực dụng, không cần chuẩn RFC đầy đủ)
```js
const emailRe = /^[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}$/;
emailRe.test("student@school.edu.vn"); // true
emailRe.test("sai@.com");              // false
```

### 5.3 Validate Số điện thoại Việt Nam
```js
const phoneRe = /^(0|\+84)(3|5|7|8|9)\d{8}$/;
phoneRe.test("0912345678");   // true
phoneRe.test("+84912345678"); // true
phoneRe.test("0212345678");   // false — đầu số không hợp lệ
```

### 5.4 Replace nâng cao với hàm callback
```js
"react, javascript, nodejs".replace(/\w+/g, w =>
  w[0].toUpperCase() + w.slice(1)
);
// "React, Javascript, Nodejs"
```

### 5.5 Split nâng cao
```js
"a, b ; c  d".split(/[,;\s]+/).filter(Boolean);
// ["a", "b", "c", "d"]
```

**Bài tập Giờ 5:** Viết regex tách các hashtag từ một đoạn caption mạng xã hội.
```js
"Học code mỗi ngày #reactjs #webdev #100daysofcode"
  .match(/#\w+/g);
// ["#reactjs", "#webdev", "#100daysofcode"]
```

---

## GIỜ 6 — Thực hành tổng hợp & Best Practices (60 phút)

### 6.1 Bài toán tổng hợp: Parse dữ liệu log
```js
const log = "[2024-06-15 10:23:45] ERROR: Không kết nối được database";

const re = /\[(?<date>[\d-]+)\s(?<time>[\d:]+)\]\s(?<level>\w+):\s(?<message>.+)/;
const { groups } = re.exec(log);

console.log(groups);
// { date: '2024-06-15', time: '10:23:45', level: 'ERROR', message: 'Không kết nối được database' }
```

### 6.2 Bài toán tổng hợp: Validate form đăng ký (React)
```js
function validate(form) {
  const errors = {};
  if (!/^[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}$/.test(form.email))
    errors.email = "Email không hợp lệ";
  if (!/^(0|\+84)(3|5|7|8|9)\d{8}$/.test(form.phone))
    errors.phone = "Số điện thoại không hợp lệ";
  if (!/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test(form.password))
    errors.password = "Mật khẩu cần ≥8 ký tự, có hoa/thường/số";
  return errors;
}
```

### 6.3 Best Practices — Lưu ý khi dạy/dùng Regex
1. **Đừng cố dùng regex cho mọi thứ.** Với HTML/JSON phức tạp, dùng parser thật (DOMParser, JSON.parse).
2. **Luôn test regex với dữ liệu biên** (chuỗi rỗng, ký tự đặc biệt, unicode tiếng Việt có dấu).
3. **Đặt tên group** (`?<name>`) để code dễ đọc hơn khi regex phức tạp.
4. **Cẩn thận "catastrophic backtracking"** — tránh lồng nhiều `(...)+` hoặc `(...)*` gây treo trình duyệt với chuỗi dài.
5. Công cụ hỗ trợ trực quan: regex101.com (có giải thích từng phần, hỗ trợ tiếng Việt/unicode).

### 6.4 Bài tập cuối — Tổng hợp (giao về nhà)
Yêu cầu học viên viết regex cho:
1. Mã sinh viên: 2 chữ cái + 6 số, ví dụ `SV202456`
2. URL hợp lệ bắt đầu bằng `http://` hoặc `https://`
3. Trích xuất tất cả số trong một đoạn văn bản
4. Kiểm tra chuỗi là palindrome đơn giản không dùng regex thuần (gợi ý: regex không mạnh cho bài này — dùng để học viên hiểu giới hạn của regex)

---

## Tổng kết lộ trình 6 giờ

| Giờ | Chủ đề | Trọng tâm |
|---|---|---|
| 1 | Nhập môn | Literal, class, anchor |
| 2 | Quantifier | `* + ? {n,m}`, greedy/lazy |
| 3 | Group | Capture, non-capture, named group, alternation |
| 4 | Lookaround | Lookahead/lookbehind |
| 5 | Flags & ứng dụng | `g i m s`, email, phone, replace/split |
| 6 | Tổng hợp | Parse log, validate form, best practices |
