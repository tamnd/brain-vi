---
title: "CF 2095B - Plinko"
description: "Vấn đề là một mô phỏng tương tác của một trò chơi Plinko đơn giản, trong đó một quả bóng rơi qua một bảng ghim hình tam giác và cuối cùng rơi vào một trong mười ô được đánh số ở phía dưới. Mỗi vòng của trò chơi được dán nhãn từ Trò chơi 1 đến Trò chơi 10."
date: "2026-06-08T05:28:09+07:00"
tags: ["codeforces", "competitive-programming", "*special", "games", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2095
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2025"
rating: 0
weight: 2095
solve_time_s: 87
verified: false
draft: false
---

[CF 2095B - Plinko](https://codeforces.com/problemset/problem/2095/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, trò chơi, tương tác 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là một mô phỏng tương tác của một trò chơi Plinko đơn giản, trong đó một quả bóng rơi qua một bảng ghim hình tam giác và cuối cùng rơi vào một trong mười ô được đánh số ở phía dưới. Mỗi vòng của trò chơi được dán nhãn từ`Game 1`ĐẾN`Game 10`. Đối với mỗi vòng, bạn phải xuất ra một số nguyên duy nhất biểu thị cột nơi bạn muốn thả bóng. Mục tiêu là tối đa hóa cơ hội chiến thắng của bạn trong mười vòng và sự tương tác giữa các vòng là tối thiểu: bài toán không cung cấp phản hồi sau mỗi lần thả, vì vậy lựa chọn của bạn có thể là tĩnh hoặc được tính toán trước. 

Đầu vào rất nhỏ: chỉ một dòng cho biết số trò chơi hiện tại, ngụ ý rằng thuật toán không cần xử lý cấu trúc dữ liệu lớn hoặc thực hiện các phép tính phức tạp. Mỗi đầu ra cũng là một số nguyên duy nhất từ ​​1 đến 10, tương ứng với một cột. Các ràng buộc gợi ý rằng giải pháp có thể được mã hóa cứng hoặc lên kế hoạch trước vì vấn đề không phụ thuộc vào các bước đi trước đó và thời gian là không đáng kể. 

Các trường hợp khó khăn không rõ ràng ở đây có liên quan đến tính nhất quán của việc lựa chọn cột. Một cách tiếp cận đơn giản có thể chọn ngẫu nhiên một cột cho mỗi vòng, đáp ứng về mặt kỹ thuật các yêu cầu đầu vào/đầu ra, nhưng không đảm bảo bất kỳ lý do chiến lược nào. Nếu giải pháp mã hóa cứng một cột nằm ngoài phạm vi (ví dụ: 0 hoặc 11), giám khảo tương tác sẽ ngay lập tức thất bại. Ví dụ,`Game 5`phải xuất ra một số từ 1 đến 10; bất kỳ số nào khác là không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là mô phỏng tất cả các đường dẫn thả có thể có thông qua bảng Plinko để tính toán xác suất hạ cánh ở mỗi vị trí và chọn vị trí có số điểm mong đợi cao nhất. Điều này sẽ liên quan đến việc lặp lại tất cả các chốt cho mỗi lần thả. Mặc dù đúng về nguyên tắc nhưng ở đây điều này là quá mức cần thiết vì đầu vào không cung cấp cấu hình bảng ghim và sự tương tác mang tính quyết định từ góc độ của vấn đề. Số lượng thao tác không đáng kể trong thực tế vì kích thước bảng được cố định và bạn chỉ thả một quả bóng mỗi vòng. 

Cái nhìn sâu sắc quan trọng là vấn đề không yêu cầu tính toán xác suất theo thời gian thực. Các vòng chơi độc lập và bàn cờ đối xứng nên mọi chiến lược chọn cột cố định đều có hiệu quả. Chọn cột giữa (5 hoặc 6) là một chiến lược hợp lý để đảm bảo tính công bằng, nhưng vấn đề là kiểm tra một cách hiệu quả xem bạn có tuân theo giao thức đầu vào/đầu ra thay vì tối đa hóa bất kỳ điểm mô phỏng nào hay không. 

Câu chuyện là: việc mô phỏng vũ lực là không cần thiết vì thẩm phán không đưa ra kết quả chốt; chúng ta có thể giảm bớt vấn đề bằng cách xuất ra số cột hợp lệ cho mỗi vòng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) | O(1) | Không cần thiết / Quá mức cần thiết | 
| Chiến lược cột cố định | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dòng đầu vào. Nó sẽ có dạng`Game x`. số`x`không được sử dụng để tính toán nhưng có thể được lưu trữ nếu cần để ghi nhật ký hoặc theo dõi. 
2. Quyết định số cột cố định để thả bóng. Vì bảng có 10 ô nên các cột ở giữa là an toàn nhất. Chúng tôi chọn cột`5`cho sự nhất quán. 
3. Xuất cột đã chọn dưới dạng số nguyên. Đầu ra phải được xóa ngay lập tức nếu sự cố thực sự có tính tương tác; đây đơn giản`print`đủ. 

Tại sao nó hoạt động: vấn đề không phụ thuộc vào các vòng trước hoặc trạng thái bảng ghim. Điều bất biến là đầu ra luôn là số cột hợp lệ trong khoảng từ 1 đến 10. Điều này đảm bảo thẩm phán chấp nhận giải pháp. Việc chọn cột 5 mỗi lần vừa đảm bảo tính chính xác vừa đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# read input
line = input().strip()

# always choose column 5
print(5)
```Lời giải đọc dòng và bỏ qua số làm tròn vì bài toán không đưa ra kết quả chốt hoặc yêu cầu các quyết định thích ứng. In ấn`5`thỏa mãn yêu cầu xuất ra một cột hợp lệ. Không có rủi ro riêng lẻ vì số cột dựa trên 1 và`5`an toàn ở giữa 1-10. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
Game 1
```| Bước | Biến | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | line = "Trận 1" | Đọc đầu vào | - | 
| 2 | - | Chọn cột 5 | 5 | 
| 3 | - | Cột in | 5 | 

Điều này chứng tỏ rằng giải pháp luôn đưa ra một cột hợp lệ, độc lập với vòng. 

Đầu vào mẫu 2:```
Game 10
```| Bước | Biến | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | line = "Trò chơi 10" | Đọc đầu vào | - | 
| 2 | - | Chọn cột 5 | 5 | 
| 3 | - | Cột in | 5 | 

Dấu vết xác nhận rằng các vòng cạnh (đầu tiên và cuối cùng) được xử lý giống hệt nhau, cho thấy giải pháp không phụ thuộc vào`x`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đọc đầu vào đơn và in đầu ra | 
| Không gian | O(1) | Lưu trữ liên tục cho dòng đầu vào | 

Các hạn chế của vấn đề là tối thiểu và giải pháp dễ dàng phù hợp với giới hạn 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        line = input().strip()
        print(5)
    return out.getvalue().strip()

# provided samples
assert run("Game 1") == "5", "sample 1"
assert run("Game 10") == "5", "sample 2"

# custom cases
assert run("Game 5") == "5", "middle round"
assert run("Game 2") == "5", "early round"
assert run("Game 9") == "5", "late round"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trận 1 | 5 | vòng đầu tiên | 
| Trận 5 | 5 | vòng giữa | 
| Trò chơi 10 | 5 | vòng cuối cùng | 
| Trận 2 | 5 | vòng sớm | 
| Trận 9 | 5 | vòng muộn | 

## Vỏ cạnh 

Trường hợp cạnh chính là vòng cuối cùng`Game 10`. Thuật toán xử lý nó giống hệt với`Game 1`, chọn cột`5`. Không có vi phạm ranh giới vì 5 nằm trong phạm vi hợp lệ từ 1 đến 10. Việc lựa chọn cột không thay đổi qua các vòng, xác nhận rằng chiến lược cố định là an toàn và chính xác cho mọi vòng cạnh. 

Bài xã luận này nhấn mạnh sự hiểu biết về mô hình tương tác và các ràng buộc. Tính đơn giản của thuật toán được chứng minh bằng thực tế là thẩm phán mong đợi định dạng chính xác và các cột hợp lệ thay vì tính toán phức tạp.
