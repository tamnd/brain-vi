---
title: "CF 104686H - ​​Hạt dao"
description: "Chúng tôi được cấp ba chuỗi. Chúng ta bắt đầu bằng một chuỗi cơ sở s và chúng ta được phép lấy một chuỗi t khác và chèn nó vào bất kỳ vị trí nào bên trong s, kể cả trước ký tự đầu tiên hoặc sau ký tự cuối cùng. Điều này tạo ra một chuỗi kết hợp mới."
date: "2026-06-29T08:51:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104686
codeforces_index: "H"
codeforces_contest_name: "2022-2023 ICPC Central Europe Regional Contest (CERC 22)"
rating: 0
weight: 104686
solve_time_s: 26
verified: false
draft: false
---

[CF 104686H - Phần chèn](https://codeforces.com/problemset/problem/104686/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp ba chuỗi. Chúng tôi bắt đầu với một chuỗi cơ sở`s`, và chúng tôi được phép lấy một chuỗi khác`t`và chèn nó vào bất kỳ vị trí nào bên trong`s`, kể cả trước ký tự đầu tiên hoặc sau ký tự cuối cùng. Điều này tạo ra một chuỗi kết hợp mới. Sau khi thực hiện chèn xong ta đếm chuỗi thứ 3 được bao nhiêu lần`p`xuất hiện dưới dạng chuỗi con liền kề trong chuỗi kết quả. Các lần xuất hiện có thể trùng nhau nên mọi kết quả trùng khớp hợp lệ đều được tính độc lập. 

Nhiệm vụ không phải là xây dựng chuỗi tốt nhất một cách rõ ràng mà là suy luận về tất cả các vị trí chèn có thể có. Đối với mỗi chỉ số`k`TRONG`0 … |s|`, chúng tôi xem xét việc chèn`t`tại vị trí đó và tính xem có bao nhiêu lần xuất hiện của`p`xuất hiện. Chúng ta phải xuất ra bốn giá trị: số lần xuất hiện tối đa trên tất cả các vị trí, số lượng vị trí đạt được mức tối đa đó và trong số các vị trí đó chỉ số hợp lệ nhỏ nhất và lớn nhất. 

Các ràng buộc rất lớn: mỗi chuỗi có thể lên tới 100.000 ký tự. Bất kỳ giải pháp nào tính toán lại kết quả khớp chuỗi con từ đầu cho từng vị trí chèn sẽ quá chậm vì có`O(n)`các vị trí và mỗi lần kiểm tra có thể tốn kém`O(n)`, dẫn đến`O(n^2)`hành vi không thể thực hiện được. 

Một khó khăn chính là sự xuất hiện của`p`có thể nằm giữa ranh giới chèn. Các trận đấu có thể bắt đầu trong`s`và tiếp tục vào`t`, hoặc bắt đầu vào`t`và tiếp tục vào hậu tố của`s`. Một phương pháp đơn giản chỉ đếm số lần xuất hiện bên trong mỗi phần một cách độc lập sẽ bỏ lỡ các mẫu xuyên biên giới này. 

Các trường hợp cạnh phát sinh khi`p`dài hơn cả hai`s`hoặc`t`, hoặc khi`p`trùng lặp rất nhiều với chính nó. Trong những trường hợp như vậy, các phương pháp phân tách đơn giản không thành công vì chúng không tính đến sự liên kết tiền tố-hậu tố một phần qua điểm chèn. 

## Phương pháp tiếp cận 

Giải pháp brute-force thử mọi vị trí chèn`k`. Đối với mỗi`k`, chúng tôi xây dựng chuỗi kết quả và chạy tìm kiếm chuỗi con tiêu chuẩn cho`p`, ví dụ như sử dụng KMP. Cái này đã tốn rồi`O(|s|)`theo từng vị trí, đưa ra`O(|s|^2)`tổng thời gian. Ngay cả khi tối ưu hóa, việc xây dựng lại hoặc mô phỏng mỗi lần chèn vẫn lặp lại hầu hết công việc một cách không cần thiết. 

Quan sát quan trọng là việc chèn`t`thay đổi câu trả lời một cách có cấu trúc. Bất kỳ sự xuất hiện của`p`ở chuỗi cuối cùng thuộc đúng một trong ba loại. Nó hoàn toàn ở bên trong`s`, hoàn toàn bên trong`t`, hoặc nó vượt qua ranh giới giữa chúng. Hai loại đầu tiên độc lập với vị trí chèn ngoại trừ các ca; chỉ những lần xuất hiện xuyên biên giới mới phụ thuộc vào vị trí chúng tôi chèn vào. 

Điều này gợi ý nên tách vấn đề thành đóng góp tĩnh và đóng góp động. Chúng ta có thể tính toán trước tất cả các lần xuất hiện của`p`bên trong`s`và bên trong`t`. Khó khăn duy nhất còn lại là đếm số lần xuất hiện khi tiền tố của`p`đang ở trong`s`và hậu tố ở trong`t`, hoặc ngược lại qua các điểm nối. 

Để xử lý vấn đề này một cách hiệu quả, chúng tôi sử dụng phương pháp khớp kiểu tiền tố-hàm (tính toán đường viền KMP). Đối với mọi vị trí trong`s`Và`t`, chúng tôi tính toán có bao nhiêu`p`khớp dưới dạng hậu tố kết thúc ở đó hoặc tiền tố bắt đầu từ đó. Điều này cho phép chúng tôi liệt kê tất cả các điểm phân chia tiềm năng trong đó`p`vượt qua ranh giới. Sau đó, đối với mỗi vị trí chèn, chúng ta có thể tổng hợp các đóng góp bằng cách sử dụng tổng tiền tố trên các cách sắp xếp hợp lệ. 

Bước cuối cùng là giảm tính toán lại cho mỗi vị trí. Thay vì tính toán lại số lượng xuyên biên giới cho mỗi`k`, chúng tôi tính toán trước số lượng căn chỉnh được kích hoạt cho mỗi chỉ mục chèn bằng cách sử dụng các mảng sai phân hoặc tích lũy sự kiện qua các khoảng căn chỉnh hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | s | ^2 + | 
| Tối ưu | O( | s | + | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các trường hợp xảy ra`p`dưới dạng đóng góp từ ba nguồn: bên trong`s`, bên trong`t`và những điểm đi qua điểm chèn. 

1. Tính tất cả các lần xuất hiện của`p`bên trong`s`và bên trong`t`sử dụng KMP. 

Điều này đưa ra hai số đếm cơ sở độc lập với vị trí chèn. Những điều này tạo thành một đường cơ sở không đổi được thêm vào cho mọi ứng cử viên`k`. 
2. Xây dựng mảng tiền tố hàm cho`p`và sử dụng chúng để tính toán thông tin phù hợp trên`s`Và`t`. 

Đối với mỗi vị trí trong`s`, chúng tôi xác định có bao nhiêu`p`có thể kết thúc ở vị trí đó dưới dạng kết hợp hậu tố. Tương tự, với mỗi vị trí trong`t`, chúng tôi xác định kết quả khớp tiền tố của`p`bắt đầu từ đó. Điều này là cần thiết vì sự xuất hiện giao cắt được xác định bởi sự phân chia`p = A + B`, Ở đâu`A`kết thúc bằng`s`Và`B`bắt đầu vào`t`. 
3. Liệt kê tất cả các độ dài được chia của`p`. 

Đối với một vị trí chia`x`, tiền tố`p[0:x]`phải xuất hiện kết thúc bằng`s`, và hậu tố`p[x:]`phải xuất hiện bắt đầu từ`t`. Bằng cách sử dụng các mảng so khớp được tính toán trước, chúng ta đánh dấu tất cả các vị trí cuối hợp lệ trong`s`tiền tố hỗ trợ đó`x`và tất cả các vị trí bắt đầu hợp lệ trong`t`hậu tố hỗ trợ đó`x`. 
4. Chuyển đổi các điều kiện khớp hợp lệ này thành đóng góp cho các vị trí chèn. 

Sự phân chia góp phần vào tất cả các chỉ số chèn`k`mặt trời ở đâu
