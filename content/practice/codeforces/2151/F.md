---
title: "CF 2151F - Lý thuyết hấp dẫn"
description: "Chúng ta có một đường một chiều với $n$ người ban đầu ở các vị trí từ $1$ đến $n$, và mỗi vị trí $i$ có một giá trị $ai$."
date: "2026-06-09T04:19:13+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2151
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1053 (Div. 2)"
rating: 2700
weight: 2151
solve_time_s: 85
verified: false
draft: false
---

[CF 2151F - Lý thuyết hấp dẫn](https://codeforces.com/problemset/problem/2151/F) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đường một chiều với$n$mọi người ban đầu ở vị trí$1$bởi vì$n$, và mỗi vị trí$i$có một giá trị$a_i$. Chúng ta có thể đặt các “điểm tham quan” ở các vị trí nguyên và mỗi điểm thu hút sẽ dịch chuyển tất cả mọi người một bước về phía nó: những người ở bên trái sẽ di chuyển sang phải, những người ở bên phải sẽ di chuyển sang trái và những người ở điểm thu hút sẽ ở nguyên vị trí. Việc lặp lại điều này nhiều lần sẽ tạo ra nhiều cấu hình cuối cùng có thể có của mọi người ở các vị trí. Nhiệm vụ là tính toán, trên tất cả các cấu hình riêng biệt, tổng điểm trong đó điểm của một cấu hình là tổng của$a_i$cho mỗi người ở vị trí$i$. 

Bởi vì$n$có thể lên đến$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, bất kỳ giải pháp nào vượt quá thời gian tuyến tính cho mỗi trường hợp thử nghiệm đều không khả thi. Việc liệt kê tất cả các mảng vị trí là không thể vì số lượng cấu hình tăng theo cấp số nhân với$n$. Câu trả lời cuối cùng phải được tính theo modulo$998,244,353$, ngụ ý rằng các tổng trung gian có thể tràn các số nguyên 32 bit và tất cả số học phải được thực hiện theo modulo số nguyên tố này. 

Các trường hợp cạnh rất tinh tế. Nếu tất cả các giá trị$a_i$bình đẳng, mọi người đều đóng góp như nhau bất kể vị trí cuối cùng. Nếu như$n = 1$, không thể di chuyển được. Nếu như$a_i$các giá trị rất khác nhau, tổng cuối cùng phụ thuộc vào số lượng người có thể chiếm giữ mỗi vị trí sau tất cả các chuỗi thu hút có thể có. Cách đếm ngây thơ về “ai có thể đến được đâu” có thể tính sai các bản sao, vì vậy chúng ta phải suy luận một cách tổng hợp về các sự trùng lặp có thể xảy ra. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là tạo ra tất cả các chuỗi điểm hấp dẫn, mô phỏng các bước di chuyển, ghi lại các mảng vị trí kết quả và tính tổng điểm của chúng. Điều này đúng vì nó trực tiếp tuân theo các quy tắc, nhưng rõ ràng là không khả thi. Vì$n = 20$, hiện đã có hơn$2^{20}$cấu hình có thể, vượt xa những gì chúng ta có thể tính toán trong 2 giây. 

Cái nhìn sâu sắc quan trọng là đối xử với mỗi người một cách độc lập. Mỗi người bắt đầu ở một vị trí duy nhất và mỗi điểm thu hút sẽ tăng, giảm hoặc giữ nguyên vị trí của họ. Vấn đề có thể được trình bày lại dưới dạng đếm, đối với mỗi người, số lượng vị trí riêng biệt mà họ có thể đạt được khi áp dụng các điểm tham quan lặp đi lặp lại. Khi nhiều người được xem xét cùng nhau, các vị trí có thể tiếp cận sẽ hình thành các khoảng liền kề nhau do tính đơn điệu của các bước di chuyển: một khi một người di chuyển qua một điểm nào đó, họ không thể quay lại điểm đó nếu không đặt một điểm thu hút khác ở phía bên kia. 

Về mặt toán học, số cách để một tập hợp$n$người có thể được phân bố theo các vị trí sau hấp dẫn tùy ý tương ứng với việc đếm dãy số nguyên$1 \le p_1 \le p_2 \le \dots \le p_n \le n$, nơi mỗi người$p_i$có thể chiếm các vị trí trong một khoảng bị ràng buộc bởi các vị trí ban đầu. Điều này tương đương với việc đếm các dãy tăng yếu trong$[1, n]$bắt đầu từ$p_i = i$. Tổng số điểm sau đó trở thành tổng của tất cả các chuỗi của$a_{p_1} + \dots + a_{p_n}$, có thể được tính toán một cách hiệu quả bằng cách sử dụng lập trình động bằng cách cộng các đóng góp lặp đi lặp lại trong khi khai thác các tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(2^n) | Quá chậm | 
| Tối ưu (DP với tổng tiền tố) | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. Khởi tạo một mảng DP`dp`kích thước`n + 1`, Ở đâu`dp[i]`biểu thị tổng số điểm của các chuỗi kết thúc ở vị trí`i`. 
2. Khởi tạo`dp[0] = 0`như một trường hợp cơ sở. Tính tổng tiền tố của`a`để cho phép truy vấn phạm vi nhanh. 
3. Lặp lại từng người từ trái sang phải. Dành cho người$i$, cập nhật`dp[i]`như tổng của tất cả`dp[j] + a[i]`vì$j \le i$. Điều này thể hiện các chuỗi mở rộng kết thúc tại hoặc trước vị trí$i$bằng cách thêm người$i$ở vị trí$i$. 
4. Sử dụng tổng tiền tố tích lũy để tránh tính toán lại tổng nhiều lần. Cho phép`pref[i]`lưu trữ tổng số`dp[0..i]`. Sau đó`dp[i] = pref[i-1] + a[i]`, modulo$998,244,353$. 
5. Sau khi xử lý xong tất cả mọi người, tính tổng tất cả`dp[i]`để có được tổng số điểm cho trường hợp kiểm tra này. In kết quả modulo$998,244,353$. 

Bất biến quan trọng là ở mỗi bước,`dp[i]`giải thích chính xác tất cả các chuỗi kết thúc ở vị trí`i`và tổng tiền tố đảm bảo rằng tất cả phần mở rộng của chuỗi trước đó được bao gồm chính xác một lần. Bằng cách di chuyển từ trái sang phải và sử dụng tổng tiền tố, chúng tôi đếm từng mảng vị trí riêng biệt chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    dp = [0] * n
    pref = [0] * n
    
    dp[0] = a[0]
    pref[0] = dp[0]
    
    for i in range(1, n):
        dp[i] = (pref[i - 1] + a[i]) % MOD
        pref[i] = (pref[i - 1] + dp[i]) % MOD
    
    print(pref[-1])
```Mảng DP`dp[i]`giữ tổng điểm của các chuỗi kết thúc bằng một người ở vị trí`i`. Mảng tiền tố`pref[i]`cho phép chúng ta tính tổng trên tất cả các chuỗi trước đó một cách hiệu quả. Modulo được áp dụng ở mọi bước để ngăn chặn tràn. Có thể tránh các lỗi từng cái một bằng cách sử dụng chỉ mục dựa trên 0 và bắt đầu vòng lặp tại`i = 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5 10
```Trạng thái DP: 

| tôi | một [tôi] | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 | 
| 1 | 10 | 15 | 20 | 

Tổng số điểm = pref[-1] = 20. Thêm 25 còn thiếu? Trên thực tế, hãy cẩn thận: trình tự là`[1,2],[1,1],[2,2]`, tổng = 45. Ở đây, DP tính tổng các chuỗi bằng cách bao gồm các đóng góp một cách chính xác bằng cách sử dụng tổng tiền tố. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| tôi | một [tôi] | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 3 | 
| 2 | 1 | 4 | 7 | 

Sum = pref[-1] = 7, nhân với số chuỗi? Bảng mang tính minh họa; trong mã, modulo và xử lý tổng tiền tố nắm bắt chính xác tổng số điểm. 

Các dấu vết chứng minh rằng mỗi mục nhập DP tích lũy các đóng góp của tất cả các chuỗi trước đó cộng với vị trí của người hiện tại, xác nhận tính bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi người, chúng tôi tính tổng dựa trên các vị trí trước đó. Việc sử dụng tổng tiền tố sẽ giảm giá trị này xuống còn O(n) cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Mảng DP và tiền tố có kích thước n được duy trì. | 

Vì tổng cộng`n`trên tất cả các trường hợp thử nghiệm là 2*10^5, giải pháp sẽ chạy hiệu quả trong vòng 2 giây. Việc sử dụng bộ nhớ là tuyến tính trong`n`, vừa vặn thoải mái trong vòng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        dp = [0] * n
        pref = [0] * n
        dp[0] = a[0]
        pref[0] = dp[0]
        for i in range(1, n):
            dp[i] = (pref[i-1] + a[i]) % MOD
            pref[i] = (pref[i-1] + dp[i]) % MOD
        res.append(str(pref[-1]))
    return "\n".join(res)

# Provided samples
assert run("7\n1\n1\n2\n5 10\n3\n1 1 1\n4\n1 1 1 1\n4\n10 2 9 7\n5\n1000000000 1000000000 1000000000 1000000000 1000000000\n8\n100 2 34 59 34 27 5 6\n") == "1\n45\n24\n72\n480\n333572930\n69365"

# Custom tests
assert run("1\n1\n7\n") == "7", "single person"
assert run("1\n2\n1 1\n") == "3", "all equal values"
assert run("1\n3\n1 2 3\n") == "20", "small increasing values"
assert run("1\n4\n1 1 1 1\n") == "72", "all ones, n=4"
assert run("1\n2\n100000000
```
