---
title: "CF 176A - Kinh doanh thương mại"
description: "Chúng ta có một số hành tinh. Trên mỗi hành tinh, mỗi loại vật phẩm có ba giá trị: - Giá mua một đơn vị trên hành tinh đó. - Giá mà hành tinh đó mua một đơn vị từ chúng ta. - Số lượng đơn vị có sẵn để mua."
date: "2026-06-02T17:04:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 176
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2012 - Round 2"
rating: 1200
weight: 176
solve_time_s: 48
verified: false
draft: false
---

[CF 176A - Kinh doanh thương mại](https://codeforces.com/problemset/problem/176/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số hành tinh. Trên mỗi hành tinh, mỗi loại vật phẩm đều có ba giá trị: 

- Giá mua một căn trên hành tinh đó. 
- Giá mà hành tinh đó mua một đơn vị từ chúng ta. 
- Số lượng đơn vị có sẵn để mua. 

Qwerty chọn chính xác một hành tinh nơi mọi hoạt động mua hàng được thực hiện và chính xác một hành tinh nơi mọi hoạt động mua bán được thực hiện. Việc mua và bán chỉ xảy ra một lần. Anh ta có thể mua bất kỳ tập hợp con nào của các mặt hàng có sẵn, tùy thuộc vào giới hạn sức chứa của tàu`k`. 

Nếu một mục thuộc loại`j`được mua trên hành tinh`A`và được bán trên hành tinh`B`, lợi nhuận từ một đơn vị là:`profit = sell_price[B][j] - buy_price[A][j]`Mục tiêu là tối đa hóa tổng lợi nhuận. 

Quan sát đầu tiên là số tiền thực tế cần thiết để mua không thành vấn đề. Khoản vay không có lãi suất nên chỉ có lợi nhuận ròng mới quan trọng. Chúng tôi chỉ quan tâm đến việc mặt hàng nào tạo ra lợi nhuận dương và chúng tôi có thể vận chuyển bao nhiêu mặt hàng trong số đó. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Có tối đa 10 hành tinh và tối đa 100 loại vật phẩm. Sức chứa hàng hóa tối đa là 100. Giải pháp kiểm tra từng cặp hành tinh có thứ tự là hoàn toàn hợp lý vì có nhiều nhất`10 * 10 = 100`những cặp như vậy. 

Giới hạn dung lượng 100 là con số quan trọng nhất. Nó gợi ý rằng các thuật toán tỷ lệ thuận với`k`,`mk`, hoặc thậm chí`nmk`có giá cả phải chăng một cách dễ dàng. Mặt khác, việc thử tất cả các tập hợp con của các loại vật phẩm là không thể vì`m`có thể đạt tới 100. 

Một điểm tinh tế là các mục chỉ được chia thành các đơn vị nguyên. Nếu một giao dịch có lợi nhuận cho phép mua 50 bản và một giao dịch khác cho phép mua 30 bản, chúng ta không thể đơn giản mua cả hai khi`k = 60`. Chúng ta phải chọn những đơn vị riêng lẻ tốt nhất. 

Một sai lầm dễ mắc phải khác là cho rằng chúng ta chỉ nên xem xét các hành tinh khác nhau để mua và bán. Tuyên bố không cấm sử dụng cùng một hành tinh cho cả hai hành động. Lựa chọn như vậy chỉ đơn giản là tạo ra lợi nhuận không dương vì mỗi giá bán đều nhỏ hơn giá mua tương ứng, nhưng thuật toán vẫn phải xử lý nó một cách tự nhiên. 

Coi như:```
2 1 1
A
10 5 1
B
20 15 1
```Mua A và bán B mang lại lợi nhuận`15 - 10 = 5`, vậy đáp án là 5. 

Một giải pháp bất cẩn chỉ so sánh tổng giá hoặc chỉ các loại mặt hàng có thể bỏ lỡ giao dịch sinh lời này. 

Một trường hợp đặc biệt khác xảy ra khi mọi giao dịch có thể xảy ra đều mất tiền:```
2 1 10
A
```
