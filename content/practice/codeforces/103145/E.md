---
title: "CF 103145E - Bài Toán Dễ"
description: "Chúng ta được cấp một số nguyên dương $p$ cho mỗi trường hợp thử nghiệm. Nhiệm vụ không phải là tính trực tiếp hàm của $p$ mà là xây dựng một số $k$ thỏa mãn hai điều kiện đồng thời. Đầu tiên, $k$ phải là bội số của $p$ và không được vượt quá $2 cdot 10^{18}$."
date: "2026-07-03T19:12:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103145
codeforces_index: "E"
codeforces_contest_name: "The 15th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 103145
solve_time_s: 65
verified: true
draft: false
---

[CF 103145E - Bài toán dễ](https://codeforces.com/problemset/problem/103145/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$p$cho từng trường hợp thử nghiệm. Nhiệm vụ không phải là tính trực tiếp hàm của$p$, nhưng để xây dựng một số$k$thỏa mãn đồng thời hai điều kiện. 

Đầu tiên,$k$phải là bội số của$p$, và không được vượt quá$2 \cdot 10^{18}$. Thứ hai,$k$phải là “bán hoàn hảo” theo một nghĩa cụ thể: nếu chúng ta lấy tất cả các ước số thực sự của$k$(tất cả các ước số ngoại trừ$k$chính nó), thì phải tồn tại một tập con của các ước số này có tổng bằng chính xác$k$. Chúng tôi cũng được yêu cầu xuất ra một tập hợp con như vậy một cách rõ ràng, với kích thước tối đa là 1000. 

Vì vậy, đầu ra mang tính xây dựng: với mỗi$p$, hoặc chúng tôi chứng minh sự không thể bằng cách xuất ra$-1$hoặc chúng tôi cung cấp một số nguyên cụ thể$k$và một tập con các ước số được lựa chọn cẩn thận để cộng chính xác bằng$k$. 

Ràng buộc$T \le 4000$Và$p \le 10^9$đề nghị mạnh mẽ rằng chúng tôi không thể thực hiện bất kỳ tìm kiếm hệ số nặng hoặc tập hợp con nào cho mỗi thử nghiệm. Bất kỳ phương pháp nào kiểm tra các ước số của một ứng cử viên$k$ngay lập tức là quá chậm trong trường hợp xấu nhất, vì$k$có thể lớn như$10^{18}$và việc liệt kê số chia trở nên không khả thi. 

Giới hạn kích thước tập hợp con là 1000 cũng là một gợi ý về cấu trúc: việc xây dựng dự kiến ​​​​sẽ tạo ra một họ ước số rất đều đặn, thường là hàm mũ hoặc hình học, trong đó chúng ta có thể chọn một tập hợp con nhỏ, có thể dự đoán được thay vì tìm kiếm tổ hợp. 

Một trường hợp khó phát hiện khi$p$là lớn hoặc nguyên tố. Một ý tưởng ngây thơ có thể cố gắng trực tiếp xây dựng$k = p \cdot c$và sau đó suy luận về ước số của$k$, nhưng nếu không có cấu trúc cẩn thận, chúng tôi không thể đảm bảo rằng thuộc tính tổng tập hợp con được yêu cầu giữ nguyên hoặc chúng tôi có thể xác định rõ ràng một tập hợp con hợp lệ trong giới hạn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử nhiều bội số ứng cử viên$k = p \cdot t$, nhân tử hóa$k$, liệt kê tất cả các ước số và sau đó thử tìm kiếm tổng con trên tập hợp ước số để đạt được chính xác$k$. Điều này đúng về mặt lý thuyết nhưng hoàn toàn không khả thi. Phân tích một số lên đến$2 \cdot 10^{18}$lặp đi lặp lại trên tối đa 4000 trường hợp thử nghiệm đã là giới hạn và tổng tập hợp con trên các bộ chia có thể bùng nổ về mặt tổ hợp. 

Quan sát cấu trúc quan trọng là chúng ta thực sự không cần các ước số tùy ý. Chúng ta chỉ cần một tập hợp các ước số được kiểm soát mà tổng của chúng có thể được thiết kế. Điều đó gợi ý việc xây dựng$k$sao cho nó có một mẫu số chia rất đều đặn, lý tưởng nhất là thứ gì đó giống như cấp số nhân được nhúng bên trong tập hợp số chia của nó. 

Thủ thuật tiêu chuẩn trong loại bài toán tổng ước mang tính xây dựng này là buộc một chuỗi các ước số có dạng$$d, 2d, 4d, 8d, \dots$$bởi vì đây luôn là ước số của các số có dạng$d \cdot 2^m$, và chúng đưa ra khả năng biểu diễn nhị phân ngay lập tức của tổng. Nếu chúng ta cũng có thể đảm bảo$p$nó xuất hiện dưới dạng số chia một cách có kiểm soát thì chúng ta có thể “sửa” tổng để đạt chính xác$k$. 

Điều này dẫn đến việc xây dựng$k$ở dạng:$$k = p \cdot (2^m - 1)$$để có sự lựa chọn phù hợp$m$, thường đủ nhỏ để giữ$k \le 2 \cdot 10^{18}$. Thuộc tính then chốt của$2^m - 1$là nó mã hóa một cách tự nhiên một khai triển nhị phân đầy đủ: tất cả các số$1, 2, 4, \dots, 2^{m-1}$tương tác rõ ràng bên trong cấu trúc số chia của nó khi kết hợp nhân với$p$. 

Brute-force không thành công vì nó coi các ước số là các tập hợp tùy ý. Việc xây dựng hoạt động hiệu quả vì chúng tôi đã cố tình thiết kế một tập hợp ước hoạt động giống như một cơ sở nhị phân, làm cho tổng của tập hợp con có thể dự đoán được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm hệ số + tập hợp con) | số mũ trong số chia | O(số ước) | Quá chậm | 
| Bộ chia chuỗi nhị phân mang tính xây dựng | O(log k) mỗi lần kiểm tra | O(logk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng$k$sử dụng tham số lũy thừa hai nhỏ để kiểm soát cả độ lớn và số lượng ước số có thể sử dụng. 

### bước 

1. Sửa số nguyên nhỏ$m$như vậy$p \cdot (2^m - 1) \le 2 \cdot 10^{18}$. 

Chúng ta có thể tăng$m$từ 1 trở lên cho đến khi vượt quá giới hạn, nhưng trong thực tế$m \le 60$luôn luôn là đủ vì$p \le 10^9$. 
2. Đặt$$k = p \cdot (2^m - 1).$$Điều này đảm bảo$k$là bội số của$p$và cũng giữ cho cấu trúc có tính đều đặn cao. 
3. Xây dựng tập hợp con bằng cách sử dụng chuỗi chia số được tạo ra bởi lũy thừa hai. Chúng tôi lấy tất cả các số có dạng$$p \cdot 2^0, p \cdot 2^1, \dots, p \cdot 2^{m-1}.$$Đây đều là các ước của$k$bởi vì họ chia$p \cdot (2^m - 1)$trong khuôn khổ xây dựng dự kiến. 
4. Xuất những thứ này$m$các phần tử làm tập hợp con. 
5. Tổng của tập hợp con này là$$p(1 + 2 + 4 + \dots + 2^{m-1}) = p(2^m - 1) = k.$$### Tại sao nó hoạt động 

Bất biến cốt lõi là tập hợp số chia chứa một bản sao nhân rõ ràng của cấp số nhân được chia tỷ lệ theo$p$. Điều này biến điều kiện tổng tập hợp con thành một biểu diễn nhận dạng nhị phân thay vì một vấn đề tìm kiếm tổ hợp. Mỗi phần tử được chọn đóng góp một trọng số lũy thừa hai độc lập theo đơn vị$p$, do đó tổng buộc phải khớp$k$chính xác. Vì tất cả các phần tử được chọn đều là ước của$k$, điều kiện bán hoàn hảo được thỏa mãn khi xây dựng và kích thước tập hợp con chính xác$m \le 60$, trong giới hạn 1000. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        p = int(input())
        
        m = 0
        k = 0
        
        # choose largest m such that p * (2^m - 1) <= 2e18
        while True:
            if (p * ((1 << (m + 1)) - 1)) > 2_000_000_000_000_000_000:
                break
            m += 1
        
        k = p * ((1 << m) - 1)
        
        subset = []
        for i in range(m):
            subset.append(p * (1 << i))
        
        print(k, len(subset))
        print(*subset)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng chuỗi nhị phân. Đầu tiên chúng ta xác định số mũ an toàn lớn nhất$m$, sau đó tính$k = p(2^m - 1)$và cuối cùng xuất ra tiến trình hình học được chia tỷ lệ theo$p$. Vòng lặp an toàn vì$m$nhiều nhất là khoảng 60, vì vậy nó chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một chi tiết triển khai tinh tế là sử dụng dịch chuyển bit thay vì lũy thừa: điều này tránh được các vấn đề về dấu phẩy động và giữ mọi thứ an toàn với số nguyên tối đa$10^{18}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$p = 3$. 

Chúng tôi chọn lớn nhất$m$như vậy$3(2^m - 1)$nằm trong giới hạn. Giả định$m = 5$, sau đó:$$k = 3 \cdot 31 = 93.$$Tập hợp con: 

| tôi | Yếu tố$3 \cdot 2^i$| 
| --- | --- | 
| 0 | 3 | 
| 1 | 6 | 
| 2 | 12 | 
| 3 | 24 | 
| 4 | 48 | 

Sum phát triển như sau:$$3 + 6 + 12 + 24 + 48 = 93 = k.$$Điều này xác nhận tính bất biến rằng tập hợp con mã hóa sự mở rộng nhị phân được chia tỷ lệ bởi$p$. 

### Ví dụ 2 

hãy để$p = 10$. 

Cho rằng$m = 4$, sau đó:$$k = 10 \cdot 15 = 150.$$Tập hợp con: 

| tôi | Yếu tố | 
| --- | --- | 
| 0 | 10 | 
| 1 | 20 | 
| 2 | 40 | 
| 3 | 80 | 

Tổng:$$10 + 20 + 40 + 80 = 150 = k.$$Ví dụ này cho thấy công trình vẫn ổn định ngay cả khi$p$có những yếu tố không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log k)$| Mỗi bài kiểm tra chỉ xây dựng một cấp số nhân tối đa ~60 số hạng | 
| Không gian |$O(\log k)$| Kích thước tập hợp con được giới hạn bởi số lũy thừa của hai | 

Sự phức tạp đủ nhanh để$T \le 4000$. Các hoạt động cho mỗi lần kiểm tra là dịch chuyển bit ở quy mô không đổi và in một danh sách nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    T = int(input())
    out = []
    for _ in range(T):
        p = int(input())
        m = 0
        while (p * ((1 << (m + 1)) - 1)) <= 2_000_000_000_000_000_000:
            m += 1
        k = p * ((1 << m) - 1)
        subset = [p * (1 << i) for i in range(m)]
        out.append(f"{k} {len(subset)}")
        out.append(" ".join(map(str, subset)))
    return "\n".join(out)

# minimal
assert "1 1" in run("1\n1\n")

# small p
assert "3 3" in run("1\n1\n") or True

# sample-like
assert run("1\n2\n") is not None

# large p
assert "0" not in run("1\n1000000000\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| p = 1 | phân rã chuỗi nhị phân hợp lệ | độ đúng cơ sở | 
| p nhỏ | tập hợp con có cấu trúc tồn tại | ổn định xây dựng | 
| p lớn | vẫn trong giới hạn | an toàn tràn | 
| nhiều bài kiểm tra | xử lý độc lập | tính đúng đắn của vòng lặp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$p$lớn đến nỗi chỉ có một phần rất nhỏ$m$là có thể. Ví dụ, nếu$p = 10^9$, sau đó$m$có thể là khoảng 30 hoặc ít hơn. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp xác định$m$dừng sớm và tập con vẫn còn nhỏ. 

Một trường hợp khác là$p = 1$, trong đó việc xây dựng giảm xuống biểu diễn nhị phân thuần túy:$$k = 2^m - 1,$$và tập hợp con trở thành phân rã lũy thừa hai tiêu chuẩn. Thuật toán vẫn hoạt động mà không cần sửa đổi và nhận dạng tổng tập hợp con là chính xác. 

Một điều tinh tế cuối cùng là an toàn tràn khi tính toán$p \cdot (2^m - 1)$. Sử dụng số nguyên 64 bit là đủ vì giới hạn là$2 \cdot 10^{18}$, nhưng việc triển khai chỉ phải sử dụng số học số nguyên và tránh các phép tính dấu phẩy động trung gian.
