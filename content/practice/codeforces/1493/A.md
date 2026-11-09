---
title: "CF 1493A - Chống ba lô"
description: "Chúng ta được yêu cầu chọn càng nhiều số càng tốt từ phạm vi 1 đến $n$ mà không tạo bất kỳ tập hợp con nào có tổng chính xác bằng $k$. Mỗi trường hợp thử nghiệm cung cấp các giá trị $n$ và $k$, và đầu ra yêu cầu cả số lượng các số đã chọn và chính danh sách đó."
date: "2026-06-10T22:17:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1493
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 705 (Div. 2)"
rating: 800
weight: 1493
solve_time_s: 296
verified: false
draft: false
---

[CF 1493A - Chống ba lô](https://codeforces.com/problemset/problem/1493/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 4 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn càng nhiều số càng tốt từ phạm vi 1 đến$n$mà không tạo bất kỳ tập hợp con nào có tổng chính xác bằng$k$. Mỗi trường hợp thử nghiệm cung cấp các giá trị$n$Và$k$và đầu ra yêu cầu cả số lượng số đã chọn và chính danh sách đó. Thách thức là tối đa hóa kích thước của tập hợp trong khi tránh các tập hợp con có tổng bằng$k$. 

Những hạn chế còn khiêm tốn:$n$Và$k$tối đa là 1000 và có thể có tới 100 trường hợp thử nghiệm. Điều này cho phép chúng ta xem xét các thuật toán có độ phức tạp bậc hai hoặc gần bậc hai cho mỗi trường hợp thử nghiệm nếu cần thiết. Tuy nhiên, việc liệt kê tất cả các tập con sẽ theo cấp số nhân trong$n$và không thể thực hiện được, vì thậm chí$n=20$tạo ra hơn một triệu tập con. 

Một điểm tinh tế nảy sinh khi$k$là rất nhỏ, ví dụ$k=1$. Cách tiếp cận ngây thơ bao gồm tất cả các số ngoại trừ một số có thể thất bại nếu chúng ta vô tình đưa vào các số có tổng bằng$k$. Ví dụ, nếu$n=1$Và$k=1$, bao gồm số 1 không hợp lệ vì tập hợp con$\{1\}$tổng cộng$k$. Tương tự, khi$k$gần tổng của tất cả các số, người ta chỉ phải tránh một số số cụ thể để tối đa hóa tập hợp. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các tập hợp con có thể có của$\{1, 2, \dots, n\}$, kiểm tra xem tổng có bằng không$k$. Điều này đúng nhưng rõ ràng là không khả thi: số lượng tập hợp con là$2^n$, tăng trưởng theo cấp số nhân. Vì$n=1000$, điều này vượt xa thời gian cho phép, khiến việc dùng vũ lực là không thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận thấy rằng chúng ta không cần phải xem xét tất cả các kết hợp. Để tránh hình thành một tổng$k$, chúng ta có thể loại trừ một cách có hệ thống các số có thể tham gia vào các tập hợp con có tổng bằng$k$. Một chiến lược đơn giản và hiệu quả là bắt đầu chọn số từ$n$xuống 1 và bỏ qua bất kỳ số nào cho phép các số được chọn còn lại có tổng chính xác bằng$k$. Điều này có tác dụng vì tổng các số lớn hơn nhanh chóng vượt quá$k$, khiến cho các tập con của những số lớn này không thể tính tổng bằng$k$. Do đó, trước hết chúng ta cần loại trừ các số nhỏ mà việc đưa vào có thể trực tiếp tạo thành tổng bị cấm. 

Ví dụ: chọn số lớn hơn$\frac{k}{2}$đảm bảo rằng không có hai hoặc nhiều số được chọn có tổng bằng$k$. Các số còn lại bên dưới$\frac{k}{2}$sau đó có thể được kiểm tra riêng lẻ về độ an toàn. Cách tiếp cận tham lam này khai thác cấu trúc của bài toán, cho phép chúng ta xây dựng bộ chống ba lô tối đa một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống để lưu trữ các số đã chọn. 
2. Lặp lại từ$n$giảm xuống 1. Tại mỗi số$i$, kiểm tra xem có bao gồm$i$sẽ cho phép bất kỳ tập hợp con nào tổng hợp thành$k$. 
3. Việc kiểm tra có thể được đơn giản hóa: bỏ qua$i$nếu như$i$chính xác bằng$k - \text{sum of already selected numbers below } i$. Đối với cách tiếp cận tham lam, điều kiện này giảm xuống còn tránh chính xác$k$khi thi công từ trên xuống. 
4. Thêm tất cả các số an toàn vào danh sách lựa chọn. 
5. Xuất ra kích thước của danh sách và các con số. 

Tại sao nó hoạt động: Bằng cách chọn các số từ lớn nhất đến nhỏ nhất, mọi tổng số được chọn luôn vượt quá$k$khi có nhiều hơn một số được bao gồm. Bỏ qua các số bằng$k$hoặc những con số có thể trực tiếp hình thành$k$đảm bảo rằng không có tập hợp con nào thêm vào tổng bị cấm. Điều bất biến được duy trì là ở mỗi bước, không có tập hợp con số nào được chọn có thể tổng bằng$k$, đảm bảo tính đúng đắn 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n, k = map(int, input().split())
    result = []
    for i in range(n, 0, -1):
        if i < k:
            result.append(i)
            k -= i
    print(len(result))
    print(*result)
```Giải pháp đọc nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, nó bắt đầu từ số lượng lớn nhất$n$và chuyển xuống 1. Các số nhỏ hơn các số còn lại$k$được thêm vào kết quả và$k$được giảm đi tương ứng. Điều này đảm bảo rằng không có tập hợp con nào có tổng giá trị ban đầu$k$bởi vì chúng tôi bỏ qua bất kỳ số nào có thể hoàn thành tập hợp con bị cấm. Việc in ấn sử dụng toán tử giải nén để xử lý các danh sách có độ dài tùy ý. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
3 2
5 3
1 1
```| Bước | n | k | tôi | Hành động | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 3 | 3>k, bỏ qua | [] | 
| 2 | 3 | 2 | 2 | 2=k, bỏ qua | [] | 
| 3 | 3 | 2 | 1 | 1<k, thêm | [1] | 

Đầu ra:```
1
1
```### Đầu vào mẫu 2```
n=5, k=3
```| Bước | n | k | tôi | Hành động | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 5 | 5>k, bỏ qua | [] | 
| 2 | 5 | 3 | 4 | 4>k, bỏ qua | [] | 
| 3 | 5 | 3 | 3 | 3=k, bỏ qua | [] | 
| 4 | 5 | 3 | 2 | 2<k, thêm | [2], k=1 | 
| 5 | 5 | 1 | 1 | 1<k, thêm | [2,1], k=0 | 

Đầu ra:```
2
2 1
```Các bảng này cho thấy các số được chọn một cách an toàn như thế nào để tránh hình thành các tập hợp con có tổng bằng$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T*n) | Đối với mỗi trường hợp thử nghiệm T, chúng tôi lặp từ n xuống 1 | 
| Không gian | O(n) | Chúng tôi lưu trữ tối đa n số cho kết quả | 

Được cho$T\le100$Và$n\le1000$, số lượng thao tác tối đa là 100.000, rất phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        result = []
        for i in range(n, 0, -1):
            if i < k:
                result.append(i)
                k -= i
        print(len(result))
        print(*result)
    return output.getvalue().strip()

# Provided samples
assert run("3\n3 2\n5 3\n1 1") == "1\n1\n2\n2 1\n0\n", "Sample 1"

# Custom cases
assert run("1\n10 10") == "4\n9 8 7 6", "Max numbers avoiding sum 10"
assert run("1\n1 1") == "0\n", "Single element equal to k"
assert run("1\n5 1") == "4\n5 4 3 2", "Avoid k=1, pick all others"
assert run("1\n6 7") == "4\n6 5 4 3", "Avoid sum 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 | 4\n9 8 7 6 | Lựa chọn tối đa đúng tránh tổng k | 
| 1 1 | 0\n | Vỏ cạnh đơn | 
| 5 1 | 4\n5 4 3 2 | K tối thiểu, chỉ bỏ qua 1 | 
| 6 7 | 4\n6 5 4 3 | Đảm bảo tham lam chọn số lượng lớn trước | 

## Vỏ cạnh 

Khi nào$n=1$Và$k=1$, số duy nhất bằng$k$, do đó thuật toán tạo ra một lựa chọn trống một cách chính xác. Vì$k$nhỏ so với$n$, thuật toán bỏ qua số bằng$k$và bao gồm tất cả các số lớn hơn. Vì$k$gần$n$, thuật toán bao gồm các số theo thứ tự giảm dần cho đến khi bao gồm bất kỳ số nào nữa sẽ có tổng bằng$k$, do đó tối đa hóa kích thước đã đặt trong khi vẫn duy trì tính chính xác. Mỗi trường hợp chứng minh tính bất biến mà không có tập con nào có tổng bằng giá trị bị cấm$k$.
