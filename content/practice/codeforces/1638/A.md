---
title: "CF 1638A - Ngược"
description: "Chúng ta được cho một hoán vị có độ dài $n$, là một mảng chứa tất cả các số nguyên từ $1$ đến $n$ đúng một lần. Chúng ta được phép chọn chính xác một phân đoạn liền kề của hoán vị và đảo ngược nó."
date: "2026-06-10T04:30:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1638
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 771 (Div. 2)"
rating: 800
weight: 1638
solve_time_s: 92
verified: false
draft: false
---

[CF 1638A - Đảo ngược](https://codeforces.com/problemset/problem/1638/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, là một mảng chứa tất cả các số nguyên từ$1$ĐẾN$n$đúng một lần. Chúng ta được phép chọn chính xác một phân đoạn liền kề của hoán vị và đảo ngược nó. Mục tiêu là tạo ra hoán vị nhỏ nhất về mặt từ điển có thể bằng cách sử dụng phép đảo ngược đơn lẻ này. Thứ tự từ điển có nghĩa là chúng ta so sánh từng phần tử của mảng từ trái sang phải và vị trí đầu tiên chúng khác nhau sẽ xác định mảng nào nhỏ hơn. 

Ví dụ, với hoán vị`[2,1,3]`, đảo ngược`[2,1]`sản xuất`[1,2,3]`, nhỏ hơn bản gốc. Nếu hoán vị đã được sắp xếp, việc đảo ngược bất kỳ phân đoạn nào không phải là một phần tử sẽ làm cho hoán vị trở nên tồi tệ hơn, do đó, hành động tối ưu là đảo ngược một phân đoạn có độ dài 1, khiến mảng không thay đổi một cách hiệu quả. 

Những ràng buộc chỉ ra rằng$n$có thể lên tới 500 và có thể có tới 500 trường hợp thử nghiệm. Điều này có nghĩa là một cách tiếp cận bạo lực ngây thơ, thử tất cả các phân đoạn có thể, đó là$O(n^3)$nếu chúng ta kiểm tra từng phần tử hiệu ứng của sự đảo ngược, thì sẽ quá chậm trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một chiến lược xác định phân khúc tốt nhất để đảo chiều hiệu quả. 

Các trường hợp biên không rõ ràng bao gồm các mảng đã được sắp xếp, các mảng trong đó phần tử đầu tiên đã là 1 nhưng phần còn lại chưa được sắp xếp và các mảng trong đó nhiều phần tử tối thiểu có thể xuất hiện sau đó. Ví dụ,`[1,4,2,3]`phải đảo ngược`[4,2]`đặt số 2 ngay sau số 1, cho`[1,2,4,3]`. Một cách tiếp cận bất cẩn có thể luôn cố gắng đảo ngược bắt đầu từ phần tử đầu tiên, bỏ lỡ các khả năng nhỏ hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là thử từng cặp chỉ số$(l, r)$, đảo ngược phân đoạn đó và so sánh hoán vị kết quả với kết quả tốt nhất hiện tại. Điều này sẽ liên quan đến$O(n^2)$đảo ngược, mỗi chi phí$O(n)$để xây dựng một mảng mới, đưa ra$O(n^3)$. Điều này làm việc cho nhỏ$n$nhưng quá chậm đối với$n = 500$và 500 trường hợp thử nghiệm. 

Quan sát quan trọng là để làm cho hoán vị nhỏ hơn về mặt từ điển, chúng ta nên cố gắng đặt số nhỏ nhất có thể càng sớm càng tốt. Vị trí đầu tiên mà hoán vị khác với danh tính (hoán vị được sắp xếp) là nơi chúng ta nên hành động. Khi chúng tôi tìm thấy phần tử đầu tiên không tối thiểu cho vị trí của nó, chúng tôi xác định số nhỏ nhất sau nó và đảo ngược phân đoạn từ phần tử không chính xác đầu tiên sang vị trí của số tối thiểu đó. Điều này đảm bảo sự cải thiện sớm nhất có thể về thứ tự từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại hoán vị từ trái sang phải để tìm chỉ số đầu tiên`i`Ở đâu`p[i]`không bằng`i+1`. Đây là vị trí mà hoán vị hiện tại bắt đầu khác với thứ tự đã sắp xếp. 
2. Nếu không như vậy`i`tồn tại thì hoán vị đã được sắp xếp. Đảo ngược phân đoạn một phần tử`[1,1]`để đáp ứng yêu cầu "chính xác một lần đảo ngược". 
3. Ngược lại tìm vị trí`j`của phần tử nhỏ nhất trong hậu tố bắt đầu từ`i`. Đây chính là yếu tố chúng tôi muốn đưa lên vị trí`i`. 
4. Đảo ngược phân đoạn`[i, j]`. Thao tác này sẽ di chuyển phần tử nhỏ nhất đến vị trí sớm nhất có thể và đẩy các phần tử lớn hơn theo sau nó. 
5. Xuất ra hoán vị kết quả. 

Tại sao nó hoạt động: Bằng cách đặt phần tử nhỏ nhất hiện không đúng vị trí ở chỉ mục sớm nhất nơi hoán vị lệch khỏi thứ tự được sắp xếp, chúng tôi đảm bảo rằng hoán vị kết quả là tối thiểu về mặt từ điển. Bất kỳ sự đảo ngược nào khác hoặc để lại phần tử lớn hơn ở vị trí khác nhau đầu tiên hoặc di chuyển phần tử nhỏ nhất sau đó, tạo ra hoán vị tồi tệ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        # find first index where permutation differs from sorted
        for i in range(n):
            if p[i] != i + 1:
                break
        else:
            # already sorted, reverse a single element
            print(*p)
            continue
        
        # find the smallest element in the suffix starting at i
        j = i + p[i:].index(min(p[i:]))
        
        # reverse the segment [i,j]
        p[i:j+1] = reversed(p[i:j+1])
        
        print(*p)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc`n`và hoán vị`p`. Sau đó, nó xác định phần tử đầu tiên không đúng vị trí, tìm phần tử nhỏ nhất sau nó, đảo ngược đoạn để đưa phần tử nhỏ nhất đó về phía trước và in hoán vị kết quả. Các trường hợp cạnh như mảng đã được sắp xếp được xử lý bởi`else`điều khoản về`for`vòng lặp. 

## Ví dụ đã hoạt động 

**Đầu vào:**`[3, 2, 1, 4]`| Bước | tôi | j | p sau khi đảo chiều | 
| --- | --- | --- | --- | 
| Tìm sự không khớp đầu tiên | 0 | - | - | 
| Hậu tố nhỏ nhất | 2 | 2 | - | 
| Đảo ngược`[0,2]`| 0 | 2 |`[1,2,3,4]`| 

Điều này chứng tỏ rằng việc đảo ngược phân đoạn tối thiểu sẽ tạo ra sự cải thiện về mặt từ điển sớm nhất có thể. 

**Đầu vào:**`[1, 4, 2, 3]`| Bước | tôi | j | p sau khi đảo chiều | 
| --- | --- | --- | --- | 
| Sự không phù hợp đầu tiên | 1 | - | - | 
| Hậu tố nhỏ nhất | 2 | 2 | - | 
| Đảo ngược`[1,2]`| 1 | 2 |`[1,2,4,3]`| 

Thuật toán xác định phần tử sai vị trí sớm nhất (4) và di chuyển phần tử nhỏ nhất sau nó (2) về phía trước bằng cách đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét một lần để tìm sự không khớp đầu tiên, quét một lần để tìm phần tử tối thiểu trong hậu tố, đảo ngược một lần | 
| Không gian | O(n) | Lưu trữ hoán vị | 

Cho rằng$n \le 500$Và$t \le 500$, độ phức tạp tổng thể tối đa là 500 * 500 = 250.000 thao tác, trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n1\n1\n3\n2 1 3\n4\n1 4 2 3\n5\n1 2 3 4 5\n") == "1\n1 2 3\n1 2 4 3\n1 2 3 4 5"

# Custom tests
assert run("1\n6\n6 5 4 3 2 1\n") == "1 2 3 4 5 6", "reverse full array"
assert run("1\n5\n2 3 1 5 4\n") == "1 3 2 5 4", "first element is not 1"
assert run("1\n4\n1 3 4 2\n") == "1 2 4 3", "smallest in middle"
assert run("1\n3\n1 2 3\n") == "1 2 3", "already sorted"
assert run("1\n2\n2 1\n") == "1 2", "two element swap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 5 4 3 2 1`|`1 2 3 4 5 6`| Đảo ngược toàn bộ mảng để hoán vị từ lớn nhất đến nhỏ nhất | 
|`2 3 1 5 4`|`1 3 2 5 4`| Phần tử nhỏ nhất không ở phía trước, đoạn đảo ngược sẽ di chuyển nó về phía trước | 
|`1 3 4 2`|`1 2 4 3`| Phần tử tối thiểu ở giữa hoán vị | 
|`1 2 3`|`1 2 3`| Đã sắp xếp rồi, không thay đổi | 
|`2 1`|`1 2`| Hoán đổi hai phần tử | 

## Vỏ cạnh 

Đối với hoán vị có độ dài 1 like`[1]`, thuật toán xác định rằng không có phần tử nào bị sai vị trí và chỉ xuất ra`[1]`. Đối với một hoán vị đã được sắp xếp, chẳng hạn như`[1,2,3,4,5]`, cái`for`vòng lặp kết thúc mà không bị đứt và`else`mệnh đề đảm bảo rằng chúng ta đảo ngược một đoạn có độ dài 1, giữ nguyên hoán vị. Đối với các hoán vị có số nhỏ nhất ở giữa, chẳng hạn như`[1,3,4,2]`, thuật toán xác định chính xác điểm không khớp đầu tiên (`3`tại chỉ mục 1), tìm phần tử tối thiểu trong hậu tố (`2`ở chỉ số 3), đảo ngược`[1,3]`, sản xuất`[1,2,4,3]`. Điều này xác nhận thuật toán xử lý chính xác tất cả các trường hợp cạnh không tầm thường.
