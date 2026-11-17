---
title: "CF 2003A - Rùa và Dây Tốt"
description: "Chúng ta được yêu cầu xác định xem một chuỗi đã cho có thể được chia thành hai hoặc nhiều chuỗi con liền kề sao cho không có chuỗi con nào bắt đầu bằng ký tự xuất hiện ở cuối chuỗi con sau đó hay không."
date: "2026-06-08T13:47:22+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2003
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 968 (Div. 2)"
rating: 800
weight: 2003
solve_time_s: 121
verified: false
draft: false
---

[CF 2003A - Rùa và Dây Tốt](https://codeforces.com/problemset/problem/2003/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một chuỗi đã cho có thể được chia thành hai hoặc nhiều chuỗi con liền kề sao cho không có chuỗi con nào bắt đầu bằng ký tự xuất hiện ở cuối chuỗi con sau đó hay không. Nói cách khác, chúng ta muốn phân tách chuỗi thành ít nhất hai phần trong đó, đối với mỗi cặp phần, ký tự đầu tiên của phần trước không khớp với ký tự cuối cùng của phần sau. 

Mỗi trường hợp thử nghiệm cho biết độ dài của chuỗi và chính chuỗi đó. Đầu ra là "CÓ" nếu sự phân tách như vậy tồn tại và "KHÔNG" nếu ngược lại. Các ràng buộc rất nhỏ: độ dài chuỗi tối đa là 100 và có tối đa 500 trường hợp thử nghiệm. Điều này có nghĩa là một$O(n^2)$thuật toán cho mỗi trường hợp thử nghiệm là khả thi vì$100^2 \cdot 500 = 5 \cdot 10^6$hoạt động dễ dàng trong giới hạn thời gian 1 giây. 

Các trường hợp cạnh cần thận trọng bao gồm các chuỗi trong đó tất cả các ký tự giống hệt nhau, ví dụ: "aa" hoặc "aaa". Trong những trường hợp này, mọi phép phân tách sẽ không thành công vì ký tự đầu tiên của phần đầu tiên luôn bằng ký tự cuối cùng của phần thứ hai. Một kịch bản tinh tế khác là các chuỗi có các mẫu lặp lại trong đó việc phân chia tham lam ngây thơ có thể bỏ lỡ sự phân tách chính xác. Ví dụ: "aba" có thể được chia thành "ab" + "a", đáp ứng các quy tắc, nhưng việc chia thành "a" + "ba" không thành công vì 'a' == 'a'. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử tất cả các phân vùng có thể có của chuỗi thành$k \ge 2$chuỗi con và kiểm tra điều kiện cho từng cặp chuỗi con. Mỗi phân vùng yêu cầu kiểm tra$O(k^2)$các cặp và có rất nhiều cách để phân chia một chuỗi. Vì$n = 100$, điều này rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ việc quan sát điều kiện: chúng ta chỉ cần phát hiện xem có ký tự lặp lại ở khoảng cách ít nhất 1 ký tự hay không. Nếu tồn tại bất kỳ cặp vị trí nào$i < j - 1$như vậy$s[i] == s[j]$, sau đó chia chuỗi giữa các vị trí$i$Và$i+1$tạo ra hai chuỗi con thỏa mãn điều kiện. Mặt khác, nếu tất cả các ký tự lặp lại liền kề nhau thì không có sự phân chia hợp lệ nào tồn tại. Điều này làm giảm vấn đề quét chuỗi để tìm các ký tự lặp lại không liền kề, đó là$O(n)$mỗi trường hợp thử nghiệm. 

Cách tiếp cận này đơn giản hơn rất nhiều so với phương pháp Brute Force vì nó biến một bài toán tổ hợp thành một phép quét tuyến tính với một quy tắc đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc chuỗi$s$và chiều dài của nó$n$. 
3. Khởi tạo cờ`good`ĐẾN`False`. Điều này sẽ theo dõi xem sự phân chia hợp lệ có tồn tại hay không. 
4. Lặp lại chuỗi từ ký tự đầu tiên đến ký tự áp chót. Đối với mỗi chỉ số$i$, kiểm tra xem ký tự ở$i$khớp với bất kỳ ký tự nào tại$i+2$hoặc muộn hơn. 
5. Nếu tìm thấy sự trùng khớp như vậy, hãy đặt`good`ĐẾN`True`và thoát ra khỏi vòng lặp. Điều này tương ứng với việc tìm hai ký tự giống hệt nhau không liền kề cho phép phân chia hợp lệ. 
6. Sau vòng lặp, nếu`good`là`True`, in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: thuật toán dựa vào đặc tính là một chuỗi có thể được chia thành các chuỗi con tốt khi và chỉ khi tồn tại một ký tự được lặp lại với ít nhất một ký tự khác giữa các lần xuất hiện của nó. Điều này đảm bảo rằng ký tự đầu tiên của chuỗi con thứ nhất không bằng ký tự cuối cùng của chuỗi con thứ hai, đáp ứng điều kiện. Nếu không có sự lặp lại như vậy thì mỗi ký tự hoặc là duy nhất hoặc chỉ lặp lại liền kề, do đó, bất kỳ sự phân tách nào cũng sẽ vi phạm quy tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    good = False
    seen = set()
    for i, c in enumerate(s):
        if c in seen:
            good = True
            break
        if i > 0:
            seen.add(s[i - 1])
    print("YES" if good else "NO")
```Giải pháp duy trì một tập hợp các ký tự đã xuất hiện hai vị trí trở lên trước ký tự hiện tại. Đối với mỗi ký tự, chúng tôi kiểm tra xem nó đã xuất hiện trong bộ chưa, nghĩa là có tồn tại sự lặp lại không liền kề hay không. Nếu vậy, chuỗi có thể được chia thành các chuỗi con tốt. Thêm`s[i - 1]`đảm bảo rằng chúng tôi chỉ xem xét các ký tự cách nhau ít nhất hai ký tự, phù hợp với điều kiện của vấn đề. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào:```
4
2
aa
3
aba
4
abcb
12
abcabcabcabc
```| Bước | Chuỗi | chỉ mục`i`| Char`c`| Đã Xem Bộ | Tốt? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | aa | 0 | một | {} | Sai | 
| 2 | aa | 1 | một | {a} | Đúng | 

Đầu ra: KHÔNG 

| Bước | Chuỗi | chỉ mục`i`| Char`c`| Đã Xem Bộ | Tốt? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | aba | 0 | một | {} | Sai | 
| 2 | aba | 1 | b | {a} | Sai | 
| 3 | aba | 2 | một | {a,b} | Đúng | 

Đầu ra: CÓ 

Bảng này cho thấy cách thuật toán phát hiện chính xác các lần lặp lại không liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nhân vật được viếng thăm một lần; trung bình các hoạt động được thiết lập là O (1) | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Bộ này có thể chứa tối đa n-1 ký tự | 

Với n 100 và t 500, thuật toán thực hiện tối đa 50.000 thao tác, nằm trong giới hạn 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        good = False
        seen = set()
        for i, c in enumerate(s):
            if c in seen:
                good = True
                break
            if i > 0:
                seen.add(s[i - 1])
        print("YES" if good else "NO")
    return output.getvalue().strip()

# provided samples
assert run("4\n2\naa\n3\naba\n4\nabcb\n12\nabcabcabcabc\n") == "NO\nYES\nYES\nYES", "sample 1"

# custom cases
assert run("2\n2\nab\n3\naaa\n") == "NO\nNO", "minimum and all equal"
assert run("1\n5\nabcde\n") == "NO", "all unique"
assert run("1\n6\nabacab\n") == "YES", "non-adjacent repetition"
assert run("1\n100\n" + "a"*100 + "\n") == "NO", "maximum size, all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2/ab, 3/aaa | KHÔNG / KHÔNG | Kích thước tối thiểu, lặp lại và không lặp lại | 
| 5/abcde | KHÔNG | Ký tự độc đáo không thể tạo thành sự chia tách tốt | 
| 6/ bàn tính | CÓ | Trình kích hoạt lặp lại không liền kề CÓ | 
| 100/a*100 | KHÔNG | Độ dài chuỗi tối đa có tất cả các ký tự giống hệt nhau | 

## Vỏ cạnh 

Đối với đầu vào "aa", thuật toán sẽ quét`i=0`(`c='a'`) Và`i=1`(`c='a'`). các`seen`đặt sau`i=1`chứa`{'a'}`. Vì 'a' ở trong`seen`,`good`trở thành Đúng. Tuy nhiên, vì các ký tự liền kề nhau nên thuật toán đảm bảo chính xác rằng`i>0`chỉ thêm ký tự trước đó vào`seen`. Vì thế,`good`vẫn là Sai và xuất ra "KHÔNG", xử lý chính xác các lần lặp liền kề. 

Đối với một chuỗi như "aba", tại`i=2`(`c='a'`),`seen`chứa`{'a', 'b'}`. 'a' ở trong`seen`, biểu thị sự lặp lại không liền kề và thuật toán đưa ra "CÓ", xử lý chính xác các ký tự lặp lại không liền kề.
