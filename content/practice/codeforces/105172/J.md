---
title: "CF 105172J - Chia dãy (bản dễ)"
description: "Chúng ta được cho một dãy số nguyên và chúng ta phải cắt nó thành chính xác $k$ các phần liên tiếp. Sau khi phân chia chuỗi, chúng ta xem xét bên trong mỗi phần và đếm xem có bao nhiêu mảng con bên trong phần đó có tổng chính xác bằng một giá trị cố định $x$."
date: "2026-06-27T08:25:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105172
codeforces_index: "J"
codeforces_contest_name: "The 20th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105172
solve_time_s: 27
verified: false
draft: false
---

[CF 105172J - Chia dãy (phiên bản dễ)](https://codeforces.com/problemset/problem/105172/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta phải cắt nó thành chính xác$k$miếng liên tiếp. Sau khi tách chuỗi, chúng ta xem xét bên trong mỗi phần và đếm xem có bao nhiêu mảng con bên trong phần đó có tổng bằng một giá trị cố định$x$. Chi phí cuối cùng của một phân vùng là tổng của những chi phí này trên tất cả$k$các phân đoạn và chúng tôi muốn giảm thiểu chi phí đó bằng tất cả các cách hợp lệ để phân chia chuỗi. 

Điểm mấu chốt là mỗi phân đoạn đều đóng góp độc lập vào chi phí sau khi được cố định, nhưng việc lựa chọn vị trí cắt sẽ ảnh hưởng đến cách phân nhóm các mảng con và do đó có bao nhiêu “tổng bằng”.$x$” các khoảng được tính bên trong mỗi phân đoạn. 

Những hạn chế$n \le 3000$Và$k \le 20$ngay lập tức gợi ý rằng bất cứ thứ gì hình khối trong$n$có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến việc chia nhỏ theo cấp số nhân hoặc tính toán lại chi phí phân đoạn từ đầu cho tất cả các phân vùng đều quá chậm. Ngưỡng điển hình ở đây là khoảng$n^2 k$, đó là về$1.8 \cdot 10^8$, ranh giới nhưng có thể chấp nhận được trong DP được tối ưu hóa. 

Một nỗ lực ngây thơ sẽ cố gắng liệt kê tất cả các cách để chia thành$k$các phân đoạn, nhưng điều đó là không thể về mặt tổ hợp vì có$\binom{n-1}{k-1}$sự lựa chọn, đã là thiên văn đối với$n=3000, k=20$. 

Một sai lầm ngây thơ thứ hai là tính toán lại số lượng mảng con hợp lệ cho mỗi phân đoạn một cách lặp đi lặp lại. Ngay cả khi chúng tôi sửa các phân đoạn, việc tính toán số lượng mảng con có tổng bằng$x$bên trong mỗi phân đoạn trong$O(n^2)$và việc nhúng nó vào trong tìm kiếm phân vùng dẫn đến ít nhất$O(n^3)$hoặc tệ hơn. 

Một trường hợp khó nhận thấy là khi$k = n$. Khi đó mọi đoạn đều có độ dài 1, do đó không tồn tại phân đoạn nào dài hơn 1. Câu trả lời đơn giản là số phần tử bằng$x$. Bất kỳ DP nào giả định các phân đoạn phải có độ dài ít nhất là 2 sẽ không thành công ở đây. 

Một trường hợp cạnh khác là khi$k = 1$. Sau đó, chúng tôi hoàn toàn không phân chia và chúng tôi chỉ đếm tất cả các mảng con trong mảng đầy đủ có tổng là$x$. Điều này đóng vai trò là đường cơ sở chính xác cho phương pháp đếm mảng con. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét quan điểm bạo lực. Nếu chúng ta sửa một phân vùng của mảng thành$k$phân khúc, chúng ta có thể tính toán chi phí của từng phân khúc một cách độc lập. Đối với một phân khúc$[l, r]$, số mảng con có tổng$x$có thể được tính bằng cách sử dụng tổng tiền tố và bản đồ băm trong$O(r-l+1)$. Điều này đã mang lại$O(n)$trên mỗi phân đoạn, do đó$O(nk)$cho một phân vùng cố định. 

Khó khăn là có nhiều phân vùng theo cấp số nhân. Đang thử mọi cách để đặt$k-1$việc cắt giảm là không thể thực hiện được. 

Quan sát quan trọng là chi phí bên trong một phân khúc chỉ phụ thuộc vào điểm cuối của nó chứ không phụ thuộc vào cấu trúc toàn cầu. Điều này làm cho vấn đề trở thành một vấn đề lập trình động phân vùng theo khoảng thời gian cổ điển. Chúng tôi xác định DP nơi chúng tôi quyết định từng bước nơi mỗi phân đoạn kết thúc. 

Để thực hiện chuyển đổi hiệu quả, chúng tôi tính toán trước chi phí của mỗi khoảng thời gian$[l, r]$, là số mảng con bên trong nó có tổng bằng$x$. Quá trình tiền xử lý này có thể được thực hiện trong$O(n^2)$sử dụng tổng tiền tố và bản đồ tần số cho mỗi điểm bắt đầu. 

Khi có bảng chi phí này, vấn đề sẽ trở thành: phân vùng tiền tố$1..i$vào trong$j$phân khúc giảm thiểu tổng chi phí. Quá trình chuyển đổi sẽ thử tất cả các vị trí cắt trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các phân vùng | hàm mũ | O(1) | Quá chậm | 
| Chi phí tính toán trước khoảng thời gian + DP |$O(n^2 + n^2 k)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định tổng tiền tố$p[i]$, Ở đâu$p[i]$là tổng của số đầu tiên$i$các phần tử. Điều này cho phép chúng ta tính tổng bất kỳ mảng con nào trong thời gian không đổi. 

Chúng tôi tính toán trước một bảng$cost[l][r]$, đếm xem có bao nhiêu mảng con bên trong$[l, r]$có tổng chính xác$x$. Đối với mỗi cố định$l$, chúng tôi mở rộng$r$và duy trì một bản đồ băm của các tổng tiền tố được thấy bên trong phân đoạn đó. 

Sau đó chúng tôi xác định DP: 

hãy để$dp[j][i]$là chi phí tối thiểu để phân chia tiền tố$1..i$vào chính xác$j$phân đoạn. 

Quá trình chuyển đổi xem xét đoạn cuối cùng bắt đầu tại vị trí$t+1$và kết thúc tại$i$, Vì thế:$$dp[j][i] = \min_{t < i} dp[j-1][t] + cost[t+1][i]$$Câu trả lời là$dp[k][n]$. 

### Các bước 

1. Tính tổng tiền tố$p[i]$. Điều này cho phép truy vấn tổng mảng con theo thời gian không đổi. 
2. Tính toán trước$cost[l][r]$cho tất cả$1 \le l \le r \le n$. Đối với mỗi$l$, duy trì bản đồ tần số của các tổng tiền tố trong khi mở rộng$r$, và đếm tần suất$p[r] - p[i] = x$. 
3. Khởi tạo mảng DP với giá trị vô cùng và đặt$dp[0][0] = 0$. Điều này thể hiện một tiền tố trống được chia thành các phân đoạn bằng 0. 
4. Đối với mỗi số đoạn$j$từ 1 đến$k$, tính toán tất cả$dp[j][i]$vì$i \ge j$. 
5. Đối với mỗi$i$, thử tất cả các vị trí cắt trước đó$t$từ$j-1$ĐẾN$i-1$, đang cập nhật$dp[j][i]$sử dụng chi phí khoảng thời gian được tính toán trước. 
6. Trở về$dp[k][n]$. 

### Tại sao nó hoạt động 

DP duy trì sự bất biến rằng$dp[j][i]$lưu trữ chi phí tối ưu theo mọi cách để phân vùng tiền tố$1..i$vào chính xác$j$các đoạn liền kề. Mọi phân vùng hợp lệ của$1..i$vào trong$j$phân đoạn phải kết thúc bằng một số phân đoạn cuối cùng$[t+1, i]$và phần trước phải là phân vùng tối ưu của$1..t$vào trong$j-1$phân đoạn. Vì chi phí của từng bộ phận là độc lập và được nắm bắt đầy đủ bởi$cost[l][r]$, phép truy hồi khám phá tất cả giá trị
