---
title: "CF 1657E - Sao MST"
description: "Chúng ta có một đồ thị đầy đủ trên n đỉnh được dán nhãn. Mỗi cạnh nhận được trọng số nguyên từ 1 đến k. Đỉnh 1 đóng vai trò đặc biệt. Xét ngôi sao có tâm ở đỉnh 1, bao gồm tất cả các cạnh (1, v) cho v 1."
date: "2026-06-10T03:29:55+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "graph-matchings", "math"]
categories: ["algorithms"]
codeforces_contest: 1657
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 125 (Rated for Div. 2)"
rating: 2200
weight: 1657
solve_time_s: 124
verified: true
draft: false
---

[CF 1657E - Sao MST](https://codeforces.com/problemset/problem/1657/E) 

**Xếp hạng:** 2200 
**Tags:** tổ hợp, dp, so khớp đồ thị, toán 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ hoàn chỉnh về`n`các đỉnh được dán nhãn. Mỗi cạnh nhận được một trọng số nguyên giữa`1`Và`k`. 

đỉnh`1`đóng một vai trò đặc biệt. Xét ngôi sao có tâm ở đỉnh`1`, bao gồm tất cả các cạnh`(1, v)`vì`v > 1`. Tổng trọng lượng của ngôi sao này chỉ đơn giản là tổng của những trọng lượng đó`n - 1`trọng lượng cạnh. 

Một đồ thị được gọi là đẹp khi trọng số của cây khung nhỏ nhất của nó đúng bằng trọng số của ngôi sao đó. 

Nhiệm vụ là đếm xem có bao nhiêu đồ thị có trọng số hoàn chỉnh thỏa mãn điều kiện này, modulo`998244353`. 

Các ràng buộc là dấu hiệu đầu tiên cho thấy lời giải phải mang tính tổ hợp cao. Một đồ thị hoàn chỉnh có 250 đỉnh chứa hơn 31.000 cạnh. Ngay cả việc viết ra tất cả các bài tập biên là không thể. Số lượng đồ thị có trọng số có thể là$$k^{\binom n2},$$lớn về mặt thiên văn. 

Vì cả hai`n`Và`k`nhiều nhất là`250`, một giải pháp lập trình động với khoảng`O(n^2 k)`hoặc`O(n^3)`trạng thái là thực tế. Bất cứ điều gì cố gắng suy luận trực tiếp về các biểu đồ riêng lẻ hoặc tính toán MST đều hoàn toàn nằm ngoài tầm với. 

Phần khó khăn là hiểu điều kiện MST thực sự có ý nghĩa gì. 

Xét ba đỉnh`1, u, v`. Giả định$$w(u,v) < \max(w(1,u), w(1,v)).$$Rồi ở ngôi sao, hai cạnh càng nặng`(1,u)`Và`(1,v)`có thể được thay thế bằng`(u,v)`, tạo ra một cây bao trùm có trọng số nhỏ hơn rất nhiều. Điều đó sẽ làm cho ngôi sao không tối ưu. 

Ví dụ, nếu$$w(1,u)=5,\quad w(1,v)=3,\quad w(u,v)=2,$$ngôi sao có sự đóng góp`5 + 3`từ hai đỉnh này, nhưng thay thế cạnh`(1,u)`qua`(u,v)`giảm tổng trọng lượng xuống`3`. 

Cách tiếp cận bất cẩn chỉ có thể kiểm tra xem bản thân ngôi sao có phải là MST hay không. Điều kiện thực tế mạnh hơn: trọng lượng MST phải bằng trọng lượng sao. Vì ngôi sao đã là cây bao trùm nên đẳng thức có nghĩa là ngôi sao phải là cây bao trùm tối thiểu. 

Một ví dụ nhỏ hữu ích là`n = 3`,`k = 2`. 

Cho phép$$a=w(1,2),\quad b=w(1,3),\quad c=w(2,3).$$Ngôi sao là một MST chính xác khi$$c \ge \max(a,b).$$Các bài tập hợp lệ là: 

| một | b | c hợp lệ | 
| --- | --- | --- | 
| 1 | 1 | 1,2 | 
| 1 | 2 | 2 | 
| 2 | 1 | 2 | 
| 2 | 2 | 2 | 

Tổng cộng = 5, khớp với đầu ra mẫu. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Gán trọng số cho mọi cạnh, chạy thuật toán MST, tính trọng số sao và so sánh hai giá trị. 

Vấn đề là số lượng nhiệm vụ. Một biểu đồ hoàn chỉnh chứa$$\binom n2$$các cạnh, vì vậy số lượng đồ thị có trọng số là$$k^{\binom n2}.$$Vì`n = 250`, điều này là hoàn toàn không thể. 

Quan sát quan trọng là điều kiện MST có thể được viết lại dưới dạng ràng buộc cục bộ trên mọi cạnh không phải sao. 

Cho phép$$a_i = w(1,i).$$Đây là trọng số của cạnh ngôi sao. 

Ngôi sao là MST khi và chỉ khi mọi cạnh giữa các đỉnh không phải gốc đều thỏa mãn$$w(i,j) \ge \max(a_i,a_j).$$Tại sao? 

Nếu cạnh nào đó nhỏ hơn mức tối đa đó, chúng ta có thể thay thế cạnh sao nặng hơn và thu được cây bao trùm nhẹ hơn. 

Ngược lại, nếu mọi cạnh không phải sao ít nhất cũng đạt mức tối đa đó thì với mọi cạnh`(i,j)`con đường duy nhất trong ngôi sao chứa trọng lượng`a_i`Và`a_j`, và trọng số lớn nhất trên đường đi đó chính xác là`max(a_i,a_j)`. Cạnh`(i,j)`không bao giờ rẻ hơn cạnh lớn nhất trên đường đi, đó chính xác là đặc tính MST được đưa ra bởi thuộc tính chu trình. Ngôi sao sau đó là một MST. 

Bây giờ vấn đề trở thành phép gán các giá trị`a_i`. 

Khi trọng số của ngôi sao đã được cố định, mọi cạnh`(i,j)`có$$k-\max(a_i,a_j)+1$$những lựa chọn hợp lệ. 

Như vậy câu trả lời bằng$$\sum_{a_2,\dots,a_n} \prod_{i<j} \bigl(k-\max(a_i,a_j)+1\bigr).$$Việc đánh giá trực tiếp vẫn không thể thực hiện được vì có`k^(n-1)`nhiệm vụ ngôi sao. 

Cái nhìn sâu sắc còn lại là chỉ có số lượng đỉnh có trọng lượng của mỗi ngôi sao mới quan trọng. 

Chúng tôi xử lý trọng lượng từ`1`ĐẾN`k`và xây dựng tập các đỉnh có trọng số cạnh sao đã được xác định. Điều này dẫn đến DP về mức trọng lượng và số lượng đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^{\binom n2})$| Rất lớn | Quá chậm | 
| DP tối ưu |$O(k n^2)$|$O(kn)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`N = n - 1`. Chúng ta chỉ cần gán trọng số sao cho các đỉnh`2...n`. 

Định nghĩa:$$dp[w][t]$$là số cách sau khi xử lý giá trị trọng số`1...w`, chính xác là ở đâu`t`nhiều nhất các đỉnh đã nhận được trọng số sao`w`. 

Ban đầu,$$dp[0][0]=1.$$Giả sử chúng ta đang xử lý trọng lượng`w`. 

Cho rằng`t`các đỉnh đã được gán trọng số nhỏ hơn. 

Chọn`x`các đỉnh mới có trọng lượng sao trở nên chính xác`w`. 

Sau đó, số đếm mới trở thành$$t+x.$$### Đếm các đóng góp của cạnh 

Mỗi đỉnh mới được thêm vào đều có trọng số sao chính xác`w`. 

Xét các cặp có trọng lượng sao tối đa bằng`w`. 

Có hai loại: 

1. Một điểm cuối giữa cái cũ`t`đỉnh và một trong số`x`đỉnh mới. 

Đếm:$$t \cdot x.$$1. Cả hai điểm cuối trong số`x`đỉnh mới. 

Đếm:$$\frac{x(x-1)}2.$$Tổng cộng:$$E=tx+\frac{x(x-1)}2.$$Với mỗi cặp như vậy, cạnh tương ứng`(i,j)`có thể nhận bất kỳ giá trị nào trong$$[w,k].$$Do đó mỗi cặp đóng góp$$k-w+1$$sự lựa chọn. 

Tổng số tiền đóng góp là$$(k-w+1)^E.$$### Đếm đỉnh nào nhận được trọng số`w`Trong số còn lại`N-t`đỉnh, chọn`x`trong số họ:$$\binom{N-t}{x}.$$Như vậy quá trình chuyển đổi là$$dp[w][t+x] += dp[w-1][t] \cdot \binom{N-t}{x} \cdot (k-w+1)^E.$$Sau khi xử lý tất cả các giá trị trọng số, mỗi đỉnh phải được gán:$$\text{answer}=dp[k][N].$$### Tại sao nó hoạt động 

DP xây dựng nhiều tập trọng số cạnh sao theo cấp độ. 

Khi cân nặng`w`được xử lý, tất cả các cặp đỉnh có trọng số điểm cuối lớn hơn chính xác là`w`trở nên quyết tâm hoàn toàn. Mỗi cặp như vậy đóng góp độc lập một cách chính xác`k-w+1`các lựa chọn cho cạnh không phải sao của nó. 

Mỗi cặp được tính một lần, cụ thể là khi trọng số điểm cuối lớn hơn của nó xuất hiện lần đầu tiên. Số mũ$$tx+\frac{x(x-1)}2$$chính xác là số cặp có trọng lượng sao tối đa bằng`w`. 

Nhân tất cả những đóng góp độc lập này sẽ tính mỗi biểu đồ đẹp chính xác một lần và mọi biểu đồ được tính đều thỏa mãn đặc tính MST ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, k = map(int, input().split())

    N = n - 1

    C = [[0] * (N + 1) for _ in range(N + 1)]
    C[0][0] = 1
    for i in range(1, N + 1):
        C[i][0] = C[i][i] = 1
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    max_pairs = N * (N - 1) // 2

    pw = [[1] * (max_pairs + 1) for _ in range(k + 1)]
    for w in range(1, k + 1):
        base = k - w + 1
        for e in range(1, max_pairs + 1):
            pw[w][e] = pw[w][e - 1] * base % MOD

    dp = [[0] * (N + 1) for _ in range(k + 1)]
    dp[0][0] = 1

    for w in range(1, k + 1):
        for t in range(N + 1):
            cur = dp[w - 1][t]
            if cur == 0:
                continue

            rem = N - t

            for x in range(rem + 1):
                nt = t + x

                pairs = t * x + x * (x - 1) // 2

                ways = cur
                ways = ways * C[rem][x] % MOD
                ways = ways * pw[w][pairs] % MOD

                dp[w][nt] = (dp[w][nt] + ways) % MOD

    print(dp[k][N])

if __name__ == "__main__":
    main()
```Bảng kết hợp đếm các đỉnh nhận được trọng số sao cụ thể. Từ`N ≤ 249`, một tam giác Pascal đơn giản là đủ. 

Số mũ chuyển tiếp$$t x + \frac{x(x-1)}2$$là trái tim của giải pháp. Nó đếm chính xác các cặp đỉnh có trọng số sao được chỉ định tối đa sẽ trở thành trọng số hiện tại. 

Các giá trị lũy thừa được tính toán trước vì các cơ số giống nhau xuất hiện nhiều lần. Nếu không có quá trình xử lý trước này, việc lũy thừa mô-đun lặp đi lặp lại bên trong DP sẽ tốn kém một cách không cần thiết. 

Nhà nước sử dụng`N = n - 1`bởi vì chỉ các đỉnh khác với gốc mới cần gán trọng số sao. Mức giảm nhỏ này giữ cho công thức sạch hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
```Đây`N = 2`. 

| Trọng lượng w | t trước | x đã chọn | nt | cặp | yếu tố | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 1 | 
| 1 | 0 | 1 | 1 | 0 | 2 | 
| 1 | 0 | 2 | 2 | 1 | 2 | 
| 2 | tiếp tục DP | | | | | 

Sau khi tất cả quá trình chuyển đổi kết thúc: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| dp[2][2] | 5 | 

Trả lời:```
5
```Ví dụ này chứng tỏ rằng DP đang đếm đồng thời các phép gán trọng số sao và các phép gán cạnh không phải sao hợp lệ. 

### Ví dụ 2 

đầu vào:```
2 3
```Chỉ có một cạnh ngôi sao. 

| Trọng lượng w | t trước | x | nt | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 
| 3 | 0 | 1 | 1 | 

Không có cạnh nào không phải là sao tồn tại, do đó mọi lựa chọn về trọng số của cạnh đơn đều hợp lệ. 

| Trạng thái cuối cùng | Giá trị | 
| --- | --- | 
| dp[3][1] | 3 | 

Trả lời:```
3
```Điều này xác nhận rằng công thức xử lý chính xác biểu đồ nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(kn^2)$| Đối với mỗi trọng số, lặp lại số lượng được chỉ định hiện tại và số lượng mới được thêm vào | 
| Không gian |$O(kn)$| Bảng DP cộng với tính toán trước tổ hợp | 

Với`n, k ≤ 250`, số lần chuyển tiếp DP là khoảng$$250 \times 250 \times 250 \approx 1.6\times10^7,$$vừa vặn thoải mái trong giới hạn 6 giây trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    MOD = 998244353

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    N = n - 1

    C = [[0] * (N + 1) for _ in range(N + 1)]
    C[0][0] = 1
    for i in range(1, N + 1):
        C[i][0] = C[i][i] = 1
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    max_pairs = N * (N - 1) // 2

    pw = [[1] * (max_pairs + 1) for _ in range(k + 1)]
    for w in range(1, k + 1):
        base = k - w + 1
        for e in range(1, max_pairs + 1):
            pw[w][e] = pw[w][e - 1] * base % MOD

    dp = [[0] * (N + 1) for _ in range(k + 1)]
    dp[0][0] = 1

    for w in range(1, k + 1):
        for t in range(N + 1):
            cur = dp[w - 1][t]
            if cur == 0:
                continue

            rem = N - t
            for x in range(rem + 1):
                pairs = t * x + x * (x - 1) // 2
                dp[w][t + x] = (
                    dp[w][t + x]
                    + cur * C[rem][x] * pw[w][pairs]
                ) % MOD

    return str(dp[k][N])

# provided sample
assert run("3 2\n") == "5"

# minimum graph
assert run("2 1\n") == "1"

# one edge, three possible weights
assert run("2 3\n") == "3"

# official sample
assert run("4 4\n") == "571"

# larger official sample
assert run("6 9\n") == "310640163"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`|`1`| Đồ thị nhỏ nhất có thể | 
|`2 3`|`3`| Không tồn tại cạnh không phải sao | 
|`3 2`|`5`| Đếm ràng buộc MST cơ bản | 
|`4 4`|`571`| Mẫu chính thức | 
|`6 9`|`310640163`| Mẫu chính thức lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
```Chỉ có một cạnh trong đồ thị. Ngôi sao là toàn bộ đồ thị và cũng là cây bao trùm duy nhất. DP có`N = 1`, gán trọng số cho đỉnh duy nhất`1`, và trả về`1`. 

Coi như:```
2 5
```Một lần nữa không có cạnh không phải sao. Bất kỳ trọng lượng nào từ`1`ĐẾN`5`hoạt động. Quá trình chuyển đổi chỉ cần chọn một trong năm cấp độ cho đỉnh đơn độc, tạo ra câu trả lời`5`. 

Coi như:```
3 2
```Cấu hình nguy hiểm là$$w(1,2)=2,\quad w(1,3)=1,\quad w(2,3)=1.$$Một phương pháp đếm đơn giản có thể chấp nhận nó vì tất cả các trọng số đều nằm trong phạm vi. Đặc tính của chúng tôi bác bỏ nó bởi vì$$1 < \max(2,1).$$DP không bao giờ tính biểu đồ này, vì các cạnh có trọng lượng sao tối đa bằng`2`chỉ được phép giá trị từ`[2,2]`. 

Coi như:```
3 1
```Mỗi trọng lượng cạnh buộc phải`1`. Ngôi sao có trọng lượng`2`, mọi cây bao trùm đều có trọng số`2`, và câu trả lời là chính xác`1`. DP xử lý việc này một cách tự nhiên vì mọi hệ số nhân đều trở thành`1`.
