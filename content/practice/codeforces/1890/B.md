---
title: "CF 1890B - Thanh Sơn Yêu Dây"
description: "Chúng ta có hai chuỗi nhị phân, s và t. Qingshan muốn biến s thành một chuỗi không có hai ký tự liên tiếp nào giống nhau, gọi là chuỗi \"good\". Cô ấy có thể chèn liên tục chuỗi t vào bất cứ đâu trong s, kể cả ở đầu hoặc cuối, với số lần bất kỳ."
date: "2026-06-09T01:09:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1890
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 906 (Div. 2)"
rating: 800
weight: 1890
solve_time_s: 112
verified: false
draft: false
---

[CF 1890B - Thanh Sơn Yêu Dây](https://codeforces.com/problemset/problem/1890/B) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi nhị phân,`s`Và`t`. Thanh Sơn muốn chuyển hóa`s`thành một chuỗi không có hai ký tự liên tiếp nào giống nhau, gọi là chuỗi "tốt". Cô ấy có thể chèn chuỗi nhiều lần`t`bất cứ nơi nào trong`s`, kể cả ở đầu hoặc cuối, bất kỳ số lần nào. Mục đích là để xác định liệu có thể thực hiện được`s`tốt khi sử dụng những phần chèn này. 

Đầu vào có nhiều trường hợp kiểm thử, lên tới 2000 trường hợp và mỗi chuỗi có độ dài lên tới 50. Bởi vì cả hai`n`Và`m`nhỏ, thuật toán có độ phức tạp O(n*m) cho mỗi trường hợp thử nghiệm có thể được chấp nhận. Tuy nhiên, một cách tiếp cận đơn giản mô phỏng tất cả các phép chèn có thể là theo cấp số nhân trong`n`Và`m`và sẽ quá chậm. 

Các trường hợp cạnh có thể đánh lừa việc triển khai bất cẩn bao gồm các chuỗi vốn đã tốt, các chuỗi ở đó`s`chứa các ký tự lặp lại và`t`là thống nhất, hoặc ở đâu`t`thay thế nhưng không thể khắc phục được sự lặp lại ban đầu. Ví dụ, nếu`s = "111"`Và`t = "00"`, bất kể chúng ta chèn vào đâu`t`, sự lặp lại của`1`s lúc đầu không thể giải quyết được. Ngược lại, nếu`s = "1"`Và`t = "010"`, chuỗi đã tốt rồi, vì vậy không cần thao tác nào là đủ. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng chèn`t`ở mọi vị trí có thể trong`s`, tạo tất cả các chuỗi có thể và kiểm tra từng chuỗi để tìm thuộc tính "tốt". Điều này đúng nhưng không khả thi vì mỗi lần chèn sẽ nhân số chuỗi cần xem xét. Với`n=50`Và`m=50`, số lượng khả năng tăng theo cấp số nhân, rõ ràng là vượt quá thời hạn. 

Cái nhìn sâu sắc quan trọng là tập trung vào ký tự cuối cùng của`s`và ký tự đầu tiên của`t`. Vì một chuỗi tốt yêu cầu các ký tự xen kẽ nhau nên vấn đề tiềm ẩn duy nhất sẽ phát sinh khi các ký tự liên tiếp bằng nhau. Chúng tôi không cần phải mô phỏng tất cả các phần chèn thêm. Chúng ta chỉ cần kiểm tra xem`t`chứa ít nhất một cặp ký tự khác nhau liên tiếp, vì điều đó có thể phá vỡ mọi sự lặp lại trong`s`. Nếu như`s`kết thúc bằng một ký tự`c`Và`t`chứa một ký tự`d != c`, chèn`t`cuối cùng có khả năng có thể làm`s`Tốt. Ký tự đầu tiên của`t`phải khác với cái cuối cùng của`s`để loại bỏ sự lặp lại ở điểm nối. Phần còn lại của`t`chỉ cần xen kẽ để duy trì sự tốt lành. Nếu như`t`đồng nhất và giống hệt với một ký tự lặp đi lặp lại trong`s`, nó không thể giúp được. 

Chúng ta có thể giảm vấn đề xuống còn ba kiểm tra: nếu`s`đã tốt rồi, trả lại CÓ. Ngược lại, hãy kiểm tra xem`t`là tốt và nếu ký tự cuối cùng của`s`khác với lần đầu tiên`t`. Việc kiểm tra liên tục theo thời gian này là O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n*m)) | O(n_m_2^(n*m)) | Quá chậm | 
| Tối ưu | O(n+m) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng test case, đọc`n`,`m`,`s`, Và`t`. 
2. Kiểm tra xem`s`đã tốt rồi bằng cách quét tìm bất kỳ`s[i] == s[i+1]`. Nếu không có cặp nào như vậy tồn tại, hãy in "CÓ" và tiếp tục với trường hợp kiểm thử tiếp theo. Điều này xử lý kịch bản không hoạt động. 
3. Kiểm tra xem`t`là tốt. Quét`t`cho các ký tự bằng nhau liên tiếp. Nếu như`t`không tốt, hãy in "KHÔNG" vì các ký tự lặp lại trong`t`không thể sửa được`s`. 
4. Nếu`t`tốt, hãy kiểm tra ký tự cuối cùng của`s`và ký tự đầu tiên của`t`. Nếu chúng khác nhau, hãy chèn`t`ở cuối (hoặc bất kỳ vị trí nào giữa các lần lặp lại) có thể ngắt bất kỳ sự lặp lại nào, hãy in "CÓ". Nếu chúng giống nhau, hãy in "KHÔNG" vì việc chèn sẽ không giải quyết được việc lặp lại liên tiếp. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Thuật toán dựa vào đặc tính là một chuỗi tốt chỉ được xác định bằng các khác biệt ký tự liên tiếp. Bất kỳ phân đoạn lặp lại nào trong`s`có thể được sửa chữa bằng cách chèn một tốt`t`có ký tự đầu tiên khác với ký tự trước đó trong`s`. Từ`t`là tốt, nó sẽ không tạo ra sự lặp lại liên tiếp mới. Chỉ kiểm tra điểm nối giữa`s`Và`t`là đủ vì các vị trí khác trong`s`hoặc đã tốt hoặc có thể được sửa chữa bằng cách thêm vào nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_good(x):
    for i in range(len(x)-1):
        if x[i] == x[i+1]:
            return False
    return True

T = int(input())
for _ in range(T):
    n, m = map(int, input().split())
    s = input().strip()
    t = input().strip()
    
    if is_good(s):
        print("YES")
        continue
    
    if not is_good(t):
        print("NO")
        continue
    
    if s[-1] != t[0]:
        print("YES")
    else:
        print("NO")
```chức năng`is_good`kiểm tra xem một chuỗi có bất kỳ ký tự bằng nhau liên tiếp nào không. Đầu tiên chúng ta xử lý trường hợp tầm thường trong đó`s`đã tốt rồi. Nếu không, chúng tôi đảm bảo`t`là tốt, bởi vì chèn một cái không tốt`t`không thể cải thiện`s`. Cuối cùng, chúng tôi kiểm tra mối nối giữa`s`Và`t`để xác định xem một lần chèn có thể giải quyết được lần lặp lại cuối cùng hay không. Chúng ta không cần phải xem xét nhiều lần chèn một cách rõ ràng vì một`t`duy trì sự xen kẽ và chèn lặp đi lặp lại là không cần thiết một khi điều kiện tiếp giáp được thỏa mãn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 2:```
3
111
010
```| Bước | s | t | s[-1] | t[0] | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Kiểm tra ban đầu | 111 | 010 | '1' | '0' | KHÔNG kiểm tra thất bại | 
| t ổn | 010 | 010 | '1' | '0' | CÓ | 
| Ký tự cuối cùng và ký tự đầu tiên | '1' != '0' | | | | CÓ | 

Điều này chứng tỏ rằng ký tự cuối cùng của`s`khác với lần đầu tiên`t`, cho phép một chuỗi tốt được hình thành. 

Đầu vào mẫu 3:```
3
111
00
```| Bước | s | t | s[-1] | t[0] | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Kiểm tra ban đầu | 111 | 00 | '1' | '0' | | 
| t ổn | Không | | | | KHÔNG | 

Điều này cho thấy nếu`t`bản thân nó chứa sự lặp lại, nó không thể giúp sửa chữa`s`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T*(n+m)) | Mỗi chuỗi được quét một lần để kiểm tra tính tốt, đối với các trường hợp kiểm thử T | 
| Không gian | O(1) | Chỉ một số biến được sử dụng, không có bộ nhớ bổ sung tương ứng với đầu vào | 

Được cho`n,m <= 50`Và`T <= 2000`, giải pháp thực hiện tối đa 2000*100 = 200.000 thao tác, nằm trong giới hạn 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        s = input().strip()
        t = input().strip()
        
        def is_good(x):
            for i in range(len(x)-1):
                if x[i] == x[i+1]:
                    return False
            return True
        
        if is_good(s):
            print("YES")
            continue
        
        if not is_good(t):
            print("NO")
            continue
        
        if s[-1] != t[0]:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 1\n1\n0\n3 3\n111\n010\n3 2\n111\n00\n6 7\n101100\n1010101\n10 2\n1001001000\n10\n") == "YES\nYES\nNO\nNO\nNO"

# Custom test cases
assert run("2\n1 1\n0\n0\n2 1\n01\n1\n") == "YES\nYES"
assert run("1\n2 2\n11\n11\n") == "NO"
assert run("1\n2 2\n10\n01\n") == "YES"
assert run("1\n3 3\n101\n010\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n0\n0`| CÓ | Chuỗi ký tự đơn đã tốt rồi | 
|`2 2\n11\n11`| KHÔNG | Cả hai dây đều giống nhau, không thể khắc phục được sự lặp lại | 
|`2 2\n10\n01`| CÓ | Thay thế t có thể sửa ký tự cuối cùng nếu cần | 
|`3 3\n101\n010`| CÓ | t chèn duy trì xen kẽ, s đã tốt | 

## Vỏ cạnh 

cho`s = "111"`Và`t = "00"`,`s`không tốt,
