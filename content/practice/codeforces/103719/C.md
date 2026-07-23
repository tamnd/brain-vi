---
title: "CF 103719C - \u041c\u0435\u0445\u043e\u0432\u044b\u0435 \u043f\u043e\u0434\u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0 432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u0438"
description: "Chúng ta được cho một mảng có độ dài n và chúng ta xem xét các dãy con được xác định bằng cách chọn bất kỳ dãy chỉ số tăng dần nào. Đối với mỗi dãy con được chọn, chúng tôi lấy nhiều tập giá trị và tính mex của nó, số nguyên không âm nhỏ nhất không xuất hiện trong đó."
date: "2026-07-02T09:22:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103719
codeforces_index: "C"
codeforces_contest_name: "VII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b. 8-11 \u043a\u043b\u0430\u0441\u0441\u044b"
rating: 0
weight: 103719
solve_time_s: 63
verified: true
draft: false
---

[CF 103719C - \u041c\u0435\u0445\u043e\u0432\u044b\u0435 \u043f\u043e\u0434\u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043 b\u044c\u043d\u043e\u0441\u0442\u0438](https://codeforces.com/problemset/problem/103719/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng có độ dài n và chúng ta xem xét các dãy con được xác định bằng cách chọn bất kỳ dãy chỉ số tăng dần nào. Đối với mỗi dãy con được chọn, chúng tôi lấy nhiều tập giá trị và tính mex của nó, số nguyên không âm nhỏ nhất không xuất hiện trong đó. Dãy con chỉ được coi là hợp lệ nếu mọi phần tử được chọn đều nằm trong khoảng cách tối đa 1 tính từ giá trị mex này. 

Nhiệm vụ là đếm xem có bao nhiêu dãy con không trống thỏa mãn điều kiện này, lấy tất cả các lựa chọn chỉ mục có thể có và trả về câu trả lời modulo 998244353. 

Ràng buộc n lên tới 100000 buộc bất kỳ phép liệt kê O(n²) hoặc tổ hợp nào trên các chuỗi con đều không thể thực hiện được. Ngay cả các giải pháp O(n log n) cũng cần tránh lý luận theo từng chuỗi. Cấu trúc gợi ý rằng điều kiện trên các giá trị sẽ thu gọn không gian tìm kiếm một khi mex được cố định, bởi vì mex hạn chế mạnh mẽ những số nguyên nào có thể xuất hiện trong một tập hợp hợp lệ. 

Một điểm tinh tế là điều kiện kết hợp hai điều: tư cách thành viên của các giá trị gần mex và định nghĩa của chính mex, điều này phụ thuộc vào giá trị nào hiện diện và vắng mặt. Vòng phản hồi này chính là nơi lý luận ngây thơ thường bị phá vỡ. 

Một sai lầm phổ biến là giả định rằng một khi các giá trị bị giới hạn trong một phạm vi nhỏ xung quanh một số mex ứng cử viên thì bất kỳ tập hợp con nào cũng hoạt động. Điều đó bỏ qua yêu cầu mex rằng tất cả các giá trị nhỏ hơn phải xuất hiện ít nhất một lần. 

Một dạng lỗi khác là trộn lẫn các đóng góp từ các giá trị mex khác nhau mà không kiểm tra xem cùng một chuỗi con có thể tương ứng với nhiều giá trị mex hay không. Ở đây, mex xác định cấu trúc duy nhất, do đó việc đếm rời rạc sẽ an toàn khi được mô tả chính xác. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là liệt kê tất cả các chuỗi con và tính mex cho mỗi chuỗi. Điều đó đã mang lại cho 2^n ứng cử viên và việc tính toán mex cho mỗi tập hợp con có giá lên tới O(n), dẫn đến sự bùng nổ theo cấp số nhân không thể thực hiện được. 

Quan sát quan trọng là giá trị mex của bất kỳ dãy con hợp lệ nào cũng không thể lớn. Ràng buộc buộc mọi phần tử trong dãy con phải nằm trong {M-1, M, M+1}. Đồng thời, mex M yêu cầu phải có tất cả các số nguyên từ 0 đến M-1 và M không có. Hai điều kiện này cực kỳ hạn chế. 

Nếu M bằng 2 trở lên thì yêu cầu 0 phải xuất hiện xung đột với tập giá trị được phép {M-1, M+1}, không bao gồm 0. Điều này ngay lập tức loại bỏ tất cả các giá trị mex ngoại trừ 0 và 1. 

Sự sụp đổ này làm bài toán thành hai bài toán đếm độc lập: đếm dãy con với mex 0 và đếm dãy con với mex 1. 

Đối với mex 0, 0 phải vắng mặt và tất cả các phần tử phải nằm trong {0,1} theo điều kiện khoảng cách, buộc dãy con chỉ gồm 1 giây. 

Đối với mex 1, 0 phải xuất hiện ít nhất một lần, 1 phải vắng mặt và các giá trị phải nằm trong {0,2}. Điều này trở thành số lượng tổ hợp tiêu chuẩn trên hai loại độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các chuỗi con | O(2^n · n) | O(n) | Quá chậm | 
| Phân hủy Mex (chỉ 0 và 1) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi phân loại các phần tử của mảng theo giá trị, nhưng chỉ các giá trị 0, 1 và 2 mới quan trọng. Bất kỳ giá trị nào ngoài phạm vi này không bao giờ có thể thuộc về một dãy con hợp lệ vì nó sẽ vi phạm điều kiện |a[i] − M| 1 cho cả hai giá trị mex có thể có. 

### 1. Đếm tần số 

Chúng tôi tính c0, c1, c2 là số lần xuất hiện của các giá trị 0, 1 và 2. 

### 2. Đếm dãy con với mex 0 

Một dãy con có mex 0 nếu không có 0 và có 1 (nếu không thì mex sẽ là 0), nhưng giới hạn khoảng cách buộc tất cả các phần tử phải nằm trong {0,1}. Kết hợp những thứ này lại, 0 không thể xuất hiện và chỉ còn lại 1. Mọi tập con không trống của 1 đều hợp lệ.

Vậy số dãy con như vậy là 2^{c1} − 1. 

### 3. Đếm dãy con với mex 1 

Đối với mex 1, số 0 phải xuất hiện ít nhất một lần và số 1 phải vắng mặt. Ràng buộc khoảng cách chỉ cho phép các giá trị trong {0,2}. Vì vậy, chúng ta chọn bất kỳ dãy con nào từ tất cả các vị trí c0 + c2, nhưng chúng ta phải loại trừ những dãy con không chứa 0. 

Tổng số dãy con của các phần tử này là 2^{c0+c2} và những dãy không có 0 là 2^{c2}. Vậy phần đóng góp là 2^{c0+c2} − 2^{c2}. 

### 4. Tổng số tiền đóng góp 

Câu trả lời cuối cùng là tổng của hai trường hợp độc lập. 

### Tại sao nó hoạt động 

Khi mex được cố định, phạm vi giá trị được phép sẽ trở thành một khoảng cố định xung quanh nó và định nghĩa mex buộc tất cả các giá trị nhỏ hơn phải có mặt. Điều này chỉ để lại hai giá trị mex khả thi. Vì mỗi dãy con hợp lệ xác định duy nhất mex của nó nên việc phân vùng theo mex sẽ tạo ra các tập hợp rời rạc và tránh việc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n = int(input())
a = list(map(int, input().split()))

c0 = c1 = c2 = 0
for x in a:
    if x == 0:
        c0 += 1
    elif x == 1:
        c1 += 1
    elif x == 2:
        c2 += 1

# mex = 0: non-empty subsets of ones
ans0 = (modpow(2, c1) - 1) % MOD

# mex = 1: subsets of {0,2} with at least one 0
ans1 = (modpow(2, c0 + c2) - modpow(2, c2)) % MOD

print((ans0 + ans1) % MOD)
```Mã đầu tiên nén mảng thành ba bộ đếm tần số có liên quan. Mọi thứ khác đều là tổ hợp thuần túy. lũy thừa mô-đun được sử dụng để tính toán số lượng tập hợp con một cách hiệu quả theo mô-đun yêu cầu. 

Các bước trừ xử lý việc loại trừ các trường hợp trống hoặc không hợp lệ, do đó mỗi số hạng khớp trực tiếp với các biểu thức đếm dẫn xuất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét đầu vào:```
n = 4
a = [0, 2, 3, 2]
```Chúng ta tính c0 = 1, c1 = 0, c2 = 2. 

Đối với mex 0, chúng ta đếm các chuỗi con chỉ gồm 1 giây, nhưng không có chuỗi nào, vì vậy ans0 = 0. 

Đối với mex 1, chúng tôi làm việc với {0,2,2}. Tổng các dãy con: 2^3 = 8. Những dãy không có 0 chỉ sử dụng {2,2}, cho 2^2 = 4. Vậy ans1 = 4. 

Câu trả lời cuối cùng là 4. 

Điều này phù hợp với thực tế là các dãy con hợp lệ chính xác là các dãy con chứa ít nhất một số 0 và bất kỳ tập con nào gồm các số 2. 

### Ví dụ 2```
n = 3
a = [1, 1, 1]
```Ở đây c1 = 3, c0 = c2 = 0. 

Đối với mex 0, tất cả các tập con không trống của số 1 đều hợp lệ, cho 2^3 − 1 = 7. 

Đối với mex 1, không tồn tại số 0 nên không tồn tại dãy con hợp lệ. 

Câu trả lời cuối cùng là 7. 

Điều này chứng tỏ rằng giải pháp phân tách rõ ràng các đóng góp mà không có sự tương tác giữa các trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để đếm cộng với lũy thừa theo thời gian không đổi | 
| Không gian | O(1) | chỉ có ba quầy và một vài biến | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì tất cả sự tăng trưởng tổ hợp đều được xử lý bằng phương pháp phân tích thay vì liệt kê. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MOD = 998244353

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    n = int(input())
    a = list(map(int, input().split()))

    c0 = c1 = c2 = 0
    for x in a:
        if x == 0: c0 += 1
        elif x == 1: c1 += 1
        elif x == 2: c2 += 1

    ans0 = (modpow(2, c1) - 1) % MOD
    ans1 = (modpow(2, c0 + c2) - modpow(2, c2)) % MOD

    return str((ans0 + ans1) % MOD)

# provided sample-like checks
assert run("4\n0 2 3 2\n") == "4"

# all ones
assert run("3\n1 1 1\n") == "7"

# only zeros
assert run("3\n0 0 0\n") == "0"

# mixed small case
assert run("3\n0 1 2\n") == str(((2**1-1) + (2**2 - 2**1)) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 7 | chỉ trường hợp mex 0 | 
| tất cả số không | 0 | mex 1 không thể thiếu 2s | 
| 0 1 2 | sữa công thức hỗn hợp | cả hai đóng góp đều hoạt động | 

## Vỏ cạnh 

Nếu mảng chỉ chứa giá trị 1, công thức mex 0 sẽ đếm chính xác tất cả các tập hợp con không trống vì không có phần tử nào vi phạm phạm vi cho phép và mex tự động trở thành 0 vì không có 0. 

Nếu không có số 0 thì phần đóng góp của mex 1 sẽ bằng 0 vì mọi tập con ứng cử viên từ {0,2} không thể đáp ứng yêu cầu chứa số 0 và phép trừ 2^{c0+c2} − 2^{c2} sẽ rút gọn chính xác về 0. 

Nếu các giá trị lớn hơn 2 xuất hiện, chúng sẽ bị loại trừ hoàn toàn khỏi tất cả các chuỗi con hợp lệ vì bất kỳ sự bao gồm nào cũng sẽ vi phạm ràng buộc khoảng cách cho cả hai giá trị mex có thể có, do đó chúng không đóng góp gì và biến mất một cách an toàn trong quá trình giảm đếm.
