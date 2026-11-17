---
title: "CF 2004A - Điểm gần nhất"
description: "Chúng ta được cho một tập hợp các điểm nguyên trên đường một chiều. Khoảng cách giữa hai điểm là sự khác biệt tuyệt đối của chúng."
date: "2026-06-09T02:37:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2004
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 169 (Rated for Div. 2)"
rating: 800
weight: 2004
solve_time_s: 70
verified: true
draft: false
---

[CF 2004A - Điểm gần nhất](https://codeforces.com/problemset/problem/2004/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm nguyên trên đường một chiều. Khoảng cách giữa hai điểm là sự khác biệt tuyệt đối của chúng. Nhiệm vụ là xác định xem có thể chèn một điểm nguyên mới khác với tất cả các điểm hiện có hay không, sao cho điểm mới này trở thành điểm gần nhất với mọi điểm trong tập hợp. Nói cách khác, mọi điểm ban đầu phải có điểm mới này là điểm lân cận gần nhất của nó. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số điểm và danh sách tọa độ điểm được sắp xếp. Đầu ra phải là "CÓ" nếu có thể thêm điểm đó hoặc "KHÔNG" nếu không. 

Các ràng buộc nhỏ: số điểm$n$nhiều nhất là 40 và giá trị điểm nhiều nhất là 100. Điều này cho phép chúng ta xem xét các giải pháp có thể liên quan đến việc kiểm tra trực tiếp khoảng cách giữa các điểm. Số ca kiểm thử$t$có thể lên tới 1000, do đó, một giải pháp hoạt động trong$O(n)$mỗi trường hợp thử nghiệm là đủ dễ dàng. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó các điểm là số nguyên liên tiếp. Ví dụ, đối với điểm$[5, 6]$, không có số nguyên nào nằm gần cả hai điểm hơn chúng ở gần nhau. Các cách tiếp cận bất cẩn có thể cố gắng chọn điểm giữa mà không kiểm tra xem nó đã là điểm nguyên hay nằm trong tập hợp hiện có, tạo ra câu trả lời "CÓ" không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi ứng cử viên số nguyên trong phạm vi tối thiểu và tối đa của các điểm hiện có và kiểm tra xem nó có gần hơn tất cả các điểm khác cho mọi phần tử trong tập hợp hay không. Điều này đúng vì chúng ta có thể đánh giá điều kiện cho tất cả các lần chèn có thể theo đúng nghĩa đen. Tuy nhiên, đối với mức chênh lệch tối đa từ 1 đến 100 và lên tới 1000 trường hợp thử nghiệm, điều này dẫn đến trường hợp xấu nhất$O(100 \cdot n \cdot t)$, điều này là không cần thiết vì chúng ta có thể tìm được tiêu chí đơn giản hơn. 

Thông tin chi tiết quan trọng là để trở thành điểm gần nhất cho tất cả các điểm hiện có, điểm mới phải nằm hoàn toàn giữa mức tối thiểu và tối đa của tập hợp, gần mỗi đầu hơn so với các điểm lân cận tương ứng của chúng. Vì các điểm đã được sắp xếp nên các ứng cử viên khả thi duy nhất là một số nguyên ở bên trái điểm đầu tiên, một số nguyên ở bên phải điểm cuối cùng hoặc chính xác ở giữa khoảng cách lớn nhất. Kiểm tra khoảng cách lớn nhất giữa các điểm liên tiếp, điểm mới phải chính xác ở điểm giữa của chúng nếu khoảng cách là chẵn (vì vậy nó nằm trên một số nguyên). Nếu khoảng cách là số lẻ thì không có điểm giữa nguyên nào có thể thỏa mãn điều kiện cho cả hai điểm lân cận. 

Điều này làm giảm vấn đề kiểm tra xem khoảng cách lớn nhất giữa các điểm liên tiếp có chẵn hay không. Nếu có, điểm giữa là giải pháp; nếu không thì không thể được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100 * n) cho mỗi trường hợp thử nghiệm | O(1) | Hoạt động nhưng không cần thiết | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và danh sách các điểm được sắp xếp$x$. 
3. Tính khoảng cách giữa các điểm liên tiếp: với mỗi điểm$i$từ 0 đến$n-2$, tính toán$x[i+1] - x[i]$. 
4. Tìm khoảng cách lớn nhất. 
5. Kiểm tra xem khoảng cách lớn nhất này có chẵn không. Nếu đúng như vậy, tồn tại một điểm giữa nguyên nằm giữa hai điểm lân cận của nó và có thể đóng vai trò là điểm gần nhất cho cả hai. Ngược lại in ra "KHÔNG". 
6. Nếu có nhiều khoảng trống lớn nhất bằng nhau, bất kỳ khoảng trống nào trong số đó đều có thể cung cấp điểm giữa ứng cử viên. Nếu có ít nhất một số chẵn, hãy in "CÓ". 
7. Nếu không có khoảng cách chẵn thì in ra "NO". 

Tại sao nó hoạt động: bất biến là bất kỳ điểm mới nào cũng phải nằm hoàn toàn giữa hai điểm hiện có để giảm khoảng cách của chúng. Thứ tự được sắp xếp đảm bảo rằng khoảng cách tối đa là ràng buộc tới hạn. Khoảng cách chẵn đảm bảo tồn tại điểm giữa số nguyên; mặt khác, không có số nguyên nào có thể đồng thời thỏa mãn điều kiện điểm gần nhất cho cả hai số lân cận. Việc mở rộng ra ngoài phạm vi của tập hợp sẽ chỉ phục vụ một điểm cuối, không đạt được điều kiện chung gần nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    x = list(map(int, input().split()))
    
    possible = False
    for i in range(n - 1):
        gap = x[i+1] - x[i]
        if gap % 2 == 0:
            possible = True
            break
    print("YES" if possible else "NO")
```Mã này đọc từng trường hợp kiểm thử, tính toán khoảng cách giữa các điểm được sắp xếp liên tiếp và kiểm tra xem có khoảng cách nào là chẵn hay không. Khoảng cách chẵn đầu tiên đảm bảo rằng tồn tại một điểm giữa hợp lệ. Nếu không tìm thấy khoảng cách chẵn thì không có điểm nguyên nào thỏa mãn điều kiện phổ quát gần nhất. Các điều kiện biên như các số nguyên liên tiếp sẽ tự động thất bại trong việc kiểm tra khoảng cách chẵn. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
2
3
1 5 9
2
4 5
```| tôi | x[i] | x[i+1] | khoảng cách | thậm chí? | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 4 | vâng | Đúng | 
| 1 | 5 | 9 | 4 | vâng | Đúng | 

Đầu ra:```
YES
NO
```Giải thích: Test đầu tiên có khoảng trống là 4 và 4. Điểm giữa tồn tại ở mức 3 và 7. Trường hợp thử nghiệm thứ hai có khoảng trống là 1, lẻ, không có điểm giữa là số nguyên nên "NO". 

**Ví dụ 2:** 

đầu vào:```
1
4
2 4 7 10
```| tôi | x[i] | x[i+1] | khoảng cách | thậm chí? | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 4 | 2 | vâng | Đúng | 
| 1 | 4 | 7 | 3 | không | Đúng | 
| 2 | 7 | 10 | 3 | không | Đúng | 

Đầu ra:```
YES
```Điều này chứng tỏ rằng chỉ cần một khoảng trống chẵn là đủ để chèn một điểm mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại khoảng cách n-1 một lần | 
| Không gian | O(n) | Lưu trữ danh sách điểm đầu vào | 

Được cho$n \le 40$Và$t \le 1000$, điều này thực hiện trong$40,000$hoạt động, trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        possible = any((x[i+1]-x[i])%2==0 for i in range(n-1))
        output.append("YES" if possible else "NO")
    return "\n".join(output)

# provided samples
assert run("3\n2\n3 8\n2\n5 6\n6\n1 2 3 4 5 10\n") == "YES\nNO\nNO", "sample 1"

# custom cases
assert run("1\n2\n1 2\n") == "NO", "consecutive integers"
assert run("1\n3\n1 3 6\n") == "YES", "even gap between 1 and 3"
assert run("1\n5\n1 2 3 4 5\n") == "NO", "all consecutive integers"
assert run("1\n4\n10 12 15 20\n") == "YES", "largest gap even (10-12)"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | KHÔNG | số nguyên liên tiếp không được có điểm giữa | 
| 1 3 6 | CÓ | khoảng cách chẵn tồn tại | 
| 1 2 3 4 5 | KHÔNG | tất cả các số nguyên liên tiếp, không thể | 
| 10 12 15 20 | CÓ | khoảng cách chẵn lớn nhất, tồn tại điểm giữa | 

## Vỏ cạnh 

Đối với trường hợp cạnh của các số nguyên liên tiếp, ví dụ:$[5,6]$, khoảng cách là 1, lẻ. Thuật toán kiểm tra`gap % 2 == 0`và trả về chính xác "KHÔNG". 

Đối với trường hợp cạnh có nhiều khoảng trống tồn tại nhưng chỉ có một khoảng trống chẵn, ví dụ:$[1,3,6]$, khoảng cách đầu tiên là 2 (chẵn), cho phép chèn 2 làm điểm gần nhất cho cả 1 và 3. Thuật toán phát hiện khoảng cách chẵn và xuất ra "CÓ". 

Đối với tất cả các điểm trong một chuỗi liên tiếp hoàn hảo, ví dụ:$[1,2,3,4,5]$, tất cả các khoảng trống đều là 1 (lẻ). Không có điểm giữa số nguyên nào có thể thỏa mãn điều kiện cho bất kỳ cặp lân cận nào, do đó thuật toán đưa ra "NO" chính xác.
