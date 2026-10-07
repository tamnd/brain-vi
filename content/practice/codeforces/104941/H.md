---
title: "CF 104941H - Nó phù hợp như thế nào?"
description: "Chúng ta được cung cấp một chuỗi có thể thay đổi $s$ và một mẫu $p$ chứa các chữ cái viết thường và các dấu sao đại diện. Một ngôi sao có thể được thay thế bằng bất kỳ chuỗi nào (có thể trống), độc lập với các ngôi sao khác."
date: "2026-06-28T18:18:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "H"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 22
verified: false
draft: false
---

[CF 104941H - Nó phù hợp như thế nào?](https://codeforces.com/problemset/problem/104941/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi có thể thay đổi$s$và một mẫu$p$chứa các chữ cái viết thường và các dấu sao đại diện. Một ngôi sao có thể được thay thế bằng bất kỳ chuỗi nào (có thể trống), độc lập với các ngôi sao khác. Mẫu khớp với một chuỗi nếu sau khi thay thế mọi dấu sao, chúng ta có thể thu được chuỗi đó một cách chính xác. 

Sau mỗi lần cập nhật một ký tự thành$s$, chúng ta cần trả lời liệu có tồn tại ít nhất một chuỗi con liền kề của chuỗi hiện tại hay không$s$phù hợp với mẫu$p$. 

Vì vậy, nhiệm vụ không phải là khớp toàn bộ chuỗi mà là kiểm tra sự tồn tại của “cửa sổ tốt” trong$s$sau mỗi lần cập nhật. 

Các ràng buộc tách biệt hai đối tượng một cách rõ ràng. Chuỗi$s$lớn, lên đến$2 \cdot 10^5$, và nó thay đổi nhiều lần, lên đến$2 \cdot 10^4$. mẫu$p$rất nhỏ, tối đa 200 ký tự. Sự bất đối xứng này là gợi ý về cấu trúc chính: tiền xử lý và lý luận lấy mẫu làm trung tâm là bắt buộc, trong khi chuỗi phải được xử lý linh hoạt. 

Một cách tiếp cận đơn giản sẽ quét tất cả các chuỗi con sau mỗi lần cập nhật, điều này là không thể ngay lập tức:$O(n^2)$chuỗi con lên đến$2 \cdot 10^4$cập nhật đã vượt quá mọi giới hạn. 

Có một vài trường hợp thất bại tinh vi gây ra sự tham lam ngây thơ trong việc kết hợp. 

Vấn đề đầu tiên là hành vi sao trống. Ví dụ, mẫu`"a*b"`trận đấu`"ab"`(sao trống) và `"axxb*
