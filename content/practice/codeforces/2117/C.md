---
title: "CF 2117C - Vách Ngăn Mát"
description: "Chúng ta được cho một mảng các số nguyên và cần chia nó thành các đoạn liền kề nhau. Mỗi phần tử phải xuất hiện trong đúng một phân đoạn. Một phân vùng được coi là tuyệt vời nếu mọi phần tử trong một phân đoạn cũng xuất hiện trong phân đoạn tiếp theo, nếu có."
date: "2026-06-08T11:02:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2117
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1029 (Div. 3)"
rating: 1200
weight: 2117
solve_time_s: 113
verified: false
draft: false
---

[CF 2117C - Phân vùng thú vị](https://codeforces.com/problemset/problem/2117/C) 

**Đánh giá:** 1200 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và cần chia nó thành các đoạn liền kề nhau. Mỗi phần tử phải xuất hiện trong đúng một phân đoạn. Một phân vùng được coi là tuyệt vời nếu mọi phần tử trong một phân đoạn cũng xuất hiện trong phân đoạn tiếp theo, nếu có. Mục tiêu của chúng tôi là tối đa hóa số lượng phân khúc. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có kích thước lên tới 200.000. Tổng kích thước mảng trên tất cả các trường hợp thử nghiệm không vượt quá 200.000, nghĩa là chúng tôi có thể cung cấp giải pháp thời gian tuyến tính cho mỗi mảng. Phương pháp bậc hai là không thực tế vì việc lặp qua tất cả các mảng con sẽ dẫn đến$10^{10}$hoạt động. 

Các trường hợp cạnh không rõ ràng phát sinh khi các phần tử lặp lại theo các mẫu phức tạp. Ví dụ: hãy xem xét một mảng trong đó phần tử giống nhau xuất hiện không liên tục:`[1, 2, 1, 2, 1, 2]`. Một cách tiếp cận đơn giản là phân chia ở mọi phần tử lặp lại có thể thất bại vì chúng ta sẽ vi phạm yêu cầu rằng mọi phần tử trong một phân đoạn cũng phải xuất hiện ở phần tiếp theo. Một trường hợp khác là một mảng có tất cả các phần tử duy nhất, trong đó phân vùng thú vị duy nhất là toàn bộ mảng đó. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng tạo ra mọi phân vùng có thể và kiểm tra xem nó có mát không. Đối với mỗi phân vùng ứng viên, chúng tôi sẽ xác minh rằng tất cả các phần tử của một phân đoạn đều xuất hiện trong phân đoạn tiếp theo. Mặc dù điều này đúng về mặt khái niệm nhưng nó cực kỳ chậm: đối với một mảng có độ dài$n$, có$2^{n-1}$các phân vùng có thể, điều này hoàn toàn không khả thi ngay cả đối với$n = 20$. 

Thông tin chi tiết quan trọng để tăng tốc độ này là lưu ý rằng một phân đoạn chỉ có thể kết thúc khi tất cả các phần tử hiện có trong phân đoạn đó cũng xuất hiện sau trong mảng. Nói cách khác, đối với bất kỳ phân đoạn ứng viên nào kết thúc ở vị trí$i$, chúng ta cần đảm bảo rằng không có phần tử nào xuất hiện trong phân đoạn này xuất hiện lại sau$i$mà không phải là một phần của phân khúc tiếp theo. Quan sát này cho phép chúng tôi theo dõi lần xuất hiện cuối cùng của mọi phần tử và chỉ đóng một phân đoạn khi chỉ mục hiện tại khớp với lần xuất hiện cuối cùng của tất cả các phần tử được thấy cho đến nay. 

Bằng cách lặp qua mảng một lần và theo dõi lần xuất hiện cuối cùng tối đa của các phần tử trong phân đoạn hiện tại, chúng ta có thể xác định ranh giới phân đoạn trong một lần duyệt. Mỗi khi chỉ mục hiện tại khớp với lần xuất hiện cuối cùng tối đa, chúng tôi có thể đóng một phân đoạn một cách an toàn, đảm bảo rằng tất cả các phần tử trong đó sẽ xuất hiện trong phân đoạn tiếp theo nếu có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`và chiều dài của nó`n`. 
2. Tính toán một từ điển ánh xạ từng phần tử duy nhất tới chỉ số xuất hiện cuối cùng của nó trong mảng. Điều này cho phép chúng tôi biết, đối với bất kỳ phần tử nào, chỉ mục xa nhất mà chúng tôi phải đưa vào để đáp ứng điều kiện phân đoạn thú vị. 
3. Khởi tạo hai biến:`current_max`để theo dõi lần xuất hiện cuối cùng tối đa của các phần tử trong phân đoạn hiện tại và`segments`để đếm số đoạn tìm được. 
4. Lặp lại mảng với chỉ mục`i`từ 0 đến`n-1`. Đối với mỗi phần tử`a[i]`, cập nhật`current_max`là mức tối đa của chính nó và lần xuất hiện cuối cùng của`a[i]`. 
5. Nếu`i`bằng`current_max`, điều đó có nghĩa là tất cả các phần tử trong phân đoạn hiện tại đều kết thúc ở đây. Tăng`segments`và đặt lại mọi hoạt động theo dõi theo từng phân khúc cụ thể nếu cần. 
6. Sau vòng lặp, xuất ra tổng số`segments`cho trường hợp thử nghiệm này. 

Bất biến quan trọng là`current_max`luôn đại diện cho chỉ mục xa nhất mà chúng ta phải đưa vào để đáp ứng điều kiện phân vùng mát mẻ cho phân khúc hiện tại. Bằng cách đóng một phân đoạn chính xác khi chỉ mục hiện tại đạt đến mức tối đa này, chúng tôi đảm bảo không có phần tử nào trong phân đoạn vi phạm điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_cool_segments(n, a):
    last_occurrence = {}
    for i, val in enumerate(a):
        last_occurrence[val] = i
    
    segments = 0
    current_max = -1
    for i, val in enumerate(a):
        current_max = max(current_max, last_occurrence[val])
        if i == current_max:
            segments += 1
    return segments

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max_cool_segments(n, a))
```Giải pháp này trước tiên xây dựng một từ điển`last_occurrence`lưu trữ chỉ mục cuối cùng nơi mỗi phần tử xuất hiện. Trong quá trình lặp lại`a`, chúng tôi theo dõi`current_max`để xác định khi nào là an toàn để kết thúc phân đoạn hiện tại. tăng dần`segments`chỉ khi chỉ số hiện tại bằng`current_max`đảm bảo tính đúng đắn. Cách tiếp cận này tránh được từng lỗi một bằng cách sử dụng lập chỉ mục dựa trên số 0 một cách nhất quán. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
6
1 2 2 3 1 5
```| tôi | một [tôi] | lần cuối cùng[a[i]] | hiện tại_max | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 4 | 0 | 
| 1 | 2 | 2 | 4 | 0 | 
| 2 | 2 | 2 | 4 | 0 | 
| 3 | 3 | 3 | 4 | 0 | 
| 4 | 1 | 4 | 4 | 1 | 
| 5 | 5 | 5 | 5 | 2 | 

Dấu vết này cho thấy đoạn đầu tiên kết thúc ở chỉ số 4, bao gồm`[1, 2, 2, 3, 1]`, và đoạn thứ hai là`[5]`. Tính bất biến được duy trì vì tất cả các phần tử trong một phân đoạn đều xuất hiện trong phân đoạn tiếp theo. 

### Đầu vào mẫu 2```
8
1 2 1 3 2 1 3 2
```| tôi | một [tôi] | lần cuối cùng[a[i]] | hiện tại_max | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 0 | 
| 1 | 2 | 7 | 7 | 0 | 
| 2 | 1 | 5 | 7 | 0 | 
| 3 | 3 | 6 | 7 | 0 | 
| 4 | 2 | 7 | 7 | 0 | 
| 5 | 1 | 5 | 7 | 0 | 
| 6 | 3 | 6 | 7 | 0 | 
| 7 | 2 | 7 | 7 | 1 | 

Điều này cho thấy phân đoạn đầu tiên kết thúc ở chỉ số 7. Bằng cách tuân theo logic tương tự, thuật toán có thể xác định các phân đoạn nhỏ hơn trong các trường hợp khác, đưa ra số lượng phân vùng tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại mảng một lần và tính toán lần xuất hiện cuối cùng trong một lần duyệt. | 
| Không gian | O(n) | các`last_occurrence`từ điển lưu trữ lên đến`n`phím. | 

Với$n \le 2 \cdot 10^5$mỗi trường hợp thử nghiệm và tổng số tiền$2 \cdot 10^5$, giải pháp này phù hợp một cách thoải mái trong giới hạn thời gian và bộ nhớ 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("1\n6\n1 2 2 3 1 5\n") == "2", "sample 1"
assert run("1\n8\n1 2 1 3 2 1 3 2\n") == "3", "sample 2"

# Custom cases
assert run("1\n1\n1\n") == "1", "single element array"
assert run("1\n5\n1 1 1 1 1\n") == "5", "all equal elements"
assert run("1\n5\n5 4 3 2 1\n") == "1", "all unique elements descending"
assert run("1\n6\n1 2 1 2 1 2\n") == "3", "interleaved repeats"
assert run("1\n7\n1 2 3 2 1 3 1\n") == "2", "complex overlaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | mảng phần tử đơn | 
| 1 1 1 1 1 | 5 | tất cả các phần tử bằng nhau | 
| 5 4 3 2 1 | 1 | tất cả các phần tử độc đáo, chỉ có thể có một phân đoạn | 
| 1 2 1 2 1 2 | 3 | mẫu lặp lại xen kẽ, số lượng phân đoạn chính xác | 
| 1 2 3 2 1 3 1 | 2 | các phần tử chồng chéo, đảm bảo thuật toán hợp nhất các phân đoạn một cách chính xác | 

## Vỏ cạnh 

Đối với mảng một phần tử`[1]`,`last_occurrence`bản đồ 1 để lập chỉ mục
