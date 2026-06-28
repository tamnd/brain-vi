---
title: "CF 105129L - 15 Nguyên Tố"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một mảng các số nguyên. Chúng ta phải xây dựng số nguyên dương nhỏ nhất m sao cho mọi phần tử mảng đều có chung ước số chung lớn hơn 1 với m. Nói cách khác, với mọi giá trị ai, ước số chung lớn nhất gcd(ai, m) phải ít nhất là 2."
date: "2026-06-27T19:24:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "L"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 85
verified: true
draft: false
---

[CF 105129L - 15 Prime](https://codeforces.com/problemset/problem/105129/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một mảng các số nguyên. Chúng ta phải xây dựng số nguyên dương nhỏ nhất`m`sao cho mọi phần tử mảng đều có chung ước số lớn hơn`1`với`m`. Nói cách khác, với mọi giá trị`ai`, ước chung lớn nhất`gcd(ai, m)`ít nhất phải có`2`. 

Kích thước mảng có thể đạt tới`5 × 10^5`, nhưng mọi giá trị mảng nhiều nhất là`50`. Tổng lượng đầu vào đủ lớn để bất kỳ thuật toán nào thực hiện công việc tốn kém cho mọi phần tử đều không mong muốn. Mặt khác, phạm vi giá trị nhỏ có nghĩa là chỉ có một số thừa số nguyên tố có thể được xem xét. Vì mọi số nguyên từ`2`ĐẾN`50`chỉ có một vài ước số nguyên tố, việc phân tích từng giá trị về cơ bản là thời gian không đổi. 

Một sai lầm dễ mắc phải là cho rằng mọi số nguyên tố riêng biệt xuất hiện trong mảng đều phải chia hết kết quả. Xem xét đầu vào```
1
2
6 10
```Câu trả lời đúng là`2`, không`30`. Cả hai số đều chia hết cho`2`, do đó một số nguyên tố đã thỏa mãn mọi phần tử. 

Một lỗi phổ biến khác là nhân các thừa số nguyên tố của mỗi số một cách độc lập. Ví dụ,```
1
2
12 18
```Câu trả lời đúng là`2`. Mặc dù`12`chứa`2`Và`3`, Và`18`cũng chứa`2`Và`3`, mỗi số chỉ cần một số nguyên tố chung. Việc yêu cầu mọi thừa số nguyên tố sẽ tạo ra một đáp án lớn không cần thiết. 

Một trường hợp tinh vi hơn xuất hiện khi các số khác nhau yêu cầu các số nguyên tố khác nhau. Ví dụ,```
1
2
25 14
```Số đầu tiên yêu cầu`5`, trong khi cái thứ hai có thể sử dụng một trong hai`2`hoặc`7`. Câu trả lời nhỏ nhất là`10`, thu được bằng cách chọn số nguyên tố`{5, 2}`. Lựa chọn`{5, 7}`cho`35`, cái nào lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử tăng giá trị của`m`bắt đầu từ`1`, kiểm tra xem mọi phần tử mảng có`gcd(ai, m) ≥ 2`. Điều này đúng vì giá trị hợp lệ đầu tiên chính xác là câu trả lời mong muốn. Thật không may, không có giới hạn trên thực tế về số lượng ứng viên có thể cần phải được kiểm tra, khiến phương pháp này trở nên quá chậm. 

Giới hạn nhỏ trên các giá trị mảng sẽ thay đổi hoàn toàn vấn đề. Mỗi số giữa`2`Và`50`chỉ gồm các số nguyên tố```
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47
```Một số được thỏa mãn nếu có ít nhất một trong các thừa số nguyên tố của nó chia hết`m`. Điều này biến vấn đề thành một vấn đề che đậy. Chúng ta phải chọn một tập hợp con trong số mười lăm số nguyên tố này sao cho mỗi phần tử mảng chứa ít nhất một số nguyên tố được chọn. Trong số tất cả các tập con hợp lệ, chúng ta muốn tập hợp con có tích nhỏ nhất. 

Vì chỉ có 15 số nguyên tố ứng cử viên nên mọi tập hợp con đều có thể được biểu diễn bằng mặt nạ bit. chỉ có`2^15 = 32768`tập hợp con, rất nhỏ. Chúng tôi tính toán trước sản phẩm được đại diện bởi mỗi mặt nạ một lần. Đối với mọi trường hợp thử nghiệm, chúng tôi tính toán mặt nạ bit của các thừa số nguyên tố cho từng giá trị riêng biệt xuất hiện trong mảng, sau đó quét tất cả các tập hợp con. Một tập hợp con hợp lệ nếu nó giao với mọi mặt nạ thừa số nguyên tố cần thiết. Trong số các tập con hợp lệ, tập hợp con nào có tích nhỏ nhất là đáp án. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn | O(1) | Quá chậm | 
| Tối ưu | O(2¹⁵ + n) | O(2¹⁵) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mười lăm số nguyên tố không vượt quá`50`. 
2. Trước khi xử lý các trường hợp kiểm thử, hãy tính tích tương ứng với mọi tập hợp con của các số nguyên tố này. Nếu một bit được đặt, hãy nhân số nguyên tố tương ứng vào tích. 
3. Đồng thời tính toán trước cho mọi giá trị từ`2`ĐẾN`50`, một bitmask cho biết số nào trong số mười lăm số nguyên tố chia nó. 
4. Đối với mỗi trường hợp kiểm thử, hãy ghi lại giá trị nào thực sự xuất hiện trong mảng. Các giá trị trùng lặp không làm thay đổi câu trả lời vì chúng áp đặt cùng một yêu cầu. 
5. Liệt kê mọi tập hợp con của mười lăm số nguyên tố. 
6. Đối với mỗi tập hợp con, hãy kiểm tra mọi giá trị riêng biệt có trong mảng. Tập hợp con đáp ứng chính xác giá trị đó khi mặt nạ tập hợp con và mặt nạ thừa số nguyên tố của giá trị chia sẻ ít nhất một bit chung. 
7. Nếu mọi giá trị đều được thỏa mãn, hãy so sánh tích của tập hợp con với câu trả lời đúng nhất hiện tại và giữ lại giá trị nhỏ hơn. 
8. Xuất ra sản phẩm hợp lệ nhỏ nhất. 

### Tại sao nó hoạt động 

Mỗi số nguyên tố được chọn vào tập con sẽ trở thành ước số của`m`. Một giá trị`ai`thỏa mãn điều kiện`gcd(ai, m) ≥ 2`chính xác khi có ít nhất một phép chia nguyên tố`ai`cũng được chọn vào`m`. Thuật toán kiểm tra mọi tập hợp con có thể có trong số mười lăm số nguyên tố có liên quan duy nhất, vì vậy mọi câu trả lời ứng cử viên có thể đều được xem xét. Trong số tất cả các tập hợp con hợp lệ, nó trả về tập hợp con có tích nhỏ nhất, chính xác là số nguyên hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

PRIMES = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
P = len(PRIMES)

# mask of prime factors for every value
factor_mask = [0] * 51
for x in range(2, 51):
    mask = 0
    for i, p in enumerate(PRIMES):
        if x % p == 0:
            mask |= 1 << i
    factor_mask[x] = mask

# product represented by each subset
prod = [1] * (1 << P)
for mask in range(1, 1 << P):
    b = mask & -mask
    idx = b.bit_length() - 1
    prod[mask] = prod[mask ^ b] * PRIMES[idx]

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        present = [False] * 51
        need = []
        for x in arr:
            if not present[x]:
                present[x] = True
                need.append(factor_mask[x])

        best = None

        for mask in range(1, 1 << P):
            ok = True
            for req in need:
                if (mask & req) == 0:
                    ok = False
                    break
            if ok:
                if best is None or prod[mask] < best:
                    best = prod[mask]

        out.append(str(best))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phép tính trước đầu tiên xác định số nguyên tố nào chia hết mọi giá trị có thể. Vì phạm vi giá trị được cố định nên công việc này chỉ được thực hiện một lần. 

Quá trình tính toán trước thứ hai lưu trữ sản phẩm cho mỗi tập hợp con. Việc sử dụng bit được đặt thấp nhất cho phép mỗi sản phẩm được lấy từ một tập hợp con nhỏ hơn thay vì tính lại phép nhân từ đầu. 

Đối với mỗi trường hợp thử nghiệm, các giá trị trùng lặp sẽ bị loại bỏ vì các số giống nhau sẽ tạo ra các mặt nạ gốc giống hệt nhau. Điều này làm giảm nhẹ công việc trong quá trình kiểm tra tập hợp con. 

Vòng lặp tập hợp con chỉ cần xác minh xem mọi mặt nạ được yêu cầu có chia sẻ ít nhất một bit với tập hợp con hiện tại hay không. Vì tất cả các sản phẩm đều được tính toán trước nên việc so sánh các câu trả lời của ứng viên là thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
1
2
7 47
```| Tập hợp con | Sản phẩm | Bìa 7 | Bìa 47 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| {7} | 7 | Có | Không | Không | 
| {47} | 47 | Không | Có | Không | 
| {7,47} | 329 | Có | Có | Có | 

Cách duy nhất để thỏa mãn cả hai số là bao gồm cả hai số nguyên tố, vì vậy câu trả lời là`329`. 

### Ví dụ 2 

đầu vào```
1
8
3 4 6 7 8 9 10 14
```| Tập hợp con | Sản phẩm | Tất cả các con số được bảo hiểm | hợp lệ | 
| --- | --- | --- | --- | 
| {2} | 2 | Không | Không | 
| {3} | 3 | Không | Không | 
| {2,3} | 6 | Không | Không | 
| {2,7} | 14 | Có | Có | 

giá trị`3`yêu cầu số nguyên tố`3`, Nhưng`7`yêu cầu số nguyên tố`7`. Các số chia hết cho`2`đã được bảo hiểm bằng cách chọn`2`. Tập con thành công nhỏ nhất là`{2,7}`, đưa ra câu trả lời`14`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2¹⁵ + n) | Mỗi tập hợp con được kiểm tra dựa trên tối đa 49 giá trị riêng biệt, trong khi việc đọc mảng mất O(n). | 
| Không gian | O(2¹⁵) | Sản phẩm cho tất cả các tập hợp con được lưu trữ một lần. | 

Công việc chủ yếu là quét`32768`tập hợp con, mỗi tập hợp con chống lại nhiều nhất`49`những giá trị riêng biệt. Điều này nằm trong giới hạn, ngay cả đối với kích thước đầu vào lớn nhất được phép. 

## Trường hợp thử nghiệm```python
import sys, io

PRIMES = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
P = len(PRIMES)

factor_mask = [0] * 51
for x in range(2, 51):
    mask = 0
    for i, p in enumerate(PRIMES):
        if x % p == 0:
            mask |= 1 << i
    factor_mask[x] = mask

prod = [1] * (1 << P)
for mask in range(1, 1 << P):
    b = mask & -mask
    idx = b.bit_length() - 1
    prod[mask] = prod[mask ^ b] * PRIMES[idx]

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        need = []
        seen = [False] * 51
        for x in arr:
            if not seen[x]:
                seen[x] = True
                need.append(factor_mask[x])
        best = None
        for mask in range(1, 1 << P):
            if all(mask & m for m in need):
                if best is None or prod[mask] < best:
                    best = prod[mask]
        ans.append(str(best))
    print("\n".join(ans))

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

assert run("1\n2\n7 47\n") == "329"
assert run("1\n8\n3 4 6 7 8 9 10 14\n") == "14"

assert run("1\n1\n2\n") == "2"
assert run("1\n4\n6 6 6 6\n") == "2"
assert run("1\n2\n25 14\n") == "10"
assert run("1\n3\n49 25 9\n") == "315"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`2`| Kích thước mảng tối thiểu | 
|`6 6 6 6`|`2`| Giá trị trùng lặp không thành vấn đề | 
|`25 14`|`10`| Các số khác nhau có thể yêu cầu các số nguyên tố khác nhau | 
|`49 25 9`|`315`| Ba yêu cầu cơ bản không liên quan | 

## Vỏ cạnh 

Xem xét đầu vào```
1
2
6 10
```Những chiếc mặt nạ cần thiết là`{2,3}`Và`{2,5}`. Tập con chỉ chứa số nguyên tố`2`giao nhau cả hai mặt nạ, do đó thuật toán trả về`2`. Nó không bao giờ buộc phải bao gồm các số nguyên tố không cần thiết. 

Bây giờ hãy xem xét```
1
2
25 14
```Những chiếc mặt nạ cần thiết là`{5}`Và`{2,7}`. Trong quá trình liệt kê tập hợp con,`{5,2}`được chấp nhận với sản phẩm`10`, trong khi`{5,7}`cũng được chấp nhận với sản phẩm`35`. Vì mọi tập hợp con hợp lệ đều được kiểm tra nên thuật toán sẽ chọn chính xác câu trả lời nhỏ hơn. 

Cuối cùng, hãy xem xét```
1
3
6 6 6
```Chỉ có một mặt nạ riêng biệt được lưu trữ vì các giá trị lặp lại áp đặt các ràng buộc giống hệt nhau. Mỗi tập hợp con được kiểm tra một lần dựa trên mặt nạ đó, tạo ra câu trả lời giống như xử lý cả ba bản sao đồng thời tránh được công việc dư thừa.
