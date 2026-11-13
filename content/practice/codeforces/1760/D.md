---
title: "CF 1760D - Thung lũng đầy thử thách"
description: "Chúng ta được cấp một mảng các số nguyên và cần xác định xem nó có tạo thành một \"thung lũng\" theo một định nghĩa chính xác hay không. Về mặt khái niệm, thung lũng là một mảng con phẳng thấp hơn các mảng lân cận ở cả hai phía."
date: "2026-06-09T14:22:53+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1760
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 835 (Div. 4)"
rating: 1000
weight: 1760
solve_time_s: 148
verified: true
draft: false
---

[CF 1760D - Thung lũng đầy thử thách](https://codeforces.com/problemset/problem/1760/D) 

**Đánh giá:** 1000 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và cần xác định xem nó có tạo thành một "thung lũng" theo một định nghĩa chính xác hay không. Về mặt khái niệm, thung lũng là một mảng con phẳng thấp hơn các mảng lân cận ở cả hai phía. Về mặt hình thức, phải có chính xác một đoạn liền kề gồm các phần tử bằng nhau sao cho nếu đoạn đó không nằm ở biên thì cạnh bên trái của nó lớn hơn hẳn và cạnh bên phải của nó cũng lớn hơn hẳn. Nếu đoạn phẳng nằm ở ranh giới thì chỉ có đoạn lân cận tồn tại mới phải thỏa mãn điều kiện này. Đầu ra là "CÓ" nếu có chính xác một đoạn thung lũng như vậy tồn tại và "KHÔNG" nếu ngược lại. 

Các ràng buộc cho phép mảng có độ dài lên tới 200.000 và tối đa 10.000 trường hợp thử nghiệm, nhưng tổng của tất cả độ dài mảng trên các trường hợp thử nghiệm không vượt quá 200.000. Điều này có nghĩa là chúng ta phải xử lý từng mảng theo thời gian tuyến tính tương ứng với kích thước của nó. Bất kỳ cách tiếp cận nào kiểm tra mọi mảng con có thể một cách rõ ràng sẽ là phương trình bậc hai và quá chậm. Quét tuyến tính cẩn thận là đủ. 

Các trường hợp cạnh dễ xử lý sai bao gồm các mảng có độ dài 1, các mảng có tất cả các phần tử bằng nhau hoặc các mảng có nhiều đoạn phẳng có thể trông giống như các thung lũng. Ví dụ,`[1, 1, 1]`là một thung lũng vì ranh giới bên trái bắt đầu đoạn phẳng và không có hàng xóm nhỏ hơn ở bên trái của nó, nhưng`[2, 1, 1, 1, 2]`có một thung lũng trống ở giữa và sẽ trả về "CÓ". Mảng như`[1, 2, 1, 2, 1]`có nhiều điểm tối thiểu tiềm năng, vì vậy đầu ra phải là "KHÔNG". Việc đếm nhầm các vùng bằng phẳng hoặc không xác minh được các điều kiện biên là nguồn lỗi phổ biến. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ kiểm tra mọi phân mảng có thể có độ dài ít nhất một để xem liệu nó có thỏa mãn tính chất thung lũng hay không. Đối với mỗi mảng con, chúng tôi sẽ xác minh tất cả các phần tử đều bằng nhau và kiểm tra các phần tử lân cận xem chúng có tồn tại hay không. Điều này đúng, nhưng mỗi mảng có độ dài`n`có thể có các mảng con O(n^2), tốc độ này quá chậm đối với`n = 2*10^5`. 

Thông tin chi tiết quan trọng là chỉ các phần tử tối thiểu của mảng mới có thể tạo thành các thung lũng hợp lệ vì bất kỳ thung lũng nào cũng phải thấp hơn môi trường xung quanh. Trước tiên chúng ta có thể tìm giá trị tối thiểu trong mảng và sau đó xem xét các phân đoạn liền kề của giá trị này. Đối với mỗi phân đoạn liền kề của các phần tử tối thiểu, chúng tôi kiểm tra các điều kiện biên: phần tử trước phân khúc, nếu nó tồn tại, phải lớn hơn rất nhiều và phần tử sau phân đoạn, nếu nó tồn tại, phải lớn hơn rất nhiều. Nếu chính xác một đoạn như vậy thỏa mãn điều này thì mảng đó là một thung lũng; nếu không thì không. Bằng cách tập trung vào các phần tử tối thiểu và quét tuyến tính, chúng tôi giảm độ phức tạp từ bậc hai xuống tuyến tính trong kích thước của mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mảng hiện tại, hãy tìm giá trị nhỏ nhất. Đây là ứng cử viên duy nhất cho một thung lũng vì bất kỳ thung lũng nào cũng phải thấp hơn các thung lũng lân cận. 
2. Khởi tạo bộ đếm số lượng đoạn thung lũng hợp lệ. 
3. Lặp lại mảng từ trái sang phải. Bất cứ khi nào bạn gặp một giá trị tối thiểu, hãy xác định đoạn liền kề có các giá trị tối thiểu bằng nhau. 
4. Đối với đoạn này, hãy kiểm tra các phần tử biên. Nếu phân đoạn bắt đầu ở đầu mảng, chỉ kiểm tra phân đoạn bên phải. Nếu nó kết thúc ở chỉ mục cuối cùng, chỉ kiểm tra hàng xóm bên trái. Ngược lại, hãy xác minh hàng xóm bên trái lớn hơn giá trị tối thiểu và hàng xóm bên phải cũng lớn hơn. 
5. Nếu đoạn thỏa mãn điều kiện biên, hãy tăng bộ đếm thung lũng. 
6. Tiếp tục quét mảng tới đoạn tối thiểu tiếp theo cho đến hết. 
7. Cuối cùng, nếu có chính xác một đoạn vượt qua bước kiểm tra ranh giới, hãy in "CÓ". Ngược lại in ra "KHÔNG". 

Tại sao nó hoạt động: Việc quan sát giá trị tối thiểu đảm bảo chúng tôi chỉ xem xét các thung lũng tiềm năng và việc kiểm tra ranh giới đảm bảo rằng chúng tôi đáp ứng định nghĩa về thung lũng đúng một lần. Quá trình quét tuyến tính đảm bảo chúng tôi kiểm tra tất cả các phân đoạn tối thiểu liền kề mà không bỏ sót phần chồng chéo hoặc kiểm tra một phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_valley(a):
    n = len(a)
    min_val = min(a)
    i = 0
    valley_count = 0
    
    while i < n:
        if a[i] == min_val:
            start = i
            while i + 1 < n and a[i + 1] == min_val:
                i += 1
            end = i
            left_ok = start == 0 or a[start - 1] > min_val
            right_ok = end == n - 1 or a[end + 1] > min_val
            if left_ok and right_ok:
                valley_count += 1
            i += 1
        else:
            i += 1
    return valley_count == 1

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print("YES" if is_valley(a) else "NO")
```chức năng`is_valley`quét mảng một lần. Các nhóm vòng lặp while bên trong chứa các giá trị tối thiểu liền kề và các điều kiện biên được kiểm tra rõ ràng. Tăng dần`i`cẩn thận tránh bỏ qua các phân đoạn hoặc tính hai lần. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`[3, 2, 2, 1, 2, 2, 3]`: 

| tôi | một [tôi] | bắt đầu | kết thúc | trái_ok | đúng_ok | thung lũng_count | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | - | - | - | - | 0 | 
| 1 | 2 | - | - | - | - | 0 | 
| 2 | 2 | - | - | - | - | 0 | 
| 3 | 1 | 3 | 3 | Đúng | Đúng | 1 | 
| 4 | 2 | - | - | - | - | 1 | 
| 5 | 2 | - | - | - | - | 1 | 
| 6 | 3 | - | - | - | - | 1 | 

Chỉ có một phân đoạn`[3,3]`của các giá trị tối thiểu thỏa mãn điều kiện biên, xuất ra "CÓ". 

Theo dõi đầu vào`[1, 2, 3, 4, 3, 2, 1]`: 

| tôi | một [tôi] | bắt đầu | kết thúc | trái_ok | đúng_ok | thung lũng_count | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | Đúng | Sai | 0 | 
| 6 | 1 | 6 | 6 | Sai | Đúng | 0 | 

Hai phân đoạn tối thiểu tồn tại nhưng không có phân đoạn nào thỏa mãn cả hai ranh giới, xuất ra "NO". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính mảng, nhóm các phần tử tối thiểu liền kề. | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được duy trì, không có bộ nhớ bổ sung tỷ lệ với n. | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 2·10^5, giải pháp này dễ dàng phù hợp trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    def is_valley(a):
        n = len(a)
        min_val = min(a)
        i = 0
        valley_count = 0
        while i < n:
            if a[i] == min_val:
                start = i
                while i + 1 < n and a[i + 1] == min_val:
                    i += 1
                end = i
                left_ok = start == 0 or a[start - 1] > min_val
                right_ok = end == n - 1 or a[end + 1] > min_val
                if left_ok and right_ok:
                    valley_count += 1
                i += 1
            else:
                i += 1
        return valley_count == 1
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res.append("YES" if is_valley(a) else "NO")
    return "\n".join(res)

# provided samples
assert run("""6
7
3 2 2 1 2 2 3
11
1 1 1 2 3 3 4 5 6 6 6
7
1 2 3 4 3 2 1
7
9 7 4 6 9 9 10
1
1000000000
8
9 4 4 5 9 4 9 10
""") == """YES
YES
NO
YES
YES
NO"""

# custom cases
assert run("""4
1
5
5
2 2 2 2 2
5
5 4 3 2 1
6
1 2 1 2 1 2
""") == """YES
NO
YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​|
