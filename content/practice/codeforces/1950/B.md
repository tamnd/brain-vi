---
title: "CF 1950B - Nâng cấp"
description: "Chúng tôi được yêu cầu tạo một mẫu bàn cờ được làm bằng các ô $2 nhân 2$ lớn hơn. Mỗi ô được điền đầy đủ ký tự hoặc được điền đầy đủ .. Kích thước của lưới được xác định bởi một số nguyên đầu vào $n$ và lưới cuối cùng có kích thước $2n nhân 2n."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 800
weight: 1950
solve_time_s: 45
verified: true
draft: false
---

[CF 1950B - Nâng cấp](https://codeforces.com/problemset/problem/1950/B) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tạo một mẫu bàn cờ được làm bằng các tấm lớn hơn$2 \times 2$gạch lát. Mỗi ô được điền đầy đủ ký tự`#`hoặc chứa đầy`.`. Kích thước của lưới được xác định bởi một số nguyên đầu vào$n$và lưới cuối cùng có kích thước$2n \times 2n`. The top-left $Luôn luôn là ô 2 \times 2$`#`và các ô xếp xen kẽ theo kiểu bàn cờ theo cả chiều ngang và chiều dọc. 

Ví dụ, khi$n=2$, lưới là$4 \times 4$, và phía trên bên trái$2 \times 2$ngói là`#`. Hàng xóm của nó ở bên phải là`.`, người hàng xóm bên dưới là`.`, và hàng xóm chéo là`#`. 

Những hạn chế là khiêm tốn.$n$có thể nhiều nhất là 20, vì vậy kích thước lưới tối đa là$40 \times 40$, đủ nhỏ để chúng ta có thể xây dựng toàn bộ lưới trong bộ nhớ và in nó theo đúng nghĩa đen. Độ phức tạp về thời gian không phải là yếu tố hạn chế ở đây, nhưng chúng ta phải tôn trọng các quy tắc tạo khuôn một cách cẩn thận. 

Các trường hợp cạnh có thể gây khó khăn cho việc triển khai đơn giản bao gồm$n=1$, nơi lưới chỉ$2 \times 2$. Một cách tiếp cận bất cẩn có thể cố gắng luân phiên các ô riêng lẻ thay vì$2 \times 2$khối, tạo ra đầu ra sai. Một nguồn lỗi tiềm ẩn khác là sắp xếp sai các ô sao cho các hàng hoặc cột không giữ nguyên mẫu xen kẽ khi di chuyển từ một ô.$2 \times 2$chặn tiếp theo. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là lặp lại từng hàng và từng cột trong$2n \times 2n$lưới, tính toán xem ô cụ thể đó có nên`#`hoặc`.`dựa trên hàng và cột của nó. Đối với mỗi ô, chúng ta có thể chia chỉ số hàng và cột cho 2 để xác định ô nào$2 \times 2$ô mà nó thuộc về, sau đó sử dụng tổng các chỉ số ô đó theo modulo 2 để quyết định ký tự. Cách tiếp cận này là chính xác và để đạt được mức tối đa$n=20$nó dẫn đến$1600$hoạt động cho mỗi trường hợp thử nghiệm, điều này không đáng kể. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần bất cứ điều gì phức tạp hơn thế này. Cấu trúc bài toán cho phép chúng ta ánh xạ trực tiếp từng ô tới ô của nó bằng cách kiểm tra tính chẵn lẻ và chia số nguyên đơn giản. Không cần đệ quy hoặc lập trình động. Mỗi hàng bao gồm các chuỗi lặp lại của hai ký tự giống hệt nhau vì mỗi ô trải dài trên hai cột. Tương tự, mỗi ô trải dài hai hàng, do đó nội dung hàng lặp lại một lần cho hàng thứ hai của ô. Nhận biết$2 \times 2$bản chất của các ô xếp cho phép chúng ta tạo ra lưới một cách hiệu quả bằng các vòng lặp lồng nhau đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Đã chấp nhận | 
| Tối ưu hóa với sự lặp lại của ô xếp | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Lặp lại từng trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc`n`và tính toán kích thước lưới như`2n`. 
3. Lặp lại`2n`hàng sử dụng chỉ mục`i`. Xác định hàng gạch ngang này bằng phép chia số nguyên:`tile_row = i // 2`. 
4. Khởi tạo một chuỗi trống cho hàng hiện tại. Lặp lại`2n`cột sử dụng chỉ mục`j`. Xác định đây là cột xếp dọc nào bằng phép chia số nguyên:`tile_col = j // 2`. 
5. Tính tính chẵn lẻ của`tile_row + tile_col`. Nếu nó chẵn thì nối thêm`#`đến chuỗi hàng hiện tại; nếu lẻ, nối thêm`.`. Điều này thực hiện mẫu gạch xen kẽ. 
6. Sau khi xây dựng xong chuỗi hàng, hãy in ngay. Lặp lại cho đến khi tất cả các hàng của ca kiểm thử được in. 
7. Không in thêm dòng trống giữa các test case. 

Tại sao nó hoạt động: Mỗi$2 \times 2$gạch được xác định bởi`(i//2, j//2)`. Tính tổng các chỉ số hàng và cột của khối ảnh và kiểm tra tính chẵn lẻ đảm bảo rằng các khối ảnh liền kề có ký tự thay thế. Việc lặp lại từng ký tự ô cho hai hàng và hai cột sẽ tạo ra kết quả được yêu cầu`2x2`khối. Điều bất biến là mọi vị trí trong lưới đều được ánh xạ chính xác tới ô của nó và do đó có ký tự chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    size = 2 * n
    for i in range(size):
        row = ''
        tile_row = i // 2
        for j in range(size):
            tile_col = j // 2
            if (tile_row + tile_col) % 2 == 0:
                row += '#'
            else:
                row += '.'
        print(row)
```Đầu tiên chúng ta đọc số lượng test case. Đối với mỗi trường hợp, chúng tôi xác định tổng kích thước lưới là`2n`. Đối với mỗi hàng`i`,`tile_row`xác định cái nào$2 \times 2$khối ngang chúng ta đang ở. Vòng lặp bên trong xây dựng từng ký tự hàng, sử dụng`(tile_row + tile_col) % 2`để quyết định xem đó có phải là`#`hoặc`.`. Điều này đảm bảo các khối xen kẽ chính xác. Các hàng được in ngay lập tức mà không lưu toàn bộ lưới vào bộ nhớ, mặc dù việc lưu trữ cũng phù hợp với kích thước này. Sử dụng phép chia số nguyên và kiểm tra tính chẵn lẻ là một kỹ thuật phổ biến để xây dựng các mẫu bàn cờ một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1
```| tôi | gạch_row | j | gạch_col | ô_row + ô_col | char | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | # | 
| 0 | 0 | 1 | 0 | 0 | # | 
| 1 | 0 | 0 | 0 | 0 | # | 
| 1 | 0 | 1 | 0 | 0 | # | 

Đầu ra:```
##
##
```Điều này xác nhận lưới nhỏ nhất được xử lý chính xác, chỉ với một`2x2`ngói. 

### Ví dụ 2 

đầu vào:```
n = 2
```| tôi | gạch_row | j | gạch_col | ô_row + ô_col | char | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | # | 
| 0 | 0 | 1 | 0 | 0 | # | 
| 0 | 0 | 2 | 1 | 1 | . | 
| 0 | 0 | 3 | 1 | 1 | . | 
| 1 | 0 | 0 | 0 | 0 | # | 
| 1 | 0 | 1 | 0 | 0 | # | 
| 1 | 0 | 2 | 1 | 1 | . | 
| 1 | 0 | 3 | 1 | 1 | . | 
| 2 | 1 | 0 | 0 | 1 | . | 
| 2 | 1 | 1 | 0 | 1 | . | 
| 2 | 1 | 2 | 1 | 2 | # | 
| 2 | 1 | 3 | 1 | 2 | # | 
| 3 | 1 | 0 | 0 | 1 | . | 
| 3 | 1 | 1 | 0 | 1 | . | 
| 3 | 1 | 2 | 1 | 2 | # | 
| 3 | 1 | 3 | 1 | 2 | # | 

Đầu ra:```
##..
##..
..##
..##
```Dấu vết này cho thấy tổng chẵn lẻ xen kẽ các ô một cách chính xác ở cả hai chiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi lặp qua từng ô trong một`2n x 2n`lưới. | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ một hàng mỗi lần; không cần mảng lưới lớn. | 

Với những hạn chế$n \le 20$, lưới lớn nhất có 1600 ký tự, dễ quản lý bằng Python. Nhiều trường hợp thử nghiệm lên tới 20 cũng phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        size = 2 * n
        for i in range(size):
            row = ''
            tile_row = i // 2
            for j in range(size):
                tile_col = j // 2
                row += '#' if (tile_row + tile_col) % 2 == 0 else '.'
            print(row)
    return out.getvalue().strip()

# Provided sample
assert run("4\n1\n2\n3\n4\n") == (
"##\n##\n"
"##..\n##..\n..##\n..##\n"
"##
```
