---
title: "CF 104262C - Biến chứng hiệu chuẩn"
description: "Sơ đồ ghép từ năm chữ cái trong Phần 7.2.1.1 dựa vào việc che một chuỗi bit được đóng gói sao cho mỗi mặt nạ cách ly phần dưới của một từ bao gồm một số nguyên các trường chữ cái có kích thước cố định."
date: "2026-07-01T21:35:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 62
verified: false
draft: false
---

[CF 104262C - Các biến chứng khi hiệu chuẩn](https://codeforces.com/problemset/problem/104262/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Sơ đồ ghép từ năm chữ cái trong Phần 7.2.1.1 dựa vào việc che một chuỗi bit được đóng gói sao cho mỗi mặt nạ cách ly phần dưới của một từ bao gồm một số nguyên các trường chữ cái có kích thước cố định. Trong cấu trúc tiêu chuẩn, mỗi chữ cái chiếm$5$bit, do đó một từ được phân chia thành các khối có kích thước$5$, và$j$mặt nạ được thiết kế để trích xuất chính xác mức thấp nhất$5(j+1)$bit. 

Trong cài đặt đó, mặt nạ chính xác có dạng$$m_j = z \,\&\, (2^{5j+5}-1),$$kể từ số nhị phân$2^{5j+5}-1$bao gồm$5(j+1)$liên tiếp$1$bit, và do đó bảo toàn chính xác mức thấp nhất$5(j+1)$bit của$z$trong khi xóa tất cả các bit cao hơn. 

Việc sửa đổi được đề xuất thay thế điều này bằng$$m_j = z \,\&\, (25j+5 - 1).$$Giải thích điều này theo nghĩa thao tác bit dự định đòi hỏi phải đọc$25j+5$BẰNG$2^{5j+5}$, vì chỉ lũy thừa của hai sẽ tạo ra một mặt nạ bao gồm một khối liền kề có bậc thấp$1$bit. Theo cách giải thích này,$$25j+5 - 1 = 2^{5j+5}-1,$$vì vậy mỗi$m_j$trích xuất chính xác cùng một tập hợp các bit bậc thấp như trong sơ đồ ban đầu. 

Tính đúng đắn của sơ đồ ghép nối chỉ phụ thuộc vào thuộc tính lồng nhau của mặt nạ,$$m_0 \subset m_1 \subset m_2 \subset \cdots,$$nghĩa là mỗi mặt nạ kế tiếp giữ lại tất cả các bit được giữ lại bởi mặt nạ trước đó và thêm một khối bổ sung cố định$5$bit. Cả mặt nạ gốc và mặt nạ sửa đổi đều thỏa mãn tính chất này vì$$2^{5(j+1)}-1 = (2^{5j+5}-1) + 2^{5j+5},$$trong đó nối thêm một khối cao hơn rời rạc$5$bit. 

Vì tất cả các bước tiếp theo trong sơ đồ chỉ phụ thuộc vào sự cách ly nhất quán của các trường 5 bit liên tiếp chứ không phụ thuộc vào bất kỳ nhận dạng số học nào ngoài các phân vùng bit này, nên việc thay thế mặt nạ ban đầu bằng mặt nạ đã sửa đổi sẽ khiến mọi trích xuất trung gian không thay đổi. Mọi chữ cái 5 bit được nhóm vẫn được căn chỉnh và không xảy ra nhiễu xuyên trường. 

Do đó, thuật toán tạo ra sự phân tách giống hệt nhau của từ được đóng gói thành các khối năm chữ cái và mang lại kết quả ghép nối giống như cấu trúc ban đầu. 

Điều này hoàn thành việc chứng minh. ∎
