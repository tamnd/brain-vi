---
title: "CF 1604B - XOR Specia-LIS-t"
description: "Chúng ta được cấp một dãy số nguyên và được phép chia nó thành các mảng con liên tiếp. Đối với mỗi mảng con, chúng tôi tính toán độ dài của dãy con tăng dài nhất (LIS)."
date: "2026-06-10T08:10:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1604
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 752 (Div. 2)"
rating: 1100
weight: 1604
solve_time_s: 74
verified: true
draft: false
---

[CF 1604B - XOR Specia-LIS-t](https://codeforces.com/problemset/problem/1604/B) 

**Đánh giá:** 1100 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được phép chia nó thành các mảng con liên tiếp. Đối với mỗi mảng con, chúng tôi tính toán độ dài của dãy con tăng dài nhất (LIS). Mục đích là để xác định xem liệu chúng ta có thể phân vùng mảng sao cho XOR theo bit của tất cả các độ dài LIS này bằng 0 hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định độ dài chuỗi và chính chuỗi đó. Đầu ra là "CÓ" nếu phân vùng đó tồn tại và "KHÔNG" nếu ngược lại. 

Các ràng buộc chỉ ra rằng tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 300.000. Việc tính toán LIS cho các mảng con tùy ý theo cách mạnh mẽ là rất tốn kém, có khả năng là O(n²) trên mỗi mảng con, tốc độ này quá chậm. Chúng ta cần có cái nhìn sâu sắc để tránh tính toán LIS một cách rõ ràng cho tất cả các mảng con có thể có. 

Trường hợp cạnh khóa phát sinh khi dãy giảm nghiêm ngặt. Ở đây, mọi LIS đều có độ dài 1 và việc XOR nhiều đơn vị phụ thuộc vào số lượng là số lẻ hay số chẵn. Tương tự, một chuỗi tăng dần có LIS bằng toàn bộ chuỗi, làm cho XOR bằng độ dài. Các cách tiếp cận ngây thơ cho rằng việc phân vùng luôn có thể thất bại trên các chuỗi đơn điệu này. 

Một điểm tinh tế khác là các chuỗi có các phần tử lặp lại. Nếu mảng có các phần tử liền kề bằng nhau, LIS không thể phát triển, hạn chế các tùy chọn thao tác XOR. Ví dụ: [2, 2, 2] có LIS 1 ở mọi nơi, vì vậy XOR là 1 nếu tổng chiều dài là số lẻ, bằng 0 nếu tổng chiều dài là số chẵn. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Liệt kê tất cả các cách để chia mảng thành các mảng con liên tiếp, tính LIS cho từng mảng con và kiểm tra xem XOR của chúng có bằng 0 hay không. Mặc dù đúng nhưng cách tiếp cận này là hàm mũ theo n vì số lần phân chia là 2^(n-1). Ngay cả những chuỗi nhỏ (n=20) cũng tạo ra hơn một triệu phân vùng, vì vậy phương pháp này không khả thi. 

Thông tin chi tiết quan trọng xuất phát từ thực tế là LIS của chuỗi có độ dài lên tới 3 tối đa là 3 và LIS có thể tăng tối đa 1 khi thêm một phần tử lớn hơn đầu hiện tại. Nói cách khác, bất kỳ chuỗi nào cũng có thể được chia thành các mảng con có độ dài tối đa là 3 sao cho độ dài LIS là 1, 2 hoặc 3. Quan sát này cho phép chúng ta xây dựng các phân vùng sao cho XOR có thể đạt được 0 trừ khi toàn bộ chuỗi tăng nghiêm ngặt với độ dài lẻ. 

Từ đó, chúng ta rút ra cách tiếp cận tối ưu: nếu dãy có độ dài bằng 2 thì luôn có thể tìm được một phân tách hoặc tầm thường. Nếu phần tử đầu tiên nhỏ hơn phần tử thứ hai, XOR có thể không bằng 0 nếu chuỗi tăng nghiêm ngặt với độ dài lẻ, đây là trường hợp duy nhất xảy ra lỗi. Trong tất cả các trường hợp khác, chúng ta luôn có thể chia mảng thành các mảng con tạo ra XOR bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc độ dài chuỗi n và chuỗi a. Độ dài chuỗi ít nhất là 2. 
2. Kiểm tra xem n có bằng 2 hay không. Trong trường hợp này, XOR của hai giá trị LIS (cả 1 nếu phần tử đang giảm, 2 nếu tăng) luôn có thể bằng 0 bằng cách phân tách một cách thích hợp. Trả về "CÓ". 
3. Kiểm tra dãy số xem nó có tăng nghiêm ngặt không. Khởi tạo một cờ và lặp qua mảng so sánh từng phần tử với phần tử trước đó. 
4. Nếu chuỗi tăng nghiêm ngặt và độ dài n là số lẻ thì XOR của độ dài LIS không thể bằng 0. Trong trường hợp này, trả về "KHÔNG". 
5. Đối với tất cả các chuỗi khác, trả về "CÓ". Chúng ta luôn có thể xây dựng một phân vùng thành các mảng con có độ dài 1 hoặc 2 sao cho XOR bằng 0. Lý do là bất kỳ chuỗi nào có phần tử không tăng đều cho phép phân tách làm giảm XOR về 0.

Tại sao nó hoạt động: Điều bất biến là bất kỳ chuỗi nào không tăng nghiêm ngặt đều có thể được phân chia ở lần giảm đầu tiên, đảm bảo ít nhất một mảng con có LIS nhỏ hơn độ dài của nó. Điều này cho phép chúng ta điều chỉnh tổng XOR vì XOR với 1, 2 hoặc 3 luôn có thể đạt đến 0 bằng cách phân chia các phần tử còn lại thành các mảng con có độ dài tối đa là 2 hoặc 3. Chỉ một chuỗi tăng nghiêm ngặt với độ dài lẻ không thành công vì LIS của nó là toàn bộ độ dài và XOR một số lẻ có giá trị bằng nhau không thể tạo ra số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        # check strictly increasing
        strictly_increasing = all(a[i] > a[i-1] for i in range(1, n))
        if strictly_increasing and n % 2 == 1:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm. Đối với mỗi chuỗi, nó sẽ kiểm tra xem mảng có tăng nghiêm ngặt hay không bằng cách lặp lại một lần và so sánh các phần tử liền kề. Nếu chuỗi tăng nghiêm ngặt và có độ dài lẻ thì không thể XOR độ dài LIS về 0, vì vậy chúng tôi in "KHÔNG". Mặt khác, bất kỳ chuỗi nào khác đều cho phép phân vùng tạo ra số 0, vì vậy chúng tôi in "CÓ". Lựa chọn kiểm tra các chuỗi tăng dần với độ dài lẻ nắm bắt được trường hợp lỗi duy nhất, làm cho giải pháp vừa chính xác vừa hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1 3 4 2 2 1 5`Phân vùng`[1, 3, 4]`,`[2, 2]`,`[1, 5]`tạo ra độ dài LIS`[3, 1, 2]`. XOR:`3 ^ 1 = 2; 2 ^ 2 = 0`. Đầu ra:`YES`. 

| tôi | một [tôi] | nghiêm ngặt_tăng | hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | Đúng | tiếp tục | 
| 2 | 4 | Đúng | tiếp tục | 
| 3 | 2 | Sai | nghỉ | 

**Ví dụ 2** 

đầu vào:`1 3 4`Trình tự tăng dần, độ dài 3 (lẻ). Đầu ra:`NO`. 

| tôi | một [tôi] | nghiêm ngặt_tăng | hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | Đúng | tiếp tục | 
| 2 | 4 | Đúng | tiếp tục | 
| Kết thúc: strict_increasing=True, n=3 → print "NO" | | | | 

Những dấu vết này cho thấy thuật toán xác định chính xác trường hợp lỗi duy nhất và xử lý trường hợp chung một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần vượt qua trình tự để kiểm tra mức tăng nghiêm ngặt | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Để lưu trữ mảng chuỗi | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm tối đa là 3×10⁵, có thể chấp nhận được trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là tuyến tính theo kích thước chuỗi, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("4\n7\n1 3 4 2 2 1 5\n3\n1 3 4\n5\n1 3 2 4 2\n4\n4 3 2 1\n") == "YES\nNO\nYES\nYES", "sample 1"

# custom cases
assert run("1\n2\n1 2\n") == "YES", "minimum length increasing"
assert run("1\n2\n2 1\n") == "YES", "minimum length decreasing"
assert run("1\n5\n1 2 3 4 5\n") == "NO", "strictly increasing odd length"
assert run("1\n6\n1 2 3 4 5 6\n") == "YES", "strictly increasing even length"
assert run("1\n4\n2 2 2 2\n") == "YES", "all equal elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 2`| CÓ | tăng chiều dài tối thiểu | 
|`2\n2 1`| CÓ | chiều dài tối thiểu giảm dần | 
|`5\n1 2 3 4 5`| KHÔNG | tăng nghiêm ngặt độ dài lẻ | 
|`6\n1 2 3 4 5 6`| CÓ | tăng nghiêm ngặt chiều dài chẵn | 
|`4\n2 2 2 2`| CÓ | tất cả các phần tử bằng nhau, trường hợp cạnh cho XOR | 

## Vỏ cạnh 

Đối với dãy giảm nghiêm ngặt`[4, 3, 2, 1]`, LIS cho mỗi phần tử là 1. XOR của bốn phần tử là`1 ^ 1 ^ 1 ^ 1 = 0`. Thuật toán xác định chính xác nó không tăng hoàn toàn (vì mỗi phần tử tiếp theo nhỏ hơn) và in "CÓ". Điều này xác nhận rằng các chuỗi trong đó tất cả các phần tử bằng nhau hoặc giảm dần đều được xử lý chính xác. Để tăng nghiêm ngặt các chuỗi có độ dài lẻ, như`[1, 2, 3, 4, 5]`, XOR không thể bằng 0 vì tất cả các giá trị LIS là 5, 5^5^5^5^5 = 5 và thuật toán in ra "NO
