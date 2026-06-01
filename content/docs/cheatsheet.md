---
title: "Cheatsheet"
weight: 1
description: "Tham khảo nhanh với các bản xem trước trực tiếp cho Markdown, mã ngắn và nội dung cơ bản."
date: 2026-05-02T11:50:02+07:00
---

# Mánh gian lận 

Mỗi phần hiển thị cú pháp theo sau là bản xem trước trực tiếp. 
## Cảnh báo````markdown
> [!NOTE]
> General info the reader should not miss.
```` 

> [!LƯU Ý] 
> Thông tin chung người đọc không nên bỏ lỡ.````markdown
> [!TIP]
> Helpful suggestion.
```` 

> [!MẸO] 
> Đề xuất hữu ích.````markdown
> [!IMPORTANT]
> Must-know info.
```` 

> [! QUAN TRỌNG] 
> Thông tin cần biết.````markdown
> [!WARNING]
> Could cause problems.
```` 

> [!CẢNH BÁO] 
> Có thể gây ra vấn đề.````markdown
> [!CAUTION]
> Risk of harm or data loss.
```` 

> [!THẬN TRỌNG] 
> Nguy cơ bị tổn hại hoặc mất dữ liệu. 
## Chi tiết````markdown
{{</* details title="Click to expand" */>}}
Hidden content revealed on click.
{{</* /details */>}}
````{{< details title="Click to expand" >}}Nội dung ẩn được tiết lộ khi nhấp chuột.{{< /details >}}

````markdown
{{</* details title="Open by default" open=true */>}}
Visible immediately.
{{</* /details */>}}
````{{< details title="Open by default" open=true >}}Có thể nhìn thấy ngay lập tức.{{< /details >}}## Tab````markdown
{{</* tabs */>}}
{{</* tab "Go" */>}}
```đi 
fmt.Println("xin chào")```
{{</* /tab */>}}
{{</* tab "Python" */>}}
```trăn 
in ("xin chào")```
{{</* /tab */>}}
{{</* tab "Rust" */>}}
```rỉ sét 
println!("xin chào");```
{{</* /tab */>}}
{{</* /tabs */>}}
````{{< tabs >}}
{{< tab "Go" >}}
```go
fmt.Println("hello")
```
{{< /tab >}}
{{< tab "Python" >}}
```python
print("hello")
```
{{< /tab >}}
{{< tab "Rust" >}}
```rust
println!("hello");
```
{{< /tab >}}
{{< /tabs >}}## bước````markdown
{{</* steps */>}}

1. **Install Hugo**

   Download the extended binary from gohugo.io.

2. **Clone the repo**

   `git clone git@github.com:tamnd/brain.git`

3. **Run locally**

   `hugo server`

{{</* /steps */>}}
````{{< steps >}}1. **Cài đặt Hugo** 

Tải xuống tệp nhị phân mở rộng từ gohugo.io. 

2. **Sao chép kho lưu trữ**`git clone git@github.com:tamnd/brain.git`3. **Chạy cục bộ**`hugo server`

{{< /steps >}}## Huy hiệu````markdown
{{</* badge content="stable" type="info" */>}}
{{</* badge content="beta" type="warning" */>}}
{{</* badge content="deprecated" type="error" */>}}
{{</* badge content="tag" */>}}
````{{< badge content="stable" type="info" >}}
{{< badge content="beta" type="warning" >}}
{{< badge content="deprecated" type="error" >}}
{{< badge content="tag" >}}## sơ đồ nàng tiên cá````markdown
```nàng tiên cá 
đồ thị LR 
A[Viết ghi chú] --> B[brain_on.sh phát hiện thay đổi] 
B --> C[git commit + push] 
C --> D[Bản dựng hành động GitHub] 
D --> E [Trực tiếp trên trang GitHub]```
```````mermaid
graph LR
    A[Write note] --> B[brain_on.sh detects change]
    B --> C[git commit + push]
    C --> D[GitHub Actions builds]
    D --> E[Live on GitHub Pages]
```

````markdown
```nàng tiên cá 
trình tựSơ đồ 
Máy khách->>Máy chủ: NHẬN /api/ghi chú 
Máy chủ-->>Máy khách: 200 OK + JSON```
```````mermaid
sequenceDiagram
    Client->>Server: GET /api/notes
    Server-->>Client: 200 OK + JSON
```## Toán KaTeX 

Nội tuyến:```markdown
$E = mc^2$
```

$E = mc^2$Khối (ở giữa):```markdown
$$
\int_0^\infty e^{-x^2}\,dx = \frac{\sqrt{\pi}}{2}
$$
```

$$
\int_0^\infty e^{-x^2}\,dx = \frac{\sqrt{\pi}}{2}
$$## Khối mã````markdown
```đi 
gói chính 

nhập "fmt" 

chức năng chính() { 
fmt.Println("xin chào não") 
}```
```````go
package main

import "fmt"

func main() {
    fmt.Println("hello, brain")
}
```

```python
def greet(name: str) -> str:
    return f"hello, {name}"
```

```sql
SELECT title, date
FROM notes
WHERE tags @> ARRAY['go']
ORDER BY date DESC;
```

```bash
hugo server --buildDrafts --buildFuture
```## Tham khảo nhanh Markdown 

### Văn bản 

| Cú pháp | Kết quả | 
|--------|--------| 
|`**bold**`| **đậm** | 
|`*italic*`| *nghiêng* | 
| `` `mã số` `` | `mã` | 
|`~~strike~~`| ~~ đình công~~ | 

### Liên kết```markdown
[external](https://gohugo.io)
[internal]({{</* relref "cheatsheet.md" */>}})
```[bên ngoài](https://gohugo.io) 

### Danh sách nhiệm vụ```markdown
- [x] Published cheatsheet
- [x] Added dark mode toggle
- [ ] Write more notes
```- [x] Bảng cheat đã xuất bản 
- [x] Đã thêm chuyển đổi chế độ tối 
- [ ] Viết thêm ghi chú 

### Bảng```markdown
| Left | Center | Right |
|:-----|:------:|------:|
| a    |   b    |     c |
| 1    |   2    |     3 |
```| Trái | Trung tâm | Đúng | 
|:------|:------:|------:| 
| một | b | c | 
| 1 | 2 | 3 | 

### Chú thích cuối trang```markdown
This is a claim.[^source]

[^source]: The source for this claim.
```Đây là một lời khẳng định.[^source] 

[^source]: Nguồn của tuyên bố này. 
## Tham chiếu vấn đề phía trước```yaml
title: "Note title"
date: 2026-05-02
weight: 1                    # sidebar order, lower = higher
tags: ["go", "db"]
draft: false

math: true                   # enable KaTeX on this page
```
