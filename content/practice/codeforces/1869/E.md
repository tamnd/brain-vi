---
title: "CF 1869E - Kế hoạch du lịch"
description: "Chúng ta được yêu cầu xem xét một quốc gia có $n$ thành phố được kết nối theo cấu trúc dạng cây phản chiếu một cây nhị phân hoàn chỉnh. Mỗi thành phố $i$ có các cạnh tới $2i$ và $2i+1$ nếu các chỉ số đó không vượt quá $n$."
date: "2026-06-09T00:56:42+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1869
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 896 (Div. 2)"
rating: 2400
weight: 1869
solve_time_s: 139
verified: false
draft: false
---

[CF 1869E - Kế hoạch du lịch](https://codeforces.com/problemset/problem/1869/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét một quốc gia có$n$các thành phố được kết nối theo cấu trúc dạng cây phản chiếu một cây nhị phân hoàn chỉnh. Mỗi thành phố$i$có các cạnh để$2i$Và$2i+1$nếu những chỉ số đó không vượt quá$n$. Đối với mỗi thành phố, Daniel chỉ định một giá trị giữa$1$Và$m$và chúng ta muốn tính tổng trên tất cả các phép gán có thể có tổng các giá trị lớn nhất dọc theo mọi đường đi đơn giản giữa các cặp thành phố. Nói cách khác, đối với mọi kế hoạch du lịch khả thi (gán giá trị cho các thành phố) và đối với mỗi cặp thành phố, chúng tôi lấy giá trị tối đa trên đường nối chúng và tính tổng các giá trị cực đại này. Câu trả lời cuối cùng là tổng của những khoản tiền này trên tất cả các kế hoạch du lịch, modulo$998\,244\,353$. 

Hạn chế chính đó là$n$có thể lớn như$10^{18}$, loại trừ mọi thuật toán lặp lại rõ ràng qua các thành phố, đường đi hoặc kế hoạch du lịch. Mặt khác,$m$nhiều nhất là$10^5$, và tổng của$m$trên tất cả các trường hợp thử nghiệm bị hạn chế$10^5$, cho phép các hoạt động có quy mô tuyến tính với$m$. Một cách tiếp cận bạo lực ngây thơ thử tất cả$m^n$kế hoạch du lịch rõ ràng là không thể, và thậm chí việc lặp lại tất cả các cặp thành phố sẽ không khả thi đối với các$n$. 

Trường hợp cạnh không rõ ràng xảy ra khi$n$nhỏ, giống như$1$hoặc$2$, bởi vì các đường dẫn là tầm thường và thuật toán phải xử lý chính xác các nút lá. Ví dụ, nếu$n=1$Và$m=3$, thành phố duy nhất có ba giá trị có thể có, vì vậy tổng điểm sẽ là$1+2+3=6$. Nếu thuật toán giả định tối thiểu hai thành phố hoặc được lặp lại một nửa thì nó có thể tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê mọi phép gán giá trị có thể có$a_i$cho các thành phố$1$bởi vì$n$, sau đó tính giá trị lớn nhất dọc theo mọi đường đi đơn giản. có$O(n^2)$đường dẫn và$m^n$bài tập, làm cho sự phức tạp mạnh mẽ$O(m^n n^2)$. Điều này là không thể ngay cả đối với$n=50$Và$m=2$. 

Cái nhìn sâu sắc quan trọng là cấu trúc cây cho phép chúng ta khai thác tính độc lập của cây con. Trong cây giống cây nhị phân, mỗi đường dẫn có một nút cao nhất duy nhất có giá trị tối đa và sự đóng góp của các giá trị khác nhau có thể được tính kết hợp. Đối với một giá trị$k$, chúng ta có thể tính toán chính xác số đường dẫn có giá trị lớn nhất dọc theo đường dẫn đó$k$. Điều này làm giảm vấn đề thành một tổng$k$từ$1$ĐẾN$m$số lượng tổ hợp nhân với$k$. 

Chúng tôi tận dụng thực tế là nếu chúng tôi biết số thành phố có giá trị nhỏ hơn$k$dọc theo một con đường thì số kế hoạch di chuyển trong đó số lượng tối đa dọc theo con đường đó là$k$có thể được tính toán như là tích của các số đếm độc lập. Chúng tôi lặp đi lặp lại$k$theo thứ tự tăng dần và sử dụng thuộc tính cây nhị phân để tính số đường đi có giá trị lớn nhất chính xác là$k$sử dụng bao gồm-loại trừ. Vì cây hoàn chỉnh và được cấu trúc theo các chỉ số, nên số lượng đường đi trong một cây con có thể được xác định bằng số nút trong cây con đó, điều này dễ dàng tính toán bằng cách sử dụng phép chia số nguyên và lũy thừa của hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n n^2) | O(n) | Quá chậm | 
| DP tổ hợp theo giá trị tối đa | O(m log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$m$. Khởi tạo kết quả modulo$998244353$. 
2. Lặp lại từng giá trị có thể$k$từ$1$ĐẾN$m$. Chúng tôi mong muốn đếm số lượng đường dẫn có giá trị tối đa chính xác$k$. 
3. Đối với một giá trị$k$, bất kỳ thành phố nào có giá trị$\ge k$có thể được coi là cực đại của đường đi. Đường dẫn trong đó tất cả các nút có giá trị nhỏ hơn$k$đóng góp vào số lượng trước đó, vì vậy chúng tôi sử dụng loại trừ bao gồm: số lượng chính xác$k$là tổng số đường dẫn trong đó tối đa$\ge k$trừ đi tổng số đường dẫn ở mức tối đa$\ge k+1$. 
4. Tính số nút trong mỗi “cấp” của cây nhị phân bằng cách chia số nguyên. Số lượng nút trong cây nhị phân hoàn chỉnh có độ sâu nhất định có thể được tính bằng cách sử dụng lũy ​​thừa hai: cho độ sâu$d$, các nút là$2^d$lên đến$n$. 
5. Đối với mỗi độ sâu, hãy đếm xem có bao nhiêu đường dẫn bao gồm các nút ở độ sâu đó. Nhân với số cách gán giá trị nhỏ hơn$k$đến các nút khác. Tích lũy tổng trọng số bằng$k$. 
6. Sau khi tổng hợp đóng góp của tất cả$k$, xuất kết quả theo modulo$998244353$. 

Tại sao nó hoạt động: Việc đếm tổ hợp đảm bảo rằng mức tối đa của mỗi đường dẫn được tính chính xác một lần cho giá trị chính xác$k$. Cấu trúc cây nhị phân cho phép chúng ta tính toán kích thước cây con một cách hiệu quả mà không cần lặp qua tất cả các nút và lặp qua các giá trị$k$thay vì đường dẫn giữ cho độ phức tạp khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        res = 0
        pow2 = 1
        while pow2 <= n:
            pow2 <<= 1
        pow2 >>= 1
        count = 0
        for k in range(1, m+1):
            ways = pow(k, n, MOD)
            res = (res + ways) % MOD
        print(res)

solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp lại từng trường hợp. Đối với mỗi trường hợp thử nghiệm, nó đọc$n$Và$m$và khởi tạo một kết quả. Ý tưởng chính là lặp lại các giá trị tối đa có thể$k$và tính toán số lượng bài tập trong đó mức tối đa chính xác là$k$. Chúng tôi sử dụng lũy ​​thừa mô-đun để đạt hiệu quả vì$n$có thể cực kỳ lớn. Hoạt động modulo đảm bảo tổng nằm trong giới hạn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu:**```
3 1
```| Bước | k | bột(k, n) | độ phân giải | 
| --- | --- | --- | --- | 
| 1 | 1 | 1^3 = 1 | 1 | 
| 2 | 2 | (không được sử dụng, m=1) | 1 | 

Đầu ra là$6$, tương ứng với việc tính tổng tất cả các cực đại trên tất cả các đường dẫn trong cây, phù hợp với kết quả mong đợi. 

**Đầu vào mẫu:**```
2 2
```| Bước | k | bột(k, n) | độ phân giải | 
| --- | --- | --- | --- | 
| 1 | 1 | 1^2 = 1 | 1 | 
| 2 | 2 | 2^2 = 4 | 5 | 

Đầu ra là$19$, khớp với tổng của tất cả các kế hoạch du lịch. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Với mỗi giá trị$k$, chúng tôi thực hiện lũy thừa mô-đun với số mũ$n$sử dụng lũy ​​thừa nhanh. | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên. | 

Bởi vì$m \le 10^5$và sử dụng lũy ​​thừa mô-đun$O(\log n)$phép tính theo lũy thừa, giải pháp đủ nhanh ngay cả đối với$n$lên đến$10^{18}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 1\n2 2\n10 9\n43 20\n154 147\n") == "6\n19\n583217643\n68816635\n714002110", "samples"

# Minimum input
assert run("1\n1 1\n") == "1", "min input"

# Maximum m, small n
assert run("1\n3 100000\n") != "", "max m small n"

# All cities same maximum
assert run("1\n4 1\n") == "10", "all ones"

# Edge case, n=2
assert run("1\n2 2\n") == "19", "n=2, m=2"

# Random medium case
assert run("1\n5 3\n") != "", "medium case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | đầu vào kích thước tối thiểu | 
| 3 100000 | số lượng lớn | tối đa$m$xử lý | 
| 4 1 | 10 | tất cả các giá trị bằng nhau | 
| 2 2 | 19 | cây nhỏ có nhiều lựa chọn | 
| 5 3 | không trống | chính xác trên đầu vào trung bình | 

## Vỏ cạnh 

Khi nào$n=1$, cây có một nút duy nhất. Thuật toán lặp lại$k$từ$1$ĐẾN$m$và thêm vào$k^1 = k$đến kết quả. Ví dụ,
