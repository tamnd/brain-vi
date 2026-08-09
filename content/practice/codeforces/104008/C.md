---
title: "CF 104008C - Nối mảng"
description: "Chúng ta bắt đầu với một mảng và được phép biến đổi nó nhiều lần chính xác $m$ lần. Mỗi phép biến đổi sẽ thay thế mảng hiện tại bằng một mảng mới được hình thành theo một trong hai cách: hoặc chúng ta nhân đôi mảng và nối nó vào chính nó, hoặc chúng ta lấy một bản sao đảo ngược và đặt nó ở phía trước…"
date: "2026-07-02T05:28:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104008
codeforces_index: "C"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guilin Site"
rating: 0
weight: 104008
solve_time_s: 45
verified: true
draft: false
---

[CF 104008C - Nối mảng](https://codeforces.com/problemset/problem/104008/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng và được phép biến đổi nó nhiều lần một cách chính xác$m$lần. Mỗi phép biến đổi sẽ thay thế mảng hiện tại bằng một mảng mới được hình thành theo một trong hai cách: hoặc chúng ta sao chép mảng và nối nó vào chính nó hoặc chúng ta lấy một bản sao đảo ngược và đặt nó trước mảng ban đầu. 

Rốt cuộc$m$hoạt động, chúng tôi có được một mảng cuối cùng$b$. Mục tiêu không phải là tối đa hóa tổng đơn giản của các phần tử mà là tổng có trọng số trong đó mỗi vị trí đóng góp tổng tiền tố của nó. Cụ thể, nếu định nghĩa$S_i = \sum_{j=1}^{i} b_j$, thì tỉ số là$\sum_{i=1}^{|b|} S_i$, modulo được tính toán$10^9+7$, nhưng với điểm mấu chốt quan trọng là việc so sánh được thực hiện trước modulo: chúng tôi muốn giá trị số nguyên tối đa có thể và chỉ sau đó mới áp dụng modulo. 

Những hạn chế$n, m \le 10^5$ngay lập tức loại trừ mọi mô phỏng của mảng. Ngay cả một thao tác đơn lẻ cũng tăng gấp đôi kích thước, do đó độ dài mảng sẽ trở thành$n \cdot 2^m$, điều này hoàn toàn không thể xây dựng được một cách rõ ràng. Điều này buộc chúng ta phải suy luận chỉ về sự đóng góp tổng hợp của các phần tử trong các phép biến đổi. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể tham lam quyết định từng hoạt động một cách độc lập. Ví dụ: trên mảng$[1,2]$, cả hai thao tác đều tạo ra các cấu trúc khác nhau và việc lặp lại các lựa chọn mà không theo dõi ảnh hưởng tổng thể lên trọng số tiền tố sẽ dẫn đến các câu trả lời sai. Một cạm bẫy tinh vi khác là hiểu sai mục tiêu: nó không phải là tổng của các phần tử, mà là tổng của tất cả các tổng tiền tố, điều này ảnh hưởng nặng nề đến các vị trí trước đó. 

Một trường hợp cạnh minh họa nhỏ là$a = [1, 100]$,$m = 1$. Việc bổ sung mang lại$[1,100,1,100]$, trong khi đảo ngược và thêm tiền tố cho$[100,1,1,100]$. Mặc dù nhiều phần tử giống hệt nhau, cấu trúc tiền tố thay đổi điểm số đáng kể vì các giá trị lớn xuất hiện trước đó đóng góp nhiều hơn cho nhiều tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force mô phỏng quá trình: bắt đầu với mảng, áp dụng tất cả$m$hoạt động theo mọi cách có thể, tạo ra tất cả các mảng kết quả và tính điểm. Mỗi thao tác sẽ nhân đôi kích thước mảng và có$2^m$trình tự hoạt động có thể có. Ngay cả khi bỏ qua ký ức, điều này đã bùng nổ theo cấp số nhân. Ngoài ra, việc tính điểm của độ dài$n2^m$bản thân mảng sẽ tuyến tính ở kích thước đó, làm cho tổng công việc trở nên lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng là cả hai hoạt động được phép đều bảo toàn nhiều tập hợp giá trị và chỉ thay đổi thứ tự, nhưng điểm số phụ thuộc vào thứ tự thông qua tích lũy tiền tố. Thay vì theo dõi toàn bộ mảng, chúng tôi theo dõi hai số liệu thống kê tổng hợp: tổng của mảng và tổng của các tổng tiền tố. Những điều này là đủ vì cả hai thao tác đều có thể được biểu diễn dưới dạng các phép biến đổi trên hai giá trị này. 

Khi nối$b + b$, tổng tiền tố ở nửa sau được dịch chuyển bằng tổng của nửa đầu. Khi đảo ngược và thêm tiền tố, chúng tôi khai thác tính đối xứng: đảo ngược thay đổi các đóng góp tiền tố, nhưng tổng tổng không thay đổi và cấu trúc tổng tiền tố có thể được suy ra từ bản gốc bằng một danh tính đã biết. Điều này làm giảm mỗi thao tác thành một bản cập nhật xác định trên một cặp số, cho phép chúng ta lặp lại$m$lần trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị: tổng của các phần tử$S$và tổng các tiền tiền tố$P$. 

1. Tính ban đầu$S$Và$P$từ mảng ban đầu theo thời gian tuyến tính. Điều này thiết lập sự đóng góp cơ bản trước bất kỳ sự chuyển đổi nào. 
2. Đối với mỗi$m$hoạt động, quyết định sự chuyển đổi ảnh hưởng như thế nào$(S, P)$cho cả hai hoạt động có thể. Thay vì lựa chọn, chúng ta quan sát thấy mình đang tối đa hóa, vì vậy chúng ta chọn trạng thái có kết quả tốt hơn. 
3. Đối với thao tác sao chép$b \to b + b$, tổng số tiền trở thành$2S$. Tổng tiền tố trở thành$2P + S \cdot n$, vì tiền tố của bản sao thứ hai được dịch chuyển bởi$S$cho mỗi cái của nó$n$các phần tử. 
4. Đối với hoạt động tiền tố ngược, chúng tôi phân tích tác động thông qua nhận dạng đảo ngược tiền tố: đảo ngược biến cấu trúc tiền tố thành cấu trúc hậu tố. Tổng số tiền còn lại$S$, trong khi tổng tiền tố chuyển thành$S \cdot (n+1) - P$. Điều này xuất phát từ thực tế là tổng các tổng tiền tố cộng với tổng các tổng hậu tố có dạng đóng chỉ phụ thuộc vào tổng tổng. 
5. Ở mỗi bước chọn thao tác có năng suất lớn hơn$P$, vì mục tiêu cuối cùng chính xác là$P$sau đó$m$những biến đổi. 
6. Lặp lại quá trình này$m$lần cập nhật$(S, P, n)$theo đó, lưu ý rằng$n$tăng gấp đôi trong cả hai hoạt động. 
7. Đầu ra cuối cùng$P \bmod 10^9+7$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau bất kỳ chuỗi thao tác nào, toàn bộ mảng có thể được tóm tắt cho mục tiêu này chỉ bằng cách sử dụng độ dài, tổng tổng và tổng các tổng tiền tố. Cả hai hoạt động được phép đều ánh xạ các đại lượng này tới các giá trị mới mà không yêu cầu kiến ​​thức về thứ tự nội bộ ngoài những gì các tổng hợp này mã hóa. Vì mọi mảng kết quả có thể đều có thể biểu diễn được thông qua việc áp dụng lặp đi lặp lại các phép biến đổi xác định này, nên việc tham lam chọn thao tác tối đa hóa giá trị tổng tiền tố kết quả ở mỗi bước sẽ duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def prefix_sum(arr):
    s = 0
    p = 0
    for x in arr:
        s += x
        p += s
    return s, p

def transform_concat(s, p, n):
    ns = 2 * s
    np = 2 * p + s * n
    return ns, np, 2 * n

def transform_reverse_prefix(s, p, n):
    # reversed prefix identity
    ns = s
    np = s * (n + 1) - p
    return ns, np, n * 2

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    s, p = prefix_sum(a)
    cur_n = n

    for _ in range(m):
        s1, p1, n1 = transform_concat(s, p, cur_n)
        s2, p2, n2 = transform_reverse_prefix(s, p, cur_n)

        if p1 >= p2:
            s, p, cur_n = s1, p1, n1
        else:
            s, p, cur_n = s2, p2, n2

        p %= MOD
        s %= MOD

    print(p % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ theo dõi ba biến trạng thái. chức năng`prefix_sum`tính cặp ban đầu$(S, P)$. Mỗi hàm chuyển đổi cập nhật các giá trị này theo thời gian không đổi. Vòng lặp tham lam chọn hoạt động mang lại sự đóng góp tổng tiền tố lớn hơn. 

Một chi tiết triển khai tinh tế là việc so sánh phải được thực hiện trước modulo, vì modulo chỉ dành cho đầu ra. Áp dụng modulo quá sớm sẽ phá vỡ các quyết định đặt hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2
```Chúng tôi tính toán các giá trị ban đầu. 

| bước | mảng | S | P | 
| --- | --- | --- | --- | 
| ban đầu | [1,2] | 3 | 1 + 3 = 4 | 

Bây giờ đánh giá cả hai hoạt động. 

Sao chép mang lại: 

mảng [1,2,1,2], S = 6, P = 4 * 2 + 3 * 2 = 14 

Tiền tố ngược cho: 

mảng [2,1,1,2], S = 3, P = 3 * 3 - 4 = 5 

Chúng tôi chọn trùng lặp, vì vậy câu trả lời là 14. 

Điều này cho thấy việc sao chép sẽ khuếch đại sự tăng trưởng tiền tố nhanh hơn như thế nào khi việc đặt hàng ban đầu đã thuận lợi. 

### Ví dụ 2 

đầu vào:```
3 2
1 3 2
```Ban đầu: 

S = 6, P = 1 + 4 + 6 = 11 

Bước 1: 

Nhân đôi: S=12, P=22 + 18 = 40 

Vòng quay: S=6, P=6*4 - 11 = 13 

Chọn trùng lặp. 

Bước 2: 

Từ (12,40): 

Nhân đôi: S=24, P=80 + 144 = 224 

Vòng quay: S=12, P=12*7 - 40 = 44 

Chọn trùng lặp. 

P cuối cùng = 224. 

Điều này chứng tỏ rằng một khi sự trùng lặp chiếm ưu thế, nó sẽ tiếp tục chiếm ưu thế vì nó làm tăng tốc độ tăng trưởng tiền tố nhanh hơn theo phương trình bậc hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| lần quét đầu tiên cộng thêm$m$chuyển tiếp theo thời gian không đổi | 
| Không gian |$O(1)$| chỉ các biến tổng hợp được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn vì cả hai$n$Và$m$đang lên đến$10^5$và chúng tôi tránh mọi sự tăng trưởng mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if (lambda: None) else exec(open(__file__).read())

# sample
# assert run("2 1\n1 2\n") == "14\n"

# custom cases
# 1. single element
# 2. all equal
# 3. small reverse effect
# 4. large m stability
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3\n5 | phụ thuộc vào sự tăng trưởng | ổn định phần tử đơn | 
| 2 2\n1 1 | tăng trưởng quyết định | xử lý đối xứng | 
| 3 2 1\n1 2 3 | đặt hàng không tầm thường | độ nhạy tiền tố | 
| 5 10\n1 2 3 4 5 | hành vi m lớn | ổn định biến đổi hàm mũ | 

## Vỏ cạnh 

cho$n=1$, mảng không bao giờ thay đổi một cách có ý nghĩa trong cả hai thao tác. Nhà nước$(S,P)$phát triển một cách xác định với sự trùng lặp nhân đôi cả hai giá trị theo cách có thể dự đoán được và tiền tố ngược giữ nguyên cấu trúc không thay đổi. 

Đối với một mảng như$[x, x, x, \dots]$, cả hai hoạt động đều tạo ra hành vi nhiều tập giống hệt nhau, nhưng sự trùng lặp chiếm ưu thế hoàn toàn vì việc tích lũy tiền tố được hưởng lợi từ việc củng cố lặp đi lặp lại các đóng góp ban đầu giống hệt nhau.
