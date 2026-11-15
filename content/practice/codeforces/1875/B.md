---
title: "CF 1875B - Sứa và trò chơi"
description: "Chúng tôi được yêu cầu mô phỏng một trò chơi trao đổi theo lượt giữa hai người chơi Jellyfish và Gellyfish. Jellyfish sở hữu một mảng táo có giá trị nguyên và Gellyfish sở hữu một mảng khác."
date: "2026-06-09T00:57:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1875
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 901 (Div. 2)"
rating: 1200
weight: 1875
solve_time_s: 34
verified: false
draft: false
---

[CF 1875B - Sứa và trò chơi](https://codeforces.com/problemset/problem/1875/B) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trò chơi trao đổi theo lượt giữa hai người chơi Jellyfish và Gellyfish. Jellyfish sở hữu một mảng táo có giá trị nguyên và Gellyfish sở hữu một mảng khác. Họ chơi cho$k$vòng, lần lượt: Sứa di chuyển theo vòng lẻ, Gellyfish di chuyển theo vòng chẵn. Đến lượt của mình, người chơi có thể đổi một trong những quả táo của mình với một quả táo của đối thủ hoặc không làm gì cả. Cả hai đều nhằm mục đích tối đa hóa số lượng táo của mình. Chúng ta cần tính tổng cuối cùng của Jellyfish sau$k$vòng, giả sử cả hai đều chơi tối ưu. 

Các tham số đầu vào chính là kích thước của mảng$n$Và$m$, và số vòng$k$. Giá trị mỗi quả táo lên tới$10^9$, Và$k$có thể rất lớn ($10^9$), vượt xa độ dài của mảng. Điều này ngay lập tức ngụ ý rằng việc mô phỏng trực tiếp từng vòng là không khả thi. Chúng ta phải khai thác quan sát rằng sẽ không có người chơi nào hoán đổi theo cách làm giảm tổng số tiền của họ và một khi tất cả các lần hoán đổi có lợi đã hết, các vòng bổ sung sẽ không có hiệu lực. 

Các trường hợp cạnh bao gồm khi các mảng đã tối ưu (giá trị lớn nhất về phía Jellyfish), khi$k$nhỏ hơn số lượng giao dịch hoán đổi tiềm năng hoặc khi$k$vượt quá số lượng giao dịch hoán đổi cần thiết. Ví dụ: nếu Sứa có`[1, 2]`và cá gelly`[3, 4]`với$k=10^9$, chỉ có hai giao dịch hoán đổi làm tăng tổng của Jellyfish là phù hợp; các vòng còn lại không làm gì cả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại trên mỗi$k$các vòng, tìm cách hoán đổi tối ưu cho người chơi đến lượt. Trên mỗi cái
