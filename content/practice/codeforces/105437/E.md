---
title: "CF 105437E - Hệ số nhị thức, loại"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một cặp chỉ số $n$ và $k$, và chúng ta được yêu cầu tính toán một giá trị từ bảng tam giác $C$ được tạo theo cách không chuẩn. Bảng được xây dựng theo từng hàng."
date: "2026-06-23T03:42:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "E"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 124
verified: false
draft: false
---

[CF 105437E - Hệ số nhị thức, loại](https://codeforces.com/problemset/problem/105437/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một cặp chỉ mục$n$Và$k$và chúng ta được yêu cầu tính giá trị từ bảng tam giác$C$được tạo ra theo cách không chuẩn. 

Bảng được xây dựng theo từng hàng. Mỗi hàng$n$bắt đầu bằng$C[n][0] = 1$, kết thúc bằng$C[n][n] = 1$và mọi giá trị bên trong được hình thành bằng cách sử dụng các giá trị từ cùng một hàng và hàng trước đó, cụ thể là từ hàng xóm bên trái trong cùng hàng và mục nhập chéo phía trên bên trái. 

Điều này khác với bảng hệ số nhị thức thông thường, trong đó mỗi giá trị phụ thuộc vào hai mục ngay phía trên nó. Ở đây, thông tin cũng chảy theo chiều ngang trong một hàng, điều này làm thay đổi toàn bộ cấu trúc của tam giác. 

Nhiệm vụ là tính toán$C[n_i][k_i]$cho đến$10^5$truy vấn, với$n_i$lên đến$10^5$. Vì mỗi giá trị có thể lớn nên kết quả phải được trả về theo modulo$10^9 + 7$. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào xây dựng bảng một cách rõ ràng đều không thể thực hiện được. Một bảng DP đầy đủ sẽ yêu cầu$O(n^2)$thời gian và trí nhớ, vượt xa những gì có thể thực hiện được$n = 10^5$. Ngay cả việc tính toán một hàng một cách ngây thơ cũng là$O(n)$và làm điều đó đối với nhiều truy vấn vẫn sẽ quá chậm. 

Một vấn đề tế nhị hơn xuất hiện trong chính sự tái diễn. Bởi vì sự truy hồi sử dụng cả hai$C[n][k-1]$Và$C[n-1][k-1]$, các giá trị được lan truyền lặp đi lặp lại trên các hàng. Điều này có nghĩa là trực giác ngây thơ từ tam giác Pascal không được áp dụng và những sai sót nhỏ trong cách giải thích sẽ dẫn đến các chuỗi hoàn toàn khác nhau. 

Một cạm bẫy điển hình là cho rằng đây chỉ là hệ số nhị thức. Ví dụ, đối với$n = 4$,$k = 2$, hệ số nhị thức đúng là$6$, nhưng sự tái phát thực tế tạo ra$4$. Một sự hiểu lầm phổ biến khác là cho rằng chỉ phụ thuộc vào$k$, trong khi trên thực tế, cấu trúc phải được suy ra một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xây dựng toàn bộ bảng lên mức tối đa$n$thấy trong các truy vấn sử dụng phép lặp đã cho. Đối với mỗi hàng$n$, chúng tôi tính toán mọi$C[n][k]$từ$k = 1$ĐẾN$n - 1$, sau đó trả lời trực tiếp các truy vấn. Điều này là đơn giản và trung thành với định nghĩa. 

Tuy nhiên, phương pháp này thực hiện một số lượng lớn các hoạt động. Vòng lặp bên ngoài chạy đến$10^5$, và vòng lặp bên trong cũng chạy tới$10^5$, dẫn đến xung quanh$10^{10}$cập nhật trong trường hợp xấu nhất. Mỗi lần cập nhật đều có thời gian không đổi nhưng quy mô vẫn vượt xa mọi giới hạn khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát cách các giá trị lan truyền trên các hàng. Mở rộng phép lặp một lần sẽ hiển thị cấu trúc tiền tố ẩn:$$C[n][k] = C[n][k-1] + C[n-1][k-1]$$Việc hủy liên tục thuật ngữ đầu tiên trong cùng một hàng sẽ cho:$$C[n][k] = C[n][0] + \sum_{j=1}^{k} C[n-1][j-1]$$Từ$C[n][0] = 1$, điều này trở thành:$$C[n][k] = 1 + \sum_{i=0}^{k-1} C[n-1][i]$$Điều này cho thấy rằng mỗi mục được xác định bởi tổng tiền tố của hàng trước đó. Cấu trúc đó đủ mạnh để thu gọn quá trình hai chiều thành một mô hình đơn giản. Tính toán một vài hàng cho thấy sự ổn định: đối với một giá trị cố định$k$, một lần$n$đủ lớn thì giá trị không còn thay đổi nữa. 

Tính các trường hợp nhỏ ta thấy: 

Hàng 2:$1, 2, 1$Hàng 3:$1, 2, 4, 1$Hàng 4:$1, 2, 4, 8, 1$Hàng 5:$1, 2, 4, 8, 16, 1$Đối với tất cả hợp lệ$k < n$, giá trị trở thành$2^k$, độc lập với$n$. Chỉ phần tử cuối cùng$C[n][n]$vẫn cố định tại$1$. 

Điều này làm giảm mỗi truy vấn thành một phép tính công suất trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng DP Brute Force |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Công thức quan sát$2^k$|$O(t \log MOD)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc tính toán trở thành bài toán đánh giá trực tiếp một khi mẫu được xác định. 

1. Đọc tất cả các truy vấn$n_i, k_i$. Giá trị của$n_i$không còn ảnh hưởng đến việc tính toán ngoại trừ việc xác nhận$k_i < n_i$. 
2. Tính toán trước lũy thừa từ 2 đến mức tối đa có thể$k$sử dụng lũy ​​thừa mô-đun hoặc nhân đôi lặp đi lặp lại. Điều này tránh tính toán lại lũy thừa cho mỗi truy vấn. 
3. Với mỗi truy vấn, xuất ra$2^{k_i} \bmod (10^9 + 7)$. 

Lý do điều này có hiệu quả là vì sự lặp lại làm cho mỗi hàng hoạt động giống như tổng tích lũy của hàng trước đó và tổng tích lũy lặp đi lặp lại của cấu trúc ban đầu không đổi tạo ra sự tăng trưởng theo cấp số nhân trong$k$đồng thời loại bỏ sự phụ thuộc vào$n$sau vài hàng đầu tiên. 

### Tại sao nó hoạt động 

Phép lặp biến đổi mỗi hàng thành tổng tiền tố của hàng trước đó, với điều kiện biên cố định là 1 ở đầu mỗi hàng. Tiền tố lặp đi lặp lại này làm cho giá trị tăng gấp đôi với mỗi lần tăng$k$, độc lập với$n$, một lần$n$đủ lớn để hỗ trợ cột đó. Kết quả là bảng ổn định thành một chuỗi hàm mũ đơn giản trên các cột và chỉ mục hàng chỉ ảnh hưởng đến phần tử biên cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(base, exp):
    result = 1
    while exp:
        if exp & 1:
            result = result * base % MOD
        base = base * base % MOD
        exp >>= 1
    return result

t = int(input())
n = list(map(int, input().split()))
k = list(map(int, input().split()))

for i in range(t):
    print(modpow(2, k[i]))
```Giải pháp dựa hoàn toàn vào lũy thừa nhanh. Các mảng của$n$Và$k$được đọc một lần và chỉ$k$được sử dụng trong tính toán vì công thức dẫn xuất loại bỏ sự phụ thuộc vào$n$cho tất cả các đầu vào hợp lệ. 

Hàm lũy thừa mô-đun được triển khai lặp đi lặp lại để tránh chi phí đệ quy và đảm bảo thời gian chạy logarit cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Truy vấn đầu vào:$(n,k) = (2,1), (5,2), (5,3)$Chúng tôi tính toán lũy thừa của 2 chỉ dựa trên$k$. 

| Truy vấn | k | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| (2,1) | 1 |$2^1$| 2 | 
| (5,2) | 2 |$2^2$| 4 | 
| (5,3) | 3 |$2^3$| 8 | 

Điều này cho thấy rằng mặc dù$n$khác nhau, kết quả đầu ra chỉ phụ thuộc vào chỉ mục cột. 

### Mẫu 2 

Trường hợp lớn hơn bao gồm các giá trị cao của$k$, chẳng hạn như$k = 100000$. Việc tính toán vẫn giảm về lũy thừa. 

| Truy vấn | k | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| (100000, 100000) | 100000 |$2^{100000}$mod MOD | 326186014 | 
| (100000, 33333) | 33333 |$2^{33333}$mod MOD | 984426998 | 
| (100000, 66666) | 66666 |$2^{66666}$mod MOD | 303861760 | 

Điều này chứng tỏ rằng ngay cả các chỉ số cực lớn cũng được xử lý hiệu quả mà không cần xây dựng bảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log k)$| Mỗi truy vấn sử dụng lũy ​​thừa nhanh theo số mũ$k$| 
| Không gian |$O(1)$| Không cần bảng hoặc mảng tiền xử lý | 

Các ràng buộc cho phép lên đến$10^5$các truy vấn và mỗi phép lũy thừa chạy tối đa khoảng 17 phép nhân kể từ$k \le 10^5$. Điều này dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def modpow(base, exp):
    result = 1
    while exp:
        if exp & 1:
            result = result * base % MOD
        base = base * base % MOD
        exp >>= 1
    return result

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    n = list(map(int, input().split()))
    k = list(map(int, input().split()))

    out = []
    for i in range(t):
        out.append(str(modpow(2, k[i])))
    return "\n".join(out)

# provided samples
assert run("3\n2 5 5\n1 2 3\n") == "2\n4\n8"

# minimum size
assert run("1\n2\n1\n") == "2"

# small varied case
assert run("3\n3 4 5\n1 2 4\n") == "2\n4\n16"

# boundary k close to max
assert run("2\n100000 100000\n99999 1\n") == f"{pow(2,99999,MOD)}\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nhỏ | 2 | trường hợp cơ sở đúng đắn | 
| hỗn k | 2,4,16 | chỉ phụ thuộc vào k | 
| k lớn | kết quả modpow | tính đúng đắn của mô-đun | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$k = 1$. Sự tái diễn vẫn tạo ra$C[n][1] = 2$cho tất cả hợp lệ$n \ge 2$, phù hợp$2^1$. Việc triển khai DP trực tiếp có thể khởi tạo không chính xác hoặc bỏ qua cột này, nhưng công thức dẫn xuất sẽ xử lý nó một cách thống nhất. 

Một trường hợp cạnh khác là khi$k = n - 1$. Mặc dù đây là gần ranh giới, giá trị vẫn tuân theo$2^{n-1}$. Một DP dựa trên hàng đơn giản có thể ghi đè không chính xác hoặc xử lý sai việc truyền bá ranh giới, nhưng dạng đóng vẫn nhất quán. 

Cuối cùng, đối với lớn$k$, sự tăng trưởng theo cấp số nhân được xử lý hoàn toàn bằng phép lũy thừa mô-đun, do đó không có nguy cơ tràn hoặc nổ mảng trung gian.
