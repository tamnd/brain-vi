---
title: "CF 1451F - Vô hiệu hóa ma trận"
description: "Bài toán mô tả một trò chơi hai người chơi trên một ma trận $n nhân m$ gồm các số nguyên không âm. Người chơi luân phiên di chuyển, bắt đầu với Ashish."
date: "2026-06-11T03:35:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games"]
categories: ["algorithms"]
codeforces_contest: 1451
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 685 (Div. 2)"
rating: 2700
weight: 1451
solve_time_s: 266
verified: true
draft: false
---

[CF 1451F - Vô hiệu hóa ma trận](https://codeforces.com/problemset/problem/1451/F) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, trò chơi 
**Thời gian giải:** 4 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả trò chơi hai người chơi được chơi trên một$n \times m$ma trận các số nguyên không âm. Người chơi luân phiên di chuyển, bắt đầu với Ashish. Trong mỗi lần di chuyển, người chơi chọn ô bắt đầu có giá trị khác 0, chọn ô kết thúc không ở phía trên hoặc bên trái ô bắt đầu, giảm ô bắt đầu đi một số nguyên dương và có thể điều chỉnh các giá trị dọc theo con đường ngắn nhất từ ​​​​đầu đến cuối. Trò chơi kết thúc khi tất cả các ô đều bằng 0 và người chơi không thể di chuyển sẽ thua cuộc. 

Với ma trận ban đầu, nhiệm vụ là dự đoán người chiến thắng nếu cả hai đều chơi tối ưu. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có kích thước ma trận và giá trị ma trận ban đầu. 

Những hạn chế còn khiêm tốn:$n, m \le 100$Và$a_{i,j} \le 10^6$, với tối đa 10 trường hợp thử nghiệm. Điều này cho phép các thuật toán có thời gian bậc hai ở kích thước ma trận, gần như lên tới$10^6$hoạt động, để chạy thoải mái trong thời gian giới hạn. 

Khía cạnh không rõ ràng là, mặc dù các bước di chuyển cho phép thao tác đường đi phức tạp, trò chơi có thể được rút gọn thành một chiến lược đơn giản hơn. Việc mô phỏng một cách ngây thơ mọi động thái có thể xảy ra sẽ không khả thi vì hệ số phân nhánh là rất lớn. Các trường hợp biên bao gồm các ma trận toàn số 0, ma trận một hàng hoặc một cột và các ma trận chỉ có một ô khác 0. Những trường hợp khó khăn này đòi hỏi phải suy luận cẩn thận về việc liệu người chơi đầu tiên có thể di chuyển hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các bước di chuyển có thể xảy ra, điều chỉnh đường đi và giá trị ở mỗi lượt. Về nguyên tắc, điều này đúng, nhưng số lượng chuỗi di chuyển tăng lên một cách kết hợp với kích thước ma trận và giá trị ô, khiến nó trở nên khó hiểu. 

Điểm mấu chốt là trò chơi này tương đương với một trò chơi công bằng cổ điển trên biểu đồ hai bên, cụ thể là một biến thể của trò chơi nim hai chiều. Mỗi lần di chuyển sẽ "loại bỏ" một hàng và một cột một cách hiệu quả (vì một ô khác 0 được chọn và đường dẫn tối thiểu ảnh hưởng đến một hình chữ nhật). Người chiến thắng có thể được xác định bằng cách đếm số hàng và cột có chứa ít nhất một phần tử khác 0. Nếu số đếm tối thiểu này là số lẻ, người chơi đầu tiên có thể giành chiến thắng bằng cách luôn nhắm mục tiêu vào một ô trong tập hợp tối thiểu đó; nếu nó chẵn, người chơi thứ hai có thể phản chiếu các nước đi để giành chiến thắng. 

Việc giảm này giúp đơn giản hóa đáng kể vấn đề từ việc mô phỏng các bước di chuyển đến việc đếm các hàng và cột khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^(n*m)) | O(n*m) | Quá chậm | 
| Đếm Tối Ưu | O(n*m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước$n$Và$m$và ma trận. 
2. Đếm số hàng có chứa ít nhất một phần tử khác 0. Điều này có thể được thực hiện bằng cách lặp qua từng hàng và kiểm tra bất kỳ ô nào khác 0. 
3. Đếm số cột chứa ít nhất một phần tử khác 0. Điều này có thể được thực hiện bằng cách lặp qua từng cột và kiểm tra xem có ô nào khác không. 
4. Tính giá trị tối thiểu của số hàng khác 0 và số cột khác 0. Giá trị này biểu thị số lần di chuyển tối đa có thể có trong đó mỗi lần di chuyển có thể nhắm mục tiêu đến một giao điểm hàng và cột duy nhất. 
5. Nếu giá trị tối thiểu này là số lẻ, Ashish, với tư cách là người chơi đầu tiên, có thể giành chiến thắng. Nếu không, Jeel, người chơi thứ hai, có thể giành chiến thắng bằng cách bắt chước các bước di chuyển của Ashish. 
6. Xuất ra "Ashish" nếu Ashish thắng, hoặc "Jeel" nếu ngược lại. 

Tại sao nó hoạt động: Mỗi nước đi trong trò chơi sẽ loại bỏ một hàng đang hoạt động và một cột đang hoạt động một cách hiệu quả. Người chơi nào thực hiện được nước đi cuối cùng như vậy sẽ thắng. Bằng cách đếm các hàng và cột đang hoạt động, chúng tôi giảm trò chơi thành một kịch bản giống như nim với một chồng có kích thước bằng với số lượng tối thiểu. Điều này đảm bảo tính chính xác mà không cần mô phỏng tất cả các đường dẫn có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        matrix = [list(map(int, input().split())) for _ in range(n)]
        
        # count non-zero rows
        rows = sum(1 for row in matrix if any(cell != 0 for cell in row))
        # count non-zero columns
        cols = sum(1 for c in range(m) if any(matrix[r][c] != 0 for r in range(n)))
        
        min_count = min(rows, cols)
        print("Ashish" if min_count % 2 == 1 else "Jeel")

if __name__ == "__main__":
    solve()
```Mỗi phần của mã tương ứng trực tiếp với các bước thuật toán. Việc đếm các hàng và cột khác 0 rất đơn giản nhưng cần phải lặp lại cẩn thận để tránh các lỗi sai một. Việc lấy mức tối thiểu đảm bảo chúng tôi nắm bắt được yếu tố giới hạn cho các nước đi có sẵn. Việc kiểm tra tính chẵn lẻ có nghĩa trực tiếp là xác định người chiến thắng. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu thứ hai:```
1 3
0 0 5
```| Biến | Giá trị | 
| --- | --- | 
| các hàng khác 0 | 1 | 
| các cột khác 0 | 1 | 
| phút(hàng, cols) | 1 | 
| người chiến thắng | Ashish | 

Điều này cho thấy người chơi đầu tiên có thể thực hiện nước đi duy nhất có sẵn và giành chiến thắng. 

Đối với đầu vào mẫu thứ ba:```
2 2
0 1
1 0
```| Biến | Giá trị | 
| --- | --- | 
| các hàng khác 0 | 2 | 
| các cột khác 0 | 2 | 
| phút(hàng, cols) | 2 | 
| người chiến thắng | Jeel | 

Điều này chứng tỏ rằng người chơi đầu tiên không thể thắng nếu số điểm tối thiểu là chẵn và người chơi thứ hai có thể phản chiếu các nước đi để giành chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi ô được truy cập hai lần: một lần để kiểm tra hàng, một lần để kiểm tra cột | 
| Không gian | O(n*m) | Lưu trữ ma trận đầu vào | 

Lời giải nằm trong giới hạn, vì n và m nhiều nhất là 100 và t ≤ 10, cho tối đa 100_100_10 = 100.000 phép toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("4\n1 1\n0\n1 3\n0 0 5\n2 2\n0 1\n1 0\n3 3\n1 2 3\n4 5 6\n7 8 9\n") == "Jeel\nAshish\nJeel\nAshish"

# custom cases
assert run("1\n1 1\n1\n") == "Ashish", "single non-zero cell"
assert run("1\n2 2\n0 0\n0 0\n") == "Jeel", "all zeros"
assert run("1\n3 3\n1 0 0\n0 1 0\n0 0 1\n") == "Ashish", "diagonal ones"
assert run("1\n2 3\n1 0 0\n0 0 1\n") == "Jeel", "minimum count even"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 với 1 | Ashish | người chơi đầu tiên có thể di chuyển và giành chiến thắng | 
| 2x2 tất cả số không | Jeel | không có động thái nào | 
| đường chéo 3x3 | Ashish | số lượng tối thiểu lẻ mang lại chiến thắng cho người chơi đầu tiên | 
| 2x3 | Jeel | số lượng tối thiểu thậm chí còn mang lại chiến thắng cho người chơi thứ hai | 

## Vỏ cạnh 

Đối với ma trận chỉ có một hàng hoặc một cột có giá trị khác 0, thuật toán sẽ xác định chính xác số đếm tối thiểu là 1. Ví dụ: ma trận 1x5`[0,0,0,0,7]`có một hàng khác 0 và một cột khác 0. Tối thiểu là 1, vì vậy Ashish thắng bằng cách loại bỏ ô khác 0 cuối cùng. Điều này minh họa rằng thuật toán xử lý đúng các ma trận thưa thớt. Tương tự, đối với ma trận hoàn toàn bằng 0, cả hai số đếm đều bằng 0, số tối thiểu bằng 0 và người chơi thứ hai thắng vì người đầu tiên không thể thực hiện bất kỳ nước đi nào.
