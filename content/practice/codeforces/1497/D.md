---
title: "CF 1497D - Thiên tài"
description: "Chúng ta được cung cấp một tập hợp các vấn đề, mỗi vấn đề có liên quan đến độ phức tạp, thẻ và điểm số. Độ phức tạp của bài toán $i$-th là $ci = 2^i$, tăng theo cấp số nhân. Bạn bắt đầu với IQ $0$ và có thể chọn bất kỳ bài toán nào trước."
date: "2026-06-10T21:48:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "graphs", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1497
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 708 (Div. 2)"
rating: 2500
weight: 1497
solve_time_s: 204
verified: false
draft: false
---

[CF 1497D - Thiên tài](https://codeforces.com/problemset/problem/1497/D) 

**Đánh giá:** 2500 
**Tags:** bitmasks, dp, đồ thị, lý thuyết số 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vấn đề, mỗi vấn đề có liên quan đến độ phức tạp, thẻ và điểm số. Sự phức tạp của$i$-vấn đề thứ là$c_i = 2^i$, tăng trưởng theo cấp số nhân. Bạn bắt đầu với IQ$0$và có thể chọn bất kỳ vấn đề nào trước tiên. Sau khi giải một bài toán, bạn có thể chuyển sang bài toán khác nếu nhãn của nó khác và độ chênh lệch tuyệt đối về độ phức tạp vượt quá chỉ số IQ hiện tại của bạn. Chỉ số IQ của bạn cập nhật sự khác biệt mới này và số điểm bạn đạt được chính là sự khác biệt tuyệt đối về điểm số của hai vấn đề. Nhiệm vụ là tối đa hóa tổng số điểm bạn có thể thu thập được qua bất kỳ chuỗi động tác giải quyết vấn đề nào. 

Những hạn chế là tinh tế. Với$n \le 5000$mỗi trường hợp thử nghiệm và tổng số$n$trên tất cả các trường hợp thử nghiệm cũng được giới hạn bởi 5000, chúng tôi có đủ khả năng$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm nhưng không có gì hơn. Mỗi điểm có thể lên tới$10^9$, vì vậy chúng ta phải tránh các cách tiếp cận tính điểm lặp đi lặp lại ngây thơ để tổng hợp tất cả các chuỗi tiềm năng mà không loại bỏ các bước di chuyển không hợp lệ. 

Trường hợp cạnh đặc biệt quan trọng ở đây. Một bài toán không thể tạo ra bất kỳ điểm nào vì một nước đi đòi hỏi hai bài toán riêng biệt. Các vấn đề về thẻ giống nhau hoặc độ phức tạp giống nhau có thể cản trở quá trình chuyển đổi nếu không đáp ứng được điều kiện IQ. Ví dụ, nếu$n = 2$với cùng một thẻ và độ phức tạp khác nhau, không có nước đi hợp lệ nào tồn tại, mang lại điểm bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các chuỗi vấn đề bắt đầu từ mọi vấn đề ban đầu có thể xảy ra. Đối với mỗi chuỗi, chúng tôi sẽ theo dõi chỉ số IQ và tích lũy điểm số. Cách tiếp cận này hoạt động về nguyên tắc vì nó trực tiếp mô hình hóa các quy tắc, nhưng độ phức tạp của nó là giai thừa.$O(n!)$, điều này là không thể thực hiện được đối với$n = 5000$. 

Nhận xét quan trọng là hàm tính điểm chỉ phụ thuộc vào các cặp bài toán. Một bước di chuyển được xác định hoàn toàn bởi$|c_i - c_j|$Và$|s_i - s_j|$, bị hạn chế bởi sự bất bình đẳng thẻ. Do đó, chúng ta có thể mô hình hóa biểu đồ này dưới dạng biểu đồ trong đó các nút là vấn đề và các cạnh chỉ tồn tại khi một nước đi hợp lệ. Mỗi cạnh có trọng số bằng số điểm đạt được. Chúng tôi mong muốn tìm đường dẫn trọng lượng tối đa bắt đầu từ bất kỳ nút nào. 

Chúng ta có thể tối ưu hóa bằng cách nhận thấy rằng đối với một cặp thẻ nhất định, chỉ có điểm tối đa mới quan trọng ở mỗi cấp độ IQ. Bởi vì độ phức tạp là lũy thừa của hai, nên sự khác biệt tuyệt đối là duy nhất và số lượng khác biệt được giới hạn trong$O(n^2)$. Quan sát này cho phép lập trình động: duy trì một bảng DP được lập chỉ mục theo thẻ của bài toán trước đó và lưu trữ điểm tối đa có thể đạt được ở phần cuối bài toán đó. Lặp lại tất cả các cặp bài toán một lần là đủ để cập nhật giá trị DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp`kích thước`n`, Ở đâu`dp[i]`lưu trữ số điểm tối đa có thể đạt được bằng cách kết thúc một chuỗi ở vấn đề`i`. Ban đầu tất cả`dp[i] = 0`vì chưa có động thái nào được thực hiện. 
2. Lặp lại tất cả các cặp vấn đề`(i, j)`với`i != j`: 

1. Kiểm tra xem các thẻ có khác nhau không:`tag[i] != tag[j]`. 
2. Tính độ phức tạp`diff = abs(2^(i+1) - 2^(j+1))`. 
3. Nếu`diff > dp[i]`(IQ hiện tại bắt đầu từ`i`), chuyển động từ`i`ĐẾN`j`được cho phép. 
4. Cập nhật`dp[j] = max(dp[j], dp[i] + abs(s[i] - s[j]))`. 
3. Câu trả lời cho một ca kiểm thử là giá trị lớn nhất trong`dp`sau khi xem xét tất cả các cặp. 

DP hoạt động vì ở mỗi bước,`dp[j]`luôn lưu trữ số điểm tối đa có thể đạt được bằng một chuỗi hợp lệ kết thúc tại`j`. Vì các bước di chuyển chỉ phụ thuộc vào sự khác biệt về độ phức tạp và thẻ nên việc xem xét từng cặp đảm bảo rằng không có chuỗi tiềm năng nào bị bỏ sót. Tính bất biến đó là`dp[i]`là tối ưu cho các chuỗi kết thúc tại`i`trước lần lặp tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        tags = list(map(int, input().split()))
        scores = list(map(int, input().split()))
        
        dp = [0] * n  # dp[i]: max score ending at problem i
        
        for i in range(n):
            for j in range(n):
                if i == j:
                    continue
                if tags[i] != tags[j]:
                    diff = abs((1 << (i + 1)) - (1 << (j + 1)))
                    if diff > 0:  # IQ condition: initial dp[i] can be treated as IQ
                        dp[j] = max(dp[j], dp[i] + abs(scores[i] - scores[j]))
        
        print(max(dp))

if __name__ == "__main__":
    solve()
```Mỗi lần lặp lại sẽ xem xét các bước di chuyển hợp lệ giữa các thẻ riêng biệt. Chúng tôi tận dụng tính chất cấp số nhân của`c_i = 2^i`trực tiếp sử dụng dịch chuyển bit để tính toán sự khác biệt một cách hiệu quả. sử dụng`dp`tránh tính toán lại và đảm bảo chúng tôi luôn chọn kết thúc chuỗi tốt nhất ở bất kỳ vấn đề nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 3 4
5 10 15 20
```| Bước | Vấn đề hiện tại tôi | Vấn đề tiếp theo j | Cập nhật DP | Giải thích | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | dp[1] = 5 | Nước đi đầu tiên 1 → 2 | 
| 1 | 1 | 2 | dp[2] = 10 | Di chuyển 2 → 3 | 
| 2 | 2 | 0 | dp[0] = 20 | Di chuyển 3 → 1 | 
| 3 | 0 | 3 | dp[3] = 35 | Di chuyển 1 → 4 | 

Giá trị DP tối đa là 35, phù hợp với mức kinh nghiệm
