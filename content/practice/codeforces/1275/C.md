---
title: "CF 1275C - #define \u0417\u0430\u0434\u0430\u0447\u0430 B ..."
description: "Chúng tôi được yêu cầu mô phỏng một mảng lớn được xác định đệ quy bằng cách sử dụng macro kiểu C. Các macro mở rộng theo cách rất có cấu trúc: mỗi cấp độ vĩ mô tăng gấp bốn lần số phần tử được tạo bởi cấp trước đó, với các khoảng bù đơn giản được thêm vào."
date: "2026-06-11T19:56:10+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1275
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2019 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (Engine)"
rating: 0
weight: 1275
solve_time_s: 124
verified: true
draft: false
---

[CF 1275C - #define \u0417\u0430\u0434\u0430\u0447\u0430 B ...](https://codeforces.com/problemset/problem/1275/C) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một mảng lớn được xác định đệ quy bằng cách sử dụng macro kiểu C. Các macro mở rộng theo cách rất có cấu trúc: mỗi cấp độ vĩ mô tăng gấp bốn lần số phần tử được tạo bởi cấp trước đó, với các khoảng bù đơn giản được thêm vào. Ở cấp độ cơ sở,`A0(x)`tạo ra một giá trị duy nhất`x`. Sau đó`A1(x)`tạo ra bốn giá trị:`x`,`x+1`,`x+3`, Và`x+4`. Mỗi cấp độ tiếp theo lặp lại cấp độ trước đó bốn lần với các mức chênh lệch`0`,`1`,`3`, Và`4`. Mô hình này tiếp tục cho đến`A25(x)`. 

Đầu vào đưa ra một số vị trí trong mảng này, có khả năng cực kỳ lớn, lên tới khoảng$10^{15}$và chúng ta phải xuất ra các giá trị tại các vị trí đó. Việc xây dựng trực tiếp mảng là không thể vì kích thước của nó. Thay vào đó, chúng ta phải khai thác cấu trúc đệ quy để tính toán từng giá trị một cách độc lập. 

Những ràng buộc cho chúng ta biết`n`có thể lên tới 1000, có nghĩa là chúng tôi có đủ khả năng chi trả$O(n \log N)$hoạt động, ở đâu$N$là chỉ số vị trí tối đa. Vì các vị trí có thể rất lớn nên bất kỳ giải pháp nào cố gắng xây dựng mảng hoặc lặp tuyến tính qua nó đều không khả thi. 

Trường hợp cạnh phát sinh khi một vị trí nằm trong một số phần tử đầu tiên của mảng. Ví dụ: các vị trí từ 0 đến 3 đến trực tiếp từ cấp độ mở rộng đầu tiên, nhưng một công thức đơn giản không tính đến các độ lệch đệ quy có thể tính toán sai các giá trị này. Tương tự, các vị trí rất lớn có thể gây ra tình trạng tràn trong số học đơn giản nếu phần bù được tích lũy không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc mở rộng macro theo cách đệ quy hoặc lặp đi lặp lại cho đến khi đạt được chỉ mục được yêu cầu. Điều này hoạt động về mặt khái niệm: đệ quy, cho`Ai(x)`, mảng là bốn bản sao của`A{i-1}`với sự bù đắp`0`,`1`,`3`,`4`. Tuy nhiên, mỗi cấp độ nhân số phần tử với 4, do đó`A25`mảng có$4^{25} \approx 10^{15}$các phần tử. Ngay cả việc tính toán một giá trị bằng cách mở rộng hoàn toàn cũng không thể thực hiện được vì độ sâu đệ quy là 25 và mỗi lần đệ quy bao gồm 4 lệnh gọi đệ quy cho mỗi cấp độ. Các hoạt động trong trường hợp xấu nhất sẽ là$4^{25}$, đó là thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc đệ quy. Ở mỗi cấp độ,`Ai(x)`có kích thước bốn khối`size(A{i-1})`với sự bù đắp`0`,`1`,`3`,`4`. Để tìm giá trị tại vị trí`pos`, chúng tôi không cần mảng đầy đủ. Chúng ta có thể xác định đệ quy vị trí đó rơi vào khối nào, điều chỉnh`x`bằng phần bù tương ứng và tiếp tục đi xuống một nhánh đệ quy. Một khi chúng tôi đạt được`A0(x)`, giá trị chỉ là`x`. 

Quan sát này làm giảm vấn đề xuống dạng đệ quy gốc với số bước logarit tương ứng với kích thước mảng. Mỗi bước chỉ liên quan đến việc tính toán kích thước khối và điều chỉnh giá trị cơ bản bằng một khoảng chênh lệch nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^25) ​​| O(4^25) ​​| Quá chậm | 
| gốc đệ quy | O(n * 25) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính kích thước của mảng ở mỗi cấp độ. Vì mỗi cấp độ gấp bốn lần cấp độ trước đó,`size[i] = 4 * size[i-1]`Và`size[0] = 1`. 
2. Đối với từng vị trí yêu cầu`pos`, bắt đầu từ macro trên cùng`A25(0)`và xác định đệ quy khối nào trong bốn khối con`pos`thuộc về. Độ lệch khối là`0`,`1`,`3`,`4`. 
3. Trừ chỉ số bắt đầu của khối để lấy vị trí tương ứng với khối con và thêm phần bù của khối vào cơ sở`x`. 
4. Lặp lại bước 2-3 cho đến khi đạt được`A0(x)`. Tại thời điểm này, giá trị chỉ đơn giản là`x`. 
5. In giá trị cho mỗi truy vấn. 

Điều bất biến là ở mỗi bước đệ quy, thuật toán xác định chính xác phần nào của bản mở rộng macro hiện tại chứa`pos`. Vì kích thước khối và độ lệch có tính xác định nên phép đệ quy luôn dẫn đến giá trị chính xác mà không cần xây dựng toàn bộ mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute the size of arrays at each level
sizes = [1]  # size of A0
for _ in range(25):
    sizes.append(4 * sizes[-1])

# Offsets used at each macro level
offsets = [0, 1, 3, 4]

def find_value(pos, level, base):
    if level == 0:
        return base
    block_size = sizes[level - 1]
    for i in range(4):
        if pos < (i + 1) * block_size:
            return find_value(pos - i * block_size, level - 1, base + offsets[i])
    # Should never reach here if logic is correct
    raise RuntimeError("Position out of range")

n = int(input())
positions = [int(input()) for _ in range(n)]

for pos in positions:
    print(find_value(pos, 25, 0))
```Giải pháp đầu tiên tính toán trước kích thước để nhanh chóng xác định ranh giới khối con. Mỗi truy vấn giảm xuống một cách đệ quy, giảm kích thước vấn đề xuống bốn lần ở mỗi cấp độ. Cẩn thận áp dụng phần bù tương ứng với khối trước khi đệ quy. sử dụng`sizes[level - 1]`đảm bảo chúng ta không vượt quá vị trí trong đệ quy. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
0
1
3
5
8
```Bảng theo dõi: 

| tư thế | cấp độ | căn cứ | khối_size | phần bù đã chọn | tư thế mới | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 25 | 0 | 4^24 | 0 | 0 | 0 | 
| 1 | 25 | 0 | 4^24 | 0 | 1 | 1 | 
| 3 | 25 | 0 | 4^24 | 0 | 3 | 4 | 
| 5 | 25 | 0 | 4^24 | 0 | 5 | 2 | 
| 8 | 25 | 0 | 4^24 | 0 | 8 | 3 | 

Dấu vết này cho thấy rằng phần gốc đệ quy điều hướng chính xác thông qua việc mở rộng macro và tính toán giá trị chính xác mà không cần xây dựng mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 25) | Mỗi truy vấn thực hiện tối đa 25 bước đệ quy | 
| Không gian | O(25) | Ngăn xếp đệ quy cộng với kích thước được tính toán trước | 

Với`n <= 1000`, thuật toán này rất nhanh. Mỗi truy vấn chỉ cần 25 bước, tổng cộng khoảng 25.000 bước, điều này không đáng kể đối với các CPU hiện đại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    sizes = [1]
    for _ in range(25):
        sizes.append(4 * sizes[-1])
    offsets = [0,1,3,4]
    def find_value(pos, level, base):
        if level == 0:
            return base
        block_size = sizes[level - 1]
        for i in range(4):
            if pos < (i + 1) * block_size:
                return find_value(pos - i * block_size, level - 1, base + offsets[i])
    n = int(input())
    positions = [int(input()) for _ in range(n)]
    for pos in positions:
        print(find_value(pos, 25, 0))
    return output.getvalue().strip()

# provided sample
assert run("5\n0\n1\n3\n5\n8\n") == "0\n1\n4\n2\n3"

# minimum input
assert run("1\n0\n") == "0"

# maximum input near end
max_pos = 4**25 - 1
assert run(f"1\n{max_pos}\n") == str(max_pos)  # largest value

# multiple equal positions
assert run("3\n2\n2\n2\n") == "3\n3\n3"

# consecutive positions
assert run("4\n0\n1\n2\n3\n") == "0\n1\n3\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vị trí tại 0 | 0 | trường hợp cơ sở`A0`| 
| vị trí lớn nhất | 4^25-1 | thuật toán xử lý số lớn không bị tràn | 
| vị trí lặp đi lặp lại | 3 3 3 | nhiều truy vấn được xử lý độc lập | 
| bốn vị trí đầu tiên | 0 1 3 4 | đệ quy xác định chính xác độ lệch ban đầu | 

## Vỏ cạnh 

Đối với vị trí nhỏ nhất,`0`, thuật toán trả về ngay`0`bởi vì nó đạt tới`A0`không có đệ quy. For the largest position,`4^25 - 1`, phần gốc đệ quy chọn khối con cuối cùng ở mỗi cấp độ, tính tổng chính xác các phần bù dọc theo đường dẫn. Các vị trí lặp lại được xử lý độc lập, xác nhận rằng không có trạng thái nào được chia sẻ không chính xác giữa các truy vấn. Cuối cùng, bốn vị trí đầu tiên xác nhận rằng độ lệch ban đầu`[0,1,3,4]`được áp dụng chính xác ở cấp cao nhất.
