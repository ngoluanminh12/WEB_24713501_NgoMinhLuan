# HƯỚNG DẪN JQUERY — TỪ CƠ BẢN ĐẾN NÂNG CAO
### Thời lượng: 6 giờ | Chủ đề xuyên suốt: Website "Bookstore Online"

## MỤC LỤC

| Giờ | Nội dung |
|---|---|
| Giờ 1 | Giới thiệu jQuery, cách nhúng, Selectors cơ bản |
| Giờ 2 | Thao tác DOM (nội dung, thuộc tính, CSS) |
| Giờ 3 | Xử lý sự kiện (Events) |
| Giờ 4 | Hiệu ứng & Animation |
| Giờ 5 | AJAX với jQuery |
| Giờ 6 | Nâng cao: Chaining, Plugins, Form validation, Mini Project |

---

## GIỜ 1: GIỚI THIỆU JQUERY & SELECTORS (60 phút)

### 1.1 jQuery là gì?  
jQuery là một thư viện JavaScript giúp:
- Thao tác DOM dễ dàng hơn
- Xử lý sự kiện đơn giản hơn
- Tạo hiệu ứng, animation nhanh chóng
- Gửi request AJAX gọn gàng

**Khẩu hiệu:** *"Write less, do more"*

### 1.2 Cách nhúng jQuery vào trang (10 phút)

**Cách 1: Dùng CDN (khuyến nghị)**
```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Bookstore Online</title>
  <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
</head>
<body>
  <h1>Chào mừng đến Bookstore Online</h1>
  <script>
    $(document).ready(function() {
      console.log("jQuery đã sẵn sàng!");
    });
  </script>
</body>
</html>
```

**Cách 2: Tải file về và nhúng local**
```html
<script src="js/jquery-3.7.1.min.js"></script>
```

**Cú pháp rút gọn của `$(document).ready()`:**
```javascript
$(function() {
  console.log("Cách viết tắt");
});
```

### 1.3 Cú pháp cơ bản của jQuery 

```javascript
$(selector).action();
```

- `$` — ký hiệu gọi jQuery
- `selector` — "tìm" phần tử HTML nào
- `action()` — làm gì với phần tử đó

### 1.4 Selectors cơ bản (30 phút)

**Ví dụ HTML dùng chung cho phần này:**
```html
<div id="book-list">
  <div class="book" data-genre="fiction">
    <h3>Đắc Nhân Tâm</h3>
    <p class="price">120.000đ</p>
  </div>
  <div class="book" data-genre="skill">
    <h3>Tư Duy Nhanh và Chậm</h3>
    <p class="price">150.000đ</p>
  </div>
  <div class="book" data-genre="fiction">
    <h3>Nhà Giả Kim</h3>
    <p class="price">89.000đ</p>
  </div>
</div>
```

| Loại selector | Cú pháp | Ví dụ |
|---|---|---|
| Theo ID | `$("#id")` | `$("#book-list")` |
| Theo class | `$(".class")` | `$(".book")` |
| Theo thẻ | `$("tag")` | `$("h3")` |
| Theo thuộc tính | `$("[attr=value]")` | `$("[data-genre='fiction']")` |
| Con trực tiếp | `$("parent > child")` | `$("#book-list > .book")` |
| Phần tử đầu/cuối | `:first / :last` | `$(".book:first")` |
| Theo chỉ số | `:eq(n)` | `$(".book:eq(1)")` |

**Bài tập thực hành:**
```javascript
$(function() {
  // 1. Đếm số sách hiện có
  console.log($(".book").length);

  // 2. Lấy tất cả sách thể loại fiction
  $("[data-genre='fiction']").css("border", "2px solid orange");

  // 3. Lấy cuốn sách đầu tiên
  console.log($(".book:first h3").text());
});
```

---

## GIỜ 2: THAO TÁC DOM (60 phút)

### 2.1 Đọc và thay đổi nội dung (15 phút)

```javascript
// Đọc nội dung
console.log($(".book:first h3").text());

// Thay đổi nội dung text
$(".book:first h3").text("Đắc Nhân Tâm (Bản mới)");

// Thay đổi HTML (chèn thẻ được)
$(".book:first .price").html("<strong>120.000đ</strong>");

// Lấy/đặt giá trị input
$("#search-box").val("Nhà giả kim");
console.log($("#search-box").val());
```

### 2.2 Thao tác thuộc tính (attr, prop, data) 

```javascript
// Đọc/đặt thuộc tính
console.log($(".book:first").attr("data-genre"));
$(".book:first").attr("data-genre", "self-help");

// Thao tác với thuộc tính boolean (checked, disabled...)
$("#agree-checkbox").prop("checked", true);

// Đọc dữ liệu data-*
console.log($(".book:first").data("genre"));
```

### 2.3 Thao tác CSS & class

```javascript
// Đọc/đặt CSS
$(".price").css("color", "red");
$(".price").css({ "color": "red", "font-weight": "bold" });

// Thao tác class
$(".book:first").addClass("featured");
$(".book:first").removeClass("featured");
$(".book:first").toggleClass("featured");
console.log($(".book:first").hasClass("featured"));
```

### 2.4 Thêm/xóa phần tử (15 phút)

```javascript
// Thêm sách mới vào cuối danh sách
$("#book-list").append(`
  <div class="book" data-genre="skill">
    <h3>7 Thói Quen Hiệu Quả</h3>
    <p class="price">135.000đ</p>
  </div>
`);

// Thêm vào đầu danh sách
$("#book-list").prepend("<p>📚 Sách mới về!</p>");

// Chèn trước/sau một phần tử
$(".book:first").before("<hr>");
$(".book:last").after("<p>Hết danh sách</p>");

// Xóa phần tử
$(".book[data-genre='skill']:last").remove();

// Làm rỗng nội dung (giữ lại phần tử cha)
$("#book-list").empty();
```

**Bài tập thực hành:** Viết nút "Thêm sách ngẫu nhiên" — mỗi lần bấm sẽ `append()` một cuốn sách mới vào `#book-list`.

---

## GIỜ 3: XỬ LÝ SỰ KIỆN (60 phút)

### 3.1 Sự kiện click cơ bản (15 phút)

```html
<button id="btn-add-cart">Thêm vào giỏ hàng</button>
<span id="cart-count">0</span>
```

```javascript
let cartCount = 0;

$("#btn-add-cart").on("click", function() {
  cartCount++;
  $("#cart-count").text(cartCount);
});

// Cú pháp rút gọn tương đương
$("#btn-add-cart").click(function() {
  console.log("Đã click!");
});
```

### 3.2 Các sự kiện thường dùng (20 phút)

```javascript
// Sự kiện trên input
$("#search-box").on("input", function() {
  console.log("Đang gõ:", $(this).val());
});

$("#search-box").on("keyup", function(e) {
  if (e.key === "Enter") {
    console.log("Tìm kiếm:", $(this).val());
  }
});

// Sự kiện hover (mouseenter/mouseleave)
$(".book").hover(
  function() { $(this).css("box-shadow", "0 0 10px gray"); },
  function() { $(this).css("box-shadow", "none"); }
);

// Sự kiện submit form
$("#login-form").on("submit", function(e) {
  e.preventDefault(); // chặn load lại trang
  console.log("Form đã được submit");
});

// Sự kiện change (cho select, checkbox)
$("#genre-filter").on("change", function() {
  console.log("Thể loại được chọn:", $(this).val());
});
```

### 3.3 `this` trong sự kiện & Event Delegation (25 phút)

```javascript
// $(this) đại diện cho phần tử vừa nhận sự kiện
$(".book").on("click", function() {
  $(this).toggleClass("selected");
  console.log("Bạn vừa chọn:", $(this).find("h3").text());
});
```

**Vấn đề:** Các phần tử `.book` được thêm SAU khi trang tải (VD: qua `append()`) sẽ KHÔNG tự động có sự kiện click.

**Giải pháp — Event Delegation:**
```javascript
// Gắn sự kiện lên phần tử cha CÓ SẴN, "ủy quyền" xuống con (kể cả con thêm sau này)
$("#book-list").on("click", ".book", function() {
  $(this).toggleClass("selected");
});
```

**Bài tập thực hành:** Thêm nút "Xóa" (❌) vào mỗi cuốn sách bằng `append`, sau đó dùng event delegation để xử lý sự kiện click xóa sách đó khỏi danh sách.

---

## GIỜ 4: HIỆU ỨNG & ANIMATION (60 phút)

### 4.1 Ẩn/hiện cơ bản (15 phút)

```javascript
$("#btn-toggle-details").on("click", function() {
  $(".book-details").toggle();       // Ẩn/hiện tức thì
  // $(".book-details").toggle(400);  // Ẩn/hiện có hiệu ứng (ms)
});

$(".book-details").hide();
$(".book-details").show(300);
```

### 4.2 Fade & Slide (20 phút)

```javascript
// Hiệu ứng mờ dần
$(".notification").fadeIn(500);
$(".notification").fadeOut(500);
$(".notification").fadeToggle(500);
$(".notification").fadeTo(500, 0.5); // mờ đến độ trong suốt 0.5

// Hiệu ứng trượt
$(".book-details").slideDown(400);
$(".book-details").slideUp(400);
$(".book-details").slideToggle(400);
```

**Ví dụ thực tế: Accordion xem chi tiết sách**
```javascript
$("#book-list").on("click", ".book h3", function() {
  $(this).siblings(".book-details").slideToggle(300);
});
```

### 4.3 Animate tùy chỉnh (15 phút)

```javascript
$("#btn-highlight").on("click", function() {
  $(".book:first").animate({
    width: "80%",
    opacity: 0.7,
    marginLeft: "20px"
  }, 600);
});

// Callback sau khi animation hoàn tất
$(".notification").fadeIn(400, function() {
  console.log("Đã hiện xong thông báo!");
});
```

### 4.4 Điều khiển thời gian & hàng đợi hiệu ứng (10 phút)

```javascript
$(".book:first")
  .fadeOut(300)
  .fadeIn(300)
  .delay(500)
  .slideUp(300);

// Dừng animation đang chạy (tránh hiệu ứng chồng chất khi click liên tục)
$(".book-details").stop(true, true).slideToggle(300);
```

**Bài tập thực hành:** Làm nút "Giỏ hàng" khi thêm sách sẽ có hiệu ứng `animate` phóng to nhẹ rồi trở lại kích thước cũ (hiệu ứng "nảy" xác nhận đã thêm).

---

## GIỜ 5: AJAX VỚI JQUERY (60 phút)

### 5.1 Giới thiệu AJAX (10 phút)
AJAX cho phép gửi/nhận dữ liệu với server **mà không cần tải lại trang**. jQuery giúp việc này gọn hơn nhiều so với `fetch` hoặc `XMLHttpRequest` thuần.

### 5.2 `$.get()` và `$.post()` (20 phút)

```javascript
// GET request — lấy danh sách sách từ API
$.get("https://api.example.com/books", function(data) {
  console.log("Dữ liệu nhận được:", data);
  data.forEach(book => {
    $("#book-list").append(`<div class="book"><h3>${book.title}</h3></div>`);
  });
});

// POST request — gửi đơn hàng
$.post("https://api.example.com/orders", {
  bookId: 12,
  quantity: 2
}, function(response) {
  console.log("Đặt hàng thành công:", response);
});
```

### 5.3 `$.ajax()` — cách dùng đầy đủ, linh hoạt nhất (20 phút)

```javascript
$.ajax({
  url: "https://api.example.com/books",
  method: "GET",
  dataType: "json",
  beforeSend: function() {
    $("#loading").show();
  },
  success: function(data) {
    console.log("Thành công:", data);
    renderBooks(data);
  },
  error: function(xhr, status, error) {
    console.error("Lỗi:", error);
    $("#error-msg").text("Không tải được danh sách sách!").show();
  },
  complete: function() {
    $("#loading").hide();
  }
});

function renderBooks(books) {
  $("#book-list").empty();
  books.forEach(book => {
    $("#book-list").append(`
      <div class="book" data-id="${book.id}">
        <h3>${book.title}</h3>
        <p class="price">${book.price}đ</p>
      </div>
    `);
  });
}
```

### 5.4 Ví dụ thực tế: Tìm kiếm sách "sống" (Live Search) (10 phút)

```javascript
let searchTimer;

$("#search-box").on("input", function() {
  clearTimeout(searchTimer);
  const keyword = $(this).val();

  // Debounce — chỉ gọi API sau khi ngừng gõ 400ms
  searchTimer = setTimeout(function() {
    $.ajax({
      url: "https://api.example.com/books/search",
      data: { q: keyword },
      success: function(results) {
        renderBooks(results);
      }
    });
  }, 400);
});
```

**Bài tập thực hành:** Dùng [FakeStoreAPI](https://fakestoreapi.com/products) (miễn phí, không cần key) để tải danh sách "sách" (dùng tạm dữ liệu sản phẩm) và hiển thị ra `#book-list` khi trang tải xong.

---

## GIỜ 6: NÂNG CAO — CHAINING, VALIDATION, MINI PROJECT (60 phút)

### 6.1 Method Chaining (10 phút)

```javascript
// Thay vì viết nhiều dòng:
$(".book:first").addClass("featured");
$(".book:first").css("border", "2px solid gold");
$(".book:first").fadeIn(300);

// Gộp lại bằng chaining (mỗi hàm jQuery trả về chính đối tượng đó)
$(".book:first")
  .addClass("featured")
  .css("border", "2px solid gold")
  .fadeIn(300);
```

### 6.2 Duyệt DOM nâng cao: parent, children, siblings, find (10 phút)

```javascript
$(".book").on("click", "h3", function() {
  $(this).parent().toggleClass("selected");   // phần tử cha
  $(this).siblings(".price").css("color", "red"); // phần tử anh em
  $(this).closest(".book").find(".price").text("Đang chọn..."); // cha gần nhất + tìm con
});
```

### 6.3 Form Validation thực tế (20 phút)

```html
<form id="checkout-form">
  <input type="text" name="fullname" placeholder="Họ tên">
  <input type="email" name="email" placeholder="Email">
  <input type="tel" name="phone" placeholder="Số điện thoại">
  <button type="submit">Đặt hàng</button>
</form>
<div id="form-errors"></div>
```

```javascript
$("#checkout-form").on("submit", function(e) {
  e.preventDefault();
  $("#form-errors").empty();

  let errors = [];
  const fullname = $("input[name='fullname']").val().trim();
  const email = $("input[name='email']").val().trim();
  const phone = $("input[name='phone']").val().trim();

  if (fullname === "") errors.push("Vui lòng nhập họ tên.");
  if (!/^\S+@\S+\.\S+$/.test(email)) errors.push("Email không hợp lệ.");
  if (!/^0\d{9}$/.test(phone)) errors.push("Số điện thoại không hợp lệ.");

  if (errors.length > 0) {
    errors.forEach(err => {
      $("#form-errors").append(`<p style="color:red">• ${err}</p>`);
    });
    return;
  }

  console.log("Dữ liệu hợp lệ, tiến hành đặt hàng...", { fullname, email, phone });
  $(this)[0].reset();
  alert("Đặt hàng thành công!");
});
```

### 6.4 Mini Project tổng hợp: "Giỏ hàng Bookstore Online" (20 phút)

**Yêu cầu:** Kết hợp toàn bộ kiến thức trong 6 giờ để xây dựng:
1. Danh sách sách hiển thị từ mảng dữ liệu (dùng `append` để render)
2. Nút "Thêm vào giỏ" trên mỗi sách (event delegation)
3. Giỏ hàng cập nhật số lượng + tổng tiền theo thời gian thực
4. Hiệu ứng `fadeIn`/`animate` khi thêm sách vào giỏ
5. Ô tìm kiếm lọc sách theo tên (dùng `filter`/`:contains`)
6. Form thông tin giao hàng có validation

```javascript
const books = [
  { id: 1, title: "Đắc Nhân Tâm", price: 120000 },
  { id: 2, title: "Nhà Giả Kim", price: 89000 },
  { id: 3, title: "Tư Duy Nhanh và Chậm", price: 150000 }
];

let cart = [];

function renderBookList() {
  $("#book-list").empty();
  books.forEach(book => {
    $("#book-list").append(`
      <div class="book" data-id="${book.id}">
        <h3>${book.title}</h3>
        <p class="price">${book.price.toLocaleString()}đ</p>
        <button class="btn-add">Thêm vào giỏ</button>
      </div>
    `);
  });
}

function renderCart() {
  const total = cart.reduce((sum, item) => sum + item.price, 0);
  $("#cart-count").text(cart.length);
  $("#cart-total").text(total.toLocaleString() + "đ");
}

$("#book-list").on("click", ".btn-add", function() {
  const bookId = $(this).closest(".book").data("id");
  const book = books.find(b => b.id === bookId);
  cart.push(book);
  renderCart();

  $(this).text("Đã thêm ✓").addClass("added");
  $(this).animate({ opacity: 0.5 }, 150).animate({ opacity: 1 }, 150);
});

$("#search-box").on("input", function() {
  const keyword = $(this).val().toLowerCase();
  $(".book").each(function() {
    const title = $(this).find("h3").text().toLowerCase();
    $(this).toggle(title.includes(keyword));
  });
});

$(function() {
  renderBookList();
  renderCart();
});
```

---

## TỔNG KẾT — CHECKLIST KIẾN THỨC SAU 6 GIỜ

- [ ] Nhúng jQuery và dùng `$(document).ready()`
- [ ] Selectors: id, class, tag, attribute, `:first`/`:last`/`:eq()`
- [ ] Đọc/ghi nội dung: `text()`, `html()`, `val()`
- [ ] Thuộc tính & class: `attr()`, `prop()`, `data()`, `addClass()`/`removeClass()`/`toggleClass()`
- [ ] Thêm/xóa phần tử: `append()`, `prepend()`, `before()`, `after()`, `remove()`, `empty()`
- [ ] Sự kiện: `click()`, `on()`, `hover()`, `submit`, event delegation
- [ ] Hiệu ứng: `hide/show`, `fadeIn/fadeOut`, `slideUp/slideDown`, `animate()`
- [ ] AJAX: `$.get()`, `$.post()`, `$.ajax()`
- [ ] Nâng cao: chaining, duyệt DOM (`parent`, `siblings`, `closest`, `find`), form validation
- [ ] Áp dụng được vào một mini project hoàn chỉnh

**Gợi ý bài tập về nhà:** Mở rộng mini project — thêm chức năng xóa sách khỏi giỏ hàng, lưu giỏ hàng vào `localStorage` để không mất dữ liệu khi tải lại trang.
