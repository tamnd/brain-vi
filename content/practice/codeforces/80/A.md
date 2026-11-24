---
title: "CF 80A - Dự đoán của Panoramix"
description: "Hôm qua quân Gaul đã đánh bại n lính La Mã và n được đảm bảo là số nguyên tố. Hôm nay họ đã đánh bại m lính, nơi m n. Chúng ta cần quyết định xem m có chính xác là số nguyên tố tiếp theo ngay sau n hay không. Chi tiết chính là cụm từ \"next prime\"."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 80
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 69 (Div. 2 Only)"
rating: 800
weight: 80
solve_time_s: 78
verified: true
draft: false
---

[CF 80A - Dự đoán của Panoramix](https://codeforces.com/problemset/problem/80/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hôm qua quân Gaul đã đánh bại`n`Những người lính La Mã, và`n`được đảm bảo là số nguyên tố. Hôm nay họ đã đánh bại`m`người lính ở đâu`m > n`. Chúng ta cần phải quyết định xem liệu`m`chính xác là số nguyên tố tiếp theo ngay sau`n`. 

Chi tiết mấu chốt là cụm từ "tiếp theo". Việc cả hai số đều là số nguyên tố là chưa đủ. Không được có số nguyên tố nào khác giữa chúng. 

Những hạn chế là rất nhỏ. Cả hai con số đều có nhiều nhất là 50, do đó, ngay cả việc kiểm tra vũ phu rất chậm cũng có thể hoạt động thoải mái trong thời gian giới hạn. Việc kiểm tra tính nguyên tố bằng cách chia thử cho đến căn bậc hai của một số chỉ thực hiện một vài thao tác ở đây. Thậm chí kiểm tra mọi số từ`n + 1`trở lên có hiệu quả ngay lập tức. 

Mối nguy hiểm chính là hiểu sai điều gì được coi là số nguyên tố tiếp theo. 

Hãy xem xét đầu vào này:```
3 7
```Câu trả lời đúng là:```
NO
```Cả 3 và 7 đều là số nguyên tố, nhưng 5 nằm giữa chúng và cũng là số nguyên tố. Việc thực hiện bất cẩn chỉ kiểm tra xem`m`là số nguyên tố sẽ in sai`YES`. 

Một sai lầm dễ mắc phải khác là dừng lại quá muộn khi đang tìm kiếm số nguyên tố tiếp theo. 

Ví dụ:```
7 11
```Số nguyên tố tiếp theo sau 7 thực ra là 11, vì 8, 9 và 10 là hợp số. Thuật toán phải bỏ qua các số không phải số nguyên tố một cách chính xác thay vì giả sử các số nguyên tố là các số lẻ liên tiếp. 

Trường hợp cạnh cuối cùng là đầu vào hợp lệ nhỏ nhất:```
2 3
```Câu trả lời là:```
YES
```Vì 2 là số nguyên tố đầu tiên nên số nguyên tố tiếp theo sau nó là 3. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tạo ra tất cả các số nguyên tố lớn hơn`n`cho đến khi đạt được`m`. Chúng tôi có thể liên tục kiểm tra từng số nguyên về tính nguyên tố và thu thập các số nguyên tố. Khi chúng ta tìm thấy số nguyên tố đầu tiên lớn hơn`n`, chúng tôi so sánh nó với`m`. 

Điều này hoạt động vì các ràng buộc là cực kỳ nhỏ. Trong trường hợp xấu nhất, chúng tôi kiểm tra các số lên tới 50 và mỗi bài kiểm tra tính nguyên tố chỉ kiểm tra một số ít ước số. Ngay cả việc triển khai không hiệu quả cũng sẽ chạy ngay lập tức. 

Một ý tưởng brute-force hơi khác là tính toán trước mọi số nguyên tố lên tới 50 bằng cách sử dụng các vòng lặp lồng nhau, sau đó quét danh sách để xem liệu`m`ngay sau đó`n`. Điều đó cũng hoạt động thoải mái trong giới hạn. 

Giải pháp sạch hơn sử dụng một quan sát đơn giản: chúng ta không cần tất cả các số nguyên tố. Chúng ta chỉ cần số nguyên tố đầu tiên lớn hơn`n`. 

Vì vậy, thay vì tạo ra mọi thứ, chúng tôi bắt đầu từ`n + 1`và tìm kiếm lên trên cho đến khi chúng ta gặp một số nguyên tố. Theo định nghĩa, số nguyên tố đầu tiên đó là số nguyên tố tiếp theo sau`n`. Nếu nó bằng`m`, câu trả lời là`YES`; nếu không thì nó là`NO`. 

Điều này làm giảm nhiệm vụ xuống còn hai thành phần nhỏ: 

1. Hàm kiểm tra một số có phải là số nguyên tố hay không. 
2. Vòng lặp tìm số nguyên tố đầu tiên lớn hơn`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán trước tất cả các số nguyên tố lên tới 50 | O(50²) | O(50) | Đã chấp nhận | 
| Tìm kiếm trực tiếp tối ưu cho số nguyên tố tiếp theo | O(50√50) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`m`. 
2. Viết hàm trợ giúp`is_prime(x)`điều đó kiểm tra xem`x`là nguyên tố. 

Một số là số nguyên tố nếu nó không có ước số từ 2 đến`√x`. Nếu có bất kỳ ước số nào thì số đó là hợp số. 
3. Bắt đầu kiểm tra số nguyên từ`n + 1`trở lên. 

Chúng ta đang tìm số nguyên tố đầu tiên lớn hơn`n`. 
4. Với mỗi mã số ứng viên, hãy gọi`is_prime(candidate)`. 

Nếu số không phải là số nguyên tố thì tiếp tục tìm kiếm. 
5. Ngay khi tìm thấy số nguyên tố đầu tiên, hãy so sánh nó với`m`. 

Nếu chúng bằng nhau thì in`YES`. Nếu không thì in`NO`. 
6. Dừng lại ngay sau khi tìm được số nguyên tố đầu tiên. 

Chúng tôi chỉ quan tâm đến số nguyên tố tiếp theo ngay sau`n`. 

### Tại sao nó hoạt động 

Thuật toán tìm kiếm các số nguyên theo thứ tự tăng dần bắt đầu từ`n + 1`. Số đầu tiên vượt qua bài kiểm tra tính nguyên tố chính xác là số nguyên tố nhỏ nhất lớn hơn`n`, phù hợp với định nghĩa toán học của số nguyên tố tiếp theo. 

Nếu con số này bằng`m`, sau đó`m`là số nguyên tố tiếp theo sau`n`. Nếu nó khác với`m`, thì`m`không phải là số nguyên tố hoặc có một số nguyên tố khác tồn tại giữa`n`Và`m`. Trong cả hai trường hợp, câu trả lời đúng là`NO`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False

    d = 2
    while d * d <= x:
        if x % d == 0:
            return False
        d += 1

    return True

def solve():
    n, m = map(int, input().split())

    candidate = n + 1

    while True:
        if is_prime(candidate):
            if candidate == m:
                print("YES")
            else:
                print("NO")
            return

        candidate += 1

solve()
```các`is_prime`hàm thực hiện phép chia thử. Nó kiểm tra mọi ước số từ 2 đến căn bậc hai của số. Nếu bất kỳ ước số nào chia đều thì số đó là hợp số. 

Vòng lặp chính bắt đầu tại`n + 1`bởi vì chúng tôi chỉ quan tâm đến những con số lớn hơn`n`. Mỗi ứng cử viên đều được kiểm tra tính nguyên thủy. Số nguyên tố đầu tiên gặp phải tự động là số nguyên tố tiếp theo sau`n`. 

Một chi tiết triển khai tinh tế sẽ dừng ngay sau khi tìm thấy số nguyên tố đầu tiên. Tiếp tục xa hơn sẽ không chính xác vì các số nguyên tố sau không liên quan. Một chi tiết nhỏ khác là điều kiện vòng lặp`d * d <= x`. Điều này tránh thiếu các ước số bình phương hoàn hảo, chẳng hạn như 7 cho 49. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
```| Bước | ứng cử viên | is_prime(ứng cử viên) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 4 | Sai | Tiếp tục | 
| 2 | 5 | Đúng | So sánh với m | 
| 3 | 5 | Đúng | In CÓ | 

Số nguyên tố đầu tiên sau 3 là 5, khớp với`m`. Thuật toán in chính xác`YES`. 

### Ví dụ 2 

đầu vào:```
7 13
```| Bước | ứng cử viên | is_prime(ứng cử viên) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 8 | Sai | Tiếp tục | 
| 2 | 9 | Sai | Tiếp tục | 
| 3 | 10 | Sai | Tiếp tục | 
| 4 | 11 | Đúng | So sánh với m | 
| 5 | 11 | Đúng | In KHÔNG | 

Số nguyên tố đầu tiên sau 7 là 11, không phải 13. Mặc dù 13 là số nguyên tố nhưng nó không phải là số nguyên tố tiếp theo liền kề, vì vậy kết quả đúng là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(50√50) | Nhiều nhất là vài chục số được kiểm tra, mỗi số có phép chia thử cho đến căn bậc hai của nó | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Các ràng buộc là cực kỳ nhỏ nên giải pháp này dễ dàng phù hợp với giới hạn về thời gian và bộ nhớ. Ngay cả việc thực hiện chậm hơn nhiều vẫn sẽ trôi qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def is_prime(x):
        if x < 2:
            return False

        d = 2
        while d * d <= x:
            if x % d == 0:
                return False
            d += 1

        return True

    n, m = map(int, input().split())

    candidate = n + 1

    while True:
        if is_prime(candidate):
            return "YES\n" if candidate == m else "NO\n"

        candidate += 1

# provided sample
assert run("3 5\n") == "YES\n", "sample 1"

# custom cases
assert run("2 3\n") == "YES\n", "smallest primes"

assert run("3 7\n") == "NO\n", "prime exists in between"

assert run("7 11\n") == "YES\n", "skip composite numbers correctly"

assert run("47 49\n") == "NO\n", "m is not prime"

assert run("47 53\n") == "YES\n", "largest-range valid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`|`YES`| Số nguyên tố hợp lệ nhỏ nhất | 
|`3 7`|`NO`| Một số nguyên tố khác tồn tại giữa chúng | 
|`7 11`|`YES`| Các số nguyên tố liên tiếp có hợp số ở giữa | 
|`47 49`|`NO`| Số thứ hai không phải là số nguyên tố | 
|`47 53`|`YES`| Xử lý đúng gần giới hạn trên | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3 7
```Thuật toán bắt đầu từ 4. Nó kiểm tra 4, là hợp số, sau đó kiểm tra 5, là số nguyên tố. Vì 5 là số nguyên tố đầu tiên lớn hơn 3 nên nó là số nguyên tố tiếp theo sau 3. Vì 5 không bằng 7 nên thuật toán in ra`NO`. Điều này xử lý lỗi phổ biến là chỉ kiểm tra xem`m`bản thân nó là số nguyên tố. 

Bây giờ hãy xem xét:```
7 11
```Việc tìm kiếm kiểm tra 8, 9 và 10, loại bỏ tất cả chúng vì chúng là hợp số. Số nguyên tố đầu tiên được tìm thấy là 11, do đó thuật toán in ra`YES`. Điều này xác nhận rằng thuật toán bỏ qua một cách chính xác các đoạn số tổng hợp tùy ý. 

Cuối cùng, hãy xem xét trường hợp biên nhỏ nhất:```
2 3
```Thuật toán bắt đầu với ứng viên 3. Vì 3 là số nguyên tố và bằng`m`, đầu ra là`YES`. Điều này xác nhận hành vi đúng ở giới hạn dưới của các ràng buộc.
