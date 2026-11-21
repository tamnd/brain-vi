---
title: "CF 237C - Các số nguyên tố xen kẽ"
description: "Chúng ta được cho một khoảng các số nguyên từ a đến b. Chúng ta muốn chọn độ dài l sao cho mỗi đoạn liền kề có độ dài l bên trong khoảng này chứa ít nhất k số nguyên tố. Trong số tất cả các độ dài thỏa mãn điều kiện này, chúng ta cần độ dài nhỏ nhất."
date: "2026-06-04T17:02:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 237
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 147 (Div. 2)"
rating: 1600
weight: 237
solve_time_s: 162
verified: true
draft: false
---

[CF 237C - Số nguyên tố theo khoảng](https://codeforces.com/problemset/problem/237/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng số nguyên từ`a`ĐẾN`b`. Chúng tôi muốn chọn một chiều dài`l`sao cho **mọi** đoạn dài liền kề nhau`l`trong khoảng này chứa ít nhất`k`số nguyên tố. 

Trong số tất cả các độ dài thỏa mãn điều kiện này, chúng ta cần độ dài nhỏ nhất. Nếu không có độ dài nào phù hợp, chúng tôi sẽ in`-1`. 

Một cách khác để xem vấn đề là đối với độ dài ứng viên`l`, chúng tôi kiểm tra mọi cửa sổ`[x, x+l-1]`hoàn toàn phù hợp bên trong`[a, b]`. Ứng viên chỉ hợp lệ nếu mỗi cửa sổ đó chứa ít nhất`k`số nguyên tố. 

Điểm cuối khoảng nhiều nhất là`10^6`. Đây là hạn chế quan trọng. Vì thông tin nguyên tố cần được lặp đi lặp lại nên việc tính toán trước tất cả các số nguyên tố cho đến`10^6`với một cái sàng là rất rẻ. Bản thân khoảng thời gian cũng có thể chứa tới`10^6`nên mọi giải pháp kiểm tra từng cửa sổ riêng biệt theo từng độ dài có thể sẽ hoạt động gần đúng`10^12`hoạt động và hoàn toàn không khả thi. 

Cấu trúc của điều kiện gợi ý một tính chất đơn điệu. Nếu một độ dài nhất định`l`hoạt động thì bất kỳ chiều dài lớn hơn nào cũng hoạt động. Một cửa sổ lớn hơn chỉ có thể chứa ít nhất số số nguyên tố bằng cửa sổ nhỏ hơn mà nó chứa. Sự đơn điệu này hướng trực tiếp tới tìm kiếm nhị phân. 

Có một số trường hợp khó bỏ sót. 

Coi như:```
14 16 1
```Không có số nguyên tố trong khoảng. Ngay cả cửa sổ lớn nhất có thể cũng chứa các số nguyên tố bằng 0, nên câu trả lời là`-1`. Việc triển khai tìm kiếm nhị phân bất cẩn có thể không bao giờ xác minh rõ ràng rằng giải pháp tồn tại. 

Coi như:```
2 5 2
```Cửa sổ`[2,3]`chứa hai số nguyên tố nhưng`[3,4]`chỉ chứa một. Chỉ nhìn vào một vài cửa sổ là không đủ. Mọi cửa sổ đều phải thỏa mãn điều kiện. 

Coi như:```
2 2 1
```Khoảng chứa một số nguyên tố duy nhất. Độ dài duy nhất có thể là`1`, hợp lệ. Các trường hợp ranh giới trong đó`a = b`thường bộc lộ từng lỗi một trong việc xử lý cửa sổ. 

Coi như:```
8 12 1
```Số nguyên tố duy nhất là`11`. Chiều dài`4`hoạt động vì mỗi cửa sổ có độ dài 4 đều chứa`11`, trong khi chiều dài`3`thất bại vì`[8,10]`không chứa số nguyên tố. Độ dài hợp lệ tối thiểu không nhất thiết liên quan đến khoảng cách giữa các số nguyên tố liên tiếp. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi độ dài có thể`l`. Đối với mỗi độ dài, hãy kiểm tra mọi cửa sổ có kích thước đó và đếm xem nó chứa bao nhiêu số nguyên tố. Nếu tất cả các cửa sổ chứa ít nhất`k`số nguyên tố, ghi lại câu trả lời. 

Số lượng số nguyên tố bên trong một cửa sổ có thể được tính toán một cách hiệu quả bằng cách sử dụng tổng tiền tố của các giá trị nguyên tố. Với tổng tiền tố như vậy, việc kiểm tra một cửa sổ sẽ trở thành`O(1)`. Thật không may, có tới`10^6`độ dài có thể và lên đến`10^6`cửa sổ theo chiều dài. Ngay cả sau khi tối ưu hóa các truy vấn cửa sổ, tổng công việc vẫn còn khoảng`10^12`, vượt xa giới hạn. 

Quan sát chính là tính đơn điệu. 

Giả sử chiều dài`l`là hợp lệ. Mỗi cửa sổ có chiều dài`l`chứa ít nhất`k`số nguyên tố. Bây giờ hãy xem xét bất kỳ chiều dài lớn hơn`L > l`. Mỗi chiều dài-`L`cửa sổ chứa một số chiều dài-`l`cửa sổ phụ. Vì cửa sổ con đó đã chứa ít nhất`k`số nguyên tố, cửa sổ lớn hơn cũng chứa ít nhất`k`số nguyên tố. 

Điều này có nghĩa là: 

- độ dài không hợp lệ được đặt trước, 
- độ dài hợp lệ đến sau. 

Câu trả lời là độ dài hợp lệ đầu tiên, có thể tìm thấy bằng tìm kiếm nhị phân. 

Để kiểm tra độ dài ứng cử viên`l`, chúng ta trượt một cửa sổ qua`[a,b]`. Sử dụng tổng tiền tố của các số nguyên tố, chúng ta có thể tính số lượng số nguyên tố trong mỗi cửa sổ trong thời gian không đổi. Nếu bất kỳ cửa sổ nào chứa ít hơn`k`số nguyên tố, ứng viên sẽ thất bại. 

Thành phần còn lại là tiền xử lý nguyên thủy nhanh. Vì tất cả các số nhiều nhất`10^6`, một sàng tiêu chuẩn của Eratosthenes tính toán tính nguyên tố cho mọi số có liên quan trong khoảng`O(10^6 log log 10^6)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((b-a+1)2) | O(b) | Quá chậm | 
| Tối ưu | O(b log log b + (b-a+1) log(b-a+1)) | O(b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một sàng Eratosthenes lên đến`b`và xác định số nào là số nguyên tố. 
2. Xây dựng mảng tổng tiền tố`pref`Ở đâu`pref[i]`bằng số số nguyên tố từ`1`bởi vì`i`. 
3. Xác định hàm`check(l)`. 
4. Đối với mọi vị trí xuất phát`x`từ`a`ĐẾN`b-l+1`, tính số số nguyên tố trong cửa sổ`[x, x+l-1]`sử dụng:`pref[x+l-1] - pref[x-1]`5. Nếu bất kỳ cửa sổ nào chứa ít hơn`k`số nguyên tố, trả về`False`. 
6. Nếu tất cả các cửa sổ đều đáp ứng yêu cầu, hãy quay lại`True`. 
7. Trước khi tìm kiếm nhị phân, hãy xác minh rằng độ dài lớn nhất có thể hoạt động. Điều này tương ứng với toàn bộ khoảng`[a,b]`. 
8. Nếu ngay cả độ dài lớn nhất cũng không thành công, hãy in`-1`. 
9. Nếu không thì tìm kiếm nhị phân theo độ dài từ`1`ĐẾN`b-a+1`. 
10. Khi nào`check(mid)`thành công, ghi lại nó làm câu trả lời ứng cử viên và tiếp tục tìm kiếm nửa bên trái. 
11. Khi nào`check(mid)`thất bại, tìm kiếm nửa bên phải. 
12. In ra độ dài hợp lệ nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố đảm bảo rằng số nguyên tố của mỗi cửa sổ được tính toán chính xác. các`check(l)`hàm trả về true chính xác khi mỗi độ dài-`l`cửa sổ chứa ít nhất`k`số nguyên tố. 

Tính chất quan trọng nhất là tính đơn điệu. Nếu chiều dài`l`là hợp lệ, mọi độ dài lớn hơn cũng hợp lệ vì mọi cửa sổ lớn hơn đều chứa độ dài hợp lệ-`l`cửa sổ con và do đó đã chứa ít nhất`k`số nguyên tố. Do đó, tập hợp các độ dài hợp lệ tạo thành một hậu tố của tất cả các độ dài có thể có. Tìm kiếm nhị phân trên vị từ đơn điệu này tìm thấy độ dài hợp lệ nhỏ nhất, chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, k = map(int, input().split())

    is_prime = [True] * (b + 1)
    if b >= 0:
        is_prime[0] = False
    if b >= 1:
        is_prime[1] = False

    p = 2
    while p * p <= b:
        if is_prime[p]:
            start = p * p
            for x in range(start, b + 1, p):
                is_prime[x] = False
        p += 1

    pref = [0] * (b + 1)
    for i in range(1, b + 1):
        pref[i] = pref[i - 1] + (1 if is_prime[i] else 0)

    def count_primes(l, r):
        return pref[r] - pref[l - 1]

    def check(length):
        end_start = b - length + 1
        for start in range(a, end_start + 1):
            if count_primes(start, start + length - 1) < k:
                return False
        return True

    n = b - a + 1

    if not check(n):
        print(-1)
        return

    lo, hi = 1, n
    ans = n

    while lo <= hi:
        mid = (lo + hi) // 2

        if check(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Sàng tính toán tính nguyên tố cho tất cả các số có thể xuất hiện trong bất kỳ cửa sổ nào. Vì mọi truy vấn đều yêu cầu số nguyên tố theo từng khoảng nên tổng tiền tố sẽ được tạo ngay sau đó. 

các`check()`hàm là vị từ cốt lõi được sử dụng bởi tìm kiếm nhị phân. Nó quét tất cả các cửa sổ có độ dài ứng cử viên và loại bỏ độ dài đó ngay khi tìm thấy một cửa sổ xấu. Việc ra đi sớm này rất quan trọng vì nhiều ứng viên thất bại nhanh chóng. 

Việc kiểm tra sự tồn tại trước khi tìm kiếm nhị phân là điều cần thiết. Nếu toàn bộ khoảng chứa ít hơn`k`số nguyên tố, không có cửa sổ nhỏ hơn nào có thể đáp ứng được yêu cầu. Nếu không có bước này, tìm kiếm nhị phân sẽ trả về độ dài không chính xác ngay cả khi không có giải pháp nào tồn tại. 

Số lượng cửa sổ sử dụng:```
pref[r] - pref[l - 1]
```đó là lý do tại sao mảng tiền tố được lập chỉ mục từ`0`. Điều này tránh được những trường hợp đặc biệt khi`l = 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4 2
```Các số nguyên tố trong khoảng là`{2, 3}`. 

| Chiều dài | Windows | Số nguyên tố | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | [2], [3], [4] | 1, 1, 0 | Không | 
| 2 | [2,3], [3,4] | 2, 1 | Không | 
| 3 | [2,4] | 2 | Có | 

Tìm kiếm nhị phân cuối cùng đạt đến độ dài`3`, đó là độ dài hợp lệ đầu tiên. 

Đầu ra:```
3
```Ví dụ này chứng minh rằng mọi cửa sổ đều phải đáp ứng yêu cầu. Một cửa sổ thất bại là đủ để từ chối một ứng cử viên. 

### Ví dụ 2 

đầu vào:```
8 12 1
```Các số nguyên tố trong khoảng là`{11}`. 

| Chiều dài | Windows đã được kiểm tra | Kết quả | 
| --- | --- | --- | 
| 3 | [8,10] có 0 số nguyên tố | Thất bại | 
| 4 | [8,11] có 1 số nguyên tố, [9,12] có 1 số nguyên tố | Vượt qua | 

Độ dài hợp lệ tối thiểu là`4`. 

Đầu ra:```
4
```Ví dụ này cho thấy rằng một số nguyên tố duy nhất vẫn có thể tạo ra giải pháp khả thi nếu mọi cửa sổ có kích thước đủ lớn buộc phải bao gồm nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(b log log b + (b-a+1) log(b-a+1)) | Sàng cộng với tìm kiếm nhị phân, mỗi lần kiểm tra sẽ quét tất cả các cửa sổ | 
| Không gian | O(b) | Mảng nguyên thủy và tiền tố lên đến`b`| 

Từ`b ≤ 10^6`, sàng có giá cả phải chăng một cách dễ dàng. Tìm kiếm nhị phân thực hiện khoảng`20`kiểm tra nhiều nhất và mỗi lần kiểm tra sẽ quét nhiều nhất`10^6`cửa sổ. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a, b, k = map(int, input().split())

    is_prime = [True] * (b + 1)
    if b >= 0:
        is_prime[0] = False
    if b >= 1:
        is_prime[1] = False

    p = 2
    while p * p <= b:
        if is_prime[p]:
            for x in range(p * p, b + 1, p):
                is_prime[x] = False
        p += 1

    pref = [0] * (b + 1)
    for i in range(1, b + 1):
        pref[i] = pref[i - 1] + is_prime[i]

    def check(length):
        for s in range(a, b - length + 2):
            cnt = pref[s + length - 1] - pref[s - 1]
            if cnt < k:
                return False
        return True

    n = b - a + 1

    if not check(n):
        return "-1"

    lo, hi = 1, n
    ans = n

    while lo <= hi:
        mid = (lo + hi) // 2
        if check(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    return str(ans)

# provided sample
assert run("2 4 2\n") == "3", "sample 1"

# custom cases
assert run("2 2 1\n") == "1", "single prime"
assert run("14 16 1\n") == "-1", "no primes in interval"
assert run("8 12 1\n") == "4", "single prime coverage"
assert run("2 5 1\n") == "2", "boundary windows"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1`|`1`| Khoảng phần tử đơn | 
|`14 16 1`|`-1`| Không có giải pháp tồn tại | 
|`8 12 1`|`4`| Chiều dài tối thiểu bao phủ mọi cửa sổ | 
|`2 5 1`|`2`| Xử lý từng cái một trong phạm vi cửa sổ | 

## Vỏ cạnh 

Hãy xem xét:```
14 16 1
```Khoảng không chứa số nguyên tố. Cửa sổ lớn nhất có thể là`[14,16]`, có số nguyên tố bằng 0. sơ bộ`check(n)`thất bại ngay lập tức, do đó thuật toán in`-1`. Tìm kiếm nhị phân không bao giờ bắt đầu, tránh câu trả lời sai. 

Coi như:```
2 2 1
```Chỉ có một cửa sổ,`[2]`, chứa một số nguyên tố. Việc kiểm tra sự tồn tại thành công. Tìm kiếm nhị phân hoạt động trên phạm vi`[1,1]`và trả về`1`. Điều này xác minh rằng việc triển khai xử lý các khoảng thời gian có độ dài một cách chính xác. 

Coi như:```
2 5 2
```Chiều dài`2`thất bại vì cửa sổ`[4,5]`chỉ chứa một số nguyên tố. Chiều dài`3`thất bại vì`[3,5]`chỉ chứa một số nguyên tố. Chiều dài`4`thành công vì cửa sổ duy nhất là`[2,5]`, chứa ba số nguyên tố. Thuật toán kiểm tra mọi cửa sổ và không chấp nhận sai độ dài chỉ vì một số cửa sổ hoạt động. 

Coi như:```
8 12 1
```Số nguyên tố duy nhất là`11`. Về chiều dài`3`, cửa sổ đầu tiên`[8,10]`có số nguyên tố bằng 0 nên ứng viên bị loại ngay lập tức. Về chiều dài`4`, mỗi cửa sổ bao gồm`11`, vậy là ứng viên thành công. Điều này xác nhận hành vi đơn điệu mà tìm kiếm nhị phân dựa vào.
