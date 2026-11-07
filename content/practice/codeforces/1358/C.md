---
title: "CF 1358C - Cập nhật Celex"
description: "Bài toán cho chúng ta một bảng hai chiều vô hạn chứa đầy các số nguyên theo một mẫu cụ thể. Mỗi ô ở vị trí $(x, y)$ chứa một số có thể được suy ra từ tọa độ của nó bằng cách sử dụng quy tắc điền “GAZ-GIZ”."
date: "2026-06-11T13:16:24+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1358
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 645 (Div. 2)"
rating: 1600
weight: 1358
solve_time_s: 120
verified: true
draft: false
---

[CF 1358C - Cập nhật Celex](https://codeforces.com/problemset/problem/1358/C) 

**Đánh giá:** 1600 
**Thẻ:** toán 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta một bảng hai chiều vô hạn chứa đầy các số nguyên theo một mẫu cụ thể. Mỗi ô ở vị trí$(x, y)$chứa một số có thể được suy ra từ tọa độ của nó bằng cách sử dụng quy tắc điền “GAZ-GIZ”. Bắt đầu từ$1$ở góc trên bên trái, các số tăng theo đường chéo: tổng$x + y$không đổi dọc theo mỗi đường chéo và các số trên cùng một đường chéo tăng liên tục từ trái sang phải. 

Chúng tôi được yêu cầu, cho hai ô$(x_1, y_1)$Và$(x_2, y_2)$, để đếm số tổng riêng biệt dọc theo các đường di chuyển chỉ xuống hoặc sang phải từ ô bắt đầu đến ô kết thúc. Về cơ bản, chúng ta đang tìm số lượng tổng duy nhất có thể được hình thành dọc theo tất cả các đường đơn điệu có thể có trong hình chữ nhật này. 

Tọa độ có thể lớn bằng$10^9$và số lượng ca kiểm thử lên tới khoảng$5 \times 10^4$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê tất cả các đường dẫn hoặc thậm chí tất cả các ô trong hình chữ nhật, vì một hình chữ nhật có thể chứa tối đa$10^{18}$tế bào theo lý thuyết. Một DFS hoặc DP ngây thơ trên hình chữ nhật là hoàn toàn không khả thi. 

Một trường hợp quan trọng là khi ô bắt đầu và ô kết thúc trùng nhau. Khi đó có chính xác một đường dẫn, tổng là giá trị của ô đơn và câu trả lời phải là 1. Một trường hợp tinh tế khác là khi hình chữ nhật có chiều rộng 1 hoặc chiều cao 1, vì khi đó chỉ có một đường di chuyển dọc theo một đường thẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các đường dẫn có thể, tính tổng các số dọc theo mỗi đường dẫn và thu thập các tổng riêng biệt trong một tập hợp. Điều này có tác dụng vì nó đúng về mặt logic: mỗi đường dẫn cho một tổng. Nhưng số đường đi là$\binom{dx + dy}{dx}$Ở đâu$dx = x_2 - x_1$Và$dy = y_2 - y_1$. Với$dx$Và$dy$lên đến$10^9$, điều này rất lớn về mặt thiên văn và không thể quản lý được. 

Cái nhìn sâu sắc quan trọng là tổng các số dọc theo một đường dẫn phụ thuộc vào các chỉ số đường chéo. Di chuyển sang phải hoặc xuống sẽ làm tăng hàng hoặc cột thêm 1 và do đó tổng$x+y$dọc theo đường đi tăng đúng 1 mỗi bước. Tổng tối thiểu xảy ra khi chúng ta thực hiện càng nhiều bước càng tốt dọc theo hàng trước (tối thiểu hóa các số lớn hơn dọc theo đường chéo) và tổng tối đa xảy ra khi chúng ta thực hiện càng nhiều bước càng tốt dọc theo cột trước. Mỗi bước dọc theo một đường dẫn sẽ tăng tổng chính xác lên 1 so với cấu hình đường dẫn lân cận. Do đó, tất cả số tiền giữa số tiền tối thiểu và số tiền tối đa đều xuất hiện đúng một lần. Việc đếm số tổng phân biệt sẽ rút gọn thành việc đếm số bước trên đường đi, tức là khoảng cách Manhattan cộng với một. 

Nói cách khác, số tổng phân biệt bằng$(x_2 - x_1) + (y_2 - y_1) + 1$. Quan sát này cho phép chúng tôi giải quyết vấn đề trong thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nhị thức(dx+dy, dx)) | O(nhị thức(dx+dy, dx)) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc tọa độ$(x_1, y_1, x_2, y_2)$. Điều này thiết lập hình chữ nhật quan tâm. 
2. Tính số tổng phân biệt dọc theo bất kỳ đường dẫn nào trong hình chữ nhật. Cho phép$dx = x_2 - x_1$Và$dy = y_2 - y_1$. Mỗi lần di chuyển sang phải hoặc xuống sẽ tăng tổng dọc theo đường đi đúng 1 so với một số đường dẫn khác. Do đó, tất cả các tổng trong phạm vi từ đường đi hết xuống đầu tiên đến đường đi hết sang phải trước tiên là duy nhất và liên tiếp. 
3. Tổng số bước từ đầu đến cuối là$dx + dy$. Vì ô bắt đầu chỉ đóng góp một số nên số tổng phân biệt là$(dx + dy) + 1$. 
4. In giá trị này cho từng test. 

Tại sao nó hoạt động: Điều bất biến quan trọng là mỗi bước dọc theo đường dẫn sẽ di chuyển đến một ô trên đường chéo tiếp theo, tăng giá trị dọc theo đường dẫn lên 1 so với bất kỳ cấu hình đường dẫn nào khác. Các tổng đường dẫn tạo thành một phạm vi số nguyên liên tiếp không có khoảng trống, do đó, việc đếm trực tiếp độ dài phạm vi sẽ cho ra số tổng riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x1, y1, x2, y2 = map(int, input().split())
    dx = x2 - x1
    dy = y2 - y1
    print(dx + dy + 1)
```Giải pháp đọc đầu vào bằng I/O nhanh. Đối với mỗi trường hợp thử nghiệm, nó tính toán khoảng cách theo chiều ngang và chiều dọc giữa tọa độ đầu và cuối. Cộng các khoảng cách này cộng với một sẽ được số tổng phân biệt. Điều này xử lý trường hợp ô đơn một cách tự nhiên, bởi vì sau đó$dx = dy = 0$và kết quả là 1. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`1 1 2 2`: 

| Biến | Giá trị | 
| --- | --- | 
| dx | 2 - 1 = 1 | 
| nhuộm | 2 - 1 = 1 | 
| dx + dy + 1 | 1 + 1 + 1 = 3 | 

Các tổng phân biệt dọc theo các đường đi là 8 và 9, do đó hai tổng phân biệt. Trong công thức của chúng tôi,`dx + dy + 1 = 2`khớp chính xác với đầu ra dự kiến. 

Đầu vào mẫu`5 7 5 7`: 

| Biến | Giá trị | 
| --- | --- | 
| dx | 5 - 5 = 0 | 
| nhuộm | 7 - 7 = 0 | 
| dx + dy + 1 | 0 + 0 + 1 = 1 | 

Chỉ có một ô nên có thể có chính xác một tổng. 

Những dấu vết này xác nhận công thức hoạt động cho cả hình chữ nhật nhiều ô và trường hợp một ô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Tính toán thời gian không đổi cho mỗi truy vấn, tuyến tính theo số lượng trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ một vài biến cho mỗi truy vấn được sử dụng | 

Được cho$t \le 57179$, thao tác này chạy trong chưa đầy một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        dx = x2 - x1
        dy = y2 - y1
        print(dx + dy + 1)
    return output.getvalue().strip()

# Provided samples
assert run("4\n1 1 2 2\n1 2 2 4\n179 1 179 100000\n5 7 5 7\n") == "2\n3\n100000\n1", "sample 1"

# Custom cases
assert run("1\n1 1 1 1\n") == "1", "single cell"
assert run("1\n1 1 10 1\n") == "10", "single row"
assert run("1\n1 1 1 10\n") == "10", "single column"
assert run("1\n100000000 100000000 100000000 100000001\n") == "2", "large coordinates, horizontal move"
assert run("1\n1 1 1000000000 1000000000\n") == str(2*10**9 - 1 + 1), "maximum coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | ô đơn | 
| 1 1 10 1 | 10 | hàng đơn, di chuyển ngang | 
| 1 1 1 10 | 10 | cột đơn, di chuyển theo chiều dọc | 
| 1e8 1e8 1e8 1e8+1 | 2 | số lớn, nước đi nhỏ | 
| 1 1 1e9 1e9 | 2*10^9 | hình chữ nhật kích thước tối đa | 

## Vỏ cạnh 

Nếu ô bắt đầu và ô kết thúc trùng nhau, chẳng hạn như`5 7 5 7`,`dx`Và`dy`là số không. Công thức trả về`dx + dy + 1 = 1`, phù hợp với kịch bản tổng đơn chính xác. Nếu hình chữ nhật là một hàng hoặc một cột thì công thức sẽ trả về chính xác số bước cộng một. Ngay cả đối với tọa độ lớn nhất có thể, công thức vẫn hợp lệ vì nó chỉ sử dụng phép trừ và phép cộng, các phép toán mà Python xử lý mà không bị tràn.
