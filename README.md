# [HEADER-MODULE-4.5] PLUGIN WORDPRESS TÍCH HỢP EMULATORJS CHƠI GAME RETRO TRÊN TRÌNH DUYỆT

## 🎯 Mục tiêu:
Tạo 1 plugin WordPress cho phép người dùng chơi game retro trực tiếp trên trình duyệt thông qua thư viện EmulatorJS. Tích hợp sâu vào hệ thống WordPress, thân thiện frontend và có thể mở rộng module.

---

## 1. MÔ TẢ CHỨC NĂNG CHÍNH

- Hiển thị game retro (NES, SNES, GBA, PSX...) trực tiếp trên trình duyệt bằng iframe hoặc thẻ `<canvas>` do EmulatorJS render.
- Tích hợp upload ROM hoặc liên kết với thư viện ROM server.
- Lưu tiến trình, hỗ trợ fullscreen, audio, controller support (nếu có).
- Có thể nhúng game vào bài viết (shortcode/block).
- Tích hợp theme + widget + custom post type “ejs_game”.

---

## 2. CẤU TRÚC FILE PLUGIN

```
ejs-emulatorjs/
├── emulatorjs/             ← Tải thư viện EmulatorJS đầy đủ từ https://github.com/EmulatorJS
├── assets/
│   ├── css/
│   ├── js/
├── templates/
│   └── single-game.php     ← Giao diện game
├── includes/
│   ├── class-loader.php
│   ├── class-admin-settings.php
│   ├── class-game-post-type.php
├── ejs-plugin.php          ← File khởi động chính
```

---

## 3. KỸ THUẬT TRIỂN KHAI

### a. Tạo custom post type:
```php
register_post_type('ejs_game', [...]);
```

### b. Tích hợp iframe EmulatorJS:
```php
<iframe src="/emulatorjs/index.html?game=super-mario.smc&system=snes"
        width="100%" height="600px" allowfullscreen></iframe>
```

- Hoặc dùng JS nhúng trực tiếp với config:
```js
EJS_player = "#game-player";
EJS_gameUrl = "/roms/super-mario.smc";
EJS_core = "snes";
```

### c. Tạo shortcode nhúng game:
```php
function ejs_shortcode_game($atts) {
  return '<div id="game-player"></div><script>...EJS init...</script>';
}
add_shortcode('ejs_game', 'ejs_shortcode_game');
```

---

## 4. TÙY CHỌN & MỞ RỘNG

- Cho phép admin upload ROM riêng cho mỗi game
- Chọn hệ máy (dropdown: SNES, NES, GBA...) tại admin
- Cấu hình các tùy chọn: fullscreen, save slot, background, theme light/dark
- Cho phép auto resume last save state nếu user đã đăng nhập

---

## 5. HỖ TRỢ TÍNH NĂNG MỚI NHẤT TỪ EMULATORJS

- Tích hợp đầy đủ các options: ngôn ngữ, quảng cáo, giao diện, save state, speed, shader
- Tương thích với phiên bản mới nhất EmulatorJS từ GitHub
- Tương thích cả embed mode và server self-hosted mode

---

## 6. GỢI Ý MỞ RỘNG

- Kết hợp module “Game Library” đã quét ROMs tự động
- Cho phép user “chơi thử” không cần đăng nhập
- Gửi session play time tới analytics nội bộ

---

## TỔNG KẾT

- Plugin này là nền tảng cốt lõi kết nối EmulatorJS vào WordPress.
- Cần viết chuẩn module hóa, chia từng phần như CPT, template, settings.
- Tối ưu để có thể mở rộng cloud save, achievements, profile, embed...

# [HEADER-MODULE-4.6] TỐI ƯU SEO CHO CÁC TRANG FRONTEND TRONG PLUGIN

## 🎯 Mục tiêu:
Cải thiện khả năng index của công cụ tìm kiếm, tăng organic traffic cho plugin thông qua việc tối ưu SEO on-page cho tất cả các trang frontend liên quan đến game, profile, hệ máy và thể loại.

---

## 1. CẤU TRÚC HTML SEMANTIC

- Mỗi trang frontend nên có cấu trúc rõ ràng:
  + `<h1>`: tiêu đề chính (tên game, genre, hệ máy…)
  + `<meta name="description">`: mô tả ngắn tóm tắt
  + Breadcrumbs dùng schema `BreadcrumbList`
  + `<article>`, `<section>`, `<main>`, `<aside>` để phân bố nội dung

---

## 2. THẺ META VÀ OG (Open Graph)

- Tự động thêm thẻ meta:
```php
<meta property="og:title" content="Super Mario World – SNES Retro Game">
<meta property="og:description" content="Chơi Super Mario World trực tuyến với EmulatorJS. Lưu tiến trình, chia sẻ cùng bạn bè.">
<meta property="og:image" content="[ảnh thumbnail game]">
<meta property="og:url" content="[URL hiện tại]">
```

- Hỗ trợ thêm:
  + `<link rel="canonical">`
  + Twitter Card nếu có ảnh: `summary_large_image`

---

## 3. TITLE & DESCRIPTION DYNAMIC

- Gợi ý cấu trúc title:
  + Game page: `[Game Title] – EmulatorJS | Chơi retro game online`
  + Genre page: `Danh sách game [Tên Thể Loại] – EmulatorJS`
  + Profile page: `Trang cá nhân của [username] – EmulatorJS`

- Dùng hook `wp_title`, `document_title_parts` hoặc tương thích Yoast/RankMath

---

## 4. TỐI ƯU ĐƯỜNG DẪN (URL & SLUG)

- Các custom post type cần slug rõ nghĩa:
  + `game/super-mario-world`
  + `system/snes`
  + `genre/platformer`

- Sử dụng `rewrite` trong `register_post_type()` và `register_taxonomy()`

---

## 5. SITEMAP & BREADCRUMB

- Hỗ trợ Yoast SEO, RankMath sitemap tự động
- Nếu không dùng plugin SEO, tạo custom sitemap từ game CPT
- Breadcrumb theo schema:
```json
{
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Trang chủ", "item": "https://..."},
    {"@type": "ListItem", "position": 2, "name": "SNES", "item": "..."},
    {"@type": "ListItem", "position": 3, "name": "Super Mario World", "item": "..."}
  ]
}
```

---

## 6. SCHEMA & STRUCTURED DATA

- Hỗ trợ Yoast SEO, RankMath, tích hợp không phát sinh lỗi và xung đột.
- Nếu không sử dụng plugin hỗ trợ SEO, plugin có option tạo Thêm JSON-LD cho:
  + Game (`@type: VideoGame`)
  + Profile (`@type: Person`)
  + Genre list (`@type: ItemList`)

- Plugin cần tự động inject đoạn JSON-LD vào `<head>` từng trang nếu được kích hoạt.

---

## 7. MOBILE SEO & CORE WEB VITALS

- Đảm bảo layout mobile hoàn chỉnh
- Lazy load ảnh để tránh LCP chậm
- Preload font quan trọng
- Dùng Lighthouse để test chỉ số SEO frontend

---

## 8. GỢI Ý MỞ RỘNG

- Tự động phân trang SEO-friendly: `?trang=2`, `rel=prev/next`
- Hiển thị số từ khóa gợi ý tương tự bên dưới game page
- Cho phép gắn tag tùy chọn để liên kết bài viết (topic cluster)

---

## TỔNG KẾT

- Tối ưu SEO giúp plugin tăng khả năng index và thu hút truy cập tự nhiên
- Tích hợp tốt với plugin Yoast, RankMath
- Hỗ trợ cả structured data và layout chuẩn SEO mobile

# [HEADER-MODULE-4.7] TỐI ƯU HOÁ HIỆU SUẤT FRONTEND & HỖ TRỢ ĐA THIẾT BỊ

## 🎯 Mục tiêu:
Tối ưu trải nghiệm người dùng với kỹ thuật bộ nhớ đệm (caching), lazy loading, hiệu ứng tải dạng skeleton. Đồng thời đảm bảo layout giao diện của plugin tương thích đa thiết bị (responsive: PC, tablet, mobile).

---

## 1. TỐI ƯU HIỆU SUẤT FRONTEND

### a. Kỹ thuật Caching:
- Dùng `wp_cache_set()` để cache dữ liệu truy vấn game, genre, systems, etc.
- Kết hợp với `Transient API` nếu cần thời gian lưu trữ cụ thể (30p–2h):
```php
set_transient('ejs_recent_games', $games_array, HOUR_IN_SECONDS);
```

- Hạn chế gọi lại các truy vấn WP_Query nặng ở mỗi lần tải trang.

### b. Lazy Loading hình ảnh:
- Toàn bộ thumbnail, screenshot và video preview đều cần thuộc tính:
```html
<img loading="lazy" src="...">
```

- Video embed (Twitch, YouTube...) nên lazy bằng `iframe-placeholder`.

### c. Skeleton Loading (CSS):
- Hiển thị khung xám giả dạng nội dung khi chưa load xong:
```html
<div class="skeleton skeleton-title"></div>
<div class="skeleton skeleton-thumb"></div>
```

- Gợi ý dùng utility class Tailwind:
```css
.animate-pulse.bg-gray-300.rounded-lg
```

- Tích hợp khung Skeleton ở các phần:
  + Danh sách game
  + Profile page
  + Searchbar suggest
  + Chi tiết game chưa tải metadata

---

## 2. ĐẢM BẢO ĐA THIẾT BỊ (RESPONSIVE)

- Dùng breakpoint từ Tailwind hoặc CSS Grid, Flexbox:
  + `md:grid-cols-2 lg:grid-cols-4`
  + `flex-col md:flex-row gap-4`

- Các trang phải responsive:
  + Trang chủ
  + Game info page
  + Explore page
  + Profile user
  + Admin dashboard nếu frontend-based

- Thanh điều hướng và menu (mobile nav) cần toggle hợp lý:
```js
document.querySelector('.mobile-menu-btn').addEventListener('click', toggleMenu);
```

- Test kỹ trên trình giả lập:
  + Chrome DevTool → Responsive
  + Safari iOS iPhone
  + Android Chrome

---

## 3. GỢI Ý MỞ RỘNG

- Cho phép user bật/tắt chế độ tối ưu tốc độ (low data mode)
- Có thể load trước 1 phần dữ liệu qua `prefetch` hoặc `preload` HTML5
- Dùng Service Worker để cache tĩnh (nếu muốn progressive web)

---

## TỔNG KẾT

- Giảm đáng kể thời gian tải nhờ lazy load + cache
- Cảm giác UX tốt hơn nhờ skeleton loader
- Plugin trở nên chuyên nghiệp, đa nền tảng, dùng mượt trên mọi thiết bị

# [HEADER-MODULE-4.8] THANH TÌM KIẾM GAME NÂNG CAO (AJAX + AUTOCOMPLETE + THUMBNAIL)

## 🎯 Mục tiêu:
Tối ưu hóa trải nghiệm tìm kiếm game bằng thanh search bar hiện đại: hỗ trợ gợi ý tự động (autocomplete), suggest game tương tự, hiển thị thumbnail kèm tiêu đề theo thời gian thực (AJAX).

---

## 1. GIAO DIỆN TÌM KIẾM

- Vị trí: Header trang, sidebar hoặc full page `/tim-kiem/`
- Hiển thị kết quả gợi ý dạng dropdown kèm hình ảnh + tên game:
```html
+───────────────────────────────+
| 🔍 Contra III                 |
| 🕹️ SNES · 1992               |
| 🖼️ [Thumbnail bên trái]       |
+───────────────────────────────+
```

---

## 2. KỸ THUẬT TRIỂN KHAI

### a. AJAX Search:
- Tạo REST API endpoint `/wp-json/ejs/v1/search`
- Khi user gõ từ khoá, JS frontend gọi endpoint trả về JSON gồm:
```json
[
  {
    "title": "Mega Man X",
    "image": "https://.../megamanx.jpg",
    "system": "SNES",
    "link": "https://yoursite.com/game/mega-man-x"
  }
]
```

### b. Fulltext Index:
- Nếu số lượng game nhiều (>2000), cần tạo index riêng với `FULLTEXT` trên các field:
```sql
ALTER TABLE wp_ejs_games ADD FULLTEXT(title, description);
```

- Dùng `LIKE` với fallback hoặc plugin như Relevanssi cho cải thiện tìm kiếm.

---

## 3. GỢI Ý GAME LIÊN QUAN (SIMILAR)

- Nếu user tìm “Mario”, hiển thị cả:
  + Super Mario Bros
  + Mario Kart
  + Mario Party

- Logic tìm kiếm có thể dựa trên:
  + Levenshtein Distance
  + Keywords match
  + Game cùng franchise (tag hoặc category meta)

---

## 4. HIỆU SUẤT VÀ CACHE

- Dùng `wp_cache_set()` để cache kết quả query trong 10–30 phút
- Tránh query trực tiếp nhiều lần khi người dùng gõ liên tục
- Gợi ý giới hạn tối đa 8 kết quả đầu tiên (lazy)

---

## 5. MỞ RỘNG & TÙY CHỌN

- Cho phép lọc trong kết quả tìm kiếm:
  + Theo hệ máy (SNES, GBA, PS1…)
  + Theo năm phát hành
  + Theo đánh giá (rating > 4.0…)

- Tạo shortcode `[ejs_game_searchbar]` để chèn vào bất kỳ page nào
- Cho phép chuyển sang trang kết quả tìm kiếm nếu người dùng nhấn Enter

---

## TỔNG KẾT

- Tìm kiếm nhanh, có hình ảnh gợi ý giúp người dùng khám phá dễ hơn
- Giảm tỷ lệ thoát trang khi không tìm được game
- Tối ưu tốc độ truy xuất nhờ AJAX + cache + fulltext logic thông minh

# [HEADER-MODULE-4.9] KẾT NỐI PLUGIN VỚI DISCORD & TWITCH – CỘNG ĐỒNG REALTIME

## 🎯 Mục tiêu:
Tăng tính cộng đồng và trải nghiệm chia sẻ game bằng cách tích hợp plugin với nền tảng Discord và Twitch. Giúp người chơi kết nối, chia sẻ highlight, và tham gia cộng đồng yêu thích retro game.

---

## 1. TÍCH HỢP DISCORD

### a. Cung cấp liên kết mời cộng đồng Discord:
- Hiển thị ở header/footer hoặc sidebar: “🎧 Tham gia Discord”
- Tự động redirect link mời: `https://discord.gg/your-invite-code`

### b. Tạo webhook Discord:
- Cho phép admin thiết lập webhook URL để gửi sự kiện từ plugin lên channel Discord (ví dụ khi có game mới, bình luận nổi bật)
- Cài đặt tại trang Admin > Cộng đồng > Discord

#### Mẫu gửi webhook:
```php
function send_discord_webhook($message) {
  $webhook_url = get_option('ejs_discord_webhook');
  wp_remote_post($webhook_url, [
    'headers' => ['Content-Type' => 'application/json'],
    'body' => json_encode(['content' => $message]),
  ]);
}
```

### c. Gợi ý sự kiện đẩy webhook:
- Game mới được thêm
- Game trending (chơi nhiều)
- Bình luận vote cao nhất trong tuần

---

## 2. TÍCH HỢP TWITCH

### a. Liên kết tài khoản Twitch:
- Cho phép user liên kết profile với Twitch ID → hiển thị trên hồ sơ cá nhân
- Gợi ý kênh livestream Twitch của user trong trang profile

### b. Hiển thị Twitch embed:
- Embed livestream của người chơi (nếu có) hoặc một streamer retro nổi bật
```html
<iframe
  src="https://player.twitch.tv/?channel={channel_name}&parent=yourdomain.com"
  height="300"
  width="480"
  allowfullscreen>
</iframe>
```

- Cấu hình danh sách kênh Twitch mặc định từ admin dashboard (tự chọn người nổi bật)

---

## 3. TRANG CỘNG ĐỒNG

- Tạo riêng 1 trang `/cong-dong/` để embed Discord widget + Twitch nổi bật
- Có thể dùng shortcode `[ejs_community_widget]` để tùy biến chèn vào các page khác

---

## 4. MỞ RỘNG

- Thống kê số lượng người dùng đã liên kết Discord/Twitch
- Tạo badge “Streamer” nếu user là Twitch affiliate/partner
- Hỗ trợ gửi thông báo realtime qua Discord bot (nếu cần)

---

## TỔNG KẾT

- Tích hợp Discord và Twitch giúp người dùng có điểm kết nối cộng đồng mạnh mẽ.
- Tạo động lực chia sẻ, tạo nội dung, và livestream trải nghiệm chơi game.
- Tăng sự gắn bó, gợi mở phát triển cộng đồng riêng cho plugin/platform.

# [HEADER-MODULE-5.0] TRANG GỬI PHẢN HỒI & BÁO LỖI NGƯỜI DÙNG

## 🎯 Mục tiêu:
Tạo một module cho phép người dùng gửi phản hồi, góp ý, hoặc báo lỗi gặp phải khi sử dụng plugin hoặc chơi game. Đây là kênh quan trọng để cải thiện chất lượng hệ thống.

---

## 1. CHỨC NĂNG CHÍNH

- Giao diện gửi phản hồi đơn giản, dễ sử dụng
- Phân loại rõ ràng: Báo lỗi ⚠️ | Góp ý 💡 | Yêu cầu tính năng 🛠️
- Cho phép gửi ảnh hoặc video minh hoạ nếu cần
- Gửi không cần đăng nhập (nếu bật tùy chọn), có CAPTCHA bảo vệ

---

## 2. GIAO DIỆN FORM

- Vị trí: `/phan-hoi/` hoặc shortcode `[ejs_feedback_form]`
- Trường bắt buộc:
  + Họ tên (nếu đăng nhập thì auto-fill)
  + Email (hoặc user ID)
  + Chọn loại phản hồi: lỗi, góp ý, tính năng
  + Nội dung chi tiết
  + File đính kèm (tối đa 2MB)
  + CAPTCHA / honeypot spam protection

### Ví dụ UI:
```php
<select name="type">
  <option value="bug">⚠️ Báo lỗi</option>
  <option value="idea">💡 Góp ý</option>
  <option value="feature">🛠️ Yêu cầu tính năng</option>
</select>
```

---

## 3. XỬ LÝ KỸ THUẬT

- Sử dụng post type `ejs_feedback` để lưu phản hồi
- Mỗi bài feedback gồm:
```php
type (bug/idea/feature)
user_id
message
attachment (if any)
status (pending/resolved)
```

- Gửi email tới quản trị viên khi có phản hồi mới:
```php
wp_mail(get_option('admin_email'), 'Phản hồi mới từ người dùng', $message);
```

---

## 4. QUẢN LÝ TRONG ADMIN

- Menu Feedback trong Admin Dashboard
- Bộ lọc: theo loại phản hồi, trạng thái, từ khóa
- Cho phép đánh dấu “Đã xử lý” hoặc “Đang xử lý”
- Cho phép reply nội bộ hoặc gửi phản hồi tới user nếu cần

---

## 5. GỢI Ý MỞ RỘNG

- Thống kê số lượng feedback theo loại
- Cho phép hiển thị “Top góp ý người dùng” ở frontend
- Tích hợp webhook gửi về Slack/Discord

---

## TỔNG KẾT

- Là công cụ cầu nối giữa người dùng và admin.
- Góp phần cải tiến liên tục chất lượng plugin và tính năng.
- Dễ mở rộng, tích hợp tốt với hệ thống hiện tại.

# [HEADER-MODULE-5.1] HỆ THỐNG THÔNG BÁO NGƯỜI DÙNG (USER NOTIFICATIONS)

## 🎯 Mục tiêu:
Thiết lập hệ thống thông báo thời gian thực hoặc định kỳ nhằm giữ chân người dùng, giúp họ không bỏ lỡ các hoạt động quan trọng như game mới, sự kiện, bình luận, tag tên...

---

## 1. CÁC LOẠI THÔNG BÁO

- 🔔 Game mới được thêm vào
- 🆕 Tin tức, sự kiện đặc biệt từ admin (post mới, sự kiện sắp tới)
- 💬 Ai đó gửi comment lên tường cá nhân (user wall - xem Module 5.6)
- 📣 Ai đó tag người dùng bằng @username trong bất kỳ bình luận nào
- 📥 Ai đó gửi tin nhắn riêng (tùy chọn nếu có hệ thống messaging sau này)

---

## 2. CẤU TRÚC DỮ LIỆU & LƯU TRỮ

- Tạo bảng custom trong database (hoặc dùng post type `ejs_notification`)
- Mỗi thông báo gồm:
```php
user_id
type (comment/tag/game/news)
title
content
link
status (read/unread)
created_at
```

---

## 3. HIỂN THỊ THÔNG BÁO

- Icon chuông trên header, profile hoặc menu:
  + Hiển thị số lượng thông báo chưa đọc
  + Khi click vào sẽ dropdown danh sách

- Trang “Thông báo của tôi”:
  + Hiển thị toàn bộ lịch sử
  + Tùy chọn đánh dấu đã đọc / xoá / lọc theo loại

---

## 4. TÍCH HỢP SỰ KIỆN TRIGGER

- Hook vào hệ thống:
  + Khi thêm game mới → tạo notify cho toàn bộ user hoặc theo sở thích
  + Khi có comment mới vào wall → tạo notify cho owner
  + Khi có @tag → tìm user được tag và tạo notify

- Dùng hook như:
```php
do_action('ejs_user_tagged', $user_id, $context);
do_action('ejs_new_game_added', $game_id);
```

---

## 5. GỢI Ý KỸ THUẬT

- Sử dụng AJAX để lấy thông báo mới mà không reload
- REST API endpoint để đánh dấu đã đọc
- Có thể dùng WebSocket hoặc Pusher để real-time

---

## 6. MỞ RỘNG

- Gửi thông báo qua email nếu user không online
- Cho phép user chọn loại thông báo muốn nhận trong mục cài đặt
- Đổi giao diện thông báo theo Light/Dark mode

---

## TỔNG KẾT

- Hệ thống thông báo là tính năng cốt lõi để duy trì mức độ gắn kết của người dùng.
- Phối hợp chặt chẽ với các module: comment (5.2), profile (5.6), explore (5.5), events/sự kiện tương lai.

# [HEADER-MODULE-5.2] HỆ THỐNG ĐÁNH GIÁ & BÌNH LUẬN GAME

## 🎯 Mục tiêu:
Tăng mức độ tương tác người dùng bằng cách cho phép họ đánh giá game, để lại bình luận, sử dụng hệ thống comment WordPress mở rộng, tích hợp thêm vote và @mention để gọi tên người chơi khác.

---

## 1. KHU VỰC BÌNH LUẬN GAME

### Vị trí hiển thị:
- Phía cuối mỗi trang game
- Có thể đặt tab riêng: “Bình luận”, “Đánh giá”

### Kỹ thuật:
- Sử dụng `comments_template()` và đăng ký comment type riêng:
```php
add_filter('comments_array', 'filter_game_comments', 10, 2);
```

---

## 2. CHỨC NĂNG UPVOTE / DOWNVOTE

- Mỗi comment có 2 nút vote 👍 👎
- Dữ liệu lưu vào comment meta:
```php
update_comment_meta($comment_id, 'upvote', $count);
```

- Giao diện vote AJAX không reload
- Có thể sắp xếp comment theo “Vote nhiều nhất”

---

## 3. CHỨC NĂNG @TAG NGƯỜI DÙNG

- Khi gõ @ sẽ hiện gợi ý username từ hệ thống
- Tự động link tới profile người dùng:
```html
@admin → <a href="/nguoi-choi/admin">@admin</a>
```

- Có thể dùng thư viện JS như Tribute.js để hiển thị gợi ý tag

---

## 4. ĐÁNH GIÁ GAME (RATING)

- Mỗi người dùng được đánh giá mỗi game 1 lần
- Giao diện sao ⭐⭐⭐⭐⭐ (1-5)
- Lưu rating vào comment meta hoặc user meta riêng:
```php
update_comment_meta($comment_id, 'game_rating', 4);
```

- Hiển thị trung bình rating ở đầu trang game:
```php
🌟 4.2/5 (132 đánh giá)
```

---

## 5. CHỐNG SPAM & QUẢN LÝ

- Bật moderation bình luận
- Chặn spam bằng Akismet hoặc reCAPTCHA
- Giới hạn số lần đánh giá trên mỗi IP/người

---

## 6. GỢI Ý MỞ RỘNG

- Thêm biểu tượng cảm xúc trong comment (emoji picker)
- Gắn tag cảm nhận như: "Retro", "Khó", "Hay", "Thử thách", "Hài hước"
- Thống kê người dùng có nhiều bình luận vote cao nhất

---

## TỔNG KẾT

- Hệ thống đánh giá và bình luận giúp tạo nền tảng cộng đồng cho từng game.
- Kết hợp tốt với profile, wall comment, vote logic.
- Tăng nội dung UGC (user generated content) và giúp SEO cho từng game.

# [HEADER-MODULE-5.3] CHIA SẺ GAME & NHÚNG EMBED VỀ WEBSITE

## 🎯 Mục tiêu:
Tăng khả năng lan truyền nội dung game bằng việc chia sẻ mạng xã hội và cho phép người dùng nhúng iframe chơi game về website của họ – tương tự như gametuoitho.vn.

---

## 1. NÚT CHIA SẺ MẠNG XÃ HỘI

### Hiển thị tại:
- Cuối mỗi trang game
- Trong thanh điều khiển hoặc góc của trình giả lập

### Các nền tảng gợi ý:
- Facebook
- X (Twitter)
- Reddit
- LinkedIn
- Copy link trực tiếp

### Mẫu code chia sẻ:
```html
<a href="https://www.facebook.com/sharer/sharer.php?u=https://yourdomain.com/game/abc" target="_blank">
  Chia sẻ Facebook
</a>
```

Sử dụng plugin như `AddToAny`, hoặc viết custom module nếu cần thêm tracking.

---

## 2. TÍNH NĂNG NHÚNG IFRAME (EMBED GAME)

### Mục tiêu:
Cho phép người dùng copy một đoạn mã iframe để chèn vào trang web/blog của họ.

### Giao diện đề xuất:
- Nút “Nhúng game vào website của bạn”
- Khi click sẽ hiển thị đoạn iframe code, có nút sao chép:

```html
<iframe src="https://yourdomain.com/embed/doom-ii-gba" width="640" height="480" frameborder="0" allowfullscreen></iframe>
<!-- Nguồn: https://yourdomain.com | Powered by EmulatorJS -->
```

### Kỹ thuật triển khai:
- Tạo route `yourdomain.com/embed/{slug}` → load giao diện tối giản chỉ chứa trình giả lập.
- Dùng custom template `embed-game.php` để hiển thị EmulatorJS full size
- Gắn sẵn dòng source attribution dưới iframe (hoặc overlay trong giao diện)

---

## 3. BẢO MẬT EMBED

- Giới hạn domain được phép nhúng (cấu hình X-Frame-Options nếu cần)
- Thêm watermark “Nguồn: YourBrand”
- Gắn đoạn JavaScript tracking số lượt chơi qua embed

---

## 4. MỞ RỘNG

- Cho phép người dùng tự chỉnh width/height trước khi copy iframe
- Thêm lựa chọn dark/light mode giao diện iframe
- Cho phép nhúng iframe riêng cho từng hệ máy (autoconfig emulator settings)

---

## TỔNG KẾT

- Tính năng chia sẻ và embed giúp plugin lan tỏa nội dung nhanh hơn, hiệu quả hơn.
- Góp phần tăng backlink, tăng SEO và traffic tự nhiên.
- Phối hợp tốt với các module: favorite, recently played, profile, recommendation.

# [HEADER-MODULE-5.4] MỤC GAME YÊU THÍCH – FAVORITE GAMES MODULE

## 🎯 Mục tiêu:
Cho phép người dùng đánh dấu các game yêu thích bằng nút ❤️, lưu trữ trên hồ sơ cá nhân và truy cập nhanh các game yêu thích bất cứ lúc nào. Đây là tính năng cơ bản giúp tăng độ gắn bó và cá nhân hóa trải nghiệm.

---

## 1. GIAO DIỆN NÚT YÊU THÍCH (TRANG GAME)

- Hiển thị biểu tượng trái tim ❤️ trên mỗi trang chi tiết game.
- Nếu người dùng đã đăng nhập:
  + Click vào để thêm/xoá khỏi danh sách yêu thích.
  + Thay đổi màu (ví dụ: đỏ khi đã yêu thích).
- Nếu chưa đăng nhập:
  + Hiển thị modal đăng nhập khi click.

### Ví dụ mã giao diện:
```php
<a href="#" class="ejs-fav-toggle" data-game-id="123">
  ❤️ Thêm vào yêu thích
</a>
```

---

## 2. XỬ LÝ KỸ THUẬT (AJAX + USER META)

- Dùng user meta để lưu danh sách ID game yêu thích:
```php
$favorite = get_user_meta($user_id, 'ejs_favorite_games', true); // mảng game_id
```

- Tạo REST endpoint hoặc admin-ajax:
```php
add_action('wp_ajax_ejs_toggle_favorite', 'ejs_toggle_favorite_callback');
```

- Kiểm tra trạng thái: đã yêu thích hay chưa
- Tự động render lại giao diện trái tim sau khi toggle

---

## 3. HIỂN THỊ TRONG PROFILE

- Thêm tab/menu “Game yêu thích” trong trang hồ sơ người dùng (liên kết Module 5.6)
- Hiển thị theo dạng danh sách/thẻ/grid giống thư viện
- Mỗi game gồm:
  + Tên, ảnh thumbnail
  + Hệ máy, mô tả ngắn
  + Nút “Chơi ngay”

---

## 4. GỢI Ý MỞ RỘNG

- Phân loại yêu thích theo hệ máy hoặc genre
- Cho phép sắp xếp theo thứ tự đã thêm / tên / hệ máy
- Cho phép export danh sách yêu thích thành file .txt hoặc chia sẻ dưới dạng link public

---

## TỔNG KẾT

- Là một tính năng UX quan trọng giúp tăng tính cá nhân hoá.
- Kết hợp trực tiếp với trang game (Module 5.8), profile cá nhân (Module 5.6).
- Góp phần giữ chân người dùng và hình thành thói quen quay lại chơi.

# [HEADER-MODULE-5.5] TÍNH NĂNG KHÁM PHÁ – VIDEO NGẮN KIỂU REELS/TIKTOK

## 🎯 Mục tiêu:
Tạo một module "Khám phá" nơi người dùng có thể đăng tải video ngắn của chính mình chơi game retro, tương tác xã hội giống TikTok/Facebook Reels. Lấy cảm hứng từ https://gametuoitho.vn/kham-pha

---

## 1. CHỨC NĂNG CHÍNH

- Cho phép người dùng đăng video về game họ đang chơi, cảnh báo người dùng chỉ được phép đăng video gameplay hoặc game trailer, các video liên quan tới game không sẽ vi phạm policy.
- Hiển thị dạng dọc (vertical feed), swipe lên/xuống để xem video tiếp theo
- Tương tác: like ❤️, thả cảm xúc, bình luận, chia sẻ

---

## 2. TRANG KHÁM PHÁ (EXPLORE)

### URL:
`/kham-pha/` hoặc shortcode `[ejs_explore]`

### Layout:
- Hiển thị dạng vertical video
- Có nút thả cảm xúc bên phải (❤️ 🔥 😮 😢)
- Hiển thị thông tin:
  + Người đăng
  + Tên game
  + Số lượt xem, like, bình luận
  + Ngày đăng

---

## 3. GIAO DIỆN GỢI Ý (UI/UX)

- Sử dụng swiper hoặc library video stream theo dạng infinite scroll
- Tối ưu hiển thị mobile
- Hover/hold để tạm dừng video
- Like khi double tap video

---

## 4. ĐĂNG VIDEO

- Người dùng đã đăng nhập có thể đăng:
  + Video .mp4 hoặc các định dạng video phổ biến (giới hạn kích thước)
  + Mô tả ngắn
  + Gắn thẻ game (chọn từ danh sách game có sẵn)
- Dùng custom post type: `ejs_clip`
```php
register_post_type('ejs_clip', [...])
```

---

## 5. CẤU TRÚC DỮ LIỆU GỢI Ý

- Custom fields:
```php
clip_video
clip_game_id
clip_description
clip_likes
clip_views
clip_uploaded_by
```

- Comment: dùng WP comment system (type = clip)
- Like: custom REST endpoint

---

## 6. TÍCH HỢP VỚI USER PROFILE

- Hiển thị tab “Clip của tôi” trong hồ sơ
- Cho phép xóa, sửa, theo dõi lượng tương tác

---

## 7. GỢI Ý MỞ RỘNG

- Tạo trending tab cho video nổi bật
- Sắp xếp theo “mới nhất”, “xem nhiều”, “được yêu thích”
- Cho phép report clip vi phạm
- Gắn thẻ thể loại hoặc hashtags

---

## TỔNG KẾT

- Tính năng khám phá tăng sự gắn kết cộng đồng
- Cho phép người chơi show skill, chia sẻ kinh nghiệm
- Tạo nội dung UGC (User Generated Content) giúp tăng traffic tự nhiên cho web

# [HEADER-MODULE-5.6] HỆ THỐNG TÀI KHOẢN NGƯỜI DÙNG & TRANG PROFILE CÁ NHÂN

## 🎯 Mục tiêu:
Cho phép người dùng đăng ký và sử dụng tài khoản trên site để lưu tiến trình game, tạo danh sách yêu thích và sở hữu trang cá nhân hiển thị đẹp, có tương tác cộng đồng như RetroAchievements.

---

## 1. ĐĂNG KÝ & ĐĂNG NHẬP

- Sử dụng hệ thống user mặc định của WordPress
- Tuỳ biến lại trang `/wp-login.php` và `/wp-register.php` bằng plugin như Theme My Login hoặc viết riêng:
```php
wp_create_user( $username, $password, $email );
```

- Tự động chuyển hướng người dùng mới tới trang hướng dẫn (liên kết Module 5.7)

---

## 2. TRANG PROFILE CÁ NHÂN

### URL gợi ý:
`/nguoi-choi/{username}/` – dùng rewrite API để tạo friendly URL

### Hiển thị các thành phần:
- Avatar & thông tin cơ bản
- Danh sách game đã chơi
- Game yêu thích
- Tổng số giờ chơi, số lượt chơi
- Comment wall (xem phần 3)

---

## 3. BÌNH LUẬN WALL PROFILE (USER WALL)

- Cho phép người dùng khác bình luận vào tường (wall)
- Dùng custom comment loop:
```php
wp_list_comments( array( 'type' => 'profile_wall' ), $comments );
```

- Gắn tag `@username` để gọi tên người chơi khác
- Thêm chức năng upvote/downvote comment
- Phân quyền: chỉ thành viên mới được bình luận

---

## 4. KỸ THUẬT GỢI Ý

- Dùng shortcode `[ejs_user_profile id="current"]` hoặc template `user-profile.php`
- Dùng `get_user_meta()` để truy xuất:
```php
get_user_meta( $user_id, 'ejs_recently_played', true );
get_user_meta( $user_id, 'ejs_favorite_games', true );
```

- Avatar: lấy từ `get_avatar()` hoặc upload riêng
- Tự động lấy username từ URL:
```php
$user = get_user_by('slug', get_query_var('username'));
```

---

## 5. GỢI Ý MỞ RỘNG

- Cho phép follow người dùng khác
- Hiển thị huy hiệu (badge) theo thành tích
- Tích hợp hệ thống điểm cộng & cấp độ (level)
- Cho phép người dùng sửa bio, thêm mạng xã hội

---

## TỔNG KẾT

- Trang hồ sơ cá nhân là nơi để người dùng show thành tích, tiến trình và tương tác xã hội.
- Kết hợp tốt với các module: save game (6.2), comment, favorite, recently played (6.0), suggestion (6.1)
- Tạo nền tảng cộng đồng vững chắc cho hệ thống chơi game trên web.


# [HEADER-MODULE-5.7] TRANG HƯỚNG DẪN & KÊNH HỖ TRỢ NGƯỜI DÙNG

## 🎯 Mục tiêu:
Cung cấp một trang tài liệu hướng dẫn sử dụng plugin và EmulatorJS chi tiết, trực quan, thân thiện với người mới, đồng thời tích hợp kênh hỗ trợ để người dùng có thể đặt câu hỏi, báo lỗi hoặc tìm kiếm thông tin.

---

## 1. TRANG HƯỚNG DẪN (USER GUIDE)

### URL gợi ý:
`/huong-dan-su-dung/` hoặc dùng shortcode `[ejs_user_guide]`

### Nội dung đề xuất:
- Giới thiệu tổng quan plugin và EmulatorJS
- Hướng dẫn chơi game:
  + Cách tìm kiếm game
  + Cách nhấn nút “Chơi”
  + Cách lưu & tải file save
  + Toàn màn hình, phím tắt, thiết lập nút bấm
- Hướng dẫn đăng nhập / tạo tài khoản
- Cách dùng tính năng game yêu thích, đánh giá, chia sẻ
- Cách báo lỗi hoặc liên hệ admin

### Đề xuất trình bày:
- Sử dụng block layout chia theo từng chủ đề
- Có menu điều hướng nội dung bên trái (sticky TOC)
- Giao diện responsive trên mobile

---

## 2. VIDEO DEMO HƯỚNG DẪN (NẾU CÓ)

- Có thể nhúng video từ YouTube hoặc upload file MP4
- Dùng block Gutenberg “Video” hoặc custom field “guide_video”
- Đặt ở đầu hoặc cuối bài hướng dẫn

---

## 3. KÊNH HỖ TRỢ / LIÊN HỆ

- Gợi ý tích hợp form liên hệ qua plugin (Contact Form 7 / FluentForm)
- Các kênh hỗ trợ thêm:
  + Nút mở cửa sổ chat Facebook / Zalo
  + Nhúng Discord server hoặc Telegram group

---

## 4. GỢI Ý MỞ RỘNG

- Tích hợp khu vực FAQ – Câu hỏi thường gặp
- Tạo pop-up “Tip nhanh” khi người dùng lần đầu chơi game
- Tự động redirect tới trang Hướng Dẫn sau khi đăng ký tài khoản thành công lần đầu

---

## TỔNG KẾT

- Trang hướng dẫn là điểm bắt đầu quan trọng cho người mới.
- Nên được cập nhật định kỳ theo phiên bản plugin.
- Giúp giảm tải khối lượng hỗ trợ thủ công từ admin, đồng thời tăng sự chuyên nghiệp cho hệ thống.


# [HEADER-MODULE-5.8] TRANG CHƠI GAME – GIAO DIỆN CHUẨN HÓA VÀ TUỲ BIẾN CAO

## 🎯 Mục tiêu:
Xây dựng giao diện trang chơi game theo phong cách tương tự [GameTuoiTho.vn](https://gametuoitho.vn/tro-choi/super-pinball-nes), cho phép tùy biến bố cục và bổ sung các tính năng nâng cao nhằm thu hút người chơi và giữ chân lâu hơn.

---

## 1. CẤU TRÚC GIAO DIỆN CHUẨN

### Giao diện khuyến nghị gồm các khối:

```
+-------------------------------+
|      TIÊU ĐỀ GAME             |
+-------------------------------+
| [THUMBNAIL]  |  THÔNG TIN     |
|              |  - Hệ máy       |
|              |  - Thể loại     |
|              |  - Năm phát hành
|              |  - Phát triển bởi
+-------------------------------+
| [MEDIA PREVIEW BLOCK]        |
| - Ảnh bìa trước/sau (Box Art)
| - Screenshot
| - Video Trailer (nếu có)
| - Manual PDF (nếu có)        |
+-------------------------------+
| [PLAYER EMULATORJS]          |
+-------------------------------+
| [NÚT: Lưu | Tải | Toàn màn hình | Cấu hình] |
+-------------------------------+
| Mô tả game chi tiết           |
+-------------------------------+
| Gợi ý game liên quan (Module 6.1)
+-------------------------------+
| Bình luận + Đánh giá         |
+-------------------------------+
```

---

## 2. KỸ THUẬT XỬ LÝ MEDIA

- **Ảnh box art:**  
  + Lưu ở ACF hoặc post meta (`box_art_front`, `box_art_back`)
  + Hiển thị trong slider nhỏ

- **Screenshot:**  
  + Cho phép nhiều ảnh (gallery)
  + Dùng thư viện JS như SwiperJS để lướt

- **Video:**  
  + Nếu có link YouTube hoặc file `.mp4` → dùng HTML5 `<video>`
  + Tự động play + thumbnail preview

- **Manual (PDF):**  
  + Có nút “Xem Hướng Dẫn”
  + Dùng PDF.js hoặc link mở tab mới:
```html
<a href="manual-url.pdf" target="_blank">Xem Hướng Dẫn</a>
```

---

## 3. CODE GỢI Ý TRONG TEMPLATE

```php
$manual = get_field('manual_pdf');
$video = get_field('gameplay_video');
$screenshots = get_field('screenshots');
$box_front = get_field('box_art_front');
```

---

## 4. TÍNH NĂNG TÙY BIẾN NÂNG CAO

- **Toàn màn hình & auto-scale canvas** EmulatorJS.
- **Nút Save/Load** tích hợp Cloud Save (module 6.2).
- **Tự động phát hiện và cài đặt language** (module 6.3).
- **Nút cấu hình hotkey & bộ điều khiển**.
- **Dark mode toggle UI**.
- **Hỗ trợ Lazy Loading & Skeleton Loading**.

---

## 5. BỐ CỤC MOBILE RESPONSIVE

- Layout co giãn theo tỉ lệ màn hình.
- Đặt emulator phía trên, thông tin bên dưới.
- Nút bấm lớn, dễ tương tác với thiết bị cảm ứng.

---

## 4. KỸ THUẬT TRIỂN KHAI

- Tạo file template riêng: `single-retro_game.php`
- Dùng shortcode để nhúng EmulatorJS với config động:
```php
echo do_shortcode('[ejs_player game="super-pinball-nes" system="nes"]');
```

- Dùng ACF hoặc custom fields để lưu:
```php
game_system
game_genre
release_year
developer
game_description
thumbnail
```

- Gắn metadata có cấu trúc JSON-LD để hỗ trợ SEO:
```json
{
  "@context": "https://schema.org",
  "@type": "VideoGame",
  "name": "Super Pinball",
  "gamePlatform": "Nintendo Entertainment System",
  "genre": "Arcade",
  "description": "Trò chơi pinball cổ điển hấp dẫn..."
}
```

---

## 5. MỞ RỘNG GỢI Ý

- Hiển thị nút "Chơi tiếp từ chỗ đã dừng" nếu có save file.
- Tooltip mô tả chức năng từng nút.
- Tự động load lại game khi click back/forward.
- Hệ thống analytics: Thời gian chơi / lượt click nút.

---

## TỔNG KẾT

- Trang chơi game là trung tâm tương tác chính.
- Phải được tối ưu tốt về UX/UI, mobile-friendly và tốc độ tải.
- Là nền tảng để gắn kết toàn bộ module khác như Save, Gợi ý, Đánh giá, Thống kê.

# [HEADER-MODULE-5.9] DANH SÁCH GAME THEO HỆ MÁY – LỌC NÂNG CAO

## 🎯 Mục tiêu:
Tạo một trang liệt kê toàn bộ game, phân loại rõ ràng theo từng hệ máy (console/system), đi kèm hệ thống lọc nâng cao để giúp người dùng tìm kiếm hiệu quả.

---

## 1. URL & CẤU TRÚC GIAO DIỆN

- Đường dẫn gợi ý: `/danh-sach-game/` hoặc `[ejs_game_list]`
- Giao diện dạng **grid**, chia rõ từng mục theo `hệ máy` như:
  - Nintendo (NES, SNES, GBA)
  - Sony (PSX, PSP)
  - Sega (Genesis, Dreamcast)
- Mỗi khối hệ máy hiển thị game dưới dạng thumbnail + tên.

---

## 2. TÍNH NĂNG LỌC NÂNG CAO

- Bộ lọc đa điều kiện (filter bar):
  + Hệ máy (dropdown/multi-select)
  + Thể loại (genre)
  + Năm phát hành (từ/năm đến)
  + Nhà phát triển
  + Độ phổ biến (most played, most liked)
- Dùng `AJAX + REST API` để lọc không cần reload trang.
- Hỗ trợ URL query dạng:  
  `?system=snes&genre=rpg&year=1995`

---

## 3. DATA NGUỒN & CẤU TRÚC

- Game là custom post type: `retro_game`
- Metadata:
```php
'game_system' => 'snes',
'game_genre' => 'rpg',
'game_year' => '1995',
'game_developer' => 'Capcom'
```

- Đăng ký taxonomy riêng:
```php
register_taxonomy( 'game_system', 'retro_game', [...]);
register_taxonomy( 'game_genre', 'retro_game', [...]);
```

---

## 4. HIỂN THỊ KẾT QUẢ

- Mỗi game: thumbnail, tên, hệ máy, nút “Chơi ngay”
- Nếu không có game phù hợp: hiển thị thông báo “Không tìm thấy game phù hợp.”

---

## 5. GỢI Ý MỞ RỘNG

- Cho phép sắp xếp theo tên A-Z, Z-A, mới nhất, cũ nhất.
- Tích hợp pagination không reload (AJAX).
- Gợi ý keyword khi người dùng gõ vào ô tìm kiếm.

---

## TỔNG KẾT

- Trang danh sách game giúp người dùng khám phá thư viện nhanh chóng.
- Kết hợp filter nâng cao + phân loại hệ máy tăng khả năng tìm đúng game yêu thích.
- Cấu hình dễ mở rộng và liên kết chặt chẽ với hệ thống metadata game.

# [HEADER-MODULE-6.0] MỤC CHƠI GẦN ĐÂY – GỢI Ý GAME & TRUY CẬP NHANH

## 🎯 Mục tiêu:
Xây dựng một trang riêng “Chơi Gần Đây” giúp người dùng:
- Xem lại những game đã chơi gần đây.
- Xem danh sách game đã từng xem nhưng chưa chơi.
- Nhận đề xuất các game liên quan dựa trên hành vi trước đó.

---

## 1. CẤU TRÚC TRANG “CHƠI GẦN ĐÂY”

Trang sẽ hiển thị theo layout 2 grid dạng slide (carousel hoặc responsive grid):
### 1.1 SLIDE 1 – “Game bạn đã chơi gần đây”
- Dữ liệu từ action khi user click “Chơi game” → lưu vào `user_meta` hoặc `cookie`
- Mỗi entry gồm: Ảnh thumbnail, tên game, hệ máy, ngày chơi cuối

### 1.2 SLIDE 2 – “Game bạn đã xem nhưng chưa chơi”
- Dữ liệu từ action khi user truy cập trang game nhưng không click chơi
- Theo dõi qua JS event + lưu lại trong session hoặc `user_meta`
- Giao diện tương tự SLIDE 1

---

## 2. TÍCH HỢP MODULE GỢI Ý (6.1)

- Bên dưới hai slide sẽ có thêm khu vực “Gợi ý thêm game liên quan”
- Sử dụng hàm `ejs_render_game_suggestion()` (từ Module 6.1) để hiển thị game cùng thể loại/hệ máy

---

## 3. THU THẬP VÀ LƯU DỮ LIỆU

### a. Khi người dùng đăng nhập:
- Lưu dữ liệu vào bảng `usermeta`:
```php
update_user_meta( $user_id, 'ejs_recently_played', $game_slug );
update_user_meta( $user_id, 'ejs_recently_viewed', $game_slug );
```

### b. Khi người dùng không đăng nhập:
- Lưu tạm vào cookie JSON: `ejs_played`, `ejs_viewed`
- Đảm bảo thời gian lưu: 30 ngày

---

## 4. HIỂN THỊ TRANG

- URL gợi ý: `/choi-gan-day/` hoặc shortcode `[ejs_recently_played_page]`
- Có thể gọi qua PHP:
```php
echo ejs_render_recently_played_section();
```

---

## 5. GỢI Ý MỞ RỘNG

- Hiển thị tag “Chưa từng chơi” ở các game đã xem nhưng chưa chơi.
- Gộp dữ liệu khi người dùng đăng nhập (sync từ cookie sang usermeta).
- Hiển thị lịch sử chơi của bạn với thời gian tương đối: “3 ngày trước”, “2 tuần trước”...
- Cho phép xoá lịch sử bằng nút “Xoá lịch sử gần đây”.

---

## TỔNG KẾT

- Trang “Chơi gần đây” giúp người dùng cá nhân hoá trải nghiệm.
- Tăng thời gian giữ chân bằng cách gợi ý thông minh từ dữ liệu hành vi.
- Kết hợp tốt với hệ thống gợi ý game để tăng lượt tương tác.

# [HEADER-MODULE-6.1] MODULE GỢI Ý GAME – HIỂN THỊ LINH HOẠT & BẬT TẮT TÙY TRANG

## 🎯 Mục tiêu:
Tích hợp hệ thống gợi ý game tương tự như trang [GameTuoiTho.vn](https://gametuoitho.vn/), hiển thị danh sách game liên quan ở cuối trang hoặc dưới khu vực chơi game. Module có thể được bật/tắt linh hoạt tại các trang frontend khác theo nhu cầu.

---

## 1. CHỨC NĂNG CỦA MODULE GỢI Ý GAME

- Hiển thị gợi ý game liên quan dưới mỗi trang chơi game.
- Gợi ý theo:
  + Cùng hệ máy (`same_system`)
  + Cùng thể loại (`same_genre`)
  + Cùng nhà phát triển / năm phát hành (tùy chọn nâng cao)
- Dạng hiển thị: Slide ngang (carousel), hoặc grid layout responsive.
- Tùy chọn: số lượng gợi ý, random hoặc dựa theo metadata.

---

## 2. CẤU HÌNH MODULE

**Trong Admin Plugin Settings:**
- Tên section: “Game Suggestion Settings”
- Cho phép bật/tắt module bằng `checkbox`
- Cho phép chọn vị trí hiển thị:
  + `below_game_page`
  + `homepage_widget`
  + `shortcode use`
- Cho phép định nghĩa tiêu chí gợi ý ưu tiên:
  + Same system > Same genre > Random fallback

---

## 3. TRIỂN KHAI FRONTEND

- Tạo hàm render module:
```php
function ejs_render_game_suggestion( $current_game_id ) {
  // Lấy system + genre của game hiện tại
  // Truy vấn custom post type `retro_game`
  // Trả về danh sách HTML gợi ý
}
```

- Dùng tại template hoặc hook `the_content`, hoặc gọi bằng shortcode:
```php
echo do_shortcode('[ejs_game_suggestion]');
```

---

## 4. MODULE LINH HOẠT – CÀI ĐẶT RIÊNG TỪNG TRANG

- Mỗi trang (game page, trang custom) có thể bật/tắt gợi ý:
  + Sử dụng post meta `ejs_show_suggestion = true/false`
  + Admin có thể cài đặt mặc định bật toàn bộ hoặc từng bài

- Nếu là custom post type khác (trang blog), dùng filter `the_content` hoặc template part riêng.

---

## 5. GỢI Ý MỞ RỘNG

- Thêm animation nhẹ khi hover gợi ý.
- Hiển thị rating hoặc view count bên dưới game gợi ý.
- Cho phép người dùng click "Xem thêm game tương tự".
- Hệ thống cache danh sách gợi ý để tối ưu tốc độ (dùng Transients API hoặc object cache).

---

## TỔNG KẾT

- Module gợi ý game sẽ tăng retention và lượt xem trang.
- Cho phép quản trị kiểm soát nơi hiển thị gợi ý.
- Đáp ứng trải nghiệm người dùng mượt mà như các website chơi game lớn.

# [HEADER-MODULE-6.2] HỖ TRỢ CLOUD SAVE / FILE SAVE TRÊN SERVER

## 🎯 Mục tiêu:
Tích hợp hệ thống lưu trữ file save game (save state hoặc memory card) lên server (cloud save) thay vì bắt người dùng phải tải thủ công về máy tính. Người dùng có thể đăng nhập, chọn game đã chơi và tiếp tục từ đúng chỗ đã lưu.

---

## 1. CÁC ĐỊNH DẠNG SAVE HỖ TRỢ

- `.sav`, `.srm`, `.state`, `.mcr`, `.eep`, `.fcs`...
- Mỗi hệ máy sẽ có định dạng file save riêng.
- EmulatorJS hỗ trợ export/import save thông qua JS Blob hoặc `EJS_saveState()` / `EJS_loadState()`.

---

## 2. CẤU TRÚC LƯU TRỮ FILE SAVE TRÊN SERVER

```
/wp-content/uploads/ejs_saves/
├── user_123/
│   ├── snes/
│   │   └── zelda.srm
│   └── psx/
│       └── ff7.mcr
```

- Mỗi người dùng có thư mục riêng (theo `user_id`)
- Phân loại theo hệ máy + tên game

---

## 3. GIAO DIỆN LOAD/SAVE TRÊN FRONTEND

- Khi chơi game:
  + Nút “Lưu Save” → tự động gửi file save lên server qua AJAX.
  + Nút “Tải Save” → liệt kê các save hiện có từ user_id + game slug.
  + Nút “Xoá Save” → gọi endpoint xoá file.
  + Có hiển thị ngày lưu gần nhất.

- Tích hợp cùng EmulatorJS bằng:
```js
EJS_player.saveState().then(blob => {
  // Gửi blob qua AJAX để lưu file
});

EJS_player.loadState(blob);
```

---

## 4. KỸ THUẬT THỰC THI

- Tạo REST API hoặc `admin-ajax.php` endpoint:
  + `ejs_save_upload`: xử lý upload save
  + `ejs_save_list`: trả về danh sách save
  + `ejs_save_delete`: xoá file save

- Mã hóa tên file + folder để tránh trùng và lộ thông tin:
```php
$folder = wp_upload_dir()['basedir'] . "/ejs_saves/user_$user_id/$system/";
$filename = sanitize_file_name( $game_slug . '.sav' );
```

- Dùng `wp_handle_upload()` hoặc `file_put_contents()` để lưu file.
- Có thể dùng Transients hoặc usermeta để lưu metadata thời gian lưu.

---

## TỔNG KẾT

- Cloud Save giúp giữ trải nghiệm liên tục, tiện dụng.
- Đảm bảo an toàn dữ liệu, quản lý dễ dàng trên server.
- Mở rộng để sync save qua tài khoản giữa nhiều thiết bị.

# [HEADER-MODULE-6.3] TỰ ĐỘNG PHÁT HIỆN QUỐC GIA VÀ CHUYỂN NGÔN NGỮ FRONTEND EMULATORJS

## 🎯 Mục tiêu:
Tự động phát hiện vị trí địa lý (quốc gia) của người dùng dựa vào địa chỉ IP, sau đó truyền giá trị ngôn ngữ (`EJS_language`) tương ứng xuống frontend của EmulatorJS để giao diện giả lập tự động hiển thị đúng ngôn ngữ.

---

## 1. OPTION CỦA EMULATORJS

- `EJS_language`: Cấu hình ngôn ngữ cho UI của emulator.  
  Tài liệu tham khảo: https://emulatorjs.org/docs/languages

- Một số ngôn ngữ được hỗ trợ:  
  `en`, `fr`, `de`, `es`, `pt`, `ru`, `ja`, `ko`, `vi`, `zh`, ...

---

## 2. PHÁT HIỆN QUỐC GIA BẰNG IP

### Gợi ý dịch vụ:
- **IPInfo (ipinfo.io)**
- **ipapi.com**
- **GeoLite2 từ MaxMind (dạng local DB, tự host)**

### Kỹ thuật:
- Gọi API từ IPInfo hoặc ipapi để lấy `country_code` (ISO 2 ký tự).
- Ví dụ response:
```json
{
  "ip": "203.113.5.1",
  "country": "VN",
  "country_name": "Vietnam",
  "languages": "vi,en"
}
```

---

## 3. ÁNH XẠ QUỐC GIA → NGÔN NGỮ EMULATORJS

```php
$country_lang_map = [
  'VN' => 'vi',
  'US' => 'en',
  'FR' => 'fr',
  'DE' => 'de',
  'JP' => 'ja',
  'KR' => 'ko',
  'ES' => 'es',
  'PT' => 'pt',
  'CN' => 'zh',
  'RU' => 'ru'
];
```

- Dùng IP để lấy mã quốc gia → đối chiếu sang mã ngôn ngữ hỗ trợ.

---

## 4. TRIỂN KHAI VÀ TRUYỀN DỮ LIỆU SANG FRONTEND

- Gọi API geoIP khi người dùng truy cập lần đầu (hoặc qua `wp_remote_get()`).
- Lưu mã ngôn ngữ vào session hoặc cookie: `ejs_lang`.
- Khi khởi tạo EmulatorJS:
```js
EJS_player = new EmulatorJS({
  Language: 'vi' // lấy từ cookie/session
});
```

- Dùng `wp_localize_script()` hoặc `inline JS` để truyền dữ liệu.

---

## 5. GỢI Ý MỞ RỘNG

- Cho phép override ngôn ngữ bằng dropdown trong giao diện plugin.
- Có checkbox trong admin để bật/tắt auto detect.
- Cho phép người dùng lưu ngôn ngữ ưa thích (localStorage hoặc user meta).
- Cho phép hiển thị cờ quốc gia nhỏ trong UI frontend.

---

## TỔNG KẾT

- Module giúp cải thiện trải nghiệm người dùng toàn cầu bằng cách tự động hiển thị ngôn ngữ phù hợp.
- Giữ giao diện EmulatorJS thân thiện và bản địa hóa.
- Cần tối ưu cache IP để giảm call API và tăng tốc độ.

# [HEADER-MODULE-6.4] HỖ TRỢ UPLOAD FILE ROM – MỞ RỘNG MIME TYPES

## 🎯 Mục tiêu:
Cho phép quản trị viên và người dùng upload các định dạng ROM theo từng hệ máy (ví dụ: `.nes`, `.sfc`, `.gba`, `.gb`, `.n64`, `.bin`, `.iso`, `.cue`, v.v.) thông qua Media Library hoặc giao diện upload riêng, bằng cách mở rộng danh sách MIME types được WordPress hỗ trợ.

---

## 1. VẤN ĐỀ

Mặc định WordPress không cho phép upload file có định dạng không phổ biến như `.nes`, `.gba`, `.bin`, `.cue`, `.iso`, v.v... và sẽ báo lỗi kiểu:
> “Sorry, this file type is not permitted for security reasons.”

---

## 2. GIẢI PHÁP

Sử dụng hook `upload_mimes` để mở rộng danh sách MIME types được phép upload.

```php
add_filter('upload_mimes', function($mimes) {
    $mimes['nes']  = 'application/x-nintendo-nes-rom';
    $mimes['sfc']  = 'application/x-super-nintendo-sfc';
    $mimes['smc']  = 'application/x-super-nintendo-smc';
    $mimes['gba']  = 'application/x-gba-rom';
    $mimes['gb']   = 'application/x-gameboy-rom';
    $mimes['gbc']  = 'application/x-gameboy-color-rom';
    $mimes['n64']  = 'application/x-n64-rom';
    $mimes['bin']  = 'application/octet-stream';
    $mimes['iso']  = 'application/x-iso9660-image';
    $mimes['cue']  = 'application/x-cue';
    $mimes['z64']  = 'application/x-n64-rom-z64';
    return $mimes;
});
```

---

## 3. BẢO MẬT KHI UPLOAD ROM

- Kiểm tra quyền `manage_options` hoặc `upload_files` trước khi cho phép upload nếu là file ROM.
- Hạn chế upload ROM ở nơi chỉ admin có thể kiểm soát (ví dụ: `/wp-content/uploads/roms/`).
- Nếu cần cao hơn, dùng `wp_check_filetype_and_ext()` để xác minh định dạng.

---

## 4. TÍCH HỢP VỚI UI PLUGIN

- Khi người dùng upload ROM qua trang quản trị:
  + Giao diện sẽ báo thành công nếu đúng định dạng.
  + Nếu sai định dạng sẽ cảnh báo rõ lý do.
- Có thể thêm bộ lọc file (file picker): chỉ hiện file ROM tương ứng với hệ máy đang chọn.

---

## 5. MỞ RỘNG

- Cho phép mapping MIME theo từng hệ máy:
```php
$system_mime_map = [
  'snes' => ['sfc', 'smc'],
  'nes' => ['nes'],
  'gba' => ['gba'],
  'psx' => ['bin', 'cue', 'iso'],
];
```
- Tạo tab quản lý MIME upload trong plugin settings để bật/tắt định dạng ROM theo nhu cầu.

---

## TỔNG KẾT

- Việc mở rộng MIME type giúp plugin hoạt động liền mạch với hệ thống upload của WordPress.
- Đảm bảo tương thích EmulatorJS và giữ an toàn cho server khi cho phép upload file ROM.

# [HEADER-MODULE-6.5] TRANG QUẢN LÝ DANH SÁCH HỆ MÁY & THỂ LOẠI (SYSTEMS & GENRES)

## 🎯 Mục tiêu:
Cung cấp trang cấu hình trong Admin Dashboard để quản trị viên có thể:
- Xem, quản lý danh sách hệ máy (systems) mà EmulatorJS hỗ trợ.
- Thêm/sửa/xoá các thể loại game retro (genres).
- Sử dụng hệ thống này làm chuẩn để phân loại ROMs, hiển thị game và lọc nội dung tại frontend.

---

## 1. NGUỒN DỮ LIỆU & TÀI LIỆU THAM KHẢO

- Danh sách hệ máy EmulatorJS chính thức:  
  https://emulatorjs.org/docs/systems
- Ví dụ các systems cần tạo sẵn:
  + `nes`, `snes`, `gba`, `gb`, `gbc`, `n64`, `psx`, `sega32x`, `segaCD`, `mastersystem`, `dreamcast`, `arcade`, `mame`, v.v.

- Danh sách thể loại retro có thể khởi tạo:
  + `Platformer`, `Fighting`, `RPG`, `Shooter`, `Puzzle`, `Sports`, `Racing`, `Adventure`, `Beat 'em up`, `Simulation`, `Strategy`, `Pinball`, `Multiplayer`, `Horror`, `Survival`

---

## 2. THIẾT KẾ TRANG CẤU HÌNH ADMIN

**Vị trí:** `Settings > Systems & Genres`

### 2.1 TAB: “Supported Systems”
- Dạng bảng có các cột:
  + Tên hệ máy (`system_key`)
  + Nhãn hiển thị (`System Name`)
  + Ghi chú (tuỳ chọn)
  + Icon (tuỳ chọn)
- Cho phép bật/tắt từng hệ máy để hiển thị ở frontend (checkbox `enabled`)
- Cho phép reorder (sắp xếp vị trí)

### 2.2 TAB: “Game Genres”
- Bảng hiển thị danh sách các thể loại game.
- Cho phép:
  + Thêm/sửa/xoá thể loại
  + Gán icon hoặc màu sắc đại diện cho từng genre
  + Mỗi genre sẽ lưu vào taxonomy `game_genre` hoặc custom option

---

## 3. TÍCH HỢP VÀ SỬ DỤNG

- Khi thêm game (ROM), quản trị viên có thể gán:
  + Hệ máy (`system`) tương ứng → ánh xạ từ danh sách Systems
  + Thể loại (`genre`) → gợi ý từ danh sách Genres

- Các danh sách này dùng để:
  + Hiển thị filter ở frontend
  + Hiển thị thông tin game ở detail page
  + Hiển thị báo cáo thống kê (theo hệ máy / genre)

---

## 4. KỸ THUẬT THỰC THI

- Lưu dữ liệu vào option: `emulatorjs_supported_systems`, `emulatorjs_game_genres`
- Dùng `Settings API` hoặc tạo page admin riêng với `add_menu_page()`
- Lưu icon theo tên class CSS hoặc base64 (nếu dùng icon riêng)
- Cho phép export/import file JSON nếu cần sync đa site

---

## 5. MỞ RỘNG & ĐỀ XUẤT

- Cho phép tag hệ máy mặc định khi upload ROM (auto detect từ folder path).
- Cho phép tạo alias hoặc mapping `folder_name` → `system_key` nếu tên thư mục khác tên chuẩn.
- Tự động highlight các systems được dùng nhiều nhất trên dashboard.

---

## TỔNG KẾT

Trang “Systems & Genres” sẽ là trung tâm cấu hình cho việc phân loại ROMs, hiển thị frontend, và gom nhóm thống kê. Cấu hình này mang tính nền tảng, cần trực quan và dễ mở rộng.

# [HEADER-MODULE-6.6] TRANG CẤU HÌNH ADM0DE – GOOGLE ADSENSE TÍCH HỢP EMULATORJS

## 🎯 Mục tiêu:
Tạo một trang cấu hình riêng trong AdminCP để quản trị viên dễ dàng nhập, lưu trữ và quản lý các tùy chọn quảng cáo (Ad settings) như: AdURL, AdTimer, AdMode, AdSize, AdBlocked. Những tùy chọn này sẽ tự động truyền xuống EmulatorJS frontend để hiển thị quảng cáo đúng chuẩn.

---

## 1. THÔNG TIN TỪ EMULATORJS DOCUMENTATION

**Tài liệu chính thức:** https://emulatorjs.org/docs/options

**Các tùy chọn cần cấu hình:**
- `AdMode`: Bật/tắt quảng cáo (ví dụ `"onstart"`, `"paused"`, `"off"`)
- `AdUrl`: URL iframe chứa mã quảng cáo (Google Adsense hoặc provider khác)
- `AdTimer`: Thời gian đếm ngược trước khi vào game (giây)
- `AdSize`: Kích thước iframe quảng cáo (ví dụ `728x90`)
- `AdBlocked`: Tin nhắn hiển thị nếu quảng cáo bị block bởi AdBlock

---

## 2. TRANG CẤU HÌNH TRONG ADMIN

**Vị trí:** `Settings > EmulatorJS Ads` (menu riêng)

**Các field trong form:**
- `select` cho `AdMode`: `off`, `onstart`, `paused`
- `text` cho `AdUrl`: ví dụ: `https://ads.example.com/adframe.html`
- `number` cho `AdTimer`: số giây (mặc định `5`)
- `select` cho `AdSize`: `728x90`, `300x250`, `160x600`, `fullscreen`
- `textarea` cho `AdBlocked`: nội dung HTML/text thông báo

**Lưu bằng:** `update_option( 'emulatorjs_ads_config', $options_array )`

**Hiển thị trạng thái:** đã lưu chưa, thông báo lỗi nếu AdUrl không hợp lệ.

---

## 3. TRUYỀN DỮ LIỆU VÀO FRONTEND EMULATORJS

- Dùng hàm `wp_localize_script()` để truyền biến `emulatorjs_ads_config` vào JS.
- Gán biến global JS khi khởi tạo iframe EmulatorJS:
```js
EJS_player = new EmulatorJS({
  AdMode: 'onstart',
  AdUrl: 'https://ads.example.com/adframe.html',
  AdTimer: 5,
  AdSize: '728x90',
  AdBlocked: 'Vui lòng tắt Adblock để tiếp tục chơi game.'
});
```

---

## 4. MỞ RỘNG & GỢI Ý

- Cho phép cấu hình riêng cho từng hệ máy (system-specific ads).
- Hỗ trợ cấu hình json-export/import nếu triển khai multisite.
- Có nút test xem Ad hiển thị được không (iframe preview).
- Cho phép quản trị viên bật/tắt toàn bộ quảng cáo plugin với checkbox global.

---

## TỔNG KẾT

Trang `AdMode Settings` giúp quản trị viên:
- Quản lý quảng cáo dễ dàng mà không cần chạm code.
- Tích hợp trực tiếp với các option của EmulatorJS.
- Tăng doanh thu mà vẫn giữ trải nghiệm người dùng tốt.

# [HEADER-MODULE-6.7] TƯƠNG THÍCH VỚI CÁC PLUGIN PHỔ BIẾN

## 🎯 Mục tiêu:
Đảm bảo plugin EmulatorJS hoạt động ổn định và không xung đột với các plugin phổ biến trong hệ sinh thái WordPress, bao gồm các plugin SEO, bảo mật và hiệu suất.

---

## 1. YOAST SEO / RANKMATH

**Tương thích cần đạt:**
- Tất cả custom post type (`retro_game`, `retro_reel`) phải:
  + Hiển thị trong sitemap XML.
  + Có hỗ trợ title, meta description, OpenGraph tag, Twitter Card.
  + Sử dụng `add_post_type_support( 'retro_game', 'yoast-seo' )`.

**Thực hiện:**
- Khai báo `public => true`, `show_in_rest => true`, `has_archive => true` cho CPT.
- Dùng hook `wpseo_register_extra_replacements` nếu cần thêm custom variable.

---

## 2. WP SUPER CACHE / WP ROCKET / FLYINGPRESS

**Tương thích cần đạt:**
- Plugin không bị cache sai nội dung của EmulatorJS (iframe hoặc script).
- Không lưu cache trang chứa iframe hoặc dynamic game data.

**Thực hiện:**
- Dùng hook để tự thêm page vào danh sách “Không cache” nếu cần:
```php
if ( is_singular( 'retro_game' ) ) {
    define( 'DONOTCACHEPAGE', true );
}
```

- Tích hợp Lazy Load ảnh và defer script nếu plugin cache không có sẵn.

---

## 3. WORDFENCE

**Tương thích cần đạt:**
- Không bị block AJAX hoặc REST API của plugin EmulatorJS.
- Không báo false-positive về file `.rom`, `.sfc`, `.sav`.

**Thực hiện:**
- Đăng ký AJAX endpoint chuẩn (prefix `wp_ajax`, `wp_ajax_nopriv`).
- Gửi thông báo hướng dẫn nếu bị chặn qua firewall:
  “Nếu bạn thấy lỗi tải file ROM, vui lòng whitelist đường dẫn `/wp-content/uploads/roms/` trong Wordfence.”

---

## 4. KIỂM TRA TOÀN DIỆN

- Tạo `Settings > Compatibility` với checkboxes:
  + ✅ Cho phép xuất CPT vào sitemap XML
  + ✅ Disable cache trên trang game
  + ✅ Tối ưu lazyload & preload
  + ✅ Tự động cấu hình tương thích

- Có trang hướng dẫn: “Làm thế nào để plugin hoạt động tốt với Yoast, Rocket, Wordfence…”

---

## TỔNG KẾT

Plugin EmulatorJS cần:
- Hoạt động đồng bộ với hệ sinh thái plugin tối ưu hóa WordPress.
- Tự động nhận diện và cấu hình khi có plugin khác được cài.
- Giữ trải nghiệm người dùng nhất quán – không lỗi vặt, không cache sai.

# [HEADER-MODULE-GAME-MANAGEMENT] HỆ THỐNG QUẢN LÝ GAME TRONG PLUGIN EMULATORJS

## 🎯 Mục tiêu:
Xây dựng hệ thống quản trị game toàn diện trong plugin EmulatorJS – giúp admin theo dõi, thống kê, phân loại, tìm kiếm và quản lý các game được quét từ ROMs. Giao diện trực quan, lọc mạnh mẽ, phân loại chi tiết theo hệ máy, thể loại, năm phát hành và hỗ trợ mở rộng dữ liệu tự động.

---

## 1. GIAO DIỆN DASHBOARD

### 1.1 Giao diện chuyên nghiệp:
- Dùng WordPress Admin UI (hoặc React + REST API nếu cần nâng cấp UX).
- Giao diện dạng bảng hoặc card-grid.
- Bổ sung biểu đồ thống kê bằng Chart.js hoặc wp.data từ Gutenberg API.

### 1.2 Các thông tin hiển thị:
- Tổng số game hiện có.
- Số lượng game theo từng hệ máy (SNES, NES, GBA, Genesis…).
- Số lượng game theo genre.
- Top game được chơi nhiều nhất (dựa theo meta `_play_count`).
- Top genre phổ biến nhất.
- Game được quét gần đây (dựa theo `post_date`).
- Game không đầy đủ metadata hoặc media (cảnh báo đỏ).

---

## 2. BẢNG DANH SÁCH GAME (MANAGE VIEW)

- Sử dụng `WP_List_Table` hoặc `React Table` cho frontend.
- Hiển thị các cột:
  - ID / Tên game
  - Hệ máy
  - Genre
  - Ngày phát hành
  - Tình trạng metadata (✔/✖)
  - Có ảnh bìa, video, manual (icon ✓✗)
  - Số lượt chơi
  - Trạng thái (Public / Draft)
  - Hành động: Xem | Sửa | Cập nhật metadata lại

---

## 3. PHÂN LOẠI & LỌC GAME

- Cho phép lọc các game theo:
  + Hệ máy (taxonomy `game_system`)
  + Genre (taxonomy `game_genre`)
  + Năm phát hành (custom field `release_year`)
  + Trạng thái metadata đầy đủ / thiếu (boolean field)
  + Có file media đi kèm: `has_video`, `has_manual`, `has_screenshot`
  + Thứ tự theo: `name`, `views`, `last_played`

### Kỹ thuật:
- Tạo bộ lọc bằng `WP_Query` kèm `meta_query` và `tax_query`.
- Dùng `select2` hoặc Gutenberg filter UI (nếu React).
- Kết hợp AJAX để lọc động nếu cần.

---

## 4. MODULE CẬP NHẬT DỮ LIỆU

- Nút `Re-scan Metadata` cho mỗi game để gọi lại scanner và cập nhật thông tin mới từ file `.dat` hoặc `media/`.
- Tích hợp cronjob tự động quét lại toàn bộ ROMs mỗi tuần.

---

## 5. ĐỀ XUẤT THÊM (AI AUTO-TUNED)

- Thêm phân tích “Genre trending” theo thời gian.
- So sánh top hệ máy được chơi theo tuần / tháng.
- Tag game nào có hiệu suất cao (hot game).
- Đánh dấu các game chưa được admin duyệt thông tin.
- Kết nối với API ngoài (ScreenScraper, IGDB) để enrich metadata nếu local thiếu.

---

## TỔNG KẾT
- Hệ thống quản trị game là nền tảng quan trọng cho Plugin EmulatorJS.
- Giao diện trực quan, thông minh, kỹ thuật mạnh, có khả năng mở rộng tương lai (với REST API hoặc block editor UI).

# [HEADER-MODULE-6.9] TỰ ĐỘNG QUÉT ROMS & METADATA – CHUẨN SKRAPER

## 🎯 Mục tiêu:
Tạo module tự động quét ROM và metadata theo cấu trúc thư mục tiêu chuẩn tương tự phần mềm Skraper của ScreenScraper.fr. Quá trình này giúp hệ thống tự động cập nhật thông tin game và hiển thị đẹp mắt ở frontend.

---

## 1. CẤU TRÚC THƯ MỤC ROMs

```
roms/
├── snes/
│   ├── Breath of Fire (USA).sfc
│   ├── Super Famicom.dat          ← file metadata XML
│   └── media/
│       ├── box2dback/
│       │   └── Breath of Fire (USA).png
│       ├── box2dfront/
│       │   └── Breath of Fire (USA).png
│       ├── manual/
│       │   └── Breath of Fire (USA).pdf
│       ├── screenshot/
│       │   └── Breath of Fire (USA).png
│       ├── screenshottitle/
│       │   └── Breath of Fire (USA).png
│       └── videos/
│           └── Breath of Fire (USA).mp4
```

- Mỗi hệ máy là một thư mục (`snes`, `nes`, `gba`, v.v…)
- File `.dat` chứa metadata, dạng XML.

## 2. PHÂN TÍCH FILE `.DAT` – XML METADATA

- File có phần mở rộng `.dat` nhưng nội dung là XML, chứa toàn bộ metadata các game trong thư mục.
- Mỗi `<game>` node chứa:
  + `<title>`, `<description>`, `<releasedate>`, `<genre>`, `<publisher>`, `<developer>`
  + `<images>` (liên kết ảnh hoặc tên file ảnh trong thư mục media)

---

## 3. QUY TRÌNH QUÉT ROM & METADATA

- Khi admin upload ROM thủ công hoặc trỏ thư mục `/roms/[system]/`, plugin sẽ:
  + Đọc `.dat` file tương ứng trong thư mục đó.
  + Quét toàn bộ các file ROM và ánh xạ thông tin từ `.dat` dựa theo file name hoặc CRC/hash nếu có.
  + Tự động lưu trữ metadata vào hệ thống: tên game, mô tả, năm phát hành, thể loại, ảnh bìa, screenshot, video.
  + Thêm game vào custom post type nếu chưa có, cập nhật lại nếu có thay đổi.
  + Tải và gán media tương ứng như ảnh đại diện, gallery media cho từng game (screenshot, bìa, video…).

---

## 4. HIỂN THỊ TRÊN FRONTEND GAME

- Thông tin metadata đầy đủ và đẹp mặt sẽ được hiển thị ở trang frontend game:
  + Tiêu đề game
  + Mô tả
  + Hệ máy
  + Ảnh bìa trước/sau
  + Screenshot
  + Video trailer (nếu có)
  + Manual PDF (nếu có)

## 5. CẤU HÌNH & ĐỒNG BỘ

- Cho phép:
  + Upload ROM thủ công
  + Chỉ định đường dẫn `roms/` gốc qua settings
  + Nút “Quét lại metadata” toàn bộ hoặc từng hệ máy

- Nền tảng hỗ trợ:
  + File `.dat` local hoặc API ngoài (nâng cấp)
  + Quét định kỳ qua `wp_cron` (tuỳ chọn)

---

## 6. GỢI Ý & MỞ RỘNG

- Cho phép quét thêm thư mục `media/` cho boxart & screenshot.
- Cho phép mapping video/mp4 từ folder video.
- Tự gán featured image theo screenshottitle.
- Kiểm tra file trùng tên để không tạo trùng game.
- Có chế độ hiển thị danh sách các ROM chưa có metadata.

---

## TỔNG KẾT

- Module ROM Scanner là lõi của hệ thống Plugin EmulatorJS.
- Đảm bảo khả năng mở rộng, bảo trì dễ dàng, tích hợp với UI backend.
- Ánh xạ chính xác metadata từ ROM → CPT → frontend hiển thị.

Plugin phải coi đây là module lõi để vận hành thư viện game ở backend.
