---
title: "CF 104344D - Prova"
description: "Kết quả của mỗi học sinh đến từ một vũ trụ cố định rất nhỏ: có đúng ba bài toán độc lập có giá trị 1, 2 và 4 điểm."
date: "2026-07-01T18:28:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "D"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 81
verified: false
draft: false
---

[CF 104344D - Prova](https://codeforces.com/problemset/problem/104344/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Kết quả của mỗi học sinh đến từ một vũ trụ cố định rất nhỏ: có đúng ba bài toán độc lập có giá trị 1, 2 và 4 điểm. Điểm của bất kỳ học sinh nào cũng chỉ là tổng giá trị của các bài toán mà họ đã giải được, vì vậy mỗi điểm từ 0 đến 7 tương ứng với một tập hợp con duy nhất của ba bài toán này. 

Chúng ta được cho điểm của hai học sinh, Lucca và Yvens. Từ những điểm số này, chúng ta có thể suy ra chính xác tập hợp con của các vấn đề mà mỗi chúng đã giải quyết được, bởi vì không có sự mơ hồ trong việc biểu diễn các số từ 0 đến 7 bằng cách sử dụng ba trọng số này. 

Hành vi của Passinho được xác định một cách rất cụ thể. Anh ấy giải quyết mọi vấn đề đã được giải quyết bởi ít nhất một trong số Lucca hoặc Yvens, và anh ấy không giải quyết được vấn đề nào mà cả hai đều không giải quyết được. Điều này có nghĩa là tập hợp đã giải của Passinho chính xác là tập hợp của các tập hợp đã giải của hai học sinh. 

Do đó, nhiệm vụ là tính toán kết của hai tập hợp con được biểu thị ngầm bằng điểm số của chúng, sau đó chuyển đổi kết hợp đó thành điểm. 

Ràng buộc rằng cả hai điểm đều nằm trong khoảng từ 0 đến 7 ngụ ý rằng mỗi điểm tương ứng với mặt nạ 3 bit. Điều này giúp loại bỏ mọi nhu cầu về tổ hợp hoặc tìm kiếm. Toàn bộ vấn đề được chuyển sang tái cấu trúc theo từng bit trên ba bit. 

Một trường hợp thất bại tinh vi xuất hiện khi suy luận trực tiếp về điểm số thay vì tập hợp con. Ví dụ: 3 có thể là {1,2} và 5 có thể là {1,4}. Một nỗ lực ngây thơ có thể cố gắng kết hợp các số một cách số học và giả định không chính xác tính độc lập của các chữ số trong biểu diễn thập phân. Biểu diễn đúng là nhị phân trên các trọng số 1, 2 và 4, không phải chữ số cơ sở 10. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất là thử tất cả các tập con của ba bài toán của Lucca và Yvens, tính toán tập con nào khớp với điểm của chúng, sau đó hợp rõ ràng từng cặp tương thích và tính lại điểm kết quả. Vì chỉ có 2³ tập hợp con cho mỗi người nên đây là công việc liên tục, nhưng phương pháp này nặng về mặt khái niệm và che khuất cấu trúc. 

Quan sát quan trọng là mỗi điểm đã được mã hóa trực tiếp một tập hợp con gồm ba phần tử. Viết điểm ở dạng nhị phân phù hợp hoàn hảo với trọng số của bài toán: bit 0 đại diện cho vấn đề 1, bit 1 đại diện cho vấn đề 2 và bit 2 đại diện cho vấn đề 4. Do đó, bản thân điểm số là một mặt nạ bit. 

Khi điều này được nhìn thấy, điểm của Passinho chỉ đơn giản là OR của hai điểm. Phép toán OR nắm bắt chính xác sự kết hợp của các vấn đề đã được giải, vì một bit được đặt nếu ít nhất một trong hai học sinh được đặt. 

Điều này làm giảm vấn đề xuống còn một thao tác duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của các tập hợp con | O(1) | O(1) | Đã chấp nhận | 
| Bitwise HOẶC giải thích | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên L và Y từ đầu vào. Chúng đại diện cho hai mặt nạ bit trên ba bit. 
2. Giải thích mỗi số dưới dạng tập hợp con của {1, 2, 4} trong đó mỗi bit cho biết liệu vấn đề đã được giải quyết hay chưa. 
3. Tính toán hợp các bài toán đã giải bằng cách áp dụng OR theo bit giữa L và Y. Điều này hiệu quả vì OR bảo toàn một bit nếu nó có trong một trong hai toán hạng. 
4. Xuất ra số nguyên tương ứng trực tiếp với điểm của Passinho. 

### Tại sao nó hoạt động 

Mỗi vấn đề đóng góp độc lập vào điểm số cuối cùng và không có sự tương tác giữa các vấn đề. Tính độc lập này đảm bảo rằng việc biểu diễn điểm số là tuyến tính về mặt hiện diện bit. Bởi vì mọi điểm hợp lệ từ 0 đến 7 là một vectơ 3 bit duy nhất, nên tập hợp các bộ đã giải sẽ tương ứng chính xác với OR theo bit. Không có biểu diễn thay thế nào tồn tại nên kết quả được xác định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

L, Y = map(int, input().split())
print(L | Y)
```Giải pháp dựa vào việc đọc hai số nguyên và áp dụng phép toán OR theo bit. Chi tiết triển khai chính là không cần chuyển đổi hoặc giải mã; các giá trị đầu vào đã đại diện cho mặt nạ bit được ngụy trang. 

Một lỗi phổ biến là cố gắng phân tách các số thành chữ số thập phân hoặc xây dựng lại các tập hợp con theo cách thủ công. Điều đó là không cần thiết vì mã hóa đã khớp với cấu trúc nhị phân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
```| L | Y | L HOẶC Y | Kết quả | 
| --- | --- | --- | --- | 
| 000 | 000 | 000 | 0 | 

Cả hai học sinh đều không giải quyết được gì nên đoàn trống và Passinho cũng không giải được gì. 

### Ví dụ 2 

đầu vào:```
1 2
```| L | Y | L HOẶC Y | Kết quả | 
| --- | --- | --- | --- | 
| 001 | 010 | 011 | 3 | 

Lucca chỉ giải được bài toán 1 điểm, Yvens chỉ giải được bài toán 2 điểm. Passinho giải được cả hai, tổng điểm là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một thao tác bitwise duy nhất và phân tích cú pháp đầu vào | 
| Không gian | O(1) | Lưu trữ liên tục cho hai số nguyên | 

Các ràng buộc giới hạn các giá trị ở mức tối đa là 7, do đó, ngay cả một phương pháp đắt tiền hơn cũng sẽ không đáng kể. Giải pháp bitwise phù hợp thoải mái trong giới hạn và là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    L, Y = map(int, input().split())
    return str(L | Y)

# provided samples
assert run("0 0\n") == "0", "sample 1"
assert run("1 2\n") == "3", "sample 2"
assert run("5 3\n") == "7", "sample 3"

# custom cases
assert run("7 0\n") == "7", "all bits already set"
assert run("4 2\n") == "6", "disjoint higher bits"
assert run("1 4\n") == "5", "non-adjacent bits"
assert run("6 3\n") == "7", "completing missing bit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 0 | 7 | hành vi nhận dạng của OR | 
| 4 2 | 6 | kết hợp các bit cao hơn không chồng chéo | 
| 1 4 | 5 | hợp nhất bit không liền kề | 
| 6 3 | 7 | hoàn thành bảo hiểm đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả hai đầu vào giống hệt nhau, chẳng hạn như 3 và 3. Trong trường hợp đó, cả hai học sinh đều giải được những bài toán giống nhau, vì vậy Passinho cũng phải giải cùng một bộ. Phép tính L OR Y = 3 OR 3 = 3 bảo toàn chính xác giá trị ban đầu. 

Một trường hợp khác là khi một đầu vào bằng 0. Ví dụ: L = 0 và Y = 5. Phép kết hợp chỉ cần tái tạo Y. Phép toán OR cho 0 HOẶC 5 = 5, phù hợp với cách giải thích rằng Passinho giải chính xác những gì Yvens đã giải. 

Trường hợp cuối cùng là khi cả hai đầu vào đều rời rạc, chẳng hạn như 1 và 2 hoặc 1 và 4 và 2. Phép toán OR tích lũy chính xác tất cả các bit mà không bị nhiễu, tạo ra sự kết hợp đầy đủ lên đến 7 khi tất cả các bit được che phủ.
