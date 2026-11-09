---
title: "CF 1499A - Domino trên bậu cửa sổ"
description: "Chúng ta được cấp một bảng có kích thước $2 nhân n$, trong đó các ô $k1$ đầu tiên của hàng trên cùng và các ô $k2$ của hàng dưới cùng có màu trắng và các ô còn lại có màu đen. Chúng ta cũng có một số quân domino trắng $w$ và quân domino đen $b$."
date: "2026-06-10T21:26:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1499
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 106 (Rated for Div. 2)"
rating: 800
weight: 1499
solve_time_s: 270
verified: true
draft: false
---

[CF 1499A - Domino trên Windowsill](https://codeforces.com/problemset/problem/1499/A) 

**Đánh giá:** 800 
**Tags:** tổ hợp, thuật toán xây dựng, toán học 
**Thời gian giải:** 4 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tấm bảng có kích thước$2 \times n$, nơi đầu tiên$k_1$các ô ở hàng trên cùng và$k_2$các ô ở hàng dưới cùng có màu trắng và các ô còn lại có màu đen. Chúng tôi cũng được tặng một số quân domino trắng$w$và quân domino đen$b$. Mỗi domino là$2 \times 1$và có thể đặt theo chiều ngang hoặc chiều dọc nhưng phải bao phủ các ô cùng màu. Nhiệm vụ là xác định liệu có thể đặt tất cả$w + b$domino trên bảng không có sự chồng chéo. 

Đầu vào mô tả nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm,$n$có thể đi lên$1000$và số lượng ca kiểm thử$t$có thể đi lên$3000$, vì vậy bất kỳ thuật toán nào cũng phải xử lý tối đa$3 \cdot 10^6$tổng số tế bào bảng. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng tất cả các vị trí domino, vì điều đó có thể dễ dàng dẫn đến$O(n^2)$làm việc cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng xuất hiện khi một hàng không có ô trắng hoặc đen, khi quân domino phải được xếp theo chiều dọc nhưng một hàng không có đủ ô tương ứng hoặc khi có số lẻ các ô đơn màu không thể tạo thành quân domino. Ví dụ, một bảng có$n=1$,$k_1=0$,$k_2=1$, Và$w=1$không thể chứa một quân domino trắng vì chỉ có một ô trắng. Một cách tiếp cận bất cẩn chỉ kiểm tra tổng số ô trắng sẽ cho rằng có thể đặt vị trí một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc đặt quân domino ở mọi vị trí hợp lệ. Người ta có thể thử tất cả các vị trí theo chiều ngang và sau đó là các vị trí theo chiều dọc hoặc thậm chí thử tất cả các hoán vị theo cách đệ quy. Điều này đúng vì nó liệt kê mọi cấu hình hợp lệ nhưng tốc độ quá chậm. Vì$n = 1000$, số lượng vị trí tăng lên theo kiểu kết hợp và với$t = 3000$trường hợp thử nghiệm, điều này nhanh chóng vượt quá giới hạn thực tế. 

Thông tin chi tiết quan trọng là chúng tôi không cần mô phỏng các vị trí. Mỗi domino bao gồm chính xác hai ô cùng màu. Đối với quân domino dọc, số lượng tối đa chúng ta có thể đặt là$\min(k_1, k_2)$cho quân domino trắng và$\min(n - k_1, n - k_2)$cho quân domino đen. Sau khi sử dụng quân domino dọc, các ô còn lại trong mỗi hàng chỉ có thể được ghép theo chiều ngang trong cùng một hàng. Như vậy, số lượng quân domino tối đa của một màu là tổng số quân domino xếp theo chiều dọc cộng với một nửa số ô đơn màu còn lại. Điều này làm giảm vấn đề thành số học đơn giản về số lượng ô màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * t) | O(n) | Quá chậm | 
| Đếm Tế Bào | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$k_1$,$k_2$,$w$,$b$. Đây$k_1$Và$k_2$lần lượt là số ô màu trắng ở hàng trên và dưới. 
2. Tính số quân domino trắng tối đa có thể đặt được. Các quân domino dọc bị giới hạn bởi$\min(k_1, k_2)$. Các ô màu trắng còn lại ở hàng trên cùng là$k_1 - \min(k_1, k_2)$, và ở hàng dưới cùng$k_2 - \min(k_1, k_2)$. Mỗi quân domino ngang chiếm 2 ô, vậy số quân domino ngang chúng ta có thể đặt là$(\text{remaining top} + \text{remaining bottom}) // 2$. Thêm số lượng dọc và ngang để có được`max_white`. 
3. Tính số lượng quân domino đen tối đa tương tự. Số ô màu đen ở hàng trên cùng là$n - k_1$, và ở hàng dưới cùng$n - k_2$. Số quân domino dọc tối đa là$\min(n - k_1, n - k_2)$và domino ngang là các ô đen còn lại chia cho 2. Tổng các số này cho`max_black`. 
4. Nếu$w \le max_white$Và$b \le max_black$, in "CÓ"; nếu không thì in "KHÔNG". Điều này đảm bảo rằng bàn cờ có đủ không gian cho tất cả các quân domino mà không cần đặt chúng một cách rõ ràng. 

**Tại sao nó hoạt động**: Thuật toán đếm chính xác số lượng domino tối đa có thể có của mỗi màu. Các quân domino dọc luôn tối ưu để tối đa hóa việc sử dụng, còn các quân domino ngang sử dụng bất kỳ ô đơn màu nào còn sót lại. Vì các quân domino không thể chồng lên nhau và mỗi quân domino bao phủ chính xác 2 ô nên việc vượt quá mức tối đa này là không thể. Số học này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k1, k2 = map(int, input().split())
    w, b = map(int, input().split())
    
    # Maximum white dominoes
    vertical_white = min(k1, k2)
    remaining_white_top = k1 - vertical_white
    remaining_white_bottom = k2 - vertical_white
    horizontal_white = (remaining_white_top + remaining_white_bottom) // 2
    max_white = vertical_white + horizontal_white
    
    # Maximum black dominoes
    black_top = n - k1
    black_bottom = n - k2
    vertical_black = min(black_top, black_bottom)
    remaining_black_top = black_top - vertical_black
    remaining_black_bottom = black_bottom - vertical_black
    horizontal_black = (remaining_black_top + remaining_black_bottom) // 2
    max_black = vertical_black + horizontal_black
    
    if w <= max_white and b <= max_black:
        print("YES")
    else:
        print("NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử, sau đó lặp qua từng trường hợp kiểm thử. Số domino tối đa cho mỗi màu được tính riêng. Việc sử dụng phép chia số nguyên đảm bảo chỉ tính các quân domino đầy đủ. điều kiện`w <= max_white and b <= max_black`là điểm quyết định quan trọng. Phép trừ cẩn thận sẽ ngăn chặn số lượng âm và đảm bảo rằng phép chia thể hiện chính xác các cặp ô. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1 0 1`với`w=1, b=0`| Biến | Giá trị | 
| --- | --- | 
| dọc_white | 0 | 
| còn lại_white_top | 0 | 
| còn lại_white_bottom | 1 | 
| ngang_trắng | 0 | 
| max_white | 0 | 

Từ`w = 1 > max_white = 0`, đầu ra là "KHÔNG". 

**Ví dụ 2** 

đầu vào:`4 3 1`với`w=2, b=2`| Biến | Giá trị | 
| --- | --- | 
| dọc_white | 1 | 
| còn lại_white_top | 2 | 
| còn lại_white_bottom | 0 | 
| ngang_trắng | 1 | 
| max_white | 2 | 
| đen_top | 1 | 
| đen_đáy | 3 | 
| dọc_black | 1 | 
| còn lại_black_top | 0 | 
| còn lại_black_bottom | 2 | 
| ngang_đen | 1 | 
| max_black | 2 | 

Cả hai`w <= max_white`Và`b <= max_black`, xuất ra "CÓ". 

Những dấu vết này cho thấy thuật toán xử lý chính xác các hàng không đồng đều và cả vị trí domino dọc và ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm các phép tính và so sánh đơn giản | 
| Không gian | O(1) | Chỉ các biến số nguyên cho mỗi trường hợp thử nghiệm, không có mảng lớn | 

Giải pháp có tỷ lệ tuyến tính với số lượng trường hợp thử nghiệm, không phụ thuộc vào$n$, làm cho nó đủ hiệu quả cho các ràng buộc$t \le 3000$Và$n \le 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, k1, k2 = map(int, input().split())
        w, b = map(int, input().split())
        vertical_white = min(k1, k2)
        remaining_white_top = k1 - vertical_white
        remaining_white_bottom = k2 - vertical_white
        horizontal_white = (remaining_white_top + remaining_white_bottom) // 2
        max_white = vertical_white + horizontal_white
        black_top = n - k1
        black_bottom = n - k2
        vertical_black = min(black_top, black_bottom)
        remaining_black_top = black_top - vertical_black
        remaining_black_bottom = black_bottom - vertical_black
        horizontal_black = (remaining_black_top + remaining_black_bottom) // 2
        max_black = vertical_black + horizontal_black
        if w <= max_white and b <= max_black:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("5\n1 0 1\n1 0\n1 1 1\n0 0\n3 0 0\n1 3\n4 3 1\n2 2\n5 4 3\n3 1") == "NO\nYES\nNO\nYES\nYES"

# custom cases
assert run("1\n1 0 0\n0 1") == "YES", "single column, only black domino"
assert run("1\n1 1 1\n1 0") == "YES", "single column, only white domino"
assert run("1\n2 1 1\n1 1") == "YES", "two
```
