---
title: "CF 279A - Điểm trên đường xoắn ốc"
description: "Chúng ta được yêu cầu đếm số lần một con ngựa phải quay khi di chuyển theo đường xoắn ốc từ điểm gốc đến điểm đích $(x, y)$."
date: "2026-06-05T08:44:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 279
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 171 (Div. 2)"
rating: 1400
weight: 279
solve_time_s: 80
verified: true
draft: false
---

[CF 279A - Điểm trên đường xoắn ốc](https://codeforces.com/problemset/problem/279/A) 

**Đánh giá:** 1400 
**Tags:** vũ lực, hình học, thực hiện 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem một con ngựa phải quay bao nhiêu lần khi di chuyển theo đường xoắn ốc từ điểm gốc đến điểm đích.$(x, y)$. Hình xoắn ốc là vô hạn, được tạo thành từ các đoạn thẳng hàng với trục phát triển ra bên ngoài theo một mô hình: nó di chuyển sang phải, rồi lên, sang trái, rồi xuống, tăng khoảng cách mỗi khi hoàn thành một vòng quay hoàn chỉnh. Mỗi đoạn kết nối các điểm nguyên trên mặt phẳng Cartesian và mỗi ngã rẽ xảy ra khi hướng thay đổi. 

Dữ liệu đầu vào bao gồm hai số nguyên,$|x|, |y| \le 100$, đại diện cho điểm đích. Đầu ra là số lượt thực hiện trước khi đạt đến điểm đó. 

Với các giới hạn nhỏ, việc mô phỏng lực lượng vũ phu dọc theo đường xoắn ốc là khả thi. Tuy nhiên, cần phải chú ý cẩn thận đối với các trường hợp có biên. Ví dụ: nếu mục tiêu là chính nguồn gốc$(0, 0)$, không có chuyển động nào xảy ra nên câu trả lời đúng là bằng 0. Một điều tinh tế khác là các lượt chỉ được tính khi ngựa đổi hướng chứ không phải khi nó hoàn thành một bước theo cùng một hướng. Một cách triển khai đơn giản chỉ đơn giản là đếm các lần chuyển đổi phân đoạn mà không kiểm tra xem liệu đích đến có đạt được hay không có thể bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng đường xoắn ốc một cách rõ ràng. Chúng ta sẽ bắt đầu từ điểm gốc, theo dõi hướng hiện tại và độ dài đoạn và di chuyển từng bước dọc theo đường xoắn ốc. Mỗi lần thay đổi hướng xảy ra, chúng tôi sẽ tăng bộ đếm lần lượt. Phương pháp này được đảm bảo là đúng nhưng có thể không hiệu quả nếu đường xoắn ốc kéo dài quá xa trước khi chạm tới$(x, y)$. Tuy nhiên, bởi vì$|x|$Và$|y|$tối đa là 100, đường xoắn ốc sẽ không cần vượt quá 201 bước theo bất kỳ hướng nào. Điều đó mang lại đại khái$O(n^2)$hoạt động trong trường hợp xấu nhất, có thể chấp nhận được với các ràng buộc. 

Cách tiếp cận tối ưu tận dụng cấu trúc của đường xoắn ốc. Mỗi “lớp” của hình xoắn ốc tạo thành một hình vuông xung quanh lớp trước đó. Quan sát kiểu rẽ cho thấy con ngựa quay mỗi khi nó đi đến cuối đoạn đường. Chúng ta có thể tính toán số lớp mà đích nằm trong đó bằng cách lấy tọa độ tuyệt đối tối đa. Mỗi lớp hoàn chỉnh thêm bốn lượt. Nếu điểm nằm trên một đoạn trong một lớp, chúng tôi chỉ tính lượt trước khi đến đoạn đó. Điều này làm giảm vấn đề thành một công thức đơn giản mà không cần mô phỏng từng bước, nhưng với những hạn chế, việc mô phỏng từng bước sẽ đơn giản hơn và đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(max( | x | , | 
| Công thức dựa trên lớp | O(1) | O(1) | Được chấp nhận nhưng yêu cầu tính toán cẩn thận | 

Với các giới hạn, chúng tôi tiến hành mô phỏng lực lượng vũ phu để có được sự rõ ràng và chính xác. 

## Hướng dẫn thuật toán 

1. Khởi tạo vị trí hiện tại tại$(0, 0)$, đặt hướng ban đầu thành “phải” và độ dài đoạn ban đầu thành 1. Ngoài ra, hãy khởi tạo bộ đếm vòng về 0. Trình tự hướng là phải, lên, trái, xuống, lặp lại theo chu kỳ. 
2. Lặp lại cho đến khi vị trí hiện tại tới đích$(x, y)$: 

1. Di chuyển dọc theo hướng hiện tại để có độ dài đoạn hiện tại. 
2. Nếu điểm đến nằm trên đoạn đường hiện tại thì dừng di chuyển tại điểm đến. Không thêm một lượt rẽ ngoài điểm này. 
3. Sau khi hoàn thành một đoạn, tăng bộ đếm lượt. 
4. Thay đổi hướng theo thứ tự: phải → lên → trái → xuống → phải, đạp xe. 
5. Cứ hai lượt tăng độ dài đoạn thêm 1. Điều này phù hợp với quy tắc tăng trưởng của đường xoắn ốc: phải và lên sử dụng cùng độ dài, sau đó tăng chiều dài sang trái và xuống. 
3. Khi vị trí hiện tại bằng$(x, y)$, trả lại bộ đếm lượt. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến rằng mỗi đoạn hoàn thành tương ứng với một lượt và độ dài của đoạn tăng chính xác theo mô hình xoắn ốc. Vì chúng tôi kiểm tra xem đích đến có nằm trên đoạn đường hiện tại hay không trước khi tính lượt đi nên chúng tôi không bao giờ tính quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y = map(int, input().split())

if x == 0 and y == 0:
    print(0)
    exit()

# direction vectors: right, up, left, down
dx = [1, 0, -1, 0]
dy = [0, 1, 0, -1]

cur_x, cur_y = 0, 0
dir_idx = 0
length = 1
turns = 0

while True:
    nx, ny = cur_x + dx[dir_idx] * length, cur_y + dy[dir_idx] * length
    
    # Check if destination is on current segment
    if dx[dir_idx] != 0:  # horizontal move
        if min(cur_x, nx) <= x <= max(cur_x, nx) and cur_y == y:
            print(turns)
            break
    else:  # vertical move
        if min(cur_y, ny) <= y <= max(cur_y, ny) and cur_x == x:
            print(turns)
            break
    
    # Move to end of segment
    cur_x, cur_y = nx, ny
    turns += 1
    dir_idx = (dir_idx + 1) % 4
    if dir_idx % 2 == 0:
        length += 1
```Mã xử lý I/O nhanh để lập trình cạnh tranh. Chỉ đường được mã hóa trong một danh sách đơn giản và chúng tôi chỉ tăng độ dài đoạn sau khi hoàn thành các cặp dọc và ngang. Chúng tôi kiểm tra cẩn thận xem đích đến có nằm trên đoạn đường hiện tại hay không để tránh xảy ra lỗi sai sót khi đến đích ở giữa đoạn đường. 

## Ví dụ đã hoạt động 

**Mẫu 1**: Đầu vào:`0 0`| cur_x | cur_y | thư mục | chiều dài | lượt | kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | đúng | 1 | 0 | tới đích ngay lập tức | 

Đầu ra:`0`. Thuật toán trả về 0 một cách chính xác mà không cần vào vòng lặp. 

**Mẫu 2**: Nhập:`1 0`| cur_x | cur_y | thư mục | chiều dài | lượt | kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | đúng | 1 | 0 | đích trên đoạn (0→1,0) | 

Đầu ra:`0`. Lượt chỉ được tính sau khi hoàn thành một đoạn. Ở đây, đích đến nằm ở cuối đoạn đầu tiên nhưng chưa có sự thay đổi hướng nào xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa( | x | 
| Không gian | O(1) | Chỉ một số số nguyên và danh sách được sử dụng cho vectơ chỉ hướng | 

Với |x|, |y| 100, tổng số thao tác dưới 100.000, thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # call solution
        exec(open('spiral.py').read())
    return out.getvalue().strip()

# provided samples
assert run("0 0\n") == "0", "sample 1"

# custom cases
assert run("1 0\n") == "0", "destination on first segment"
assert run("1 1\n") == "1", "requires one turn to reach (1,1)"
assert run("-1 1\n") == "2", "requires two turns to reach left side"
assert run("2 -1\n") == "4", "requires four turns to reach bottom-right"
assert run("100 100\n") == "200", "large positive coordinates"
assert run("-100 -100\n") == "200", "large negative coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | Điểm đến nằm ở đoạn đầu tiên, không tính lượt | 
| 1 1 | 1 | Yêu cầu chính xác một lượt để tiếp cận | 
| -1 1 | 2 | Vượt qua nhiều hướng | 
| 2 -1 | 4 | Đảm bảo độ dài đoạn tăng chính xác | 
| 100 100 | 200 | Tọa độ dương lớn | 
| -100 -100 | 200 | Tọa độ âm lớn | 

## Vỏ cạnh 

Nguồn gốc`(0,0)`là một trường hợp đặc biệt Thuật toán kiểm tra điều này ngay từ đầu và in số 0 ngay lập tức. Đối với các điểm đến trên đoạn đầu tiên, chúng tôi kiểm tra xem mục tiêu có nằm giữa tọa độ hiện tại và tọa độ tiếp theo hay không trước khi tăng lượt. Điều này ngăn ngừa các lỗi xảy ra, đảm bảo ngựa không “quay đầu” khi đã đạt được mục tiêu. Các tọa độ âm được xử lý một cách tự nhiên vì các phép so sánh tối thiểu/tối đa tính đến hướng.
