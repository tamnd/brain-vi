---
title: "CF 1418G - Ba Lần Xuất Hiện"
description: "Chúng ta được cung cấp một mảng có độ dài $n$ và chúng ta muốn đếm xem có bao nhiêu phân đoạn liền kề có thuộc tính sau: mọi giá trị riêng biệt xuất hiện bên trong phân đoạn đó xuất hiện chính xác ba lần. Điều kiện này hạn chế một cách đáng ngạc nhiên."
date: "2026-06-11T06:51:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "hashing", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1418
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 95 (Rated for Div. 2)"
rating: 2500
weight: 1418
solve_time_s: 28
verified: false
draft: false
---

[CF 1418G - Ba lần xuất hiện](https://codeforces.com/problemset/problem/1418/G) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, chia để trị, băm, hai con trỏ 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$và chúng tôi muốn đếm có bao nhiêu phân đoạn liền kề có thuộc tính sau: mọi giá trị riêng biệt xuất hiện bên trong phân đoạn đó xuất hiện chính xác ba lần. 

Điều kiện này hạn chế một cách đáng ngạc nhiên. Nếu một số xuất hiện một hoặc hai lần trong phân đoạn thì phân đoạn đó không hợp lệ. Nếu nó xuất hiện bốn lần thì phân đoạn đó cũng không hợp lệ. Một phân đoạn hợp lệ chỉ bao gồm các giá trị có tần số bằng ba. 

Độ dài mảng có thể đạt tới$5 \cdot 10^5$. Một thuật toán bậc hai sẽ kiểm tra đại khái$2.5 \times 10^{11}$mảng con, điều này hoàn toàn không khả thi trong giới hạn thời gian năm giây. Thậm chí$O(n\sqrt n)$sẽ không thoải mái trong Python. Chúng ta cần một cái gì đó rất gần với thời gian tuyến tính. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như```
3
1 1 1
```Câu trả lời là```
1
```Toàn bộ mảng hợp lệ vì giá trị duy nhất xuất hiện đúng ba lần. 

Bây giờ hãy xem xét```
4
1 1 1 1
```Câu trả lời là```
1
```Chỉ có ba vị trí đầu tiên và ba vị trí cuối cùng tạo thành các mảng con hợp lệ. Toàn bộ mảng không hợp lệ vì tần số trở thành bốn. Bất kỳ cách tiếp cận nào chỉ kiểm tra độ dài chia hết cho ba sẽ thất bại. 

Một ví dụ tinh tế khác là```

```Câu trả lời là```

```Toàn bộ mảng hoạt động vì cả hai giá trị đều xuất hiện ba lần. Bé nhỏ
