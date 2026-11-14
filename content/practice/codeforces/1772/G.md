---
title: "CF 1772G - Đạt được xếp hạng"
description: "Monocarp muốn tăng xếp hạng cờ vua của mình từ x lên y bằng cách đấu với một nhóm n đối thủ có xếp hạng cố định. Mỗi trò chơi chỉ ảnh hưởng đến xếp hạng của Monocarp: nếu xếp hạng hiện tại của anh ta ít nhất bằng đối thủ, anh ta thắng và được 1; nếu không, anh ta thua và rớt 1."
date: "2026-06-09T12:20:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1772
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 839 (Div. 3)"
rating: 2200
weight: 1772
solve_time_s: 41
verified: false
draft: false
---

[CF 1772G - Đạt được xếp hạng](https://codeforces.com/problemset/problem/1772/G) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Monocarp muốn tăng xếp hạng cờ vua của mình từ`x`ĐẾN`y`bằng cách chơi một bộ`n`đối thủ có xếp hạng cố định. Mỗi trò chơi chỉ ảnh hưởng đến xếp hạng của Monocarp: nếu xếp hạng hiện tại của anh ta ít nhất bằng đối thủ, anh ta thắng và được 1; nếu không, anh ta thua và tụt 1. Điều khó khăn là anh ta phải đấu với đối thủ một cách đồng đều: anh ta không thể đấu với đối thủ này nhiều lần hơn đối thủ khác cho đến khi tất cả đều được chơi bằng nhau. 

Đầu vào cung cấp số lượng trường hợp thử nghiệm và đối với mỗi trường hợp thử nghiệm, số lượng đối thủ, xếp hạng ban đầu và mục tiêu của Monocarp cũng như danh sách xếp hạng của đối thủ. Đầu ra phải là số lượng trò chơi tối thiểu cần thiết để đạt được xếp hạng mục tiêu hoặc`-1`nếu điều đó là không thể. 

Những hạn chế là đáng kể:`n`có thể đạt được`2·10^5`và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi`2·10^5`. Bản thân xếp hạng có thể lên tới`10^12`, điều này ngăn các giải pháp lặp lại xếp hạng theo xếp hạng. Chúng ta phải tránh các mô phỏng bạo lực xem xét từng trò chơi riêng lẻ. 

Các trường hợp lợi thế phát sinh khi ban đầu tất cả đối thủ đều mạnh hơn Monocarp. Ví dụ,`x=1`,`y=3`, và đối thủ`[2,3]`là không thể, bởi vì bất kỳ trò chơi nào cũng sẽ khiến anh ta thua, đẩy anh ta ra xa hơn`y`. Một trường hợp tế nhị khác là khi chỉ có một đối thủ; thì ràng buộc "chơi chẵn" là không đáng kể nhưng vẫn phải được tôn trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mô phỏng các trò chơi của Monocarp theo từng vòng, chọn đối thủ theo kiểu tuần hoàn để đáp ứng ràng buộc chơi đồng đều. Sau mỗi trận đấu, hãy cập nhật xếp hạng tùy theo thắng/thua. Dừng lại khi đến`y`hoặc tuyên bố không thể nếu xếp hạng giảm xuống dưới đối thủ nhỏ nhất trong một chu kỳ. Điều này đúng nhưng quá chậm, bởi vì việc tiếp cận`y`có thể mất tới`y - x`những trò chơi có thể`10^12`trong trường hợp xấu nhất. 

Quan sát quan trọng là kết quả của một chu kỳ quay vòng của`n`trò chơi chỉ phụ thuộc vào số lượng đối thủ mà Monocarp có thể đánh bại ở mức xếp hạng hiện tại của anh ấy. Giả sử anh ta chơi một ván với mỗi đối thủ trong một chu kỳ. Cho phép`w`là số lượng đối thủ anh ta có thể đánh bại. Khi đó lợi nhuận ròng của anh ta sau một chu kỳ đầy đủ là`2·w - n`. Nếu như`2·w - n`là không tích cực, Monocarp không thể tiến bộ trừ khi cuối cùng anh ta đánh bại tất cả mọi người. Mặt khác, số chu kỳ cần thiết để đạt được`y`đơn giản là trần của`(y-x)/net_gain_per_cycle`. 

Sắp xếp xếp hạng của đối thủ cho phép chúng tôi nhanh chóng xác định`w`cho bất kỳ đánh giá hiện tại. Nếu tại bất kỳ thời điểm nào xếp hạng của Monocarp thấp hơn tất cả xếp hạng của đối thủ thì vấn đề là không thể xảy ra. Quá trình này giảm bớt vấn đề bằng việc lặp đi lặp lại việc tìm ra đối thủ tối đa mà Monocarp có thể đánh bại, tính toán lợi nhuận ròng và tính toán số chu kỳ đầy đủ cần thiết, với nhiều nhất một phần chu kỳ ở cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((y-x)·n) | O(n) | Quá chậm, không thành công với quy mô lớn`y-x`| 
| Tham lam dựa trên chu kỳ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp xếp hạng của đối thủ. Điều này cho phép chúng tôi xác định một cách hiệu quả số lượng đối thủ mà Monocarp có thể đánh bại ở bất kỳ mức xếp hạng nào. 
2. Nếu xếp hạng hiện tại của Monocarp`x`đã lớn hơn hoặc bằng tất cả các đối thủ thì anh ta có thể thắng mọi trận đấu. Số lượng trò chơi chỉ đơn giản là`y - x`. 
3. Ngược lại, lặp while`x < y`. Ở mỗi bước hãy đếm số`w`của đối thủ có xếp hạng nhỏ hơn hoặc bằng`x`. Đây là số lượng đối thủ Monocarp có thể đánh bại trong một chu kỳ. 
4. Nếu`w == 0`, Monocarp không thể thắng bất kỳ trò chơi nào trong vòng này và xếp hạng tối thiểu mà anh ta có thể đạt được sẽ giảm do hạn chế chơi đồng đều. Trong trường hợp này, đạt`y`là không thể, hãy quay lại`-1`. 
5. Tính mức tăng ròng trong xếp hạng trên mỗi chu kỳ hoàn chỉnh của`n`trò chơi như`gain = 2*w - n`. Điều này xuất phát từ chiến thắng`w`trò chơi (+1 mỗi trò) và thua`n - w`trò chơi (-1 mỗi trò chơi). 
6. Nếu`gain <= 0`Và`x < max(a_i)`, Monocarp không thể tiến bộ, quay lại`-1`. Ngược lại, hãy tính số chu kỳ cần thiết để đạt được`y`:`cycles_needed = ceil((y-x)/gain)`. Cập nhật`x`và tổng số trận đã chơi tương ứng:`games_played += cycles_needed * n`. 
7. Nếu`x`vượt quá`y`, trừ thêm trò chơi tương ứng với l
