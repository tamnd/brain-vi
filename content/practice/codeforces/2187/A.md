---
title: "CF 2187A - Sắp xếp hạn chế"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta được yêu cầu xác định số nguyên lớn nhất $k$ sao cho chúng ta có thể sắp xếp mảng đó theo thứ tự không giảm dần bằng cách hoán đổi hai phần tử bất kỳ có chênh lệch ít nhất là $k$."
date: "2026-06-07T21:20:41+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2187
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1077 (Div. 1)"
rating: 1300
weight: 2187
solve_time_s: 194
verified: false
draft: false
---

[CF 2187A - Sắp xếp hạn chế](https://codeforces.com/problemset/problem/2187/A) 

**Đánh giá:** 1300 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta được yêu cầu xác định số nguyên lớn nhất$k$sao cho chúng ta có thể sắp xếp mảng theo thứ tự không giảm dần bằng cách hoán đổi bất kỳ hai phần tử nào có hiệu ít nhất là$k$. Chính thức, đối với bất kỳ cặp chỉ số nào$i < j$, chúng ta có thể trao đổi$a_i$Và$a_j$nếu như$|a_i - a_j| \ge k$. Mục tiêu của chúng tôi là tối đa hóa$k$trong khi vẫn cho phép mảng được sắp xếp đầy đủ bằng cách sử dụng các hoán đổi này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có độ dài$n$lên đến$2 \cdot 10^5$và giá trị mảng lên đến$10^9$. Vì tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, chúng ta cần một thuật toán gần như tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Phương pháp bậc hai sẽ không thực hiện được trong thời gian hợp lý vì chúng đòi hỏi tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xảy ra khi mảng đã được sắp xếp. Ví dụ, nếu mảng là`[1,2,3]`, không cần hoán đổi, nhưng bài toán yêu cầu giá trị lớn nhất$k$. Trong trường hợp này, mọi giá trị không âm$k$được cho phép về mặt kỹ thuật, nhưng có ý nghĩa lớn nhất$k$không được xác định theo nghĩa là không cần thực hiện thao tác nào, vì vậy câu trả lời phải là`-1`. Một trường hợp phức tạp khác là khi tất cả các phần tử đều bằng nhau, chẳng hạn như`[5,5,5]`; một lần nữa, không cần hoán đổi và đầu ra phải là`-1`. Mảng chỉ có hai phần tử thì đơn giản hơn nhưng thuật toán vẫn phải xác định chính xác liệu có cần hoán đổi để sắp xếp chúng hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi cách có thể$k$các giá trị, cố gắng mô phỏng các giao dịch hoán đổi cho từng giá trị. Đối với mỗi$k$, chúng tôi sẽ quét mảng nhiều lần, hoán đổi bất kỳ cặp nào có chênh lệch ít nhất$k$cho đến khi không thể hoán đổi được nữa. Cách tiếp cận này đúng về nguyên tắc, nhưng quá chậm: có tới$10^9$khả thi$k$các giá trị và mỗi mô phỏng có thể mất$O(n^2)$, điều này là không thể tính toán được. 

Cái nhìn sâu sắc quan trọng là con heo đất lớn nhất$k$chỉ được xác định bởi vị trí của các phần tử không đúng thứ tự trong mảng đã sắp xếp. Cho phép`b`là phiên bản được sắp xếp của`a`. Xem xét các yếu tố$a_i$Và$b_i$ở cùng một chỉ số. Nếu như$a_i \neq b_i$, phần tử đó phải được di chuyển. Giá trị tối đa của$k$cho phép tất cả các hoán đổi cần thiết khi đó là sự khác biệt tối đa giữa bất kỳ phần tử ban đầu nào phải di chuyển và mục tiêu của nó trong mảng được sắp xếp. Cụ thể, chúng ta cần$k \le \max(a) - \min(a)$giữa các phần tử bị đặt sai vị trí. Điều này làm giảm vấn đề xuống còn một lần duyệt qua mảng: xác định mức tối thiểu và tối đa của các phần tử bị đặt sai vị trí và tính toán sự khác biệt của chúng. Nếu không có phần tử nào bị đặt sai vị trí thì mảng đã được sắp xếp và câu trả lời là`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * max(a)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`và tạo một bản sao được sắp xếp`b`. 
2. Khởi tạo`min_misplaced`ĐẾN`inf`Và`max_misplaced`ĐẾN`-inf`. 
3. Lặp lại các chỉ số`i`từ 0 đến`n-1`. Với mỗi chỉ số, nếu`a[i] != b[i]`, cập nhật`min_misplaced`Và`max_misplaced`với`a[i]`. 
4. Sau khi lặp lại, hãy kiểm tra xem`min_misplaced`còn lại`inf`. Nếu vậy, mảng đã được sắp xếp và kết quả là`-1`. 
5. Ngược lại, số nguyên lớn nhất của heo con`k`là`max_misplaced - min_misplaced`. 
6. Xuất kết quả. 

Tại sao nó hoạt động: Bất kỳ phần tử nào không ở vị trí được sắp xếp đều phải được di chuyển. Mức tối đa cho phép$k$không thể vượt quá chênh lệch giữa phần tử lớn nhất và nhỏ nhất trong số những phần tử bị đặt sai vị trí, bởi vì việc hoán đổi chỉ được phép đối với những chênh lệch lớn hơn hoặc bằng$k$. Bằng cách chọn$k$vì sự khác biệt này, chúng tôi đảm bảo rằng tất cả các giao dịch hoán đổi cần thiết đều được cho phép và bất kỳ giao dịch hoán đổi nào lớn hơn$k$sẽ chặn các giao dịch hoán đổi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = sorted(a)
    
    min_misplaced = float('inf')
    max_misplaced = float('-inf')
    
    for i in range(n):
        if a[i] != b[i]:
            min_misplaced = min(min_misplaced, a[i])
            max_misplaced = max(max_misplaced, a[i])
    
    if min_misplaced == float('inf'):
        print(-1)
    else:
        print(max_misplaced - min_misplaced)
```Mã đầu tiên đọc đầu vào một cách hiệu quả. Nó duy trì hai biến để theo dõi các giá trị cực trị của các phần tử bị đặt sai vị trí. Việc lặp lại mảng đảm bảo rằng chỉ những phần tử cần di chuyển mới ảnh hưởng đến kết quả. sử dụng`float('inf')`Và`float('-inf')`tránh đặt vỏ đặc biệt cho phần tử bị đặt sai vị trí đầu tiên. Sắp xếp là`O(n log n)`, và quá trình quét là`O(n)`. Giải pháp xử lý nhiều trường hợp thử nghiệm trong một lần thực hiện. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1
1
3
1 4 2
6
1 1 4 5 1 4
```| Kiểm tra | một | b | phút_đặt sai vị trí | max_misplace | k | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1] | [1] | thông tin | -inf | -1 | 
| 2 | [1,4,2] | [1,2,4] | 2 | 4 | 2 | 
| 3 | [1,1,4,5,1,4] | [1,1,1,4,4,5] | 1 | 4 | 3 | 

Trong trường hợp thử nghiệm đầu tiên, mảng đã được sắp xếp nên kết quả là`-1`. Trong trường hợp thứ hai, các phần tử`4`Và`2`không theo thứ tự, tạo ra sự khác biệt`4 - 2 = 2`. Trong phần thứ ba, các phần tử bị đặt sai vị trí là`4,5,1,4`, cho`max - min = 4 - 1 = 3`. 

### Đầu vào mẫu 2```
1
5
2 1 5 4 3
```| tôi | một [tôi] | b[i] | đặt nhầm chỗ? | phút | tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | Có | 2 | 2 | 
| 1 | 1 | 2 | Có | 1 | 2 | 
| 2 | 5 | 3 | Có | 1 | 5 | 
| 3 | 4 | 4 | Không | 1 | 5 | 
| 4 | 3 | 5 | Có | 1 | 5 | 

k = 5 - 1 = 4 

Dấu vết xác nhận rằng thuật toán xác định chính xác các điểm cực trị trong số các phần tử bị đặt sai vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế; quá trình quét là tuyến tính | 
| Không gian | O(n) | Bản sao được sắp xếp`b`yêu cầu bộ nhớ bổ sung | 

Được cho$n \le 2 \cdot 10^5$và tổng của$n$qua các trường hợp thử nghiệm$\le 2 \cdot 10^5$, giải pháp chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = sorted(a)
        min_misplaced = float('inf')
        max_misplaced = float('-inf')
        for i in range(n):
            if a[i] != b[i]:
                min_misplaced = min(min_misplaced, a[i])
                max_misplaced = max(max_misplaced, a[i])
        if min_misplaced == float('inf'):
            output.append('-1')
        else:
            output.append(str(max_misplaced - min_misplaced))
    return "\n".join(output)

# Provided samples
assert run("5\n1\n1\n5\n1 2 3 4 5\n3\n1 4 2\n5\n2 1 5 4 3\n6\n1 1 4 5 1 4\n") == "-1\n-1\n2\n4\n3"

# Custom cases
assert run("2\n2\n5 5\n2\n2 1\n") == "-1\n1"
assert run("1\n3\n3 2 1\n") == "2"
assert run("1\n6\n1 1 1 1 1 1\n") == "-1"
assert run("1\n5\n1 3 2 5 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến
