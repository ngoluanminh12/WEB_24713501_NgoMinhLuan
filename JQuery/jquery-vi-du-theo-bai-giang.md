# JQUERY — VÍ DỤ THEO TỪNG BÀI GIẢNG (COPY & CHẠY)

Dùng kèm tài liệu "Hướng dẫn jQuery 6 giờ". Gồm **19 bài**, mỗi bài có: **Mục tiêu → HTML → JS → Giải thích → Gợi ý nói khi giảng**.

---

## 0. KHUNG HTML DÙNG CHUNG (tạo 1 lần, dùng cho mọi bài)

Tạo file `index.html`. Mỗi bài chỉ cần **thay phần `<body>` và phần `<script>`**.

```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>jQuery - Bookstore Online</title>
  <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; max-width: 760px; margin: 30px auto; padding: 0 15px; }
    .book { border: 1px solid #ccc; border-radius: 6px; padding: 10px; margin: 8px 0; }
    .price { color: #c0392b; font-weight: bold; margin: 4px 0; }
    .featured { background: #fff3cd; border-color: #f1c40f; }
    .selected { background: #d6eaf8; }
    .added { background: #27ae60; color: #fff; }
    .error { color: red; }
    button { padding: 6px 12px; cursor: pointer; margin: 4px 2px; }
    input, select { padding: 6px; margin: 4px 0; }
    #box { width: 100px; height: 100px; background: #3498db; }
  </style>
</head>
<body>

  <!-- ===== DÁN HTML CỦA TỪNG BÀI VÀO ĐÂY ===== -->

  <script>
    $(function () {
      // ===== DÁN JS CỦA TỪNG BÀI VÀO ĐÂY =====
    });
  </script>
</body>
</html>
```

**Giải thích:** `$(function(){ ... })` chờ trang tải xong rồi mới chạy code — nên mọi đoạn JS bên dưới đều nằm **bên trong** hàm này. Mở **F12 → Console** để xem `console.log`.

---

# GIỜ 1 — NHẬP MÔN & SELECTORS

## Bài 1: Hello jQuery

**Mục tiêu:** Kiểm tra jQuery đã nhúng thành công, hiểu cú pháp `$(selector).action()`.

**HTML:**
```html
<h1 id="title">Chào mừng đến Bookstore Online</h1>
<p class="note">Đây là một đoạn mô tả.</p>
```

**JS:**
```javascript
console.log("jQuery phiên bản:", $.fn.jquery);
$("#title").css("color", "darkblue");
$(".note").text("Nội dung này đã bị jQuery thay đổi!");
```

**Giải thích:**
- `$.fn.jquery` in ra phiên bản → nếu thấy `3.7.1` là nhúng thành công.
- `$("#title")` = tìm phần tử, `.css(...)` = làm gì đó với nó.

**Gợi ý nói:** "Công thức của jQuery chỉ có 2 bước: **chọn** phần tử, rồi **hành động**."

---

## Bài 2: Selectors cơ bản (ID, class, thẻ)

**Mục tiêu:** Chọn phần tử theo ID, class, tên thẻ.

**HTML:**
```html
<div id="book-list">
  <div class="book"><h3>Đắc Nhân Tâm</h3><p class="price">120.000đ</p></div>
  <div class="book"><h3>Tư Duy Nhanh và Chậm</h3><p class="price">150.000đ</p></div>
  <div class="book"><h3>Nhà Giả Kim</h3><p class="price">89.000đ</p></div>
</div>
```

**JS:**
```javascript
$("#book-list").css("background", "#f4f4f4");   // theo ID
$(".book").css("border-color", "orange");        // theo class
$("h3").css("color", "green");                   // theo thẻ
console.log("Số cuốn sách:", $(".book").length); // đếm phần tử
```

**Giải thích:** `#` = ID (duy nhất), `.` = class (nhiều phần tử), không có ký hiệu = tên thẻ. `.length` cho biết tìm được bao nhiêu phần tử.

**Gợi ý nói:** Cho học viên đổi `.book` thành `.price` và quan sát kết quả.

---

## Bài 3: Selectors nâng cao & lọc

**Mục tiêu:** Chọn theo vị trí, thuộc tính, quan hệ cha-con.

**HTML:**
```html
<div id="book-list">
  <div class="book" data-genre="fiction"><h3>Nhà Giả Kim</h3></div>
  <div class="book" data-genre="skill"><h3>Đắc Nhân Tâm</h3></div>
  <div class="book" data-genre="fiction"><h3>Hai Số Phận</h3></div>
  <div class="book" data-genre="skill"><h3>Tư Duy Nhanh và Chậm</h3></div>
</div>
```

**JS:**
```javascript
$(".book:first").css("background", "#fadbd8");              // đầu tiên
$(".book:last").css("background", "#d5f5e3");               // cuối cùng
$(".book:eq(1)").css("background", "#fcf3cf");              // vị trí số 1 (đếm từ 0)
$(".book:even").css("font-style", "italic");                // vị trí chẵn (0,2,..)
$("[data-genre='fiction']").css("border", "3px solid blue");// theo thuộc tính
$("#book-list > .book").css("padding", "15px");             // con trực tiếp
$(".book h3:contains('Tư Duy')").css("color", "red");       // chứa chữ
```

**Giải thích:** `:eq(n)` đếm từ **0**. `[attr='giá trị']` lọc theo thuộc tính. `:contains('...')` lọc theo nội dung chữ.

---

# GIỜ 2 — THAO TÁC DOM

## Bài 4: text(), html(), val()

**Mục tiêu:** Đọc và thay đổi nội dung phần tử.

**HTML:**
```html
<h2 id="title">Sách bán chạy</h2>
<p id="price">120.000đ</p>
<input type="text" id="search-box" placeholder="Nhập tên sách">
<button id="btn-read">Đọc</button>
<button id="btn-change">Thay đổi</button>
```

**JS:**
```javascript
$("#btn-read").on("click", function () {
  console.log("text:", $("#title").text());
  console.log("html:", $("#price").html());
  console.log("val :", $("#search-box").val());
});

$("#btn-change").on("click", function () {
  $("#title").text("Sách mới về <b>tuần này</b>");   // <b> hiện nguyên chữ
  $("#price").html("<strong>99.000đ</strong>");       // <strong> được render
  $("#search-box").val("Nhà giả kim");
});
```

**Giải thích:**
- `text()` coi mọi thứ là **chữ thuần** (an toàn).
- `html()` **hiểu thẻ HTML**.
- `val()` dành riêng cho `input/select/textarea`.

**Gợi ý nói:** So sánh dòng `#title` và `#price` để thấy khác biệt `text` vs `html`.

---

## Bài 5: attr(), prop(), data()

**Mục tiêu:** Làm việc với thuộc tính của thẻ.

**HTML:**
```html
<img id="cover" src="https://via.placeholder.com/120x160" alt="Bìa sách">
<div id="book" data-id="12" data-genre="fiction">Nhà Giả Kim</div>
<label><input type="checkbox" id="agree"> Tôi đồng ý điều khoản</label>
<button id="btn-run">Chạy</button>
```

**JS:**
```javascript
$("#btn-run").on("click", function () {
  // attr: đọc/ghi thuộc tính HTML
  console.log("alt =", $("#cover").attr("alt"));
  $("#cover").attr("alt", "Bìa Nhà Giả Kim");

  // data: đọc thuộc tính data-*
  console.log("id =", $("#book").data("id"));
  console.log("genre =", $("#book").data("genre"));

  // prop: dùng cho true/false (checked, disabled)
  $("#agree").prop("checked", true);
  console.log("Đã tick?", $("#agree").prop("checked"));
});
```

**Giải thích:** Với `checked`, `disabled`, `selected` → **dùng `prop()`**, không dùng `attr()`. `data("id")` bỏ tiền tố `data-`.

---

## Bài 6: CSS và class

**Mục tiêu:** Đổi giao diện bằng `css()` và `addClass/removeClass/toggleClass`.

**HTML:**
```html
<div class="book" id="b1"><h3>Đắc Nhân Tâm</h3><p class="price">120.000đ</p></div>
<button id="btn-css">Đổi CSS</button>
<button id="btn-add">addClass</button>
<button id="btn-remove">removeClass</button>
<button id="btn-toggle">toggleClass</button>
```

**JS:**
```javascript
$("#btn-css").on("click", function () {
  $("#b1 .price").css({ "color": "green", "font-size": "24px" });
});
$("#btn-add").on("click", function () { $("#b1").addClass("featured"); });
$("#btn-remove").on("click", function () { $("#b1").removeClass("featured"); });
$("#btn-toggle").on("click", function () {
  $("#b1").toggleClass("featured");
  console.log("Đang có class featured?", $("#b1").hasClass("featured"));
});
```

**Giải thích:** `css({...})` truyền nhiều thuộc tính cùng lúc. **Nên ưu tiên dùng class** (`toggleClass`) hơn `css()` để tách giao diện khỏi logic.

---

## Bài 7: Thêm/xóa phần tử — "Thêm sách mới"

**Mục tiêu:** `append`, `prepend`, `before`, `after`, `remove`, `empty`.

**HTML:**
```html
<input id="in-title" placeholder="Tên sách">
<input id="in-price" placeholder="Giá">
<button id="btn-add">Thêm sách</button>
<button id="btn-prepend">Thêm lên đầu</button>
<button id="btn-clear">Xóa hết</button>
<div id="book-list"></div>
```

**JS:**
```javascript
function createBook(title, price) {
  return `<div class="book"><h3>${title}</h3><p class="price">${price}đ</p></div>`;
}

$("#btn-add").on("click", function () {
  const title = $("#in-title").val().trim();
  const price = $("#in-price").val().trim();
  if (!title || !price) { alert("Nhập đủ tên và giá!"); return; }
  $("#book-list").append(createBook(title, price));   // thêm xuống cuối
  $("#in-title, #in-price").val("");                  // làm trống ô nhập
});

$("#btn-prepend").on("click", function () {
  $("#book-list").prepend(createBook("Sách ưu tiên", "0"));  // thêm lên đầu
});

$("#btn-clear").on("click", function () {
  $("#book-list").empty();   // xóa nội dung bên trong, giữ lại #book-list
});
```

**Giải thích:** Template string (dấu `` ` ``) cho phép chèn biến bằng `${...}`. `append` = cuối, `prepend` = đầu, `empty` = xóa con, `remove` = xóa chính nó.

---

# GIỜ 3 — SỰ KIỆN

## Bài 8: Sự kiện click — Đếm giỏ hàng

**Mục tiêu:** Làm quen `.on("click")`, biến đếm.

**HTML:**
```html
<button id="btn-cart">🛒 Thêm vào giỏ</button>
<button id="btn-reset">Làm mới</button>
<p>Số sách trong giỏ: <strong id="cart-count">0</strong></p>
```

**JS:**
```javascript
let count = 0;

$("#btn-cart").on("click", function () {
  count++;
  $("#cart-count").text(count);
});

$("#btn-reset").on("click", function () {
  count = 0;
  $("#cart-count").text(count);
});
```

**Giải thích:** Biến `count` nằm **ngoài** hàm click nên giá trị được giữ giữa các lần bấm. Mỗi lần bấm: tăng biến → cập nhật giao diện.

---

## Bài 9: input, keyup, hover, change

**Mục tiêu:** Các sự kiện thường gặp khi làm form.

**HTML:**
```html
<input id="search-box" placeholder="Gõ để tìm sách...">
<p>Bạn đang gõ: <span id="preview"></span></p>

<select id="genre">
  <option value="">-- Chọn thể loại --</option>
  <option value="fiction">Tiểu thuyết</option>
  <option value="skill">Kỹ năng</option>
</select>
<p>Thể loại đã chọn: <span id="genre-show"></span></p>

<div class="book" id="hover-book"><h3>Di chuột vào tôi</h3></div>
```

**JS:**
```javascript
// input: chạy mỗi khi nội dung ô nhập thay đổi
$("#search-box").on("input", function () {
  $("#preview").text($(this).val());
});

// keyup: bắt phím Enter
$("#search-box").on("keyup", function (e) {
  if (e.key === "Enter") alert("Tìm kiếm: " + $(this).val());
});

// change: dùng cho select, checkbox
$("#genre").on("change", function () {
  $("#genre-show").text($(this).val());
});

// hover: chuột vào / chuột ra
$("#hover-book").hover(
  function () { $(this).addClass("featured"); },
  function () { $(this).removeClass("featured"); }
);
```

**Giải thích:** `$(this)` là **chính phần tử đang xảy ra sự kiện**. `hover(vào, ra)` nhận 2 hàm.

---

## Bài 10: Event Delegation — Chọn & Xóa sách (bài quan trọng)

**Mục tiêu:** Hiểu vì sao phần tử thêm động không nhận sự kiện, và cách khắc phục.

**HTML:**
```html
<button id="btn-add">Thêm sách</button>
<div id="book-list"></div>
```

**JS (bản SAI – cho học viên thử trước):**
```javascript
let n = 0;
$("#btn-add").on("click", function () {
  n++;
  $("#book-list").append(
    `<div class="book"><h3>Sách số ${n}</h3><button class="btn-del">❌ Xóa</button></div>`
  );
});

// ❌ Không chạy với sách được thêm SAU này
$(".btn-del").on("click", function () {
  $(this).closest(".book").remove();
});
```

**JS (bản ĐÚNG – thay thế đoạn trên):**
```javascript
// ✅ Gắn lên phần tử cha CÓ SẴN, "ủy quyền" xuống con
$("#book-list").on("click", ".btn-del", function () {
  $(this).closest(".book").fadeOut(300, function () { $(this).remove(); });
});

// Chọn/bỏ chọn sách khi bấm vào
$("#book-list").on("click", ".book", function () {
  $(this).toggleClass("selected");
});
```

**Giải thích:** Lúc chạy `$(".btn-del").on(...)` thì nút chưa tồn tại → không gắn được. Gắn vào `#book-list` (đã có từ đầu) với tham số thứ 2 `".btn-del"` thì mọi nút, kể cả nút tạo sau, đều hoạt động.

**Gợi ý nói:** Đây là lỗi **số 1** của người mới học jQuery — nên cho học viên tự gặp lỗi trước.

---

# GIỜ 4 — HIỆU ỨNG

## Bài 11: show / hide / toggle

**HTML:**
```html
<button id="btn-toggle">Xem/ẩn mô tả</button>
<div class="book">
  <h3>Đắc Nhân Tâm</h3>
  <p id="desc">Cuốn sách nổi tiếng về nghệ thuật ứng xử và giao tiếp.</p>
</div>
<button id="btn-hide">Ẩn</button> <button id="btn-show">Hiện</button>
```

**JS:**
```javascript
$("#btn-toggle").on("click", function () { $("#desc").toggle(400); });
$("#btn-hide").on("click", function () { $("#desc").hide(); });      // tức thì
$("#btn-show").on("click", function () { $("#desc").show(800); });   // trong 800ms
```

**Giải thích:** Có truyền số (ms) → có hiệu ứng, không truyền → đổi tức thì. `toggle` tự đảo trạng thái.

---

## Bài 12: Fade & Slide — Accordion chi tiết sách

**HTML:**
```html
<div id="book-list">
  <div class="book"><h3 style="cursor:pointer">▶ Đắc Nhân Tâm</h3>
    <div class="details" style="display:none">Tác giả: Dale Carnegie. Giá: 120.000đ.</div></div>
  <div class="book"><h3 style="cursor:pointer">▶ Nhà Giả Kim</h3>
    <div class="details" style="display:none">Tác giả: Paulo Coelho. Giá: 89.000đ.</div></div>
</div>
<hr>
<button id="b1">fadeToggle</button> <button id="b2">fadeTo 0.3</button>
<p id="msg">Thông báo mẫu</p>
```

**JS:**
```javascript
// Accordion: bấm tiêu đề → trượt mở chi tiết
$("#book-list").on("click", "h3", function () {
  $(this).siblings(".details").stop(true, true).slideToggle(300);
});

$("#b1").on("click", function () { $("#msg").fadeToggle(500); });
$("#b2").on("click", function () { $("#msg").fadeTo(500, 0.3); });
```

**Giải thích:** `siblings(".details")` tìm phần tử **anh em** có class `details`. `stop(true, true)` ngăn hiệu ứng bị chồng chéo khi bấm liên tục.

---

## Bài 13: animate() & callback

**HTML:**
```html
<button id="btn-move">Di chuyển</button>
<button id="btn-grow">Phóng to rồi thu lại</button>
<button id="btn-stop">Dừng</button>
<div id="box" style="position:relative"></div>
```

**JS:**
```javascript
$("#btn-move").on("click", function () {
  $("#box").animate({ left: "300px", opacity: 0.5 }, 800, function () {
    console.log("Đã di chuyển xong!");        // callback sau khi xong
  });
});

$("#btn-grow").on("click", function () {
  $("#box")
    .animate({ width: "160px", height: "160px" }, 300)
    .delay(300)
    .animate({ width: "100px", height: "100px" }, 300);
});

$("#btn-stop").on("click", function () { $("#box").stop(true); });
```

**Giải thích:** `animate` chỉ chạy với thuộc tính **số** (width, left, opacity…). Các hiệu ứng viết liền nhau sẽ xếp hàng chạy lần lượt. Muốn animate `left` thì phần tử cần `position: relative/absolute`.

---

# GIỜ 5 — AJAX

> Cả 3 bài dùng **FakeStoreAPI** (miễn phí, không cần key): `https://fakestoreapi.com/products`

## Bài 14: $.get() — Tải danh sách sản phẩm

**HTML:**
```html
<button id="btn-load">Tải danh sách</button>
<div id="book-list"></div>
```

**JS:**
```javascript
$("#btn-load").on("click", function () {
  $.get("https://fakestoreapi.com/products?limit=5", function (data) {
    console.log(data);                       // xem cấu trúc dữ liệu trước
    $("#book-list").empty();
    data.forEach(function (item) {
      $("#book-list").append(
        `<div class="book"><h3>${item.title}</h3><p class="price">$${item.price}</p></div>`
      );
    });
  });
});
```

**Giải thích:** `$.get(url, callback)` — server trả JSON, jQuery **tự chuyển thành mảng/đối tượng JS**. Luôn `console.log(data)` trước để biết cấu trúc.

---

## Bài 15: $.ajax() — Loading, lỗi, hoàn tất

**HTML:**
```html
<button id="btn-ok">Gọi API đúng</button>
<button id="btn-fail">Gọi API sai (giả lập lỗi)</button>
<p id="loading" style="display:none">⏳ Đang tải...</p>
<p id="err" class="error"></p>
<div id="book-list"></div>
```

**JS:**
```javascript
function loadData(url) {
  $.ajax({
    url: url,
    method: "GET",
    dataType: "json",
    beforeSend: function () {              // trước khi gửi
      $("#loading").show(); $("#err").text("");
    },
    success: function (data) {             // thành công
      $("#book-list").empty();
      data.slice(0, 4).forEach(function (p) {
        $("#book-list").append(`<div class="book"><h3>${p.title}</h3></div>`);
      });
    },
    error: function () {                   // thất bại
      $("#err").text("Không tải được dữ liệu, vui lòng thử lại!");
    },
    complete: function () {                // luôn chạy cuối cùng
      $("#loading").hide();
    }
  });
}

$("#btn-ok").on("click", function () { loadData("https://fakestoreapi.com/products"); });
$("#btn-fail").on("click", function () { loadData("https://fakestoreapi.com/khong-ton-tai"); });
```

**Giải thích:** `beforeSend → success/error → complete` là vòng đời chuẩn của 1 request. **Luôn phải xử lý `error`** — đây là điểm khác biệt giữa code demo và code thực tế.

---

## Bài 16: Live Search có debounce

**HTML:**
```html
<input id="search-box" placeholder="Gõ tên sản phẩm..." style="width:100%">
<p id="status"></p>
<div id="book-list"></div>
```

**JS:**
```javascript
let allProducts = [];
let timer;

// Tải dữ liệu 1 lần khi mở trang
$.get("https://fakestoreapi.com/products", function (data) {
  allProducts = data;
  render(allProducts);
});

function render(list) {
  $("#book-list").empty();
  $("#status").text("Tìm thấy " + list.length + " kết quả");
  list.forEach(function (p) {
    $("#book-list").append(`<div class="book"><h3>${p.title}</h3><p class="price">$${p.price}</p></div>`);
  });
}

$("#search-box").on("input", function () {
  clearTimeout(timer);                       // huỷ lần hẹn giờ trước
  const kw = $(this).val().toLowerCase();
  timer = setTimeout(function () {           // chỉ lọc sau khi ngừng gõ 300ms
    render(allProducts.filter(p => p.title.toLowerCase().includes(kw)));
  }, 300);
});
```

**Giải thích:** **Debounce** = chờ người dùng ngừng gõ rồi mới xử lý, tránh gọi liên tục mỗi phím. Ở đây lọc ở phía client; nếu API hỗ trợ tìm kiếm thì thay bằng `$.ajax` truyền `data: { q: kw }`.

---

# GIỜ 6 — NÂNG CAO & DỰ ÁN

## Bài 17: Chaining & duyệt DOM

**HTML:**
```html
<div id="book-list">
  <div class="book"><h3>Đắc Nhân Tâm</h3><p class="price">120.000đ</p><span class="tag">Kỹ năng</span></div>
  <div class="book"><h3>Nhà Giả Kim</h3><p class="price">89.000đ</p><span class="tag">Tiểu thuyết</span></div>
</div>
<button id="btn-chain">Chaining</button>
```

**JS:**
```javascript
// 1. Chaining: nối nhiều thao tác trên cùng một phần tử
$("#btn-chain").on("click", function () {
  $(".book:first")
    .addClass("featured")
    .css("border-width", "3px")
    .fadeOut(300)
    .fadeIn(300);
});

// 2. Duyệt DOM từ phần tử được bấm
$("#book-list").on("click", "h3", function () {
  const $h3 = $(this);
  $h3.parent().toggleClass("selected");                       // cha
  $h3.siblings(".price").css("color", "blue");                // anh em
  $h3.closest("#book-list").find(".tag").fadeToggle();        // lên tổ tiên rồi tìm xuống
  console.log("Cuốn sách kế tiếp:", $h3.parent().next().find("h3").text());
});

// 3. each: lặp qua từng phần tử
$(".book").each(function (index) {
  $(this).find("h3").prepend((index + 1) + ". ");
});
```

**Giải thích:** `parent / siblings / next / closest / find` giúp "đi" quanh cây DOM mà không cần đặt ID cho mọi thứ. `each` lặp qua từng phần tử, `index` bắt đầu từ 0.

---

## Bài 18: Validation form

**HTML:**
```html
<form id="checkout-form">
  <input type="text" name="fullname" placeholder="Họ tên"><br>
  <input type="email" name="email" placeholder="Email"><br>
  <input type="tel" name="phone" placeholder="SĐT (10 số, bắt đầu bằng 0)"><br>
  <button type="submit">Đặt hàng</button>
</form>
<div id="form-errors"></div>
```

**JS:**
```javascript
$("#checkout-form").on("submit", function (e) {
  e.preventDefault();                        // chặn tải lại trang
  $("#form-errors").empty();
  $("input").css("border", "");

  const fullname = $("[name='fullname']").val().trim();
  const email = $("[name='email']").val().trim();
  const phone = $("[name='phone']").val().trim();
  const errors = [];

  if (fullname === "") { errors.push("Vui lòng nhập họ tên."); $("[name='fullname']").css("border", "1px solid red"); }
  if (!/^\S+@\S+\.\S+$/.test(email)) { errors.push("Email không hợp lệ."); $("[name='email']").css("border", "1px solid red"); }
  if (!/^0\d{9}$/.test(phone)) { errors.push("Số điện thoại không hợp lệ."); $("[name='phone']").css("border", "1px solid red"); }

  if (errors.length) {
    errors.forEach(function (msg) { $("#form-errors").append(`<p class="error">• ${msg}</p>`); });
    return;
  }
  alert("Đặt hàng thành công cho " + fullname + "!");
  this.reset();
});
```

**Giải thích:** `e.preventDefault()` rất quan trọng. Gom lỗi vào mảng `errors` rồi hiển thị một lượt. `/^0\d{9}$/` = bắt đầu bằng 0, theo sau 9 chữ số.

---

## Bài 19: MINI PROJECT — Giỏ hàng Bookstore Online

**Mục tiêu:** Tổng hợp: render, delegation, cập nhật tổng tiền, tìm kiếm, hiệu ứng.

**HTML:**
```html
<h2>📚 Bookstore Online</h2>
<input id="search-box" placeholder="Tìm sách..." style="width:100%">
<div id="book-list"></div>

<h3>🛒 Giỏ hàng (<span id="cart-count">0</span>)</h3>
<div id="cart"></div>
<p><strong>Tổng tiền: <span id="cart-total">0đ</span></strong></p>
<button id="btn-clear">Xóa giỏ hàng</button>
```

**JS:**
```javascript
const books = [
  { id: 1, title: "Đắc Nhân Tâm", price: 120000 },
  { id: 2, title: "Nhà Giả Kim", price: 89000 },
  { id: 3, title: "Tư Duy Nhanh và Chậm", price: 150000 },
  { id: 4, title: "Hai Số Phận", price: 135000 }
];
let cart = [];   // mỗi phần tử: { id, title, price, qty }

const money = n => n.toLocaleString("vi-VN") + "đ";

function renderBooks() {
  $("#book-list").empty();
  books.forEach(b => {
    $("#book-list").append(`
      <div class="book" data-id="${b.id}">
        <h3>${b.title}</h3>
        <p class="price">${money(b.price)}</p>
        <button class="btn-add">Thêm vào giỏ</button>
      </div>`);
  });
}

function renderCart() {
  $("#cart").empty();
  cart.forEach(item => {
    $("#cart").append(`
      <div class="book" data-id="${item.id}">
        ${item.title} — ${money(item.price)} × ${item.qty}
        <button class="btn-minus">−</button>
        <button class="btn-plus">+</button>
        <button class="btn-remove">❌</button>
      </div>`);
  });
  const totalQty = cart.reduce((s, i) => s + i.qty, 0);
  const totalMoney = cart.reduce((s, i) => s + i.price * i.qty, 0);
  $("#cart-count").text(totalQty);
  $("#cart-total").text(money(totalMoney));
}

// Thêm vào giỏ
$("#book-list").on("click", ".btn-add", function () {
  const id = $(this).closest(".book").data("id");
  const found = cart.find(i => i.id === id);
  if (found) found.qty++;
  else cart.push({ ...books.find(b => b.id === id), qty: 1 });
  renderCart();
  $(this).addClass("added").text("Đã thêm ✓")
         .animate({ opacity: 0.4 }, 150).animate({ opacity: 1 }, 150);
});

// Tăng / giảm / xóa trong giỏ
$("#cart").on("click", ".btn-plus", function () {
  const id = $(this).closest(".book").data("id");
  cart.find(i => i.id === id).qty++;
  renderCart();
});
$("#cart").on("click", ".btn-minus", function () {
  const id = $(this).closest(".book").data("id");
  const item = cart.find(i => i.id === id);
  item.qty--;
  if (item.qty <= 0) cart = cart.filter(i => i.id !== id);
  renderCart();
});
$("#cart").on("click", ".btn-remove", function () {
  const id = $(this).closest(".book").data("id");
  cart = cart.filter(i => i.id !== id);
  renderCart();
});

$("#btn-clear").on("click", function () { cart = []; renderCart(); });

// Tìm kiếm
$("#search-box").on("input", function () {
  const kw = $(this).val().toLowerCase();
  $("#book-list .book").each(function () {
    $(this).toggle($(this).find("h3").text().toLowerCase().includes(kw));
  });
});

renderBooks();
renderCart();
```

**Giải thích:**
- **Dữ liệu (`books`, `cart`) tách khỏi giao diện** — mỗi lần dữ liệu đổi thì gọi lại `renderCart()`. Đây là tư duy nền tảng dẫn tới React sau này.
- Mọi nút thêm động đều dùng **event delegation** (bài 10).
- `toggle(true/false)` = hiện/ẩn theo điều kiện → viết lọc chỉ 1 dòng.

**Bài tập mở rộng (giao về nhà):**
1. Lưu `cart` vào `localStorage` để tải lại trang vẫn còn.
2. Thêm nút "Thanh toán" mở form (bài 18) bằng `slideDown`.
3. Thêm bộ lọc theo thể loại (bài 9 – `change`).

---

## PHỤ LỤC: LỖI THƯỜNG GẶP KHI GIẢNG

| Triệu chứng | Nguyên nhân hay gặp | Cách xử lý |
|---|---|---|
| `$ is not defined` | Quên nhúng jQuery hoặc nhúng sau code | Đặt thẻ `<script src=jquery>` **trước** code của mình |
| Chọn đúng mà không có tác dụng | Code chạy trước khi DOM tải xong | Bọc trong `$(function(){...})` |
| Nút thêm động bấm không chạy | Quên event delegation | Dùng `$(cha).on("click", ".con", ...)` |
| `animate` không di chuyển | Thiếu `position: relative/absolute` | Thêm vào CSS |
| `attr("checked")` cho kết quả lạ | Dùng sai hàm | Dùng `prop("checked")` |
| AJAX báo lỗi CORS | Mở file bằng `file://` hoặc API không cho phép | Dùng Live Server / API có bật CORS |
