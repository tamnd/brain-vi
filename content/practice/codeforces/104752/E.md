---
title: "CF 104752E - Hòn đảo kỳ lạ"
description: "Cửa hàng có một mệnh giá tiền cố định duy nhất và mỗi mặt hàng đều có giá bằng đô la. Một du khách muốn trả tiền cho một món hàng nhưng Juan chỉ có thể sử dụng những tờ tiền có mệnh giá đó."
date: "2026-06-28T22:57:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "E"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 80
verified: true
draft: false
---

[CF 104752E - Hòn đảo kỳ lạ](https://codeforces.com/problemset/problem/104752/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cửa hàng có một mệnh giá tiền cố định duy nhất và mỗi mặt hàng đều có giá bằng đô la. Một du khách muốn trả tiền cho một món hàng nhưng Juan chỉ có thể sử dụng những tờ tiền có mệnh giá đó. Nhiệm vụ là xác định cần bao nhiêu tờ tiền như vậy để tổng giá trị của chúng đạt đến giá món hàng. 

Vì mỗi hóa đơn đóng góp chính xác$A$đô la, chúng tôi đang phân tích giá một cách hiệu quả$B$thành bội số của$A$. Nếu giá không thể chia hết một cách chính xác, chúng ta vẫn cần có đủ hóa đơn để thanh toán toàn bộ giá đó, vì cách giải thích này không cho phép trả thiếu. Điều đó biến vấn đề thành tính toán số nguyên nhỏ nhất$k$như vậy$k \cdot A \ge B$. 

Các ràng buộc đi lên đến$10^9$, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Việc lặp lại hoặc mô phỏng việc bổ sung các hóa đơn sẽ cần tới$10^9$bước trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Điều này ngay lập tức gợi ý rằng câu trả lời phải được rút ra bằng cách sử dụng số học chứ không phải mô phỏng. 

Một trường hợp khó nhận thấy là khi$B$chính xác là chia hết cho$A$. Ví dụ, nếu$A = 5$Và$B = 10$, câu trả lời chính xác là 2. Nếu$A = 5$Và$B = 11$, câu trả lời là 3 chứ không phải 2, vì hai tờ tiền chỉ có 10, không đủ. 

Một trường hợp góc khác xuất hiện khi$A > B$. Ví dụ,$A = 10$,$B = 3$. Cần phải có một hóa đơn vì không thể thanh toán theo từng phần và không có hóa đơn nào sẽ không bao gồm được giá. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ liên tục trừ đi$A$từ$B$cho đến khi số dư không dương. Điều này đúng vì mỗi phép trừ tương ứng với việc sử dụng một tờ tiền. Tuy nhiên, trong trường hợp xấu nhất khi$A = 1$Và$B = 10^9$, điều này đòi hỏi$10^9$lặp đi lặp lại, quá chậm. 

Quan sát quan trọng là chúng tôi đang tính toán có bao nhiêu khối kích thước đầy đủ$A$phù hợp với$B$, cộng thêm có thể thêm một khối nếu còn dư. Đây chính xác là trần của giải đấu$B / A$. Khi chúng tôi nhận ra điều này, vấn đề sẽ giảm xuống thành một phép toán số học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trừ Brute Force |$O(B/A)$|$O(1)$| Quá chậm | 
| Phân chia trần |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Đọc số nguyên$A$Và$B$. Chúng đại diện cho giá trị hóa đơn cố định và số tiền thanh toán được yêu cầu. 
2. Tính toán xem$B$chia hết cho$A$. Nếu đúng thì câu trả lời là chính xác$B / A$, vì hóa đơn hoàn toàn phù hợp mà không lãng phí. 
3. Nếu$B$không chia hết cho$A$, tính toán$B // A + 1$. Phép chia số nguyên cho biết số lượng hóa đơn đầy đủ phù hợp và số dư bao gồm số tiền chưa thanh toán còn lại. 
4. Xuất giá trị tính toán. 

Điểm quyết định quan trọng là việc xử lý phần còn lại. Nếu chúng ta phớt lờ nó, chúng ta sẽ trả lương thấp hơn; nếu chúng tôi bù đắp vượt mức chính xác bằng một hóa đơn bổ sung, chúng tôi luôn đạt hoặc vượt mục tiêu với mức vượt mức tối thiểu. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp tương ứng với việc chọn một số nguyên$k$như vậy$kA \ge B$. Tập hợp các số nguyên như vậy bắt đầu tại$\lceil B/A \rceil$. Bất kỳ giá trị nhỏ hơn$k < \lceil B/A \rceil$ngụ ý$kA < B$, không thể trang trải được giá. Bất kỳ giá trị lớn hơn đều làm tăng chi phí một cách không cần thiết. Vì vậy, vách ngăn trần là sự lựa chọn hợp lệ tối thiểu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A = int(input().strip())
B = int(input().strip())

# compute ceiling of B / A
ans = (B + A - 1) // A

print(ans)
```Giải pháp dựa vào thủ thuật trần số nguyên cổ điển. Thay vì kiểm tra rõ ràng khả năng chia hết, nó sử dụng danh tính$\lceil B/A \rceil = (B + A - 1) // A$. Điều này tránh phân nhánh và giữ tính toán trong một biểu thức số học duy nhất. 

Phép trừ 1 bên trong tử số đảm bảo rằng các bội số chính xác không bị làm tròn sai. Ví dụ, khi$B = 10$Và$A = 5$, chúng tôi nhận được$(10 + 4) // 5 = 2$, trong khi khi$B = 11$, chúng tôi nhận được$(11 + 4) // 5 = 3$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$A = 2$,$B = 10$| Bước | A | B | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 10 | | | 
| Áp dụng công thức | 2 | 10 | (10 + 1) // 2 | 5 | 

Kết quả là 5, nghĩa là năm hóa đơn có giá trị 2 chính xác đạt đến 10. Điều này khẳng định công thức hoạt động tốt khi phép chia chính xác. 

### Ví dụ 2 

đầu vào:$A = 5$,$B = 5$| Bước | A | B | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | 5 | | | 
| Áp dụng công thức | 5 | 5 | (5 + 4) // 5 | 1 | 

Ở đây một tờ tiền là đủ vì giá cả khớp chính xác với mệnh giá. Việc tính toán không được tính quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép các giá trị lên đến$10^9$, nhưng lời giải không phụ thuộc vào phép lặp hoặc đệ quy. Nó duy trì thời gian không đổi và dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    A = int(input().strip())
    B = int(input().strip())
    ans = (B + A - 1) // A
    return str(ans)

# provided samples (interpreted as A then B per statement)
assert run("2\n10\n") == "5", "sample 1"
assert run("5\n5\n") == "1", "sample 2"
assert run("3\n10\n") == "4", "sample 3"

# custom cases
assert run("1\n1000000000\n") == "1000000000", "unit denomination"
assert run("1000000000\n1\n") == "1", "large A small B"
assert run("7\n49\n") == "7", "exact division"
assert run("7\n50\n") == "8", "off-by-one remainder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1e9 | 1e9 | cạnh mệnh giá tối thiểu | 
| 1e9, 1 | 1 | trường hợp ước số lớn | 
| 7, 49 | 7 | phép chia chính xác đúng đắn | 
| 7, 50 | 8 | hành vi trần | 

## Vỏ cạnh 

Khi nào$A = 1$, mỗi đô la cần một tờ tiền. Thuật toán tính toán$(B + 0) // 1 = B$, tạo ra kết quả tuyến tính chính xác mà không cần xử lý đặc biệt. 

Khi$A > B$, nói$A = 10$,$B = 3$, công thức cho$(3 + 9) // 10 = 1$. Điều này phù hợp với yêu cầu phải sử dụng ít nhất một hóa đơn ngay cả khi nó trả quá nhiều. 

Khi$B$chính xác là bội số của$A$, chẳng hạn như$A = 5$,$B = 20$, biểu thức mang lại$(20 + 4) // 5 = 4$, tránh làm tròn số không cần thiết và khẳng định độ kín của công thức trần.
