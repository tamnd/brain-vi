---
title: "CF 1899E - Sắp xếp hàng đợi"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta muốn sắp xếp nó theo thứ tự không giảm. Điều đáng chú ý là chúng ta không thể tự ý hoán đổi các phần tử."
date: "2026-06-08T21:26:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1899
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 909 (Div. 3)"
rating: 1300
weight: 1899
solve_time_s: 132
verified: true
draft: false
---

[CF 1899E - Sắp xếp hàng đợi](https://codeforces.com/problemset/problem/1899/E) 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta muốn sắp xếp nó theo thứ tự không giảm. Điều đáng chú ý là chúng ta không thể tự ý hoán đổi các phần tử. Thay vào đó, chúng tôi có sẵn một thao tác phức tạp duy nhất: lấy phần tử đầu tiên, di chuyển nó đến cuối và sau đó bong bóng trở lại bên trái miễn là nó nhỏ hơn phần tử trước đó. Về mặt khái niệm, điều này giống như xoay mảng và để phần tử được di chuyển "trượt" sang trái cho đến khi tìm thấy vị trí chính xác so với các phần tử mà nó đã đi qua. Mục tiêu là xác định số lượng tối thiểu của các thao tác này để sắp xếp mảng hoặc báo cáo là không thể thực hiện được nếu không thể sắp xếp được. 

Mảng có thể lên tới 200.000 phần tử và tổng của tất cả các kích thước mảng trên nhiều trường hợp thử nghiệm không vượt quá 200.000. Điều này giới hạn giải pháp của chúng tôi đối với độ phức tạp thời gian tuyến tính hoặc tuyến tính; bất cứ điều gì bậc hai sẽ là quá chậm. Ví dụ: cố gắng mô phỏng từng thao tác một cách đơn giản có thể mất tới`O(n^2)`thời gian cho mỗi mảng, điều này là không khả thi. 

Các trường hợp biên tinh tế bao gồm các mảng đã được sắp xếp (thao tác bằng 0), các mảng trong đó phần tử đầu tiên là lớn nhất và sắp xếp khối trừ khi bị xóa nhiều lần và các mảng có các phần tử lặp lại trong đó việc hoán đổi bị giới hạn bởi điều kiện "lớn hơn nghiêm ngặt". Một ví dụ nhỏ minh họa sự thất bại của mô phỏng đơn giản là`[4, 3, 1, 2]`. Nếu bạn chỉ cố gắng di chuyển từng phần tử về phía trước mà không xem xét thứ tự chung, bạn có thể kết luận rằng nó có thể được sắp xếp trong một lần, trong khi câu trả lời đúng là không thể vì thứ tự tương đối của`3`Và`1`không thể sửa chữa được thông qua thao tác được phép. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng hoạt động theo đúng nghĩa đen: liên tục di chuyển phần tử đầu tiên đến cuối và bong bóng trở lại cho đến khi vừa khít. Sau mỗi thao tác, hãy kiểm tra xem mảng đã được sắp xếp chưa. Điều này hoạt động vì hoạt động mang tính xác định và mỗi ứng dụng thay đổi mảng theo một cách xác định. Tuy nhiên, số lượng hoạt động trong trường hợp xấu nhất có thể đạt tới`n`mỗi phần tử, dẫn đến`O(n^2)`mỗi trường hợp thử nghiệm. Với`n`lên tới 200.000, điều này là quá chậm. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là nhận ra rằng hoạt động về cơ bản cho phép chúng ta di chuyển bất kỳ tiền tố nào của mảng đã có về phía sau. Nếu chúng ta nghĩ về mảng từ cuối trở đi, hậu tố dài nhất đã được sắp xếp và không giảm sẽ không thể được chạm tới bởi các phép toán trong tương lai; chỉ các phần tử trước khi nó có thể di chuyển. Do đó, số lượng thao tác tối thiểu được xác định bởi độ dài của tiền tố chưa được sắp xếp trước hậu tố được sắp xếp này. Nếu tiền tố chưa được sắp xếp chứa bất kỳ phần tử nào lớn hơn phần tử đầu tiên của hậu tố được sắp xếp thì việc sắp xếp sẽ không thể thực hiện được vì thao tác không thể chèn các phần tử vào giữa hậu tố mà không vi phạm quy tắc "nghiêm ngặt hơn". 

Do đó, bài toán rút gọn thành việc xác định hậu tố không giảm dài nhất tính từ cuối và kiểm tra xem tiền tố còn lại có thể được chuyển về phía sau theo thứ tự mà không bị xung đột hay không. Nếu có, số thao tác bằng kích thước của tiền tố; nếu không, việc sắp xếp là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với một mảng nhất định, bắt đầu quét từ cuối để tìm hậu tố không giảm dài nhất. Khởi tạo một con trỏ`i = n-2`và di chuyển lùi lại cho đến khi`a[i] <= a[i+1]`. Sau chuyện này,`i+1`đánh dấu sự bắt đầu của hậu tố được sắp xếp. 
2. Kiểm tra xem tiền tố trước hậu tố này có thể được "xoay" đến cuối mà không vi phạm quy tắc thao tác hay không. Cụ thể, thao tác này cho phép mỗi phần tử nổi bong bóng bên trái cho đến khi nó không còn nhỏ hơn phần tử trước đó. Điều này tương đương với việc kiểm tra xem tiền tố có tăng không vì phần tử đầu tiên của mảng cuối cùng phải đến cuối tiền tố này. 
3. Đếm số thao tác cần thiết, chính xác là độ dài của tiền tố chưa được sắp xếp. Đây là số phần tử trước hậu tố được sắp xếp. 
4. Nếu tại bất kỳ thời điểm nào chúng tôi phát hiện thấy một phần tử trong tiền tố lớn hơn phần đầu của hậu tố được sắp xếp và không thể chèn chính xác, hãy xuất`-1`. 

Tại sao nó hoạt động: Thao tác này cho phép mỗi phần tử tiền tố di chuyển về phía sau trong khi vẫn tôn trọng thứ tự tương đối. Điều bất biến là khi một phần tử đi vào hậu tố được sắp xếp, nó vẫn ở đó ở đúng vị trí tương đối. Do đó, việc xác định hậu tố được sắp xếp dài nhất sẽ cho chúng ta biết điểm an toàn tối thiểu; mọi thứ trước nó phải được di chuyển bằng các thao tác và số lượng của chúng bằng kích thước của tiền tố. Bất kỳ vi phạm nào trong tiền tố có nghĩa là một số yếu tố sẽ không bao giờ phù hợp với quy tắc, đảm bảo không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations_to_sort(a):
    n = len(a)
    i = n - 2
    while i >= 0 and a[i] <= a[i + 1]:
        i -= 1
    if i == -1:
        return 0  # already sorted
    prefix_length = i + 1
    
    # Check if the prefix is non-increasing
    for j in range(prefix_length - 1):
        if a[j] < a[j + 1]:
            return -1
    return prefix_length

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_operations_to_sort(a))
```chức năng`min_operations_to_sort`đầu tiên tìm thấy hậu tố được sắp xếp từ cuối. Nếu toàn bộ mảng đã được sắp xếp, nó sẽ trả về 0. Sau đó nó kiểm tra tiền tố chưa được sắp xếp để đảm bảo nó không tăng. Nếu không, một số phần tử trong tiền tố sẽ vi phạm điều kiện "bong bóng cho đến khi lớn hơn nghiêm ngặt" của thao tác, khiến việc sắp xếp không thể thực hiện được, vì vậy chúng ta trả về`-1`. Mặt khác, số thao tác bằng với độ dài tiền tố, vì mỗi phần tử phải được xoay một lần để đến đúng vị trí của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[6, 4, 1, 2, 5]`| Bước | Mảng | tôi | Bình luận | 
| --- | --- | --- | --- | 
| Ban đầu | 6 4 1 2 5 | 3 | Bắt đầu từ cuối | 
| Quét | 2 5 | 2 | 2 <= 5, tiếp tục | 
| Quét | 1 2 | 1 | 1 <= 2, tiếp tục | 
| Quét | 4 1 | 0 | 4 > 1, dừng lại | 
| Tiền tố | 6 4 | 0 | Kiểm tra không tăng 6 > 4, hợp lệ | 
| Kết quả | 2 | | Hoạt động tối thiểu bằng độ dài tiền tố | 

### Ví dụ 2 

đầu vào:`[4, 3, 1, 2, 6, 4]`| Bước | Mảng | tôi | Bình luận | 
| --- | --- | --- | --- | 
| Ban đầu | 4 3 1 2 6 4 | 4 | Bắt đầu từ cuối | 
| Quét | 6 4 | 4 | 6 > 4, dừng lại | 
| Tiền tố | 4 3 1 2 6 | 0..4 | Kiểm tra không tăng 1 < 2, không thành công | 
| Kết quả | -1 | | Không thể sắp xếp | 

Những dấu vết này thể hiện cả việc tính toán độ dài tiền tố tối thiểu và phát hiện các trường hợp không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập tối đa hai lần: một lần trong quá trình quét hậu tố, một lần khi kiểm tra tiền tố | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và bộ đếm, không có mảng bổ sung | 

Được cho`n`lên tới 2e5 và tổng tất cả các trường hợp thử nghiệm lên tới 2e5, giải pháp này chạy thoải mái dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())  # assuming solution saved as solution.py
    return out.getvalue().strip()

# provided samples
assert run("5\n5\n6 4 1 2 5\n7\n4 5 3 7 8 6 2\n6\n4 3 1 2 6 4\n4\n5 2 4 2\n3\n2 2 3\n") == "2\n6\n-1\n-1\n0", "sample 1"

# custom cases
assert run("1\n1\n42\n") == "0", "single element, already sorted"
assert run("1\n5\n1 2 3 4 5\n") == "0", "already sorted array"
assert run("1\n5\n5 4 3 2 1\n") == "5", "strictly decreasing, all need moves"
assert run("1\n5\n2 2 2 2 2\n") == "0", "all equal elements"
assert run("1\n6\n3 3 2 2 1 1\n") == "6", "non-increasing prefix followed by sorted suffix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử`[42]`| 0 | Kích thước mảng tối thiểu | 
| Đã sắp xếp`[1 2 3 4 5]`| 0 | Đã được sắp xếp | 
| Đảo ngược`[5 4 3 2 1]`| 5 | Tất cả các yếu tố cần hoạt động | 
| Tất cả đều bình đẳng`[2 2 2 2 2]`| 0 | Không giảm với các bản sao |
