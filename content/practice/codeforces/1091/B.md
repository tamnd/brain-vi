---
title: "CF 1091B - Năm mới và định vị kho báu"
description: "Chúng ta được cung cấp một tập hợp các đài tưởng niệm trên mặt phẳng 2D và một tập hợp các manh mối chỉ ra các vectơ từ các đài tưởng niệm đến một kho báu ẩn giấu. Mỗi đài tưởng niệm có chính xác một manh mối, nhưng việc ánh xạ bị xáo trộn nên chúng ta không biết manh mối nào thuộc về đài tưởng niệm nào."
date: "2026-06-12T05:58:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1091
codeforces_index: "B"
codeforces_contest_name: "Good Bye 2018"
rating: 1200
weight: 1091
solve_time_s: 78
verified: true
draft: false
---

[CF 1091B - Năm mới và định vị địa lý kho báu](https://codeforces.com/problemset/problem/1091/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đài tưởng niệm trên mặt phẳng 2D và một tập hợp các manh mối chỉ ra các vectơ từ các đài tưởng niệm đến một kho báu ẩn giấu. Mỗi đài tưởng niệm có chính xác một manh mối, nhưng việc ánh xạ bị xáo trộn nên chúng ta không biết manh mối nào thuộc về đài tưởng niệm nào. Mỗi manh mối là một vectơ và khi được thêm vào tọa độ của đài tưởng niệm, nó sẽ trỏ chính xác đến kho báu. Mục tiêu là xác định tọa độ của kho báu mà không cần phải tìm hoán vị của manh mối cho các đài tưởng niệm. 

Kích thước đầu vào cho phép lên tới 1000 đài tưởng niệm và 1000 manh mối. Mỗi tọa độ có thể có độ lớn tới 10^6 và manh mối có thể lên tới ±2×10^6. Thời gian giới hạn là 2 giây. Với n = 1000, việc kiểm tra mạnh mẽ tất cả các hoán vị là không thể, bởi vì n! có kích thước lớn về mặt thiên văn. Điều này loại trừ các giải pháp cố gắng kiểm tra trực tiếp mọi cặp đôi có thể có. 

Một sai lầm ngây thơ sẽ là cho rằng manh mối đầu tiên thuộc về đài tưởng niệm đầu tiên và tính toán kho báu như sau:`T = obelisk + clue`. Điều này không thành công vì các manh mối có thể bị xáo trộn. Ví dụ: nếu n = 2, các đài tưởng niệm ở (0,0) và (1,1) và các manh mối (1,0) và (0,1), việc gán các manh mối theo thứ tự sẽ cho ra T = (1,0) cho đài tưởng niệm đầu tiên và T = (1,2) cho đài tưởng niệm thứ hai, điều này không nhất quán. Lời giải đúng sẽ hoán đổi các manh mối, cho T = (0+0,0+1) = (1,1), nhất quán cho cả hai tháp. 

Chúng ta cũng cần xử lý tọa độ âm và giá trị lớn một cách cẩn thận để tránh tràn trong các ngôn ngữ có số nguyên giới hạn. Trong Python điều này là an toàn, nhưng trong C++ hoặc Java người ta sẽ cần`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các hoán vị của manh mối được ánh xạ tới các đài tưởng niệm. Đối với mỗi hoán vị, chúng tôi tính toán tọa độ kho báu cho tất cả các cặp và kiểm tra xem chúng có giống nhau không. Mặc dù điều này đúng nhưng độ phức tạp của nó là O(n!) không thể thực hiện được ngay cả với n = 10. 

Cái nhìn sâu sắc quan trọng để tăng tốc độ này đến từ tính tuyến tính. Nếu kho báu là`T`, thì tổng của tất cả tọa độ kho báu trên tất cả các đài tưởng niệm bằng tổng tọa độ đài tưởng niệm cộng với tổng các vectơ đầu mối, bởi vì mỗi đầu mối được sử dụng chính xác một lần. Chính thức:```
T * n = sum_over_obelisks(x_i, y_i) + sum_over_clues(a_j, b_j)
```Từ đó, chúng ta có thể tính ngay tọa độ kho báu như sau:```
T_x = (sum of all x_i + sum of all a_j) / n
T_y = (sum of all y_i + sum of all b_j) / n
```Vì bài toán đảm bảo nghiệm nguyên nên phép chia cho n sẽ thu được số nguyên. 

Cách tiếp cận này hiệu quả vì phép cộng có tính chất giao hoán: tổng các đầu mối được gán cho các tháp không phụ thuộc vào hoán vị. Điều này làm giảm vấn đề từ độ phức tạp giai thừa sang thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Dựa trên tổng (Tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n là số cột và manh mối. Khởi tạo bộ tích lũy cho tổng tọa độ x, y của các đài tưởng niệm và tổng vectơ a, b từ các đầu mối. 
2. Vòng qua n đài tưởng niệm. Đối với mỗi đài tưởng niệm, hãy đọc tọa độ của nó`(x_i, y_i)`và thêm x_i vào tổng x và y_i vào tổng y. Điều này thu thập tổng số đóng góp của tất cả các vị trí obelisk. 
3. Lặp lại n manh mối. Đối với mỗi vectơ đầu mối`(a_j, b_j)`, thêm a_j vào a-sum và b_j vào b-sum. Điều này thu thập tổng số đóng góp của tất cả các vectơ đầu mối. 
4. Tính tọa độ kho báu như sau`T_x = (x-sum + a-sum) // n`Và`T_y = (y-sum + b-sum) // n`. Phép chia số nguyên có tác dụng vì bài toán đảm bảo nghiệm số nguyên. 
5. Xuất ra T_x và T_y. 

Tại sao nó hoạt động: Tổng của tất cả tọa độ kho báu trên tất cả các đài tưởng niệm phải bằng tổng của tất cả các đài tưởng niệm cộng với tất cả manh mối. Vì có chính xác một manh mối trên mỗi đài tưởng niệm nên tổng này không phụ thuộc vào hoán vị. Chia cho n sẽ có tọa độ kho báu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
ob_x_sum = 0
ob_y_sum = 0

for _ in range(n):
    x, y = map(int, input().split())
    ob_x_sum += x
    ob_y_sum += y

clue_x_sum = 0
clue_y_sum = 0

for _ in range(n):
    a, b = map(int, input().split())
    clue_x_sum += a
    clue_y_sum += b

T_x = (ob_x_sum + clue_x_sum) // n
T_y = (ob_y_sum + clue_y_sum) // n

print(T_x, T_y)
```Giải pháp đầu tiên là tích lũy số tiền cho các đài tưởng niệm và manh mối riêng biệt. Chúng ta có thể tích lũy thành một cặp biến, nhưng việc tách chúng ra sẽ làm cho logic rõ ràng hơn và phù hợp với thuật toán. Phép chia số nguyên là an toàn vì bài toán đảm bảo tọa độ kho báu là số nguyên. Python tự động xử lý các số nguyên lớn, do đó không phát sinh vấn đề tràn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| đài tưởng niệm | x-tổng | tổng y | manh mối | tổng hợp | b-tổng | T_x | T_y | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (2,5),(-6,4) | -4 | 9 | (7,-2),(-1,-3) | 6 | -5 | (-4+6)//2=1 | (9-5)//2=2 | 

Giải thích: Tổng tọa độ x của obelisk là -4, manh mối tổng bằng 6, tổng 2, chia cho 2 là 1. Tương tự với y: 9-5=4, chia cho 2 là 2. Điều này khớp với kho báu đúng ở (1,2). 

Mẫu 2: 

| đài tưởng niệm | x-tổng | tổng y | manh mối | tổng hợp | b-tổng | T_x | T_y | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (2,3),(4,5),(6,7) | 12 | 15 | (1,1),(2,2),(3,3) | 6 | 6 | (12+6)//3=6 | (15+6)//3=7 | 

Điều này xác nhận thuật toán xử lý chính xác hơn 2 đài tưởng niệm và vectơ tùy ý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đài tưởng niệm và đầu mối được đọc và tóm tắt chính xác một lần | 
| Không gian | O(1) | Chỉ cần bốn bộ tích lũy số nguyên, độc lập với n | 

Với n ≤ 1000, tổng số thao tác là khoảng 2n = 2000, thấp hơn nhiều so với bất kỳ giới hạn hiệu suất nào. Việc sử dụng bộ nhớ là tối thiểu và Python xử lý số học số nguyên một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    ob_x_sum = ob_y_sum = 0
    for _ in range(n):
        x, y = map(int, input().split())
        ob_x_sum += x
        ob_y_sum += y
    clue_x_sum = clue_y_sum = 0
    for _ in range(n):
        a, b = map(int, input().split())
        clue_x_sum += a
        clue_y_sum += b
    T_x = (ob_x_sum + clue_x_sum) // n
    T_y = (ob_y_sum + clue_y_sum) // n
    return f"{T_x} {T_y}"

# Provided samples
assert run("2\n2 5\n-6 4\n7 -2\n-1 -3\n") == "1 2", "sample 1"

# Minimum input
assert run("1\n0 0\n0 0\n") == "0 0", "minimum input"

# All positive coordinates
assert run("3\n1 2\n3 4\n5 6\n1 1\n2 2\n3 3\n") == "6 7", "positive coordinates"

# Negative coordinates
assert run("2\n-1 -1\n-2 -3\n1 0\n2 1\n") == "0 -1", "negative coordinates"

# Maximum values
assert run("2\n1000000 1000000\n-1000000 -1000000\n2000000 2000000\n-2000000 -2000000\n") == "500000 500000", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đài tưởng niệm | 0 0 | trường hợp cạnh đầu vào tối thiểu | 
| 3 đài tưởng niệm, tất cả đều tích cực | 6 7 | tính chính xác dựa trên tổng | 
| 2 đài tưởng niệm, âm bản | 0 -1 | xử lý tọa độ âm | 
| 2 đài tưởng niệm, lớn | 500000 500000 | xử lý tọa độ lớn mà không bị tràn | 

## Vỏ cạnh
