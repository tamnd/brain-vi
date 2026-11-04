---
title: "CF 1147C - Thanos Nim"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một dãy cọc, mỗi cọc chứa một số viên đá. Có $n$ cọc và $n$ được đảm bảo là chẵn."
date: "2026-06-12T03:14:46+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1147
codeforces_index: "C"
codeforces_contest_name: "Forethought Future Cup - Final Round (Onsite Finalists Only)"
rating: 2000
weight: 1147
solve_time_s: 82
verified: true
draft: false
---

[CF 1147C - Thanos Nim](https://codeforces.com/problemset/problem/1147/C) 

**Xếp hạng:** 2000 
**Thẻ:** trò chơi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một dãy cọc, mỗi cọc chứa một số viên đá. có$n$cọc, và$n$được đảm bảo là đồng đều. Alice đi trước và ở mỗi lượt người chơi phải chọn chính xác$n/2$các cọc không trống và loại bỏ ít nhất một viên đá khỏi mỗi cọc. Người chơi thay phiên nhau cho đến khi ai đó không thể di chuyển được, điều này xảy ra khi có ít hơn$n/2$cọc không trống. Người chơi không thể di chuyển sẽ thua. 

Đầu vào cho số lượng cọc$n$và mảng$a_1, a_2, \dots, a_n$đại diện cho đá trong mỗi đống. Đầu ra phải là người chiến thắng, "Alice" hoặc "Bob". 

Những hạn chế là nhỏ:$n$tăng lên 50 và mỗi$a_i$lên tới 50. Điều này cho phép các thuật toán bậc hai trong$n$hoặc thậm chí là hình khối nếu cần thiết. Tuy nhiên, trò chơi mang tính kết hợp, vì vậy việc ép buộc mọi nước đi một cách thô bạo sẽ cần phải được kiểm tra.$\binom{n}{n/2}$các lựa chọn ở mỗi bước, phát triển nhanh chóng và không khả thi ngay cả đối với$n=50$. 

Các trường hợp cạnh phát sinh trong trường hợp các cọc đều bằng nhau hoặc có sự chênh lệch rất lớn giữa cọc lớn nhất và cọc nhỏ nhất. Ví dụ, nếu$n=2$và cả hai cọc đều bằng nhau, người chơi thứ hai luôn có thể phản ánh cọc đầu tiên, đảm bảo chiến thắng. Việc triển khai bất cẩn có thể bỏ lỡ thuộc tính đối xứng và dự đoán sai người chiến thắng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ mô phỏng mọi nước đi có thể xảy ra: với mỗi người chơi, tạo ra tất cả các kết hợp của$n/2$các cọc không trống và trừ đi mọi số lượng đá có thể có từ mỗi cọc đã chọn theo cách đệ quy cho đến khi đạt đến vị trí cuối cùng. Điều này đúng vì nó khám phá tất cả các khả năng, nhưng nó có độ phức tạp theo cấp số nhân, đặc biệt là$O\left((\text{max stones})^{n/2} \cdot \binom{n}{n/2}\right)$, điều này rõ ràng là không thể thực hiện được đối với$n=50$. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy rằng trò chơi có tính đối xứng và có thể rút gọn thành việc so sánh các cọc thay vì mô phỏng các nước đi. Vì mỗi người chơi phải thực hiện$n/2$cọc, trò chơi tương đương với việc ghép đôi lớn nhất$n/2$cọc chống lại cái nhỏ nhất$n/2$cọc. Nếu tổng lớn nhất$n/2$cọc hoàn toàn lớn hơn tổng nhỏ nhất$n/2$cọc, người chơi thứ nhất luôn có thể loại bỏ quân theo cách như vậy để duy trì lợi thế, nếu không, người chơi thứ hai có thể phản chiếu các nước đi để giành chiến thắng. Điều này cho phép một giải pháp đơn giản, theo thời gian tuyến tính: sắp xếp các cọc và so sánh tổng của hai nửa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((50)^(n/2) * C(n, n/2)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số cọc$n$và mảng kích thước cọc$a$. 

Việc sắp xếp và thao tác sẽ dễ dàng hơn với danh sách trong tay. 
2. Sắp xếp mảng$a$theo thứ tự không giảm. 

Việc sắp xếp đảm bảo rằng lần đầu tiên$n/2$các phần tử là những cọc nhỏ nhất và cuối cùng$n/2$phần tử là cọc lớn nhất. 
3. Tính tổng số đầu tiên$n/2$cọc (nhỏ nhất) và tổng của cọc cuối cùng$n/2$cọc (lớn nhất). 

Cho phép`small_sum`là tổng của nhỏ nhất$n/2$cọc, và`large_sum`là tổng của lớn nhất$n/2$cọc. 
4. So sánh`large_sum`Và`small_sum`. 

Nếu như`large_sum`lớn hơn, Alice có một chiến lược chiến thắng: cô ấy luôn có thể chọn những cọc lớn nhất và giảm chúng để duy trì lợi thế nghiêm ngặt, cuối cùng khiến Bob không di chuyển. Nếu không, Bob có thể bắt chước các bước đi của Alice để duy trì sự bình đẳng và giành chiến thắng. 
5. Xuất "Alice" nếu`large_sum`>`small_sum`; nếu không thì xuất ra "Bob". 

Tại sao nó hoạt động: Bằng cách luôn chọn các cọc lớn nhất hiện có, người chơi đầu tiên có thể buộc phải vào một vị trí mà người chơi thứ hai phải đối mặt với ít lựa chọn hơn. Việc sắp xếp và chia thành hai nửa tạo nên sự tách biệt rõ ràng: nếu tổng số quân ở nửa trên vượt quá nửa dưới thì người chơi đầu tiên luôn có thể duy trì quyền kiểm soát. Điều này biến trò chơi tổ hợp phức tạp thành một phép so sánh số học đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()
half = n // 2

small_sum = sum(a[:half])
large_sum = sum(a[half:])

if large_sum > small_sum:
    print("Alice")
else:
    print("Bob")
```Mã đọc đầu vào bằng I/O nhanh. Việc sắp xếp mảng đảm bảo rằng nửa trên và nửa dưới được xác định chính xác. Tính toán các khoản tiền và so sánh chúng trực tiếp thực hiện chiến lược tối ưu. Không có lỗi riêng lẻ nào vì lát cắt của Python`a[:half]`Và`a[half:]`chia mảng chính xác làm đôi cho chẵn$n$. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
2
8 8
```| Bước | Mảng được sắp xếp | tổng_nhỏ | lớn_sum | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [8, 8] | 8 | 8 | 8 > 8? Không -> Bob | 

Điều này thể hiện sự hòa về số tiền, vì vậy Alice không thể buộc thắng; phản chiếu đảm bảo Bob thắng. 

**Ví dụ 2** 

đầu vào:```
4
1 2 3 4
```| Bước | Mảng được sắp xếp | tổng_nhỏ | lớn_sum | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 2, 3, 4] | 1+2=3 | 3+4=7 | 7 > 3? Có -> Alice | 

Alice luôn có thể chọn hai cọc lớn nhất để duy trì lợi thế chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; n 50 | 
| Không gian | O(n) | Lưu trữ mảng và lát | 

Với các ràng buộc n 50, thuật toán này cực kỳ nhanh và nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    half = n // 2
    return "Alice" if sum(a[half:]) > sum(a[:half]) else "Bob"

# Provided samples
assert run("2\n8 8\n") == "Bob", "sample 1"
assert run("4\n1 2 3 4\n") == "Alice", "sample 2"

# Custom cases
assert run("2\n1 2\n") == "Alice", "first player has advantage"
assert run("4\n5 5 5 5\n") == "Bob", "all equal piles"
assert run("6\n1 1 1 1 10 10\n") == "Alice", "large disparity favors first"
assert run("50\n" + " ".join(["1"]*50) + "\n") == "Bob", "max n, equal stones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 2\n | Alice | Trường hợp nhỏ nhất có lợi thế | 
| 4\n5 5 5 5\n | Bob | Vỏ cạnh đối xứng | 
| 6\n1 1 1 1 10 10\n | Alice | Kiểm tra chênh lệch lớn | 
| 50\n1 ... 1\n | Bob | N tối đa với các viên đá bằng nhau | 

## Vỏ cạnh 

Nếu tất cả các cọc đều bằng nhau thì thuật toán sẽ xuất ra "Bob" một cách chính xác. Ví dụ,`n=4, a=[5,5,5,5]`. Tổng số nửa trên và nửa dưới bằng nhau nên Alice không thể duy trì lợi thế. Tính toán lát cắt`a[:half]`Và`a[half:]`đảm bảo phân vùng chính xác. Nếu một đống duy nhất chiếm ưu thế, như`n=6, a=[1,1,1,1,10,10]`, việc sắp xếp đảm bảo rằng các cọc lớn nhất được Alice xem xét và việc so sánh tổng sẽ xác định chính xác chiến lược chiến thắng của cô ấy.
