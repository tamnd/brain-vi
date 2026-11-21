---
title: "CF 2205A - Simons và Làm đẹp"
description: "Chúng ta được cho một hoán vị p có độ dài n. Chỉ số i được gọi là xấu nếu đây là lần đầu tiên nó đạt đến giá trị tối đa tại vị trí đó. Chính thức hơn, i xấu nếu p[i] bằng giá trị lớn nhất của p[1..i]."
date: "2026-06-07T19:50:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2205
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1083 (Div. 2)"
rating: 800
weight: 2205
solve_time_s: 145
verified: false
draft: false
---

[CF 2205A - Simons và làm nên vẻ đẹp](https://codeforces.com/problemset/problem/2205/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`p`chiều dài`n`. Một chỉ mục`i`được gọi là xấu nếu đây là lần đầu tiên đạt được giá trị tối đa tại vị trí đó. Chính thức hơn,`i`thật xấu xí nếu`p[i]`bằng giá trị lớn nhất của`p[1..i]`. Simons có thể thực hiện tối đa một lần hoán đổi giữa hai vị trí bất kỳ trong mảng. Nhiệm vụ của chúng ta là đưa ra một hoán vị`q`thu được từ`p`với nhiều nhất một lần hoán đổi sao cho số lượng chỉ số xấu được giảm thiểu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp`n`và mảng`p`của các số nguyên phân biệt từ`1`ĐẾN`n`. Đầu ra là một hoán vị`q`cho mỗi trường hợp thử nghiệm thỏa mãn tiêu chí độ xấu tối thiểu. 

Các ràng buộc chỉ ra`n`có thể lên tới 500 và`t`lên tới 100. Với`n`nhỏ, thậm chí là một`O(n^2)`thuật toán là khả thi, nhưng chúng ta có thể tìm ra giải pháp thời gian tuyến tính dựa trên các tính chất của hoán vị và định nghĩa của các chỉ số xấu. 

Việc triển khai đơn giản có thể thử mọi lần hoán đổi có thể và đếm các chỉ số xấu cho mỗi lần hoán đổi, nhưng điều này là không cần thiết. Các trường hợp cạnh bao gồm các hoán vị đã có thứ tự giảm dần, trong đó phần tử đầu tiên là lớn nhất hoặc các hoán vị đã tối ưu mà không có hoán đổi nào sẽ cải thiện số lượng. Ví dụ,`p = [3,1,2]`đã ở mức tối thiểu vì việc hoán đổi sẽ làm tăng độ xấu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi trường hợp thử nghiệm, lặp lại tất cả các cặp`(i,j)`với`i<j`, tráo đổi`p[i]`Và`p[j]`, đếm số lượng chỉ mục xấu và theo dõi hoán vị với số lượng tối thiểu. Mỗi lần hoán đổi yêu cầu`O(n)`để đếm độ xấu, vậy tổng độ phức tạp về thời gian là`O(n^3)`mỗi trường hợp thử nghiệm. Với`n = 500`, đây là khoảng 125 triệu thao tác cho mỗi trường hợp thử nghiệm, có thể chỉ vừa đủ trong 1 giây tùy thuộc vào hệ số không đổi, nhưng rõ ràng là dưới mức tối ưu. 

Cách tiếp cận tối ưu tận dụng thực tế là một chỉ mục sẽ xấu nếu giá trị của nó là mức tối đa đang chạy. Để giảm độ xấu, chúng tôi muốn phần tử đầu tiên phải lớn nhất vì điều đó đảm bảo không có phần tử tiếp theo nào kích hoạt mức tối đa mới cho đến khi đạt được số nhỏ hơn. Một chiến lược đơn giản là tìm phần tử lớn nhất`n`và đặt nó ở đầu mảng nếu nó chưa có ở đó. Hoán đổi phần tử đầu tiên với phần tử tối đa sẽ đạt được điều này chỉ trong một thao tác. Nếu mức tối đa đã đạt ngay từ đầu thì không cần trao đổi. Điều này đảm bảo chỉ có một chỉ số xấu khi bắt đầu, giảm thiểu sự xấu xí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và hoán vị`p`. 
3. Xác định vị trí`max_idx`của giá trị tối đa`n`trong mảng. 
4. Nếu`max_idx`không phải là vị trí đầu tiên, hoán đổi`p[0]`Và`p[max_idx]`. Điều này đảm bảo giá trị lớn nhất được đặt đầu tiên, giảm thiểu các chỉ số xấu. 
5. Xuất ra hoán vị kết quả. 

Tại sao nó hoạt động: Thuộc tính chúng tôi sử dụng là chỉ mục sẽ xấu nếu nó là lần xuất hiện đầu tiên của mức tối đa đang chạy. Bằng cách đặt phần tử lớn nhất ở vị trí đầu tiên, chúng tôi đảm bảo rằng chỉ mục xấu đầu tiên ở vị trí 1 và không có chỉ mục nào khác có thể xấu cho đến khi một số lớn hơn không tồn tại xuất hiện. Chiến lược này là tối ưu vì bất kỳ sự hoán đổi nào liên quan đến phần tử nhỏ hơn đều không thể làm giảm độ xấu ngoài việc đặt phần tử lớn nhất lên đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    max_val = n
    max_idx = p.index(max_val)
    if max_idx != 0:
        p[0], p[max_idx] = p[max_idx], p[0]
    print(" ".join(map(str, p)))
```Lời giải đầu tiên là tìm vị trí của giá trị lớn nhất trong`p`. Nếu nó không ở vị trí đầu tiên, nó sẽ hoán đổi nó với phần tử đầu tiên. Việc hoán đổi là an toàn vì nó được phép một lần cho mỗi trường hợp thử nghiệm. Chúng tôi sử dụng`p.index(n)`đó là`O(n)`, và bản thân sự hoán đổi là`O(1)`. các`print`câu lệnh đưa ra hoán vị cho từng trường hợp thử nghiệm. Không có thêm không gian ngoài mảng được sử dụng và các điều kiện biên như`n=1`được xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`p = [1, 2]`| Bước | p | giá trị tối đa | max_idx | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | [1,2] | 2 | 1 | hoán đổi vị trí 0 và 1 | 
| sau khi trao đổi | [2,1] | 2 | 0 | in [2,1] | 

Giải thích: Phần tử lớn nhất`2`được chuyển tới vị trí đầu tiên. Các chỉ số xấu được giảm thiểu xuống 1. 

**Ví dụ 2** 

đầu vào:`p = [4, 1, 3, 2, 6, 7, 8, 5]`| Bước | p | giá trị tối đa | max_idx | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | [4,1,3,2,6,7,8,5] | 8 | 6 | hoán đổi vị trí 0 và 6 | 
| sau khi trao đổi | [8,1,3,2,6,7,4,5] | 8 | 0 | in [8,1,3,2,6,7,4,5] | 

Giải thích: Phần tử lớn nhất`8`được đặt ở đầu. Các chỉ số xấu xảy ra ở vị trí 1, mức tối thiểu có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm |`p.index(n)`là O(n), hoán đổi là O(1) | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị trong bộ nhớ | 

Với`t <= 100`Và`n <= 500`, tổng số thao tác trong trường hợp xấu nhất là 50.000, dễ dàng nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        max_val = n
        max_idx = p.index(max_val)
        if max_idx != 0:
            p[0], p[max_idx] = p[max_idx], p[0]
        print(" ".join(map(str, p)))
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("5\n2\n1 2\n4\n2 3 1 4\n5\n3 2 4 5 1\n1\n1\n8\n4 1 3 2 6 7 8 5\n") == \
"2 1\n4 3 1 2\n5 2 4 3 1\n1\n8 1 3 2 6 7 4 5", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n3\n3 2 1\n") == "3 2 1", "max already first"
assert run("1\n4\n1 2 3 4\n") == "4 2 3 1", "max last"
assert run("1\n5\n2 1 5 3 4\n") == "5 1 2 3 4", "max middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | n=1 vỏ cạnh | 
| 3 2 1 | 3 2 1 | max đã có trước, không cần trao đổi | 
| 1 2 3 4 | 4 2 3 1 | hoán đổi vị trí cuối cùng tối đa | 
| 2 1 5 3 4 | 5 1 2 3 4 | tối đa ở giữa trao đổi | 

## Vỏ cạnh 

cho`n = 1`, mảng`[1]`chỉ có một phần tử và tối ưu một cách tầm thường. Thuật toán phát hiện ra rằng`max_idx = 0`, do đó không có trao đổi nào xảy ra và đầu ra là`[1]`. 

Đối với một mảng trong đó mức tối đa đã ở đầu tiên, chẳng hạn như`[3,2,1]`, thuật toán giữ nguyên hoán vị không thay đổi. Hoán đổi bất kỳ phần tử nào khác sẽ làm tăng số lượng chỉ số xấu. Thuật toán xác định chính xác rằng hoán vị ban đầu là tối ưu và không thực hiện các thao tác không cần thiết. 

Nếu mức tối đa ở vị trí cuối cùng, ví dụ:`[1,2,3,4]`, thuật toán hoán đổi`4`lên vị trí đầu tiên, đưa ra`[4,2,3,1]`. Các chỉ số xấu hiện được giảm thiểu xuống 1 ở vị trí 1, trong khi bất kỳ sự hoán đổi nào khác sẽ để lại nhiều chỉ số xấu ở các vị trí trước đó.
