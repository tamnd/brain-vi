---
title: "CF 2044C - Bài toán khó"
description: "Chúng ta được yêu cầu xếp những con khỉ vào chỗ ngồi trong một lớp học có đúng hai hàng m chỗ ngồi mỗi lớp. Những con khỉ có ba loại: một con khỉ sẽ chỉ ngồi ở hàng đầu tiên, b những con khỉ sẽ chỉ ngồi ở hàng thứ hai và c những con khỉ không có sở thích nào."
date: "2026-06-08T09:22:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2044
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 993 (Div. 4)"
rating: 800
weight: 2044
solve_time_s: 92
verified: true
draft: false
---

[CF 2044C - Vấn đề khó khăn](https://codeforces.com/problemset/problem/2044/C) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xếp những chú khỉ vào chỗ ngồi trong một lớp học có đúng hai hàng ghế.`m`mỗi chỗ ngồi. Những con khỉ có ba loại:`a`những con khỉ chỉ ngồi ở hàng đầu tiên,`b`những con khỉ sẽ chỉ ngồi ở hàng thứ hai, và`c`những con khỉ không có sự ưu tiên. Mỗi ghế chỉ có thể chứa một con khỉ. Mục tiêu là tối đa hóa tổng số khỉ ngồi. 

Đầu vào mang lại`t`trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng chỗ ngồi trên mỗi hàng`m`và số lượng`a`,`b`,`c`. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, số lượng khỉ tối đa có thể ngồi được. 

Với những hạn chế, mỗi`m`,`a`,`b`, Và`c`có thể lớn như$10^8$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại trên tất cả các chỗ ngồi hoặc tất cả các con khỉ riêng lẻ. Chúng ta cần tính toán số học trực tiếp cho mỗi trường hợp thử nghiệm. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tình huống trong đó số lượng khỉ ở một hàng cụ thể vượt quá số lượng ghế hoặc khi tổng số khỉ ở một hàng cụ thể ít hơn tổng số ghế, nhường chỗ cho chỗ ngồi linh hoạt với`c`khỉ. Ví dụ, nếu`m = 3`,`a = 6`,`b = 1`,`c = 1`, hàng đầu tiên chỉ có thể lấy 3 trong số đó`a`khỉ, để lại phần còn lại`a`khỉ không được ngồi yên. Hàng ghế thứ 2 có 3 ghế chỉ có 1`b`khỉ và 1`c`khỉ, với tổng số là 2. Một cách tiếp cận bất cẩn có thể cố gắng "di chuyển" những con khỉ giữa các hàng trái với sở thích của chúng, mang lại tổng số không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xếp từng con khỉ vào chỗ ngồi, kiểm tra mức độ ưu tiên của hàng ghế và tình trạng chỗ ngồi còn trống. Điều này đúng về nguyên tắc nhưng quá chậm vì tổng số khỉ có thể đạt tới$3 \cdot 10^8$và việc lặp lại từng con khỉ sẽ mất quá nhiều thời gian. 

Cái nhìn sâu sắc quan trọng là đây là một vấn đề số học thuần túy. Mỗi hàng có thể chứa tối đa`m`khỉ nên số khỉ ngồi ở hàng 1 là nhỏ nhất`a + c`Và`m`, và ở hàng 2 nó là giá trị tối thiểu của`b + remaining_c`Và`m`. Tuy nhiên, một phép tính đơn giản và tương đương hơn là xếp càng nhiều chỗ ngồi càng tốt vào mỗi hàng riêng lẻ: hàng 1 có thể ngồi tối đa`m`khỉ, ưu tiên`a`đầu tiên, sau đó là phần còn lại từ`c`. Điều tương tự cũng áp dụng cho hàng 2. Vì`c`khỉ linh hoạt, tổng số tối đa là`min(a + c, m) + min(b + c, m)`, nhưng chúng ta phải cẩn thận để không tính hai lần`c`. Công thức thích hợp là ngồi`a`lên đến`m`,`b`lên đến`m`, sau đó sử dụng dung lượng còn lại ở mỗi hàng cho`c`khỉ. Điều này làm giảm giải pháp thành một phép tính thời gian không đổi đơn giản cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a + b + c) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc các giá trị`m`,`a`,`b`,`c`. 
3. Tính số ghế còn trống ở hàng đầu tiên sau khi đặt`a`khỉ:`space1 = max(0, m - a)`. Điều này tượng trưng cho những chiếc ghế có thể chứa được những chú khỉ linh hoạt. 
4. Tính số ghế còn trống ở hàng thứ hai sau khi đặt`b`khỉ:`space2 = max(0, m - b)`. 
5. Số lượng`c`số khỉ có thể ngồi bị giới hạn bởi tổng không gian còn lại:`c_used = min(c, space1 + space2)`. 
6. Tổng số khỉ ngồi là`min(a, m) + min(b, m) + c_used`. Điều này đảm bảo chúng tôi không bao giờ vượt quá sức chứa của hàng và tôn trọng mọi sở thích của khỉ. 
7. In tổng số cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Chúng tôi luôn lấp đầy từng hàng ở mức công suất tối đa đồng thời tôn trọng các ràng buộc dành riêng cho từng hàng. Những con khỉ linh hoạt chỉ lấp đầy những chỗ còn lại sau khi những con khỉ ở hàng cụ thể đã ngồi. Điều này đảm bảo chúng ta đang tối đa hóa số lượng khỉ ngồi mà không vi phạm bất kỳ sở thích nào. Vì mọi phép toán đều là số học nên giải pháp đủ nhanh ngay cả đối với đầu vào lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    m, a, b, c = map(int, input().split())
    row1 = min(a, m)
    row2 = min(b, m)
    remaining_c = min(c, (m - row1) + (m - row2))
    print(row1 + row2 + remaining_c)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán số lượng khỉ có thể ngồi ở mỗi hàng bằng cách giới hạn số lượng khỉ của hàng cụ thể trong số lượng hàng. Sau đó, chúng tôi tính toán có bao nhiêu con khỉ linh hoạt (`c`) có thể xếp vừa các ghế còn lại và tính tổng các số này. sử dụng`min`đảm bảo chúng tôi không bao giờ vượt quá năng lực. sử dụng`max(0, ...)`ở đây là không cần thiết bởi vì`min(c, ...)`tự nhiên xử lý trường hợp không gian còn lại bằng không. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 2:```
3 6 1 1
```| Bước | một | b | c | hàng1 | hàng2 | còn lại_c | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 1 | 1 | phút(6,3)=3 | phút(1,3)=1 | phút(1,(3-3)+(3-1))=min(1,2)=1 | 3+1+1=5 | 

Điều này khẳng định rằng ngay cả khi`a`vượt quá`m`, chỉ một`m`những con khỉ có thể ngồi ở hàng đó và những con khỉ linh hoạt sẽ ngồi vào những chỗ trống còn lại. 

Đầu vào mẫu 1:```
10 5 5 10
```| Bước | một | b | c | hàng1 | hàng2 | còn lại_c | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 5 | 10 | phút(5,10)=5 | phút(5,10)=5 | phút(10,(10-5)+(10-5))=min(10,10)=10 | 5+5+10=20 | 

Điều này xác nhận rằng những con khỉ linh hoạt còn lại được phân bổ để lấp đầy tất cả không gian có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Tất cả các hoạt động là số học và thời gian không đổi. | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Từ bỏ$10^4$trường hợp thử nghiệm, điều này mang lại tổng cộng$O(10^4)$hoạt động, trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu và an toàn ở giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        m, a, b, c = map(int, input().split())
        row1 = min(a, m)
        row2 = min(b, m)
        remaining_c = min(c, (m - row1) + (m - row2))
        output.append(str(row1 + row2 + remaining_c))
    return "\n".join(output)

# Provided samples
assert run("5\n10 5 5 10\n3 6 1 1\n15 14 12 4\n1 1 1 1\n420 6 9 69\n") == "20\n5\n30\n2\n84"

# Custom tests
assert run("1\n1 1 1 1\n") == "2", "All equal and minimum"
assert run("1\n100000000 100000000 100000000 100000000\n") == "200000000", "Maximum inputs"
assert run("1\n3 3 3 3\n") == "6", "Equal distribution, row limits"
assert run("1\n5 0 0 10\n") == "10", "All flexible monkeys"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 2 | Đầu vào tối thiểu, tất cả đều bằng nhau | 
| 100000000 100000000 100000000 100000000 | 200000000 | Đầu vào tối đa được xử lý | 
| 3 3 3 3 | 6 | Xử lý đúng giới hạn hàng chính xác | 
| 5 0 0 10 | 10 | Tất cả những con khỉ linh hoạt đều có thể lấp đầy hàng | 

## Vỏ cạnh 

Nếu`a`vượt quá`m`Và`b`còn nhỏ, những con khỉ linh hoạt còn lại xếp vào hàng thứ hai. Vì`m = 3, a = 6, b = 1, c = 1`, hàng 1 chứa 3 con khỉ, hàng 2 chứa 1 con khỉ và 1 con khỉ linh hoạt lấp đầy chỗ còn lại ở hàng 2. Tổng cộng là`3 + 1 + 1 = 5`. Thuật toán tính toán chính xác `row1 = min(6
