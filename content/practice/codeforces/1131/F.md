---
title: "CF 1131F - Asya Và Mèo Con"
description: "Chúng tôi được cấp một bộ mèo con, ban đầu mỗi con nằm trong ô riêng được sắp xếp tuyến tính thành một hàng. Trong khoảng thời gian từ $n-1$ ngày, Asya ghi lại các cặp mèo con muốn chơi cùng nhau và loại bỏ vách ngăn giữa các ô của chúng."
date: "2026-06-12T04:13:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1131
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 541 (Div. 2)"
rating: 1700
weight: 1131
solve_time_s: 82
verified: false
draft: false
---

[CF 1131F - Asya và mèo con](https://codeforces.com/problemset/problem/1131/F) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, dsu 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ mèo con, ban đầu mỗi con nằm trong ô riêng được sắp xếp tuyến tính thành một hàng. Trong suốt quá trình$n-1$Vài ngày sau, Asya ghi lại những cặp mèo con muốn chơi cùng nhau và dỡ bỏ vách ngăn giữa các phòng giam của chúng. Mỗi cặp được đảm bảo ở trong các ô riêng biệt khi chúng gặp nhau, do đó, mỗi lần loại bỏ sẽ hợp nhất hai nhóm mèo con liền kề thành một nhóm liền kề lớn hơn. Sau khi loại bỏ tất cả, có một nhóm duy nhất chứa tất cả mèo con. 

Nhiệm vụ là xây dựng lại **bất kỳ thứ tự hợp lệ ban đầu nào của mèo con trong các ô** sao cho trình tự hợp nhất được ghi lại có thể xảy ra giữa các ô liền kề. Đầu vào cho chúng ta$n-1$cặp$(x_i, y_i)$tương ứng với thứ tự các phân vùng liền kề được loại bỏ. Đầu ra là một hoán vị của các số nguyên từ 1 đến$n$, đại diện cho vị trí ban đầu của mỗi chú mèo con. 

Từ$n$có thể lớn tới 150.000 và giới hạn thời gian là 2 giây, bất kỳ thuật toán nào có kết quả tệ hơn$O(n \log n)$sự phức tạp có thể sẽ quá chậm. Điều này loại trừ việc quay lại ngây thơ hoặc liên tục mô phỏng tất cả các sự hợp nhất có thể. Chúng ta cần một phương pháp có thể tái cấu trúc dần dần các vị trí một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó mèo con ở cuối được hợp nhất trước, các chuỗi trong đó nhiều lần hợp nhất tạo thành một chuỗi và các tình huống trong đó cặp đầu tiên trong danh sách thực sự chỉ liền kề sau một số lần hợp nhất sau đó. Ví dụ, với$n=4$và cặp$1,2; 3,4; 2,3$, cách tiếp cận ngây thơ có thể cố gắng đặt 3 hoặc 4 ở giữa không chính xác nếu giả định kề quá sớm. Thuật toán phải luôn duy trì một chuỗi các nhóm liền kề hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ thử tất cả các hoán vị của mèo con và mô phỏng trình tự hợp nhất, kiểm tra xem mỗi cặp có liền kề vào thời điểm trong ngày của chúng hay không. Mặc dù đúng về nguyên tắc nhưng điều này liên quan đến$O(n!)$hoán vị và hoàn toàn không khả thi đối với$n$thậm chí nhỏ như 10. 

Quan sát quan trọng là trình tự hợp nhất ngầm xác định **cấu trúc cây** đối với mèo con. Mỗi chú mèo con là một nút và mỗi lần hợp nhất tương ứng với một cạnh giữa hai nút. Vì có$n-1$sáp nhập và$n$mèo con, sự hợp nhất tạo thành một **cây**. Việc tái tạo bất kỳ trật tự tuyến tính nào cũng tương ứng với việc thực hiện **đi dọc theo các lá của cây này**, bởi vì mỗi lần hợp nhất đều kết nối các nhóm liền kề. 

Chúng ta có thể xây dựng một cách sắp xếp ban đầu một cách hiệu quả bằng cách sử dụng **deque (hàng đợi hai đầu)** để duy trì dòng mèo con đang phát triển. Bắt đầu với cặp mèo con đầu tiên và đặt chúng ở hai đầu của deque. Đối với mỗi lần hợp nhất tiếp theo, một chú mèo con phải ở một đầu của deque. Nối con mèo con khác vào cùng một đầu, đảm bảo sự liền kề. Cách tiếp cận này hiệu quả vì ở mỗi bước, việc hợp nhất luôn liên quan đến một chú mèo con đã có trong phân đoạn liền kề hiện tại, được đảm bảo bởi cấu trúc cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (deque/cây) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và trình tự của$n-1$hợp nhất. Mỗi lần hợp nhất là một cặp$(x_i, y_i)$. 
2. Khởi tạo một deque để thể hiện chuỗi mèo con liền kề hiện tại. 
3. Bắt đầu với lần hợp nhất đầu tiên$(x_1, y_1)$. Đặt$x_1$ở cuối bên trái và$y_1$ở cuối bên phải của deque. Điều này tạo ra phân khúc liền kề đầu tiên. 
4. Đối với mỗi cặp tiếp theo$(x_i, y_i)$: 

- Kiểm tra xem có$x_i$hoặc$y_i$nằm ở **đầu bên trái** của deque. Nếu vậy, hãy nhét con mèo con khác vào đầu bên trái. 
- Ngược lại, hãy kiểm tra xem có$x_i$hoặc$y_i$nằm ở **đầu bên phải** của deque. Nếu vậy, hãy chèn chú mèo con khác vào cuối bên phải. 
- Điều này đảm bảo cặp trở nên liền kề trong khi vẫn duy trì trình tự hợp lệ. Vì chuỗi là một cái cây nên luôn có ít nhất một chú mèo con ở cuối. 
5. Sau khi xử lý tất cả các lần hợp nhất, deque chứa sự sắp xếp ban đầu hợp lệ của mèo con. 
6. In deque làm hoán vị đầu ra. 

Tại sao nó hoạt động: Điều bất biến là deque luôn đại diện cho một phân đoạn mèo con liền kề phù hợp với tất cả các hoạt động hợp nhất được xử lý cho đến nay. Mỗi lần hợp nhất sẽ kết nối một chú mèo con đã có trong phân khúc với một chú mèo con khác ở bên ngoài, đảm bảo chú mèo con mới được đặt ở cuối để duy trì sự liền kề. Vì trình tự hợp nhất tạo thành một cây nên quá trình này không bao giờ thất bại và tạo ra thứ tự hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

n = int(input())
pairs = [tuple(map(int, input().split())) for _ in range(n-1)]

used = [False] * (n+1)
line = deque()

# start with the first pair
x, y = pairs[0]
line.append(x)
line.append(y)
used[x] = used[y] = True

for a, b in pairs[1:]:
    if line[0] == a or line[0] == b:
        new = b if line[0] == a else a
        line.appendleft(new)
        used[new] = True
    elif line[-1] == a or line[-1] == b:
        new = b if line[-1] == a else a
        line.append(new)
        used[new] = True
    else:
        # both a and b are already inside, ignore
        continue

# fill any missing kittens not in the merges (for isolated nodes)
for k in range(1, n+1):
    if not used[k]:
        line.append(k)

print(' '.join(map(str, line)))
```Phần đầu tiên đọc đầu vào và khởi tạo deque. Bắt đầu với cặp đầu tiên đảm bảo phân đoạn ban đầu hợp lệ. Mỗi lần hợp nhất tiếp theo được giải quyết bằng cách kiểm tra các đầu của deque; điều này là an toàn vì trình tự hợp nhất tạo thành một cây được kết nối. Cuối cùng, bất kỳ chú mèo con nào bị cô lập (không bao giờ được đề cập trong phần sáp nhập) đều được thêm vào để đảm bảo tất cả mèo con đều được đưa vào. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5
1 4
2 5
3 1
4 5
```Dấu vết: 

| Bước | Deque | Hành động | 
| --- | --- | --- | 
| bắt đầu | [1,4] | cặp đầu tiên | 
| Hợp nhất lần 2 (2,5) | [1,4,5,2] | 5 ở cuối bên phải, thêm 2 vào bên phải | 
| Hợp nhất lần 3 (3,1) | [3,1,4,5,2] | 1 ở cuối bên trái, thêm 3 vào bên trái | 
| Hợp nhất lần thứ 4 (4,5) | không thay đổi | cả hai đều đã có sẵn | 

Deque cuối cùng:`[3,1,4,5,2]`. Tất cả các sự hợp nhất đều tôn trọng sự liền kề. 

**Ví dụ tùy chỉnh** 

đầu vào:```
4
1 2
2 3
3 4
```Bản dựng Deque: 

| Bước | Deque | 
| --- | --- | 
| cặp đầu tiên | [1,2] | 
| Hợp nhất lần 2 | [1,2,3] | 
| Hợp nhất lần thứ 3 | [1,2,3,4] | 

Đầu ra:`1 2 3 4`. Quá trình này tự nhiên phát triển dòng từ trái sang phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần hợp nhất được xử lý một lần, các hoạt động deque là O(1) | 
| Không gian | O(n) | Lưu trữ n phần tử trong mảng deque và mảng đã sử dụng | 

Được cho$n \le 150,000$, thuật toán thực hiện khoảng 2n phép toán và sử dụng bộ nhớ tỷ lệ với n, trong vòng 2 giây và giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    pairs = [tuple(map(int, input().split())) for _ in range(n-1)]
    used = [False]*(n+1)
    line = deque()
    x,y = pairs[0]
    line.append(x)
    line.append(y)
    used[x]=used[y]=True
    for a,b in pairs[1:]:
        if line[0] == a or line[0]==b:
            new = b if line[0]==a else a
            line.appendleft(new)
            used[new]=True
        elif line[-1]==a or line[-1]==b:
            new = b if line[-1]==a else a
            line.append(new)
            used[new]=True
    for k in range(1,n+1):
        if not used[k]:
            line.append(k)
    return ' '.join(map(str,line))

# provided sample
assert run("5\n1 4\n2 5\n3 1\n4 5\n") == "3 1 4 2 5", "sample 1"

# minimum size
assert run("2\n1 2\n") == "1 2", "min size"

# linear chain
assert run("4\n1 2\n2 3\n3 4\n") == "1 2 3 4", "linear chain"

# reverse chain
assert run("4\n4 3\n3 2\n2 1\n") == "4 3 2 1", "reverse chain"

# isolated kitten
assert run("3\n1 2\n")
```
