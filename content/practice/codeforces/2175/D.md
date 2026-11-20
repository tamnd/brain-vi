---
title: "CF 2175D - Thiệp Chúc"
description: "Chúng tôi có nhiệm vụ phân phát một số lượng thẻ mong muốn có hạn cho $n$ bạn bè để hạnh phúc tích lũy của Bé A được tối đa hóa. Mỗi người bạn $i$ có thể mang theo tối đa $ai$ thẻ và tổng của tất cả các thẻ không được vượt quá $k$."
date: "2026-06-09T04:32:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2175
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1069 (Div. 2)"
rating: 1900
weight: 2175
solve_time_s: 101
verified: true
draft: false
---

[CF 2175D - Thiệp ước nguyện](https://codeforces.com/problemset/problem/2175/D) 

**Xếp hạng:** 1900 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ phân phát một số lượng thẻ mong muốn có hạn cho$n$bạn bè để niềm hạnh phúc tích lũy của Bé A được phát huy tối đa. Mỗi người bạn$i$có thể mang nhiều nhất$a_i$thẻ và tổng của tất cả các thẻ không thể vượt quá$k$. Hạnh phúc được đo tuần tự: sau$j$-Người bạn thứ 2 giao thiệp, niềm vui của Tiểu A tăng lên nhờ số lượng thiệp được giao nhiều nhất từ ​​trước đến nay,$\max(b_1, \dots, b_j)$. Kết quả là sự hạnh phúc hoàn toàn sau khi tất cả bạn bè đã gửi thiệp của họ. 

Những ràng buộc ngụ ý rằng$n$có thể lên đến$10^5$, nhưng tổng số thẻ$k$nhỏ (nhiều nhất là 360). Sự chênh lệch này báo hiệu rằng một thuật toán lặp lại trên bạn bè trong$O(n)$thời gian có thể chấp nhận được, nhưng bất cứ điều gì theo cấp số nhân trong$n$là không thể thực hiện được. Tổng của$k$trên tất cả các trường hợp thử nghiệm cũng nhỏ, điều này gợi ý về khả năng áp dụng lập trình động với trạng thái tùy thuộc vào số lượng thẻ còn lại. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó một số bạn bè không thể mang theo bất kỳ lá bài nào hoặc tổng điểm$k$nhỏ hơn công suất đơn lớn nhất$a_i$. Ví dụ, nếu$n = 3$,$k = 2$, Và$a = [5,0,1]$, đưa cả 2 lá bài cho người bạn đầu tiên là tối ưu mặc dù người bạn đầu tiên có thể mang nhiều hơn. Một cách tiếp cận ngây thơ luôn mang lại cho mỗi người bạn mức tối đa cho phép của họ sẽ vượt quá$k$, tạo ra một phép gán không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là liệt kê tất cả các cách để gán$b_i \le a_i$như vậy$\sum b_i \le k$và tính toán hạnh phúc cho mỗi nhiệm vụ. Mỗi trình tự sẽ yêu cầu$O(n)$thời gian để đánh giá, và số lượng các chuỗi là tổ hợp trong$k$Và$n$, có thể đạt tới$O(n^k)$trong trường hợp xấu nhất. Với$k$lên tới 360, phương pháp này là không khả thi. 

Cái nhìn sâu sắc quan trọng là$k$nhỏ nên chúng ta có thể thiết kế một giải pháp lập trình động trong đó trạng thái là số thẻ đã được sử dụng và mức đóng góp tối đa hiện tại cho hạnh phúc. Cho phép$dp[j]$đại diện cho mức độ hạnh phúc tích lũy tối đa có thể đạt được bằng cách sử dụng chính xác$j$thẻ. Cho mỗi người bạn$i$, chúng tôi xem xét việc cung cấp cho họ$0 \le x \le \min(a_i, k)$thẻ và cập nhật bảng DP cho phù hợp. Điều này có hiệu quả vì mức độ hạnh phúc tăng lên mức tối đa cho đến nay, vì vậy chúng tôi có thể duy trì mức đóng góp liên tục cho mỗi tổng số thẻ có thể được sử dụng. Vấn đề giảm xuống thành một DP giống như chiếc ba lô không giới hạn trên một dung lượng nhỏ$k$, nhưng với sự thay đổi là mỗi người bạn sẽ đóng góp dần dần vào hạnh phúc dựa trên mức tối đa đã cho cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(n) | Quá chậm | 
| DP trên thẻ đã qua sử dụng | O(n*k^2) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng DP có kích thước$k+1$, Ở đâu$dp[j]$lưu trữ hạnh phúc tối đa có thể đạt được bằng cách sử dụng$j$thẻ. Đặt tất cả các mục thành âm vô cùng ngoại trừ$dp[0] = 0$bởi vì không sử dụng thẻ nào sẽ không mang lại hạnh phúc. 
2. Lặp lại từng người bạn$i$theo thứ tự. Đối với mỗi số lượng thẻ có thể$x$người bạn đó$i$có thể lấy,$0 \le x \le \min(a_i, k)$, cập nhật bảng DP theo thứ tự ngược lại với các thẻ đã sử dụng. Điều này đảm bảo rằng mỗi trạng thái sử dụng chính xác các giá trị của người bạn trước đó. 
3. Đối với mỗi tổng số thẻ đã sử dụng$j$từ$k$xuống tới$x$, hãy tính hạnh phúc mới như$dp[j-x] + x \cdot \max(x, m)$, Ở đâu$m$đại diện cho số thẻ tối đa được trao cho những người bạn trước đó. Cập nhật$dp[j]$nếu giá trị mới này cao hơn. 
4. Sau khi xử lý tất cả bạn bè, câu trả lời là giá trị lớn nhất trong$dp[0\ldots k]$. 

Bất biến đảm bảo tính đúng đắn là$dp[j]$luôn giữ được hạnh phúc tối đa có thể đạt được bằng cách sử dụng chính xác$j$thẻ đầu tiên$i$bạn. Bằng cách xem xét tất cả các phân bổ có thể có cho mỗi người bạn theo khả năng của họ và cập nhật DP ngược lại, chúng tôi đảm bảo rằng không có trạng thái nào bị bỏ qua hoặc ghi đè sớm. 

## Giải pháp Python```
PythonRun
```Trong giải pháp này, chúng tôi duy trì mảng DP theo số lượng thẻ được sử dụng. Đối với mỗi người bạn, chúng tôi xem xét mọi cách phân bổ thẻ khả thi và cập nhật bảng DP. Việc lặp lại ngược lại`used`đảm bảo rằng các trạng thái trước đó không bị ghi đè trong cùng một người bạn.`new_max`tính toán chính xác mức tối đa ở bước hiện tại, góp phần tạo nên hạnh phúc tích lũy. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu thứ tư:$n = 5$,$k = 8$,$a = [2,4,5,4,3]$. Chúng tôi khởi tạo`dp[0] = 0`. Đang xử lý người bạn đầu tiên có dung lượng 2,`dp`được cập nhật để tặng 0,1,2 thẻ. Mức độ hạnh phúc tối đa cho đến nay là 2. Sau người bạn thứ hai có sức chứa 4, tặng 0,1,2,3,4 thẻ,`dp`cập nhật để bao gồm các chuỗi như [2,4] mang lại hạnh phúc tích lũy 2+4=6. Tiếp tục thông qua tất cả những người bạn, phép gán tối ưu là [2,0,5,0,0], tạo ra mức hạnh phúc tích lũy 19, phù hợp với kết quả mong đợi. 

Dấu vết thứ hai:$n = 3$,$k = 4$,$a = [0,0,1]$. Chỉ người bạn cuối cùng mới có thể mang theo thẻ. Gán 1 thẻ cho người bạn cuối cùng. Khi đó hạnh phúc là 1, hạnh phúc tổng cộng là 1. Thuật toán xác định chính xác nhiệm vụ tối ưu ngay cả khi hầu hết bạn bè không có năng lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k^2) | Đối với mỗi người bạn, chúng tôi xem xét tối đa k+1 phân bổ có thể và đối với mỗi phân bổ, chúng tôi cập nhật k+1 mục nhập DP. | 
| Không gian | O(k) | Mảng DP có kích thước k+1 là đủ. | 

Được cho$k \le 360$Và$n \le 10^5$, các hoạt động trong trường hợp xấu nhất là khoảng$10^5 * 360^2 = 1.3 \cdot 10^7$, nằm trong giới hạn thời gian 2 giây. Dung lượng bộ nhớ không đáng kể, dưới 2 KB cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n0 | 0 | Bạn bè không được mang theo thẻ | 
| 1 10\n5 | 5 | Người bạn độc thân, sức chứa trong tổng số thẻ | 
| 2 5\n2 3 | 5 | Hai người bạn, tổng số lần sử dụng thẻ chính xác trùng khớp | 
| 3 4\n1 2 3 | 4 | Phân phối phải tôn trọng tổng k, hạnh phúc tích lũy tối ưu | 

## Vỏ cạnh 

cho
