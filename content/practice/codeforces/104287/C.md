---
title: "CF 104287C - Không quét"
description: "Chúng ta đang xem xét một chuỗi $n$ vòng chơi độc lập. Trong mỗi vòng có chính xác một người chơi thắng. Một người chơi đặc biệt là Thomas và có $k$ đối thủ khác, vì vậy mỗi vòng đều có khả năng là $k+1$ người chiến thắng."
date: "2026-07-01T20:44:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "C"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 60
verified: true
draft: false
---

[CF 104287C - Không quét](https://codeforces.com/problemset/problem/104287/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xem xét một chuỗi$n$các vòng chơi độc lập. Trong mỗi vòng có chính xác một người chơi thắng. Một cầu thủ đặc biệt là Thomas, và có$k$các đối thủ cạnh tranh khác, vì vậy mỗi vòng đều có$k+1$những người chiến thắng có thể. 

Một kết quả đầy đủ của trò chơi chỉ đơn giản là một khoảng thời gian dài-$n$trình tự trong đó mỗi vị trí chọn một trong các vị trí này$k+1$người chơi là người chiến thắng trong vòng đó. Trong số tất cả các chuỗi như vậy, chúng tôi muốn tính những chuỗi mà Thomas không thắng trong mọi hiệp đấu. Nói cách khác, chúng tôi loại trừ trường hợp suy biến đơn trong đó mọi phần tử trong dãy đều là Thomas. 

Tổng số dãy có thể là$(k+1)^n$, vì mỗi vòng có$k+1$những lựa chọn độc lập. Cấu hình bị cấm duy nhất là cấu hình mà Thomas được chọn trong tất cả các trường hợp.$n$các vị trí. 

Những hạn chế là cực kỳ nhỏ:$n \le 10$Và$k \le 4$. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc liệt kê đơn giản tất cả các kết quả cũng có thể thực hiện được, vì số lượng chuỗi tối đa là$5^{10} \approx 9.7 \times 10^6$, nằm ở ranh giới nhưng vẫn có thể quản lý được trong Python được tối ưu hóa nếu được thực hiện cẩn thận. Tuy nhiên, cấu trúc đủ đơn giản nên việc liệt kê là không cần thiết. 

Một trường hợp khó nhận thấy là khi$n = 1$. Trong trường hợp đó, “Thomas được quét” có nghĩa là Thomas thắng hiệp duy nhất. Vì vậy, câu trả lời phải là tất cả người chơi ngoại trừ cấu hình duy nhất đó. Điều đó trở thành$k$, vì có$k+1$tổng số lựa chọn và chúng tôi loại trừ chính xác một lựa chọn. 

Một trường hợp cạnh khác là khi$k = 0$, nghĩa là Thomas là người chơi duy nhất. Kết quả duy nhất có thể xảy ra là quét sạch, vì vậy câu trả lời phải bằng 0 cho bất kỳ$n \ge 1$. Công thức phải xử lý việc này một cách tự nhiên. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là tạo ra tất cả các chuỗi người chiến thắng có thể có. Đối với mỗi vòng, chúng tôi chọn một trong$k+1$người chơi, tạo thành một cây có chiều sâu$n$với hệ số phân nhánh$k+1$. Điều này tạo ra chính xác$(k+1)^n$lá. Đối với mỗi lá, chúng tôi kiểm tra xem mọi vị trí có phải là Thomas hay không và trừ trường hợp không hợp lệ đó. 

Điều này hoạt động chính xác vì nó xây dựng rõ ràng tất cả các kết quả. Điểm thất bại là hiệu quả: thậm chí ở giới hạn trên$n = 10$,$k = 4$, không gian tìm kiếm tăng lên gần mười triệu chuỗi và bất kỳ chi phí bổ sung nào bên trong đệ quy hoặc cấu trúc chuỗi đều khiến nó trở nên dễ vỡ trong Python dưới các giới hạn nghiêm ngặt. 

Quan sát quan trọng là cấu trúc hoàn toàn đồng nhất qua các vòng. Mỗi vòng đều độc lập và điều kiện “Thomas thắng tất cả các vòng” tương ứng với đúng một chuỗi. Vì vậy, chúng ta không cần liệt kê bất cứ điều gì; chúng ta chỉ cần trừ cấu hình đó khỏi tổng số. 

Vì vậy, câu trả lời chỉ đơn giản là:$$(k+1)^n - 1$$Lực lượng vũ phu đếm rõ ràng cả hai phần, trong khi giải pháp tối ưu hóa trực tiếp đếm toàn bộ không gian và loại bỏ trường hợp bị cấm duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O((k+1)^n \cdot n)$|$O(n)$| Quá chậm | 
| Công thức trực tiếp |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$k$. Những điều này xác định số vòng và số lượng người chơi không phải Thomas. 
2. Tính tổng số kết quả có thể xảy ra là$(k+1)^n$, vì mỗi vòng độc lập chọn một người chiến thắng trong số$k+1$đối thủ cạnh tranh. 
3. Trừ 1 từ tổng số này để loại bỏ kết quả không hợp lệ duy nhất trong đó Thomas thắng mọi vòng đấu. 
4. Xuất ra giá trị kết quả. 

Bước lý luận duy nhất quan trọng là nhận ra rằng điều kiện không hợp lệ tương ứng với chính xác một cấu hình, không phải một phạm vi hoặc tập hợp con. Đó là những gì cho phép trừ thay vì loại trừ hoặc lập trình động. 

### Tại sao nó hoạt động 

Mỗi kết quả trò chơi hợp lệ tương ứng duy nhất với một độ dài-$n$trình tự trên một bảng chữ cái có kích thước$k+1$. Ánh xạ này mang tính chất phỏng đoán: mỗi chuỗi xác định chính xác một kết quả và mỗi kết quả xác định chính xác một chuỗi. 

Điều kiện “quét” tương ứng với một chuỗi duy nhất trong đó mọi vị trí đều là Thomas. Vì không có chuỗi nào khác chia sẻ thuộc tính này nên việc loại bỏ chính xác một chuỗi khỏi tổng số sẽ tạo ra câu trả lời đúng. Không có sự chồng chéo hoặc tương tác giữa các vòng có thể tạo thêm trường hợp bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

total = pow(k + 1, n)
print(total - 1)
```Việc thực hiện phản ánh trực tiếp việc giảm toán học. Tích hợp sẵn`pow`được sử dụng để tính lũy thừa nhanh, điều này không cần thiết đối với các ràng buộc nhỏ như vậy nhưng vẫn giữ cho mã sạch và an toàn. 

Phép trừ 1 là an toàn cho tất cả các đầu vào hợp lệ vì khi$k \ge 0$Và$n \ge 1$, chúng tôi luôn có$(k+1)^n \ge 1$, đẳng thức chỉ xảy ra tại$k = 0$, trong đó kết quả chính xác trở thành số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 2, k = 1$Người chơi là Thomas và một đối thủ. 

Tổng kết quả là$2^2 = 4$. 

| Vòng 1 | Vòng 2 | Có hiệu lực? | 
| --- | --- | --- | 
| T | T | Không | 
| T | Ồ | Có | 
| Ồ | T | Có | 
| Ồ | Ồ | Có | 

Chúng tôi trừ trường hợp không hợp lệ duy nhất$TT$, để lại 3 kết quả hợp lệ. 

Điều này xác nhận ý tưởng rằng chỉ có một cấu hình bị loại trừ. 

### Ví dụ 2:$n = 10, k = 3$Tổng kết quả là$4^{10} = 1048576$. 

Kết quả không hợp lệ duy nhất là Thomas thắng cả 10 hiệp nên chúng ta trừ 1. 

Kết quả là$1048575$. 

Không có cấu trúc nào ngoài việc đếm thống nhất, điều này chứng tỏ rằng giải pháp sẽ mở rộng hoàn toàn thông qua lũy thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Một lũy thừa và một phép trừ | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên | 

Các ràng buộc tuy nhỏ nhưng giải pháp đã tối ưu cho mọi kích thước đầu vào hợp lý. Kể cả nếu$n$lớn, phép lũy thừa tích hợp của Python sẽ xử lý nó một cách hiệu quả và logic sẽ không thay đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    return str(pow(k + 1, n) - 1)

# provided samples
assert run("2 1\n") == "3"
assert run("10 3\n") == "1048575"

# minimum n, k = 0 (only Thomas exists, always sweep)
assert run("1 0\n") == "0"

# single round with multiple opponents
assert run("1 3\n") == "3"

# all equal edge: k = 1, n = 1
assert run("1 1\n") == "1"

# slightly larger sanity check
assert run("3 2\n") == str(3**3 - 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | Chỉ có Thomas tồn tại, quét là không thể tránh khỏi | 
| 1 3 | 3 | Vòng đơn chỉ loại trừ Thomas | 
| 3 2 | 26 | Tính đúng đắn chung của công thức | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi$k = 0$. Đầu vào mô tả tình huống chỉ có Thomas tồn tại. Đối với bất kỳ$n$, mọi hiệp đấu đều phải do Thomas thắng nên chỉ có một kết quả duy nhất và luôn là hòa. Công thức cho$(0+1)^n - 1 = 0$, phù hợp trực tiếp với lý do. 

Vì$n = 1$, vấn đề giảm xuống còn việc chọn một người chiến thắng duy nhất trong số$k+1$người chơi. Chính xác một trong những lựa chọn này không hợp lệ (Thomas), vì vậy kết quả sẽ là$k$. Công thức cho$(k+1)^1 - 1 = k$, căn chỉnh chính xác. 

Đối với lớn hơn$n$, không có trường hợp cấu trúc mới nào xuất hiện vì tính độc lập giữa các vòng đảm bảo không có hiệu ứng tương tác. Mỗi vòng bổ sung chỉ nhân tổng không gian lên và cấu hình bị cấm vẫn là số ít.
