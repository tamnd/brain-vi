---
title: "CF 1399B - Sửa quà tặng"
description: "Chúng ta được tặng n món quà, trong đó mỗi món quà gồm một số kẹo nhất định và một số cam nhất định. Mỗi phần quà em đều có kẹo ai và bi cam. Mục tiêu của chúng tôi là làm cho tất cả quà tặng giống hệt nhau về cả kẹo và cam, sử dụng ít động tác nhất có thể."
date: "2026-06-11T09:03:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1399
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 661 (Div. 3)"
rating: 800
weight: 1399
solve_time_s: 89
verified: true
draft: false
---

[CF 1399B - Sửa quà tặng](https://codeforces.com/problemset/problem/1399/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`quà tặng, trong đó mỗi món quà bao gồm một số lượng kẹo nhất định và một số lượng cam nhất định. Mỗi món quà`i`có`a_i`kẹo và`b_i`cam. Mục tiêu của chúng tôi là làm cho tất cả quà tặng giống hệt nhau về cả kẹo và cam, sử dụng ít động tác nhất có thể. Trong một nước đi, chúng ta có thể ăn một viên kẹo, một quả cam hoặc cả hai từ một món quà, nhưng chúng ta không thể lấy nhiều hơn những gì món quà chứa đựng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta được cung cấp số lượng quà tặng và hai mảng: một mảng dành cho kẹo và một mảng dành cho cam. Chúng ta cần xuất ra số lần di chuyển tối thiểu cần thiết để làm cho tất cả các phần quà đều bằng nhau. 

Những hạn chế ở mức vừa phải:`n`có thể lên tới 50 và mỗi`a_i`hoặc`b_i`có thể lớn như`10^9`. Điều này có nghĩa là chúng tôi không thể mô phỏng từng bước di chuyển riêng lẻ nếu số lượng quá lớn, vì điều đó sẽ quá chậm. Thay vào đó, chúng ta cần một phương pháp tính tổng số bước đi trực tiếp từ hiệu. 

Các trường hợp khó khăn có thể gặp phải một giải pháp ngây thơ bao gồm: khi tất cả quà tặng đều có cùng số kẹo hoặc cam hoặc khi một món quà có số lượng kẹo hoặc cam rất lớn so với những món quà còn lại. Ví dụ, nếu`a = [1, 1000000000]`Và`b = [1, 1]`, một mô phỏng ngây thơ về việc loại bỏ từng viên kẹo một sẽ không khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng chọn quà nhiều lần và giảm kẹo và cam cho đến khi tất cả quà đều khớp. Điều này đúng nhưng không hiệu quả vì mỗi thao tác có thể được lặp lại tới`10^9`lần để nhận một số quà tặng. Với`n=50`, điều đó có thể dẫn đến`50 * 10^9`hoạt động, điều này là không thể trong vòng 1 giây. 

Quan sát quan trọng dẫn đến giải pháp tối ưu là số lượng kẹo mục tiêu cho tất cả quà tặng ít nhất phải là mức tối thiểu trong số tất cả các loại kẹo.`a_i`, và tương tự, số lượng cam mục tiêu ít nhất phải là số lượng tối thiểu trong số tất cả các quả cam`b_i`. Việc giảm mọi món quà xuống mức tối thiểu tương ứng luôn khả thi và yêu cầu ít bước di chuyển nhất cho mỗi món quà. Đối với một món quà duy nhất, chúng ta có thể thực hiện các động tác “cả hai cùng một lúc” một cách tối ưu. Đặc biệt, đối với một món quà có`a_i`kẹo và`b_i`cam, nếu chúng ta muốn giảm nó xuống`min_a`Và`min_b`, chúng ta có thể giảm cả hai cùng lúc bao nhiêu lần tùy thích`min(a_i - min_a, b_i - min_b)`, sau đó hoàn thành phần còn lại một cách riêng lẻ. Cách tiếp cận tham lam này đảm bảo di chuyển tối thiểu. 

So sánh cách tiếp cận tối ưu và bạo lực là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i, b_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và các mảng`a`Và`b`đại diện cho kẹo và cam. 
2. Tính toán`min_a = min(a)`Và`min_b = min(b)`. Đây là số lượng kẹo và cam mục tiêu cho tất cả quà tặng. 
3. Khởi tạo`moves = 0`. Điều này sẽ tích lũy tổng số bước di chuyển cần thiết. 
4. Lặp lại từng món quà`i`từ 0 đến`n-1`. 
5. Tặng quà`i`, tính số kẹo thừa`delta_a = a[i] - min_a`và những quả cam thừa`delta_b = b[i] - min_b`. 
6. Số lượt di chuyển của phần quà này là`max(delta_a, delta_b)`. Chúng tôi sử dụng tối đa vì chúng tôi có thể giảm đồng thời cả kẹo và cam bất cứ khi nào có thể. Ví dụ, nếu`delta_a=3`Và`delta_b=1`, một nước đi có thể giảm cả hai, chỉ để lại 2 nước đi cho kẹo, tổng cộng`max(3,1)=3`. 
7. Thêm chiêu thức cho món quà này vào`moves`. 
8. Sau khi xử lý xong tất cả quà tặng, xuất ra tổng số quà`moves`. 

Tại sao nó hoạt động: điều bất biến là chúng tôi luôn giảm thiểu chính xác từng món quà bằng số kẹo và cam tối thiểu. Bằng cách thực hiện các động tác "cả hai cùng một lúc" một cách tham lam, chúng tôi không bao giờ thực hiện việc giảm bớt một mục không cần thiết. Mỗi bước di chuyển cần thiết của món quà là độc lập, vì vậy việc tính tổng chúng sẽ mang lại mức tối thiểu chung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    min_a = min(a)
    min_b = min(b)
    
    moves = 0
    for i in range(n):
        delta_a = a[i] - min_a
        delta_b = b[i] - min_b
        moves += max(delta_a, delta_b)
    
    print(moves)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc các mảng, tính toán số kẹo và cam tối thiểu, sau đó lặp qua từng món quà để tính toán các bước di chuyển tối thiểu bằng cách sử dụng mức chênh lệch tối đa. Cách tiếp cận này trực tiếp thực hiện thuật toán tối ưu và xử lý các số rất lớn một cách hiệu quả. Sự lựa chọn của`max(delta_a, delta_b)`đảm bảo chúng tôi áp dụng mức giảm kết hợp một cách tối ưu trước tiên. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
3
3 5 6
3 2 3
```Dấu vết bước: 

| Quà tặng | a_i | b_i | phút_a | phút_b | delta_a | delta_b | di chuyển_i | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 2 | 0 | 1 | 1 | 
| 2 | 5 | 2 | 3 | 2 | 2 | 0 | 2 | 
| 3 | 6 | 3 | 3 | 2 | 3 | 1 | 3 | 

Tổng số nước đi = 1 + 2 + 3 = 6 

Điều này phù hợp với đầu ra mẫu. 

**Ví dụ 2** 

đầu vào:```
5
1 2 3 4 5
5 4 3 2 1
```Dấu vết bước: 

| Quà tặng | a_i | b_i | phút_a | phút_b | delta_a | delta_b | di chuyển_i | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 1 | 1 | 0 | 4 | 4 | 
| 2 | 2 | 4 | 1 | 1 | 1 | 3 | 3 | 
| 3 | 3 | 3 | 1 | 1 | 2 | 2 | 2 | 
| 4 | 4 | 2 | 1 | 1 | 3 | 1 | 3 | 
| 5 | 5 | 1 | 1 | 1 | 4 | 0 | 4 | 

Tổng số nước đi = 4 + 3 + 2 + 3 + 4 = 16 

Phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính min và lặp lại một lần trên n quà tặng | 
| Không gian | O(n) | Lưu trữ mảng`a`Và`b`| 

Với`t <= 1000`Và`n <= 50`, tổng số thao tác nhiều nhất là`1000*50 = 50,000`, dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
# helper function
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        min_a = min(a)
        min_b = min(b)
        
        moves = 0
        for i in range(n):
            delta_a = a[i] - min_a
            delta_b = b[i] - min_b
            moves += max(delta_a, delta_b)
        print(moves)
    return output.getvalue().strip()

# provided samples
assert run("5\n3\n3 5 6\n3 2 3\n5\n1 2 3 4 5\n5 4 3 2 1\n3\n1 1 1\n2 2 2\n6\n1 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000\n1 1 1 1 1 1\n3\n10 12 8\n7 5 4") == "6\n16\n0\n4999999995\n7"

# custom tests
assert run("1\n1\n100\n100") == "0", "single gift"
assert run("1\n2\n1 100\n1 1") == "99", "one gift large"
assert run("1\n3\n5 5 5\n2 2 2") == "0",
```
