---
title: "CF 1619A - Dây vuông?"
description: "Chúng ta có một số chuỗi và chúng ta cần xác định xem đó có phải là chuỗi vuông hay không. Một chuỗi là chuỗi vuông nếu nó có thể được biểu diễn dưới dạng một chuỗi con nào đó được nối với chính nó."
date: "2026-06-10T06:09:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1619
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 762 (Div. 3)"
rating: 800
weight: 1619
solve_time_s: 99
verified: false
draft: false
---

[CF 1619A - Chuỗi vuông?](https://codeforces.com/problemset/problem/1619/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chuỗi và chúng ta cần xác định xem đó có phải là chuỗi vuông hay không. Một chuỗi là chuỗi vuông nếu nó có thể được biểu diễn dưới dạng một chuỗi con nào đó được nối với chính nó. Ví dụ: "abab" là hình vuông vì nó được "ab" lặp lại hai lần, trong khi "abcab" không phải là hình vuông vì không có chuỗi con nào được lặp lại hai lần sẽ tạo ra chuỗi gốc. 

Đầu vào bao gồm một số nguyên`t`theo sau là`t`dây. Mỗi chuỗi có độ dài từ 1 đến 100. Đầu ra chỉ đơn giản là "CÓ" nếu chuỗi là hình vuông, nếu không thì là "KHÔNG". 

Các ràng buộc rất nhỏ: tối đa 100 chuỗi có độ dài lên tới 100. Điều này cho phép chúng tôi sử dụng các thuật toán bậc hai về độ dài chuỗi, nhưng lý tưởng nhất là chúng tôi hướng tới kiểm tra tuyến tính hoặc gần tuyến tính trên mỗi chuỗi. 

Các trường hợp cạnh cần chú ý bao gồm các chuỗi có độ dài lẻ. Ví dụ: "aaa" có độ dài 3, không thể chia đều thành hai nửa bằng nhau. Bất kỳ chuỗi có độ dài lẻ nào cũng tự động không phải là chuỗi vuông. Một trường hợp tinh vi khác là một chuỗi như "aa" hoặc "aaaa", là những chuỗi hoàn toàn vuông, nhưng việc tìm kiếm chuỗi con đơn giản có thể làm cho giải pháp trở nên phức tạp hơn nếu không cẩn thận. Một chuỗi có độ dài 1, như "a", không bao giờ là chuỗi vuông. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét tất cả các chuỗi con có độ dài có thể`n/2`và kiểm tra xem việc lặp lại chúng có tạo ra chuỗi gốc hay không. Đối với một chuỗi có độ dài`n`, chỉ có một chuỗi con ứng viên có độ dài`n/2`(nửa đầu). Kiểm tra ngây thơ sẽ cắt chuỗi thành nửa đầu và nửa sau và so sánh chúng. Mỗi so sánh mất O(n/2) thời gian. Vì có tới 100 chuỗi có độ dài lên tới 100 nên cách tiếp cận này có thể chấp nhận được, nhưng chúng ta có thể suy luận thêm. 

Quan sát quan trọng là một dây là hình vuông khi và chỉ khi độ dài của nó là số chẵn và nửa đầu bằng nửa sau. Quan sát này ngay lập tức biến vấn đề của chúng ta thành một phép kiểm tra đơn giản: nếu độ dài là số lẻ, xuất ra "NO"; nếu chiều dài bằng nhau thì so sánh trực tiếp hai nửa. Đây là tuyến tính trên mỗi chuỗi và chỉ sử dụng thêm không gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra lặp lại chuỗi con Brute Force | O(n) mỗi chuỗi | O(n) | Đã chấp nhận | 
| So sánh một nửa | O(n) mỗi chuỗi | O(1) | Đã chấp nhận | 

Ở đây phương pháp chia nửa là tối ưu và dễ thực hiện. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Lặp lại từng đầu vào chuỗi. 
3. Với mỗi chuỗi, hãy tính độ dài của nó`n`. 
4. Nếu`n`là số lẻ, in ngay "NO" vì chuỗi có độ dài lẻ không thể là hình vuông. 
5. Nếu`n`chẵn, chia chuỗi thành hai nửa: nửa đầu chứa các ký tự`s[0:n//2]`, nửa sau`s[n//2:n]`. 
6. So sánh hai nửa. Nếu chúng bằng nhau thì in ra "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: cách duy nhất để một chuỗi có thể là hình vuông là nếu nó được tạo thành từ hai nửa giống hệt nhau. Trước tiên, việc kiểm tra độ dài sẽ đảm bảo chúng ta không cố gắng phân tách một chuỗi có độ dài lẻ và việc so sánh trực tiếp các nửa sẽ đảm bảo tính chính xác vì bất kỳ sự không khớp nào ở các nửa sẽ ngay lập tức làm mất hiệu lực thuộc tính hình vuông. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    n = len(s)
    if n % 2 == 1:
        print("NO")
    else:
        mid = n // 2
        if s[:mid] == s[mid:]:
            print("YES")
        else:
            print("NO")
```Giải pháp này đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc loại bỏ dòng mới là quan trọng để đảm bảo độ dài chuỗi chính xác. Trước tiên, chúng tôi kiểm tra tính chẵn lẻ của độ dài để nắm bắt các trường hợp "KHÔNG" rõ ràng và sau đó so sánh trực tiếp hai nửa. 

## Ví dụ đã hoạt động 

**Ví dụ 1: "abab"** 

| Biến | Giá trị | 
| --- | --- | 
| s | "abab" | 
| n | 4 | 
| giữa | 2 | 
| s[:giữa] | "ab" | 
| s[giữa:] | "ab" | 
| so sánh | bằng → CÓ | 

Thuật toán xác định chính xác "abab" là hình vuông. 

**Ví dụ 2: "abcab"** 

| Biến | Giá trị | 
| --- | --- | 
| s | "abcab" | 
| n | 5 | 
| chẵn lẻ | lẻ → KHÔNG | 

Kiểm tra độ dài lẻ ngay lập tức tạo ra "KHÔNG", xử lý chính xác trường hợp cạnh này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi so sánh chuỗi là O(n), với tổng số t chuỗi | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ các lát chuỗi và số nguyên để lập chỉ mục | 

Với`t ≤ 100`Và`n ≤ 100`, tổng tác phẩm tối đa là 10.000 so sánh ký tự, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)
        if n % 2 == 1:
            print("NO")
        else:
            mid = n // 2
            if s[:mid] == s[mid:]:
                print("YES")
            else:
                print("NO")
    return output.getvalue().strip()

# provided sample
assert run("10\na\naa\naaa\naaaa\nabab\nabcabc\nabacaba\nxxyy\nxyyx\nxyxy\n") == \
"NO\nYES\nNO\nYES\nYES\nYES\nNO\nNO\nNO\nYES"

# custom cases
assert run("3\nb\ncc\ndd\n") == "NO\nYES\nYES", "single and double letters"
assert run("2\nabcdabcd\nabcabc\n") == "YES\nYES", "multiple repeated substrings"
assert run("1\na"*100+"\n") == "YES", "max size even repeated char"
assert run("1\na"*99+"\n") == "NO", "max size odd repeated char"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| b, cc, đ | KHÔNG, CÓ, CÓ | độ chính xác của cạnh một chữ cái và chữ cái kép | 
| abcdabcd, abcdabcd | CÓ, CÓ | lặp lại nhiều ký tự | 
| 'a'*100 | CÓ | kích thước tối đa thậm chí lặp đi lặp lại char | 
| 'a'*99 | KHÔNG | char lặp lại kích thước tối đa lẻ | 

## Vỏ cạnh 

Đối với chuỗi có độ dài 1 như "a", thuật toán sẽ phát hiện độ dài lẻ và đưa ra kết quả là "NO". Đối với chuỗi có độ dài 2 như "aa", nó chia thành "a" và "a", tìm chúng bằng nhau và xuất ra "CÓ". Đối với các chuỗi như "xxyy", nó chia thành "xx" và "yy", phát hiện sự bất đẳng thức và xuất ra "KHÔNG". Thuật toán xử lý chính xác tất cả các lần lặp lại tối thiểu, tối đa và không đồng nhất.
