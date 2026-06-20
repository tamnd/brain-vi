---
title: "CF 1042A - Ghế dài"
description: "Chúng tôi được phân cho một số ghế dài, mỗi ghế đã có một số người ngồi. Sau đó, một nhóm người mới đến, mỗi người phải chọn một chiếc ghế dài và ngồi ở đó."
date: "2026-06-16T17:51:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1042
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 510 (Div. 2)"
rating: 1100
weight: 1042
solve_time_s: 136
verified: true
draft: false
---

[CF 1042A - Ghế dài](https://codeforces.com/problemset/problem/1042/A) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phân cho một số ghế dài, mỗi ghế đã có một số người ngồi. Sau đó, một nhóm người mới đến, mỗi người phải chọn một chiếc ghế dài và ngồi ở đó. Sau khi mọi người đã ngồi, một chiếc ghế sẽ có số lượng người lớn nhất trong số tất cả những chiếc ghế và giá trị đó được gọi là$k$. 

Nhiệm vụ là tính toán hai điểm cực trị của tải tối đa cuối cùng này. Đầu tiên, chúng tôi muốn giá trị nhỏ nhất có thể của$k$, giả sử chúng ta phân bổ những người mới đến một cách đồng đều nhất có thể để tránh làm quá tải bất kỳ băng ghế nào. Thứ hai, chúng tôi muốn giá trị lớn nhất có thể của$k$, giả sử chúng ta cố tình tập trung mọi người để tạo thành một băng ghế càng đông càng tốt. 

Các ràng buộc đủ nhỏ để bất kỳ giải pháp nào có thể đạt tới mức gần đúng$O(n \log(\max a_i + m))$hoặc thậm chí$O(n \cdot m)$có thể vượt qua, vì$n \le 100$Và$m \le 10000$. Điều này có nghĩa là chúng ta có thể thực hiện mô phỏng tham lam hoặc tìm kiếm nhị phân trên không gian câu trả lời mà không phải lo lắng về các vấn đề hiệu suất. 

Một trường hợp thất bại tinh vi xuất hiện khi cố gắng phân phối mọi người một cách tham lam mà không cân nhắc về hậu quả trong tương lai. Ví dụ: luôn đặt một người mới vào băng ghế dự bị ít tải nhất hiện tại có thể trông hợp lý nhưng không trực tiếp đảm bảo tính tối ưu trừ khi chúng ta chính thức hóa nó như một vấn đề về năng lực. Một trường hợp khác là khi tất cả các băng ghế đều giống hệt nhau, trong đó cả câu trả lời tối thiểu và tối đa đều thu gọn về cùng một giá trị và lối suy nghĩ ngây thơ về phân phối không đồng đều vẫn có thể tạo ra lý luận sai nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Giá trị tối đa có thể có của$k$là đơn giản. Vì chúng tôi muốn một băng ghế càng lớn càng tốt, chúng tôi chỉ cần chọn băng ghế đã có nhiều người nhất và phân công tất cả$m$người mới đến đó. Không có cách phân phối nào khác có thể tăng mức tối đa hơn nữa, bởi vì việc dàn trải người ở nơi khác chỉ làm giảm số lượng được thêm vào nhóm mục tiêu. Vì vậy, trường hợp tối đa hoàn toàn được xác định bởi mức tối đa ban đầu. 

Giá trị tối thiểu có thể có của$k$có cấu trúc hơn. Chúng tôi đang cố gắng phân phối$m$người để không có băng ghế nào trở nên quá lớn. Điều này tương đương với việc yêu cầu số nhỏ nhất$k$để chúng ta có thể "nắp" tất cả các băng ghế ở độ cao$k$bằng cách sắp xếp người mới một cách thích hợp. 

Nếu chúng ta sửa một giá trị ứng viên$k$, mỗi băng ghế$i$có thể chấp nhận nhiều nhất$k - a_i$thêm người trước khi nó đến tay$k$. Nếu chúng ta cộng sức chứa này trên tất cả các băng ghế, chúng ta sẽ có được tổng số người mà chúng ta có thể chứa mà không vượt quá$k$. Nếu tổng số này ít nhất là$m$, thì có thể phân phối cho tất cả mọi người trong khi vẫn giữ được mức tối đa$k$. Nếu không thì không thể được. 

Điều này biến vấn đề thành một điều kiện đơn điệu$k$, điều này đương nhiên dẫn đến tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tham lam + Tính khả thi của tìm kiếm nhị phân |$O(n \log(m + \max a_i))$|$O(1)$| Đã chấp nhận | 
| Mô phỏng hoặc vị trí ngây thơ |$O(n \cdot m)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi tách tính toán thành hai phần độc lập: tối đa và tối thiểu. 

### Tối đa$k$1. Tính sức chứa ban đầu tối đa của tất cả các ghế dài. 
2. Thêm tất cả$m$những người mới đến cùng băng ghế đó. 
3. Trả về giá trị kết quả. 

Điều này có hiệu quả vì việc tập trung tất cả các phần bổ sung vào một băng ghế chỉ có thể tăng mức tối đa tổng thể và không có sự sắp xếp nào khác có thể vượt quá cấu trúc này. 

### Tối thiểu$k$1. Đặt phạm vi tìm kiếm cho$k$từ$\max(a_i)$ĐẾN$\max(a_i) + m$. Giới hạn dưới là mức tối đa hiện tại vì chúng tôi không thể giảm tải hiện tại. Giới hạn trên là khi tất cả những người mới đến ngồi vào một băng ghế. 
2. Đối với ứng viên cố định$k$, tính xem mỗi băng ghế có thể tiếp nhận thêm bao nhiêu người mà không vượt quá$k$, đó là$\max(0, k - a_i)$. 
3. Tính tổng các khả năng này của tất cả các băng ghế. 
4. Nếu tổng công suất ít nhất$m$, điều đó có nghĩa là chúng tôi có thể phân phối tất cả những người mới đến mà không vượt quá$k$, Vì thế$k$là khả thi. 
5. Nếu không,$k$là quá nhỏ. 
6. Sử dụng tìm kiếm nhị phân để tìm ra phương án khả thi nhỏ nhất$k$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tính khả thi là đơn điệu trong$k$. Nếu nhất định$k$cho phép tất cả$m$người được đặt, sau đó bất kỳ giá trị lớn hơn của$k$cũng cho phép bố trí vì mỗi băng ghế chỉ tăng thêm sức chứa. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân hội tụ đến ngưỡng hợp lệ nhỏ nhất mà không bỏ sót bất kỳ trường hợp biên nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, a, m):
    total = 0
    for x in a:
        if k > x:
            total += k - x
    return total >= m

n = int(input())
m = int(input())
a = [int(input()) for _ in range(n)]

mx = max(a)

# maximum k
max_k = mx + m

# minimum k via binary search
lo, hi = mx, mx + m
while lo < hi:
    mid = (lo + hi) // 2
    if can(mid, a, m):
        hi = mid
    else:
        lo = mid + 1

min_k = lo

print(min_k, max_k)
```Mã chia nhỏ vấn đề thành hai phép tính độc lập. Chức năng trợ giúp`can(k, a, m)`mã hóa điều kiện khả thi có được trước đó. Tìm kiếm nhị phân chạy trên không gian câu trả lời thay vì trên các chỉ mục, điều này an toàn vì điều kiện đơn điệu trong$k$. 

Một lỗi phổ biến là quên bao gồm`max(0, k - a_i)`logic và thay vào đó là tổng hợp những khác biệt thô, điều này sẽ cho phép những đóng góp tiêu cực từ các băng ghế vốn đã quá đầy không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
6
1
1
1
1
```| Bước | k (giữa) | Tổng công suất | Khả thi | 
| --- | --- | --- | --- | 
| 1 | 3 | (2+2+2+2)=8 | Có | 
| 2 | 2 | (1+1+1+1)=4 | Không | 
| 3 | 3 | xác nhận | Có | 

tối thiểu$k = 3$, tối đa$k = 7$. 

Điều này cho thấy năng lực tăng trưởng như thế nào cùng với$k$và tìm kiếm nhị phân xác định điểm nhỏ nhất nơi tổng công suất đáp ứng được nhu cầu. 

### Ví dụ 2 

đầu vào:```
1
10
5
```| Bước | k | Công suất | Khả thi | 
| --- | --- | --- | --- | 
| 1 | 5 | 0 | Không | 
| 2 | 15 | 10 | Có | 

Tối thiểu và tối đa đều bằng 15, thể hiện trường hợp suy biến khi chỉ có một băng ghế và tất cả các lựa chọn phân phối đều sụp đổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log(m + \max a_i))$| Tìm kiếm nhị phân trên các giá trị tối đa có thể có, mỗi lần kiểm tra sẽ quét tất cả các băng ghế | 
| Không gian |$O(1)$| Chỉ có một số bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Những hạn chế$n \le 100$Và$m \le 10000$thực hiện việc này nhanh chóng một cách thoải mái vì tối đa vài nghìn lần kiểm tra tính khả thi được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    
    n = int(input())
    m = int(input())
    a = [int(input()) for _ in range(n)]

    mx = max(a)
    max_k = mx + m

    def can(k):
        total = 0
        for x in a:
            if k > x:
                total += k - x
        return total >= m

    lo, hi = mx, mx + m
    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid):
            hi = mid
        else:
            lo = mid + 1

    min_k = lo
    return f"{min_k} {max_k}"

# provided sample
assert run("4\n6\n1\n1\n1\n1\n") == "3 7"

# all same, single bench
assert run("1\n10\n5\n") == "15 15"

# already large spread
assert run("3\n0\n5\n2\n8\n") == "8 8"

# skewed distribution
assert run("3\n5\n10\n1\n1\n") == "10 15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các băng ghế bằng nhau | kết quả đối xứng | xử lý phân phối thống nhất | 
| băng ghế đơn | tối thiểu/tối đa giống hệt nhau | cấu trúc thoái hóa | 
| không bổ sung | tối đa không thay đổi | độ chính xác cơ bản | 
| lệch lớn max | hành vi nhị phân đúng | xử lý công suất không đồng đều | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi chỉ có một băng ghế. Trong trường hợp đó, mọi người mới đến đều phải đến đó, vì vậy cả kết quả tối thiểu và tối đa đều giảm về cùng một giá trị. Thuật toán xử lý việc này một cách tự nhiên vì hàm dung lượng giảm xuống còn một thuật ngữ duy nhất và tìm kiếm nhị phân ngay lập tức hội tụ thành$a_1 + m$. 

Một trường hợp khác là khi tất cả các băng ghế đã có giá trị lớn và$m$là nhỏ. Tìm kiếm nhị phân vẫn hoạt động vì tính khả thi chỉ phụ thuộc vào việc liệu ngưỡng tăng nhẹ có thể hấp thụ tất cả người mới hay không và phạm vi tìm kiếm bắt đầu ở mức tối đa hiện tại, đảm bảo không xem xét các dự đoán thấp hơn không hợp lệ. 

Cuối cùng, khi tất cả các băng ghế đều bằng nhau, tính đối xứng phân bố có nghĩa là độ trải rộng luôn tối ưu để giảm thiểu mức tối đa. Quá trình kiểm tra tính khả thi sẽ tự động nắm bắt điều này bằng cách coi mỗi băng ghế đều đóng góp công suất giống nhau, do đó việc tìm kiếm hội tụ chính xác ở ngưỡng cân bằng.
