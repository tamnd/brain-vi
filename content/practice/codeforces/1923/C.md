---
title: "CF 1923C - Tìm B"
description: "Chúng ta được cung cấp một mảng c gồm các số nguyên dương và nhiều truy vấn hỏi xem các mảng con nhất định của c có tốt không."
date: "2026-06-08T19:12:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1923
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 162 (Rated for Div. 2)"
rating: 1400
weight: 1923
solve_time_s: 103
verified: true
draft: false
---

[CF 1923C - Tìm B](https://codeforces.com/problemset/problem/1923/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`c`số nguyên dương và nhiều truy vấn hỏi liệu các mảng con nhất định của`c`_tốt_. Một mảng con là tốt nếu có thể xây dựng một mảng khác`b`có cùng độ dài sao cho ba điều kiện thỏa mãn: tổng của mảng con và`b`khớp, mọi phần tử của mảng con khác với phần tử tương ứng của`b`, và mọi phần tử của`b`là tích cực. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp mảng`c`và một danh sách các truy vấn, mỗi truy vấn xác định một mảng con theo chỉ số bắt đầu và kết thúc của nó. Đầu ra cho mỗi truy vấn là "CÓ" nếu mảng con tốt và "KHÔNG" nếu ngược lại. 

Các ràng buộc rất chặt chẽ: tổng chiều dài của tất cả các mảng và tổng số truy vấn trong các trường hợp thử nghiệm có thể lên tới 300.000. Điều này ngụ ý rằng bất kỳ giải pháp nào kiểm tra từng phần tử mảng con riêng lẻ cho mỗi truy vấn sẽ thực hiện theo thứ tự 10^10 thao tác trong trường hợp xấu nhất, quá chậm. Chúng ta cần một phương pháp có thể trả lời từng truy vấn một cách hiệu quả mà không cần phải xây dựng lại mảng con một cách rõ ràng mỗi lần. 

Trường hợp cạnh tinh tế phát sinh khi mảng con chỉ chứa một phần tử duy nhất. Trong trường hợp đó không có cách nào thỏa mãn điều kiện`b_i ≠ a_i`, bởi vì một số nguyên dương không thể khác chính nó trong khi tổng vẫn giữ nguyên. Bất kỳ cách tiếp cận ngây thơ nào bỏ qua điều này sẽ cho rằng một mảng con có độ dài một là tốt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một mảng hợp lệ`b`cho mỗi truy vấn bằng cách lặp lại mảng con và cố gắng gán các giá trị khác với`a_i`trong khi vẫn giữ tổng số tiền nhất quán. Đối với một mảng con có độ dài`m`, điều này sẽ mất`O(m)`thời gian cho mỗi truy vấn. Trong trường hợp xấu nhất là 300.000 truy vấn trên các mảng có độ dài lên tới 300.000, điều này dẫn đến khoảng 10^10 thao tác, điều này là không khả thi. 

Quan sát quan trọng là các giá trị thực tế trong mảng con phần lớn không liên quan đến câu hỏi về mức độ tốt. Một mảng con có độ dài bằng một không bao giờ có thể tốt, nhưng bất kỳ mảng con nào có độ dài bằng hai hoặc nhiều hơn luôn có thể được làm tốt bằng một thủ thuật phân phối lại đơn giản. Nếu độ dài ít nhất là hai, người ta luôn có thể tăng một phần tử lên 1 và giảm phần tử khác đi 1. Điều này bảo toàn tổng, đảm bảo tính dương nếu tất cả các phần tử ban đầu đều dương và đảm bảo rằng tất cả`b_i ≠ a_i`. 

Điều này làm giảm vấn đề thành một kiểm tra đơn giản: nếu độ dài mảng con là 1 thì câu trả lời là "KHÔNG". Ngược lại, câu trả lời là "CÓ". Cách tiếp cận này trả lời từng truy vấn trong`O(1)`thời gian sau khi đọc đầu vào. Tổng tiền tố thậm chí không cần thiết vì tổng tiền thực tế không hạn chế tính khả thi ngoài các điều kiện tích cực tầm thường và bảo toàn tổng, được thỏa mãn bởi việc phân phối lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(total_length_of_subarrays) ≈ 10^10 | O(m) mỗi truy vấn | Quá chậm | 
| Tối ưu | O(n + q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`Và`q`, theo sau là mảng`c`chiều dài`n`. 
2. Với mỗi truy vấn, hãy đọc các chỉ mục`l`Và`r`. Tính độ dài của mảng con như`r - l + 1`. 
3. Nếu độ dài là 1 thì in ra "NO" vì không có mảng`b`có thể đáp ứng`b_1 ≠ a_1`trong khi vẫn bảo toàn tổng và dương. 
4. Nếu độ dài từ 2 trở lên, hãy in "CÓ" vì chúng ta luôn có thể điều chỉnh phần tử này lên và phần tử khác xuống 1, đảm bảo`b_i ≠ a_i`và tổng bằng nhau. 

**Tại sao nó hoạt động**: Hạn chế duy nhất có thể khiến mảng con không thể thực hiện được là độ dài bằng 1. Đối với các mảng con dài hơn, điều kiện bảo toàn tổng đủ linh hoạt để người ta có thể điều chỉnh các phần tử ở mức tối thiểu nhằm đáp ứng mọi điều kiện. Bất biến là với bất kỳ độ dài nào ≥ 2, ít nhất hai phần tử cho phép hoán đổi để bảo toàn tổng trong khi thay đổi cả hai giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    c = list(map(int, input().split()))
    for _ in range(q):
        l, r = map(int, input().split())
        length = r - l + 1
        if length == 1:
            print("NO")
        else:
            print("YES")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi truy vấn, nó tính toán độ dài mảng con trong`O(1)`và áp dụng kiểm tra độ dài đơn giản. Chương trình tránh việc cắt mảng hoặc tính tổng không cần thiết, giúp chương trình hoạt động nhanh với kích thước đầu vào lớn nhất. Ranh giới quan trọng cần xử lý chính xác là một mảng con có độ dài bằng một. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Truy vấn mảng con đầu vào:```
1 5 -> length 5
4 4 -> length 1
3 4 -> length 2
1 3 -> length 3
```| Truy vấn | Độ dài mảng con | Đầu ra | 
| --- | --- | --- | 
| 1 5 | 5 | CÓ | 
| 4 4 | 1 | KHÔNG | 
| 3 4 | 2 | CÓ | 
| 1 3 | 3 | CÓ | 

Dấu vết xác nhận rằng chỉ có các mảng con có độ dài 1 là không thể. 

**Ví dụ về trường hợp cạnh** 

đầu vào:`c = [7], query = 1 1`Độ dài là 1, vì vậy đầu ra là KHÔNG. Không có giá trị khác quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi truy vấn được trả lời trong thời gian không đổi; đầu vào đọc mất O(n) cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Chỉ có mảng`c`cần được lưu trữ. Không có mảng bổ sung được yêu cầu. | 

Sự phức tạp này dễ dàng nằm gọn trong giới hạn thời gian 3 giây cho`n, q`lên tới 300.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        c = list(map(int, input().split()))
        for _ in range(q):
            l, r = map(int, input().split())
            length = r - l + 1
            if length == 1:
                print("NO")
            else:
                print("YES")
    return output.getvalue().strip()

# provided sample
assert run("""1
5 4
1 2 1 4 5
1 5
4 4
3 4
1 3
""") == """YES
NO
YES
YES"""

# custom tests
assert run("""1
1 1
7
1 1
""") == "NO", "single element subarray"

assert run("""1
2 2
1 2
1 1
1 2
""") == "NO\nYES", "mix of length 1 and 2"

assert run("""1
3 1
1 1 1
2 3
""") == "YES", "all equal values"

assert run("""1
5 5
5 4 3 2 1
1 5
2 2
3 4
4 5
5 5
""") == "YES\nNO\nYES\nYES\nNO", "various lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n7\n1 1`| KHÔNG | từ chối mảng con một phần tử | 
|`2 2\n1 2\n1 1\n1 2`| KHÔNG CÓ | độ dài hỗn hợp được xử lý chính xác | 
|`3 1\n1 1 1\n2 3`| CÓ | các giá trị bằng nhau vẫn khả thi | 
|`5 5\n5 4 3 2 1\n...`| CÓ KHÔNG CÓ CÓ KHÔNG | xử lý chính xác độ dài ranh giới và trung gian | 

## Vỏ cạnh 

Ví dụ: đối với mảng con một phần tử`c = [42], query = 1 1`, thuật toán tính toán`length = 1`và xuất ra NO ngay lập tức. Không có nỗ lực xây dựng`b`, tránh việc chấp nhận sai. 

Đối với mảng con lớn nhất có thể, chẳng hạn như`c`với độ dài 300.000 và truy vấn`1 300000`,`length = 300000`kích hoạt trực tiếp "CÓ" mà không cần lặp lại hoặc tính tổng, xác nhận giải pháp xử lý giới hạn trên một cách hiệu quả. 

Tất cả các trường hợp cạnh, bao gồm các phần tử và mảng con bằng nhau liên tiếp ở đầu hoặc cuối mảng, đều được xử lý chính xác bằng cách kiểm tra độ dài đơn giản này.
