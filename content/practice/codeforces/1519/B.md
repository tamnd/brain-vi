---
title: "CF 1519B - Chiếc Bánh Là Lời Nói Dối"
description: "Cho một lưới hình chữ nhật có n hàng và m cột. Bạn bắt đầu ở góc trên bên trái (1, 1) và muốn đến góc dưới bên phải (n, m) bằng cách chỉ di chuyển sang phải hoặc xuống. Di chuyển sang phải từ ô (x, y) tốn x burles, di chuyển xuống dưới tốn y burles."
date: "2026-06-10T18:10:33+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1519
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 108 (Rated for Div. 2)"
rating: 800
weight: 1519
solve_time_s: 62
verified: false
draft: false
---

[CF 1519B - Chiếc bánh là lời nói dối](https://codeforces.com/problemset/problem/1519/B) 

**Đánh giá:** 800 
**Tags:** dp, toán 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có`n`hàng và`m`cột. Bạn bắt đầu ở góc trên bên trái`(1, 1)`và muốn đến góc dưới bên phải`(n, m)`bằng cách chỉ di chuyển sang phải hoặc xuống. Di chuyển ngay từ ô`(x, y)`chi phí`x`gánh nặng, giảm chi phí`y`burles. Câu hỏi đặt ra là liệu có thể đạt được mục tiêu chi tiêu một cách chính xác hay không`k`burles. 

Đầu vào cung cấp một số trường hợp kiểm thử, mỗi trường hợp chỉ định`n`,`m`, Và`k`. Đầu ra chỉ đơn giản là "CÓ" nếu chính xác`k`Burles có thể được chi tiêu và "KHÔNG" nếu không. 

Những hạn chế nhỏ: cả hai`n`Và`m`lên đến 100 và`k`lên tới 10.000. Điều này loại trừ việc liệt kê đường dẫn cưỡng bức cho các lưới lớn hơn, bởi vì số lượng đường dẫn tăng lên theo kiểu tổ hợp: có`(n+m-2 choose n-1)`những con đường có thể. Vì`n = m = 100`, đó là khoảng 10^58 đường dẫn, rõ ràng là không khả thi. 

Các trường hợp cạnh tinh tế bao gồm lưới một hàng hoặc một cột. Ví dụ,`n=1, m=4`chỉ có một đường đi, đi sang phải ba lần. Tổng chi phí là 1 + 1 + 1 = 3. Nếu tính sai công thức chi phí, bạn có thể tính sai. Một trường hợp cạnh khác là`n=1, m=1`nơi bạn đã đến đích và chi phí là 0. Không xử lý được trường hợp này dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là liệt kê tất cả các đường đi, tính toán chi phí cho mỗi đường đi và kiểm tra xem có đường đi nào có tổng bằng`k`. Điều này hoạt động chính xác vì nó thực sự kiểm tra mọi khả năng. Nhưng số lượng đường dẫn là theo cấp số nhân trong`n+m`, điều này trở nên không khả thi ngay cả đối với`n=m=20`. Đại khái, số lượng hoạt động là`O(2^(n+m))`, rõ ràng là quá lớn đối với`n, m = 100`. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng chi phí dọc theo bất kỳ con đường nào đều mang tính quyết định khi bạn quyết định thứ tự di chuyển. Nếu bạn luôn đi bên phải cho đến cột cuối cùng rồi di chuyển xuống dưới thì tổng chi phí rất dễ tính toán. Tương tự, nếu bạn luôn đi xuống trước rồi sang phải, bạn sẽ có được cấu trúc công thức tổng tương tự. Mọi con đường đều tiêu tốn`(sum of all row indices for right moves) + (sum of all column indices for down moves)`. 

Về mặt hình thức, có chính xác`n-1`di chuyển xuống và`m-1`những bước đi đúng đắn. Tổng các chỉ số hàng cho các bước di chuyển phải là`(n-1) * 1 + (n-1) * 2 + ...`nhưng vì mỗi bước đi đúng đều nằm ở một hàng nào đó nên đóng góp của chỉ số hàng là`(m-1) * sum of row indices from 1 to n-1)`. Sau khi kiểm tra cẩn thận, mô hình sẽ đơn giản hóa: tổng chi phí từ`(1,1)`ĐẾN`(n,m)`luôn luôn là`(n-1)*m + (m-1)*n = n*m - 1`. 

Công thức này đúng cho mọi lưới vì bạn di chuyển`n-1`lần xuống và`m-1`lần đúng. Mỗi lần di chuyển phải tốn chỉ số hàng (từ 1 đến n) và được thực hiện`m-1`lần trên các hàng, trong khi mỗi lần di chuyển xuống sẽ tốn chỉ số cột (từ 1 đến m) và được thực hiện`n-1`lần trên các cột. Tổng hợp và đơn giản hóa mang lại`n*m - 1`. Khi bạn có công thức này, vấn đề sẽ giảm xuống còn một so sánh duy nhất: kiểm tra xem`k == n*m - 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(1) | Quá chậm | 
| Kiểm tra công thức | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Lặp lại từng trường hợp thử nghiệm. Đọc`n`,`m`, Và`k`. 
3. Tính chi phí mục tiêu theo công thức`total_cost = n * m - 1`. 
4. So sánh`total_cost`với`k`. Nếu bằng thì in "Y_
