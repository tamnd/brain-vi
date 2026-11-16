---
title: "CF 188A - Số lục giác"
description: "Chúng ta được yêu cầu tính số lục giác thứ n. Các số lục giác biểu thị một mẫu số hình tượng trong đó mỗi số đếm tổng số ô tạo thành một hình lục giác với các lớp liên tiếp. Công thức cho số lục giác thứ n được cho bởi $Hn = 2n^2 - n$."
date: "2026-06-05T00:32:29+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "A"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1100
weight: 188
solve_time_s: 75
verified: true
draft: false
---

[CF 188A - Số lục giác](https://codeforces.com/problemset/problem/188/A) 

**Đánh giá:** 1100 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số lục giác thứ _n_. Các số lục giác biểu thị một mẫu số hình tượng trong đó mỗi số đếm tổng số ô tạo thành một hình lục giác với các lớp liên tiếp. Công thức cho số lục giác thứ _n_ được đưa ra bởi$H_n = 2n^2 - n$. Về mặt khái niệm, số lục giác đầu tiên là một ô đơn, số thứ hai thêm một lớp xung quanh để tạo thành một hình lục giác lớn hơn, v.v. 

Đầu vào là một số nguyên duy nhất$n$biểu thị số lục giác nào cần tính toán, bị giới hạn ở 1   n    100. Đầu ra là một số nguyên duy nhất, giá trị của số lục giác tương ứng. 

Với hạn chế$n \le 100$, việc tính toán vẫn còn nhỏ. Ngay cả giá trị lớn nhất,$H_{100} = 2 \cdot 100^2 - 100 = 19900$, dễ dàng phù hợp với số nguyên 32 bit hoặc 64 bit tiêu chuẩn. Điều này có nghĩa là không cần xử lý đặc biệt các số nguyên lớn hoặc tối ưu hóa để thực hiện. Một cách tiếp cận ngây thơ đã có hiệu quả rồi, nhưng việc suy luận cẩn thận về công thức sẽ đảm bảo tính đúng đắn. 

Các trường hợp cạnh tiềm năng bao gồm đầu vào nhỏ nhất$n = 1$sẽ xuất ra 1 và xác nhận tỷ lệ công thức chính xác cho$n = 100$. Việc thực hiện bất cẩn có thể áp dụng sai công thức, quên mất phép trừ$n$hoặc sử dụng phép tính dấu phẩy động gây ra lỗi làm tròn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng số lục giác lặp đi lặp lại bằng cách tính tổng các ô trong mỗi lớp cho đến$n$-lớp thứ. Ví dụ, người ta có thể tính tổng$1 + 6 + 12 + 18 + \dots$trong đó mỗi thuật ngữ tương ứng với các ô được thêm vào mỗi lớp mới. Mặc dù đúng nhưng cách tiếp cận này không cần thiết vì công thức$H_n = 2n^2 - n$trực tiếp đưa ra câu trả lời. Việc thêm các lớp lặp đi lặp lại là dư thừa và làm tăng độ phức tạp, tuy nhiên đối với$n \le 100$nó vẫn sẽ thực thi ngay lập tức. 

Quan sát quan trọng là bài toán cung cấp một công thức dạng đóng. Nhận thức được điều này cho phép tính toán trực tiếp trong thời gian không đổi. Không có sự phụ thuộc vào các số trước đó ngoài tính toán$n^2$và thực hiện các phép tính đơn giản. Vấn đề giảm xuống việc đánh giá biểu thức bậc hai cho đầu vào$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tổng các lớp lặp đi lặp lại) | O(n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức (tính trực tiếp) | O(1) | O(1) | Được chấp nhận và tối ưu | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào. Điều này thể hiện số lục giác nào chúng ta cần tính toán. 
2. Tính toán$n^2$. Điều này thể hiện sự tăng trưởng bậc hai của các số lục giác khi số lớp tăng lên. 
3. Nhân$n^2$bằng 2. Mẫu số lục giác nhân đôi số hạng bình phương để tính các ô mở rộng ra ngoài theo sáu hướng. 
4. Trừ$n$từ kết quả. Điều này khắc phục việc đếm quá mức ô trung tâm trong bước nhân đôi, căn chỉnh với công thức$H_n = 2n^2 - n$. 
5. In kết quả. Đây là giá trị của$n$-số lục giác thứ 

Tại sao nó hoạt động: Điều bất biến là công thức đếm chính xác số lượng ô xếp theo hình lục giác lên đến$n$-lớp thứ. Mỗi lớp thêm một số ô có thể dự đoán được và số hạng bậc hai thể hiện sự tăng trưởng, trong khi phép trừ tuyến tính sẽ điều chỉnh cho ô trung tâm duy nhất được tính nhiều lần. Vì chúng ta áp dụng công thức một cách trực tiếp nên không có bước lặp nào có thể gây ra lỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
hex_num = 2 * n * n - n
print(hex_num)
```Mã đọc một số nguyên duy nhất, tính số lục giác bằng công thức dạng đóng và in nó. Nhân trước khi trừ đảm bảo số học số nguyên xuyên suốt. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh, mặc dù ở đây sự khác biệt là không đáng kể do chỉ có một dòng. Thứ tự nhân tránh lỗi ưu tiên của toán tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```| Bước | n | 2*n^2 | 2*n^2 - n | hex_num | 
| --- | --- | --- | --- | --- | 
| Tính toán | 3 | 18 | 18 - 3 = 15 | 15 | 

Tính toán phù hợp với kết quả đầu ra dự kiến. Mỗi bước đều phù hợp với thuật toán, xác nhận rằng công thức hoạt động với n nhỏ. 

### Ví dụ 2 

đầu vào:```
1
```| Bước | n | 2*n^2 | 2*n^2 - n | hex_num | 
| --- | --- | --- | --- | --- | 
| Tính toán | 1 | 2 | 2 - 1 = 1 | 1 | 

Điều này theo dõi đầu vào nhỏ nhất. Đầu ra xác nhận thuật toán xử lý chính xác các trường hợp biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện bất kể n. | 
| Không gian | O(1) | Chỉ có một biến số nguyên duy nhất được sử dụng. | 

Cho mức tối đa$n = 100$, các hoạt động là tầm thường. Giải pháp dễ dàng thực thi trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    return str(2 * n * n - n)

# Provided sample
assert run("3\n") == "15", "sample 1"

# Custom cases
assert run("1\n") == "1", "minimum n"
assert run("100\n") == "19900", "maximum n"
assert run("50\n") == "4950", "mid-range n"
assert run("2\n") == "6", "small n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Ranh giới đầu vào tối thiểu | 
| 100 | 19900 | Ranh giới đầu vào tối đa | 
| 50 | 4950 | Độ chính xác tầm trung | 
| 2 | 6 | Độ chính xác đầu vào nhỏ | 

## Vỏ cạnh 

cho$n = 1$, thuật toán tính toán$2*1^2 - 1 = 1$, chính xác là số lục giác đầu tiên. Vì$n = 100$, tính toán$2*100^2 - 100 = 19900$xác nhận tỷ lệ công thức một cách chính xác. Cả hai trường hợp đều chứng minh thuật toán xử lý các giới hạn cực độ mà không có lỗi sai lệch một hoặc tràn số nguyên. Thuật toán áp dụng nhất quán công thức cho bất kỳ$n$trong những ràng buộc.
