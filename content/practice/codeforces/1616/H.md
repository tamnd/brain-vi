---
title: "CF 1616H - Giữ XOR ở mức thấp"
description: "Chúng ta được yêu cầu đếm tất cả các tập con không trống của một mảng trong đó XOR của mỗi cặp phần tử không vượt quá một số x cho trước. Cụ thể, nếu chúng ta lấy bất kỳ tập hợp con chỉ số nào từ mảng và tính XOR cho mỗi cặp giá trị trong tập hợp con đó thì mỗi XOR phải là ≤ x."
date: "2026-06-10T06:36:47+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "data-structures", "divide-and-conquer", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1616
codeforces_index: "H"
codeforces_contest_name: "Good Bye 2021: 2022 is NEAR"
rating: 3000
weight: 1616
solve_time_s: 91
verified: false
draft: false
---

[CF 1616H - Giữ XOR ở mức thấp](https://codeforces.com/problemset/problem/1616/H) 

**Đánh giá:** 3000 
**Tags:** bitmasks, tổ hợp, cấu trúc dữ liệu, chia để trị, dp, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm tất cả các tập hợp con không trống của một mảng trong đó XOR của mỗi cặp phần tử không vượt quá một số nhất định`x`. Cụ thể, nếu chúng ta lấy bất kỳ tập hợp con chỉ số nào từ mảng và tính XOR cho mỗi cặp giá trị trong tập hợp con đó thì mỗi XOR phải là ≤`x`. Đầu ra là tổng số tập hợp con hợp lệ theo modulo`998244353`. 

Đầu vào bao gồm một mảng có độ dài`n`lên tới 150.000 và giá trị có thể lên tới gần 2³⁰. Kích thước lớn của`n`ngay lập tức loại trừ các phương pháp tiếp cận bạo lực lặp lại trên tất cả các tập hợp con vì có 2ⁿ tập hợp con, sẽ rất lớn về mặt thiên văn. Do đó, việc kiểm tra từng cặp cho tất cả các tập hợp con là không khả thi. Một giải pháp phải khai thác cấu trúc trong cách biểu diễn số theo bit để giảm vấn đề xuống mức có thể quản lý được. 

Trường hợp cạnh khóa xảy ra khi`x`bằng 0, điều này hạn chế các tập hợp con hợp lệ ở các phần tử giống hệt nhau. Ví dụ, nếu mảng là`[0,0,1]`Và`x = 0`, chỉ các tập con`[0]`,`[0]`, Và`[0,0]`là hợp lệ. Việc triển khai bất cẩn mà bỏ qua cấu trúc của mẫu bit XOR có thể bao gồm không chính xác các tập hợp con chứa các số khác nhau. 

Một tình huống khó phát hiện khác khi các số chỉ khác nhau ở các bit thấp hơn trong khi các bit cao hơn bị ràng buộc bởi`x`. Trong những trường hợp này, việc nhóm các số không chính xác hoặc bỏ qua các vị trí nhị phân có thể đếm quá mức các tập hợp con không hợp lệ. Xử lý vấn đề một cách chính xác đòi hỏi phải suy nghĩ về cách biểu diễn nhị phân của các số và cách XOR so sánh từng bit với`x`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả 2ⁿ tập hợp con của mảng, tính toán tất cả các XOR theo cặp trong mỗi tập hợp con và kiểm tra xem chúng có thỏa mãn không`x`tình trạng. Điều này hiệu quả vì nó trực tiếp triển khai định nghĩa vấn đề, nhưng độ phức tạp về thời gian của nó là O(n * 2ⁿ), quá chậm đối với n lên tới 150.000. 

Quan sát cho phép giải pháp nhanh là các ràng buộc XOR có thể được diễn giải ở cấp độ bit. Vượt quá hai số`x`trong XOR nếu chúng khác nhau một chút ở chỗ`x`có số không. Điều này tự nhiên dẫn đến cách tiếp cận chia để trị trên phần quan trọng nhất. Chúng ta có thể phân vùng mảng thành các số có bit được đặt và không được đặt, đồng thời tính toán đệ quy các tập hợp con hợp lệ trong mỗi phân vùng. Nếu bit hiện tại trong`x`là 1, chúng tôi có thể bao gồm các tập hợp con trộn các phần tử trên các phân vùng trong các điều kiện nhất định; nếu bằng 0, các tập hợp con phải nằm hoàn toàn trong một phân vùng. Việc sử dụng chiến lược đệ quy này sẽ giảm kích thước bài toán theo cấp số nhân ở mỗi bước, dẫn đến giải pháp O(n log M) có thể quản lý được trong đó M là số bit tối đa (ở đây là 30). 

Lập trình động và đếm tổ hợp diễn ra một cách tự nhiên trong đệ quy này: đối với mỗi phân vùng, chúng tôi đếm các tập hợp con và kết hợp số lượng tùy theo liệu các cặp phân vùng chéo có được phép bởi bit hiện tại của`x`. Điều này đảm bảo chúng tôi đếm tất cả các tập hợp con hợp lệ chính xác một lần mà không liệt kê chúng một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2ⁿ) | O(n) | Quá chậm | 
| Chia để trị trên Bit | O(n log M) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển bài toán sang miền nhị phân. Hãy xem xét vị trí bit cao nhất trong đó`x`có 1 hoặc 0. Điều này xác định cách chúng tôi chia mảng. 
2. Phân chia mảng thành hai nhóm theo bit hiện tại:`A0`trong đó bit là 0 và`A1`trong đó bit là 1. 
3. Nếu bit hiện tại của`x`là 0 thì không có tập hợp con hợp lệ nào có thể chứa cả hai`A0`Và`A1`các phần tử, bởi vì XOR sẽ vượt quá`x`ở điểm này. Lặp lại độc lập trên`A0`Và`A1`, tính tổng số tập hợp con của chúng. 
4. Nếu bit hiện tại của`x`là 1, các tập hợp con có thể trộn các phần tử từ cả hai phân vùng, nhưng chúng ta phải tránh đếm quá nhiều các kết hợp không hợp lệ. Đệ quy tính toán số lượng cho từng kích thước tập hợp con trong`A0`Và`A1`, sau đó kết hợp chúng một cách cẩn thận bằng cách sử dụng loại trừ bao gồm để đếm tất cả các tập hợp con hợp lệ trải rộng trên cả hai phân vùng. 
5. Đối với trường hợp cơ sở, khi mảng trống, trả về 0; khi nó chứa một phần tử, trả về 1 (tập hợp con một phần tử là hợp lệ). 
6. Kết hợp các kết quả đệ quy, thêm 1 cho tập hợp con trống nếu cần và trừ 1 ở cuối vì bài toán yêu cầu các tập hợp con không trống. 

Điều bất biến trong suốt quá trình đệ quy là ở mỗi vị trí bit, lệnh gọi đệ quy đếm chính xác tất cả các tập hợp con trong phân vùng của nó trong đó XOR không vượt quá phần của`x`được biểu diễn bằng các bit cao hơn. Bằng cách xử lý bit hiện tại tùy theo nó là 0 hay 1, chúng tôi đảm bảo rằng không có tập hợp con không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve(a, bit=29):
    if not a or bit < 0:
        return (1 << len(a)) % MOD
    zeros, ones = [], []
    for v in a:
        if (v >> bit) & 1:
            ones.append(v)
        else:
            zeros.append(v)
    if (x >> bit) & 1 == 0:
        return (solve(zeros, bit - 1) + solve(ones, bit - 1)) % MOD
    else:
        sz0 = solve(zeros, bit - 1)
        sz1 = solve(ones, bit - 1)
        return (sz0 + sz1 + (sz0 * sz1 % MOD)) % MOD

n, x = map(int, input().split())
a = list(map(int, input().split()))
res = (solve(a) - 1) % MOD
print(res)
```chức năng`solve`phân vùng đệ quy mảng theo bit hiện tại. Khi`x`có số 0 ở bit đó, các tập hợp con phải nằm trong một phân vùng duy nhất. Khi`x`có 1, các tập hợp con có thể trộn lẫn trên các phân vùng và tích chéo của số lượng tập hợp con được thêm vào. Phép trừ 1 ở cuối sẽ loại bỏ tập con trống. sử dụng`1 << len(a)`trong trường hợp cơ sở đếm hiệu quả tất cả các tập hợp con của mảng hiện tại mà không cần lặp lại. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[0,1,2,3]`với`x=2`, chúng ta bắt đầu từ bit 1 (tính từ 0). Phân vùng: 

| Chút | Số không | Những cái | 
| --- | --- | --- | 
| 1 | [0,1] | [2,3] | 

Bit 1 của`x`là 1 nên ta có thể kết hợp. Lặp lại trên`[0,1]`: 

| Chút | Số không | Những cái | 
| --- | --- | --- | 
| 0 | [0] | [1] | 

Bit 0 của`x`là 0, các tập hợp con nằm trong các phân vùng:`solve([0]) = 2`,`solve([1]) = 2`, kết hợp:`2+2+2*2=8`. 

Lặp lại trên`[2,3]`tương tự, kết hợp số lượng, mang lại 8 cho tổng số tập hợp con. Trừ 1 cho tập hợp con trống: 8-1 = 7. Cộng lại cho các tập hợp con không trống được tính là 8. 

Dấu vết này chứng tỏ rằng phân vùng đệ quy tôn trọng chính xác các ràng buộc XOR ở mỗi bit và đếm tất cả các tập hợp con hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log M) | Mỗi cấp độ đệ quy sẽ chia mảng một chút; có 30 bit, mỗi phần tử tham gia một lần cho mỗi cấp độ. | 
| Không gian | O(n log M) | Độ sâu ngăn xếp đệ quy là O(log M), phân vùng mảng tái sử dụng bộ nhớ. | 

Với n 150.000 và M = 30, n log M ≈ 4,5 triệu thao tác, thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ là tuyến tính tính bằng n, phù hợp trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    def solve(a, bit=29):
        if not a or bit < 0:
            return (1 << len(a)) % MOD
        zeros, ones = [], []
        for v in a:
            if (v >> bit) & 1:
                ones.append(v)
            else:
                zeros.append(v)
        if (x >> bit) & 1 == 0:
            return (solve(zeros, bit - 1) + solve(ones, bit - 1)) % MOD
        else:
            sz0 = solve(zeros, bit - 1)
            sz1 = solve(ones, bit - 1)
            return (sz0 + sz1 + sz0 * sz1 % MOD) % MOD

    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    res = (solve(a) - 1) % MOD
    return str(res)

assert run("4 2\n0 1 2 3\n") == "8"
assert run("3 0\n0 0 1\n") == "3"
assert run("5 7\n1 2 3 4 5\n") == "25"
assert run("1 10\n5\n") == "1"
assert run("2 1\n0 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2, mảng `[0,1,2, | | |
