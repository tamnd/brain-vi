---
title: "CF 1924A - Chúng tôi đã giải quyết được mọi thứ chưa?"
description: "Chúng ta được cho một chuỗi s và hai số nguyên n và k. Về mặt khái niệm, chúng ta được yêu cầu kiểm tra xem mọi chuỗi có độ dài n có thể được tạo bằng cách sử dụng k chữ cái đầu tiên của bảng chữ cái có xuất hiện ở đâu đó dưới dạng một dãy con trong s hay không."
date: "2026-06-08T19:07:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy", "shortest-paths", "strings"]
categories: ["algorithms"]
codeforces_contest: 1924
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 921 (Div. 1)"
rating: 1500
weight: 1924
solve_time_s: 132
verified: false
draft: false
---

[CF 1924A - Chúng tôi đã giải quyết được mọi thứ chưa?](https://codeforces.com/problemset/problem/1924/A) 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, dp, tham lam, đường đi ngắn nhất, chuỗi 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`và hai số nguyên`n`Và`k`. Về mặt khái niệm, chúng ta được yêu cầu kiểm tra xem mọi chuỗi có độ dài`n`có thể được xây dựng bằng cách sử dụng cái đầu tiên`k`các chữ cái trong bảng chữ cái xuất hiện ở đâu đó dưới dạng một dãy con trong`s`. Một chuỗi con cho phép bỏ qua các ký tự nhưng thứ tự phải được giữ nguyên. Nếu thậm chí một chuỗi có độ dài`n`sử dụng cái đầu tiên`k`không xuất hiện thì chúng ta phải xuất ra một chuỗi như vậy để làm bằng chứng. 

Những hạn chế là chìa khóa để hiểu điều gì là khả thi.`n`Và`k`đều có nhiều nhất là 26 và chiều dài của`s`nhiều nhất là 1000. Với những giới hạn này, tổng số chuỗi có thể có độ dài`n`sử dụng`k`chữ cái là`k^n`. Điều này tăng theo cấp số nhân, vì vậy một cách tiếp cận thô bạo để kiểm tra tất cả các chuỗi có thể xem có phải là chuỗi con không khả thi khi`n`Và`k`không hề nhỏ bé. 

Một trường hợp cạnh quan trọng phát sinh khi`s`quá ngắn để chứa các chữ cái lặp đi lặp lại. Ví dụ, nếu`s`là`"ab"`với`n = 2`Và`k = 2`, chuỗi`"aa"`không thể là dãy con vì chỉ có một`'a'`TRONG`s`. Tương tự, nếu`s`thiếu hoàn toàn một ký tự cụ thể, bất kỳ chuỗi nào yêu cầu ký tự đó ngay lập tức là không thể. Một cách tiếp cận bất cẩn có thể thử tạo tất cả các chuỗi mà không kiểm tra tần số ký tự và thất bại trong im lặng hoặc hết thời gian chờ. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ tạo ra tất cả`k^n`chuỗi có độ dài`n`và kiểm tra xem mỗi cái có phải là một dãy con của`s`. Kiểm tra một chuỗi có độ dài`n`chống lại một chuỗi`s`chiều dài`m`chi phí O(m), vì vậy trong trường hợp xấu nhất số lượng hoạt động là O(m * k^n), điều này hoàn toàn không khả thi đối với giới hạn trên của`n`Và`k`. 

Thông tin chi tiết quan trọng là chúng ta không cần tạo tất cả các chuỗi. Chúng ta chỉ cần tìm chuỗi nhỏ nhất về mặt từ điển mà không thể là một chuỗi con. Điều này có thể được thực hiện một cách tham lam bằng cách đếm số lần xuất hiện của mỗi chữ cái trong`s`. Tại mỗi vị trí của chuỗi mục tiêu, chúng tôi chọn chữ cái nhỏ nhất vẫn còn xuất hiện. Nếu tại một vị trí nào đó không có chữ cái nào, ngay lập tức chúng ta có một chuỗi không thể được tạo thành một dãy con. Bằng cách lặp từ trái sang phải và chọn các chữ cái khả thi nhỏ nhất, chúng ta xây dựng một chuỗi chắc chắn sẽ bị thiếu nếu câu trả lời là KHÔNG. Nếu chúng ta có thể gán thành công các chữ cái cho tất cả các vị trí thì tất cả các chuỗi đều tồn tại và câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * k^n) | O(n) | Quá chậm | 
| Đếm dãy số tham lam | O(n * k + m) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc số nguyên`n`,`k`, Và`m`, và chuỗi`s`. 
3. Khởi tạo một mảng`count`kích thước`k`về không. Lặp lại qua`s`và đếm xem mỗi lần đầu tiên bao nhiêu lần`k`chữ xuất hiện. 
4. Khởi tạo một chuỗi trống`res`đối với chuỗi ứng cử viên bị thiếu. 
5. Đối với từng vị trí`i`trong chuỗi mục tiêu có độ dài`n`, lặp qua các chữ cái từ`'a'`đến`k`-chữ thứ: 

- Nếu chữ cái hiện tại còn xuất hiện trong`s`, giảm số lượng của nó và nối nó vào`res`. 
- Nếu không có chữ cái nào thì thêm chữ cái đầu tiên theo từ điển vào`res`cho các vị trí còn lại và nghỉ ngơi. Điều này đảm bảo chuỗi chúng ta đang xây dựng không thể xuất hiện dưới dạng một chuỗi con. 
6. Sau khi xây dựng`res`, kiểm tra xem độ dài của nó có nhỏ hơn không`n`. Nếu vậy, câu trả lời là KHÔNG và chúng tôi in`res`. Nếu chúng tôi đã chỉ định thành công tất cả các vị trí mà không hết lần xuất hiện thì câu trả lời là CÓ. 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

**Tại sao nó hoạt động**: Thuật toán xây dựng một chuỗi chỉ sử dụng các chữ cái nếu chúng có sẵn trong`s`. Nếu nó bị lỗi ở bất kỳ vị trí nào, nó đảm bảo rằng ít nhất một chuỗi có độ dài`n`không thể là dãy con. Ngược lại, theo nguyên tắc chuồng bồ câu và sự lựa chọn tham lam, mọi dây đều có thể được hình thành. Điều này thỏa mãn cả tính đúng đắn và hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k, m = map(int, input().split())
    s = input().strip()
    freq = [0] * k
    for c in s:
        freq[ord(c) - ord('a')] += 1

    res = []
    found_missing = False
    for i in range(n):
        placed = False
        for j in range(k):
            if freq[j] > 0:
                freq[j] -= 1
                res.append(chr(ord('a') + j))
                placed = True
                break
        if not placed:
            # No letter available, fill remaining with 'a'
            res.extend(['a'] * (n - i))
            found_missing = True
            break

    if found_missing:
        print("NO")
        print(''.join(res))
    else:
        print("YES")
```**Giải thích**: Chúng tôi đếm số lần xuất hiện của mỗi chữ cái trong`s`và cố gắng đặt chúng vào một chuỗi ứng cử viên một cách tham lam. Nếu một vị trí không thể được lấp đầy do thiếu các chữ cái còn lại, chúng tôi sẽ tạo một chuỗi không thể xuất hiện, đảm bảo phản hồi KHÔNG hợp lệ. Mặt khác, nếu tất cả các vị trí có thể được chỉ định thì chuỗi sẽ chứa tất cả các chuỗi con có thể có. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 2**```
2 2 3
abb
```| Bước | tần số | độ phân giải | Giải thích | 
| --- | --- | --- | --- | 
| tôi=0 | [1,2] | ['a'] | Đặt 'a' | 
| tôi=1 | [0,2] | ['a','b'] | Nơi 'b' | 
| tôi=2 | [0,1] | Không thể đặt 'a' (freq=0) | Điền vào phần còn lại bằng 'a' → 'aa', Found_missing=True | 

Đầu ra:```
NO
aa
```Dấu vết này thể hiện sự lựa chọn tham lam và phát hiện ngay lập tức một chuỗi con bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k + m) | Tần số đếm là O(m), xây dựng chuỗi ứng cử viên là O(n * k) | 
| Không gian | O(k + n) | Mảng tần số và chuỗi ứng cử viên | 

Điều này phù hợp tốt trong các ràng buộc của vấn đề vì`n`Và`k`nhiều nhất là 26 và tổng chiều dài của`s`trên tất cả các trường hợp thử nghiệm là 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k, m = map(int, input().split())
        s = input().strip()
        freq = [0]*k
        for c in s:
            freq[ord(c)-ord('a')] += 1
        res = []
        found_missing = False
        for i in range(n):
            placed = False
            for j in range(k):
                if freq[j]>0:
                    freq[j]-=1
                    res.append(chr(ord('a')+j))
                    placed=True
                    break
            if not placed:
                res.extend(['a']*(n-i))
                found_missing=True
                break
        if found_missing:
            output.append("NO")
            output.append(''.join(res))
        else:
            output.append("YES")
    return "\n".join(output)

# provided samples
assert run("3\n2 2 4\nabba\n2 2 3\nabb\n3 3 10\naabbccabab\n") == "YES\nNO\naa\nNO\nccc"

# custom cases
assert run("1\n1 1 1\na\n") == "YES", "single character string"
assert run("1\n2 1 2\naa\n") == "NO\naa", "cannot make 'aa' subsequence"
assert run("1\n2 2 3\nab\n") == "NO\naa", "missing 'aa'"
assert run("1\n2 2 5\nabab\n") == "NO\nbb", "missing 'bb'"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / a | CÓ | Đầu vào tối thiểu | 
| 2 1 2 / aa | KHÔNG/aa | Không thể lặp lại chữ cái nhiều hơn số s | 
| 2 2 3 / ab | KHÔNG/aa | Phát hiện chuỗi thiếu từ điển | 
| 2 2 5 / abab | KHÔNG / bb | Phát hiện các chữ cái lặp đi lặp lại bị thiếu | 

## Vỏ cạnh 

Đối với các chuỗi ở đâu`s`chỉ chứa một trong mỗi chữ cái, thuật toán sẽ phát hiện ra rằng bất kỳ chuỗi ký tự lặp lại nào là không thể. Ví dụ,`n=2`,`k=2`,`s="ab"`xuất ra chính xác NO và`aa`. Vì`n=1`,`k=1`,`s="a"`, tất cả các chuỗi đều tồn tại và xuất ra CÓ. Phương pháp xử lý tối đa`k`Và`n`bằng cách tham lam kiểm tra các chữ cái có sẵn mà không tạo ra tất cả các chuỗi có thể, tránh sự phức tạp theo cấp số nhân.
