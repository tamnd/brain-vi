---
title: "CF 1971A - Bài toán sắp xếp đầu tiên của tôi"
description: "Nhiệm vụ yêu cầu chúng ta đọc các cặp số nguyên nhỏ và xuất ra từng cặp theo thứ tự được sắp xếp: số nhỏ hơn trước, sau đó là số lớn hơn."
date: "2026-06-08T17:19:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1971
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 944 (Div. 4)"
rating: 800
weight: 1971
solve_time_s: 88
verified: true
draft: false
---

[CF 1971A - Vấn đề sắp xếp đầu tiên của tôi](https://codeforces.com/problemset/problem/1971/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta đọc các cặp số nguyên nhỏ và xuất ra từng cặp theo thứ tự được sắp xếp: số nhỏ hơn trước, sau đó là số lớn hơn. Mỗi trường hợp thử nghiệm bao gồm hai số nguyên,$x$Và$y$và đầu vào bắt đầu bằng một số$t$đại diện cho số lượng cặp như vậy chúng ta cần xử lý. Đầu ra phải chứa cặp được sắp xếp tương ứng cho từng trường hợp thử nghiệm. 

Các ràng buộc cực kỳ chặt chẽ: cả hai số nguyên đều nằm trong khoảng từ 0 đến 9 và có thể có tới 100 trường hợp thử nghiệm. Vì số lượng quá nhỏ nên chúng ta không cần cấu trúc dữ liệu phức tạp hoặc thuật toán sắp xếp. Thuật toán chỉ cần xác định số nào trong hai số nguyên nhỏ hơn và số nào lớn hơn, một so sánh duy nhất cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh khi hai số nguyên bằng nhau. Ví dụ: nếu cặp đầu vào là$3$Và$3$, đầu ra phải là$3 3$. Một cách tiếp cận ngây thơ chỉ hoán đổi khi$x > y$hoạt động chính xác ở đây, nhưng việc quên trường hợp đẳng thức hoặc thử các thủ thuật dựa trên phép trừ có thể dẫn đến kết quả không chính xác. Một trường hợp cạnh nhỏ khác là khi cả hai số đều bằng 0, trường hợp này cũng yêu cầu đầu ra$0 0$. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu đã là tối ưu trong kịch bản này. Đối với mỗi cặp, bạn so sánh$x$Và$y$. Nếu như$x > y$, bạn trao đổi chúng; nếu không, hãy để chúng như vậy. Phương pháp này đúng vì mỗi cặp chỉ có hai phần tử và một phép so sánh duy nhất là đủ để sắp xếp chúng. Số lượng hoạt động cho đầu vào trong trường hợp xấu nhất, với 100 trường hợp thử nghiệm, là 100 so sánh, điều này không đáng kể. 

Không cần đến kỹ thuật tiên tiến hơn như sắp xếp đầy đủ, vì kích thước và cấu trúc của bài toán làm cho phương pháp so sánh đơn vừa đơn giản vừa hiệu quả. Cái nhìn sâu sắc quan trọng là nhận ra rằng việc sắp xếp hai số chỉ là phép gán so sánh và có điều kiện, đồng thời bộ máy sắp xếp lớn sẽ tốn chi phí không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$t$đại diện cho số lượng các trường hợp thử nghiệm. Điều này thiết lập số lần lặp mà chúng tôi sẽ thực hiện. 
2. Với mỗi test, đọc hai số nguyên$x$Và$y$. 
3. So sánh hai số nguyên. Nếu như$x > y$, hoán đổi giá trị của chúng. Điều này đảm bảo rằng$x$luôn là số nhỏ hơn hoặc bằng nhau. 
4. In$x$Và$y$theo thứ tự, cách nhau một dấu cách. Đây là định dạng đầu ra bắt buộc. 
5. Lặp lại các bước 2-4 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: tính bất biến được duy trì trong suốt thuật toán là sau khi so sánh và có thể hoán đổi, số đầu tiên của cặp không bao giờ lớn hơn số thứ hai. Vì đặc tả đầu ra chỉ yêu cầu cặp theo thứ tự không giảm nên bất biến này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y = map(int, input().split())
    if x > y:
        x, y = y, x
    print(x, y)
```Mã đầu tiên được đọc$t$sử dụng`input()`. Vòng lặp chạy chính xác$t$lần, xử lý một trường hợp thử nghiệm cho mỗi lần lặp.`map(int, input().split())`phân tích hai số nguyên từ mỗi dòng. Hoán đổi có điều kiện đảm bảo số nhỏ hơn sẽ được ưu tiên trước và`print(x, y)`xuất chúng một cách chính xác. Vì Python xử lý nguyên bản các số nguyên nhỏ nên không cần cân nhắc đặc biệt về ranh giới hoặc tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 9`| x | y | Điều kiện x > y? | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 9 | Sai | 1 9 | 

Giải thích: 1 nhỏ hơn 9, không xảy ra hoán đổi. Đầu ra khớp với thứ tự sắp xếp. 

### Ví dụ 2 

đầu vào:`8 4`| x | y | Điều kiện x > y? | Đầu ra | 
| --- | --- | --- | --- | 
| 8 | 4 | Đúng | 4 8 | 

Giải thích: 8 lớn hơn 4 nên việc hoán đổi xảy ra, tạo ra thứ tự tăng dần chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một so sánh duy nhất và trao đổi tùy chọn. Với t ≤ 100 thì điều này không đáng kể. | 
| Không gian | O(1) | Không cần thêm bộ nhớ tỷ lệ với t, chỉ cần các biến tạm thời cho x và y. | 

Thuật toán thoải mái trong giới hạn thời gian chạy 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        if x > y:
            x, y = y, x
        print(x, y)
    return output.getvalue().strip()

# Provided sample
assert run("10\n1 9\n8 4\n1 4\n3 4\n2 0\n2 4\n6 9\n3 3\n0 0\n9 9\n") == \
"1 9\n4 8\n1 4\n3 4\n0 2\n2 4\n6 9\n3 3\n0 0\n9 9"

# Custom cases
assert run("1\n0 0\n") == "0 0", "both zero"
assert run("1\n9 0\n") == "0 9", "maximum boundary"
assert run("1\n5 5\n") == "5 5", "equal values"
assert run("3\n1 2\n2 1\n3 3\n") == "1 2\n1 2\n3 3", "mixed ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0 0`| Cả hai số đều có giá trị nhỏ nhất có thể | 
|`9 0`|`0 9`| Hoán đổi cần thiết cho giá trị tối đa và tối thiểu | 
|`5 5`|`5 5`| Cả hai số đều bằng nhau, không cần hoán đổi | 
|`1 2\n2 1\n3 3`|`1 2\n1 2\n3 3`| Các đơn hàng hỗn hợp có số lượng bằng nhau | 

## Vỏ cạnh 

Ví dụ: khi hai số bằng nhau`3 3`, thuật toán so sánh chúng. Kể từ khi điều kiện`x > y`là sai, không có sự hoán đổi nào xảy ra. Đầu ra in vẫn còn`3 3`, điều đó đúng. Đối với đầu vào`0 0`, logic tương tự cũng được áp dụng, duy trì tính bất biến rằng số thứ nhất không bao giờ lớn hơn số thứ hai. Đối với đầu vào ranh giới tối đa như`9 0`, trao đổi đảm bảo đầu ra`0 9`. Thuật toán xử lý nhất quán tất cả các tình huống vì nó dựa trên một so sánh đơn giản, duy nhất.
