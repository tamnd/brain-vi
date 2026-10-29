---
title: "CF 106192I - \u041d\u0430\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u0436\u0443\u043a\u043e\u0432"
description: "Thành phố được mô hình hóa như một mạng lưới các giao lộ $n lần n$. Mỗi giao lộ $(x, y)$ nằm trên đường thẳng đứng $x$ và đường ngang $y$."
date: "2026-06-21T09:48:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106192
codeforces_index: "I"
codeforces_contest_name: "\u041f\u0435\u0440\u043c\u0441\u043a\u0430\u044f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106192
solve_time_s: 47
verified: true
draft: false
---

[CF 106192I - \u041d\u0430\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u0436\u0443\u043a\u043e\u0432](https://codeforces.com/problemset/problem/106192/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố được mô hình hóa như một$n \times n$lưới các nút giao. Mỗi ngã tư$(x, y)$nằm trên một con đường thẳng đứng$x$và một con đường nằm ngang$y$. Chúng ta cần bố trí người bảo vệ ở một số ngã tư, mỗi người bảo vệ “che” chính xác một đường dọc và một đường ngang, nghĩa là mỗi người bảo vệ đóng góp một lần vào hàng$x$và một lần vào cột$y$. 

Yêu cầu là mỗi hàng, mỗi cột phải có chính xác$k$lính canh đặt trên các giao lộ thuộc về nó. Mỗi giao lộ có thể chứa nhiều nhất một người bảo vệ, vì vậy chúng tôi đang lựa chọn một cách hiệu quả các ô của một$n \times n$ma trận sao cho tổng hàng và tổng mỗi cột bằng chính xác$k$. 

Đầu ra cũng yêu cầu số lượng lính canh tối thiểu có thể. Vì mỗi người bảo vệ đóng góp vào đúng một hàng và một cột nên việc đếm sự đóng góp của hàng sẽ cho$n \cdot k$tổng số đóng góp và mỗi người bảo vệ đóng góp chính xác một vào số hàng. Điều này đã ghim câu trả lời cho$m = n \cdot k$, vì vậy nhiệm vụ không phải là tối ưu hóa theo nghĩa thông thường mà là xây dựng một cấu hình hợp lệ với nhiều ô được chọn đó. 

Những hạn chế$n \le 1000$Và$k \le n$ngụ ý chúng ta cần một$O(nk)$hoặc$O(n^2)$xây dựng tồi tệ nhất. Bất cứ điều gì liên quan đến luồng hoặc kết hợp sẽ là quá mức cần thiết nhưng vẫn khả thi về mặt kỹ thuật. Cấu trúc gợi ý mạnh mẽ về một mô hình tổ hợp trực tiếp. 

Một trường hợp thất bại tinh tế xuất hiện khi$k = 0$. Sau đó, không nên đặt tấm bảo vệ nào và đầu ra phải trống sau khi in số 0. Một trường hợp ranh giới khác là$k = n$, trong đó mỗi hàng và cột phải chứa tất cả$n$cột, buộc lưới đầy đủ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là coi nó như một cấu trúc đồ thị hai bên: các hàng ở một bên, các cột ở bên kia và mỗi bảo vệ là một cạnh. Chúng ta cần một đồ thị hai bên trong đó mọi đỉnh ở hai bên đều có bậc chính xác$k$. Người ta có thể cố gắng tìm kiếm một biểu đồ như vậy bằng cách liên tục thêm các cạnh và kiểm tra các ràng buộc hoặc chạy cấu trúc luồng tối đa trong đó mỗi hàng và cột có dung lượng$k$. Điều này hiệu quả vì mỗi giải pháp hợp lệ tương ứng với một đa đồ thị lưỡng cực có ràng buộc về mức độ hoàn hảo. 

Tuy nhiên, việc xây dựng dựa trên dòng chảy xây dựng một mạng lưới với$O(n^2)$các cạnh tiềm năng và chạy ít nhất$O(n^3)$hoặc tương tự tùy thuộc vào việc thực hiện. Điều này là không cần thiết vì cấu trúc đối xứng và đều đặn. 

Quan sát quan trọng là chúng ta chỉ cần một$k$-biểu đồ lưỡng cực đều đặn giữa hai phân vùng có kích thước bằng nhau$n$. Những đồ thị như vậy có cấu trúc tuần hoàn đơn giản: nối từng hàng$i$ĐẾN$k$các cột liên tiếp, bao quanh modulo$n$. Điều này tự động đảm bảo mỗi hàng có chính xác$k$các cạnh theo cách xây dựng và mỗi cột cũng nhận được chính xác$k$các cạnh vì sự thay đổi phân phối các điểm cuối một cách đồng đều. 

Điều này làm giảm vấn đề từ vấn đề thỏa mãn ràng buộc sang nhiệm vụ tạo mẫu xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng ràng buộc dòng chảy / vũ phu |$O(n^3)$|$O(n^2)$| Quá chậm | 
| Xây dựng tuần hoàn |$O(nk)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu các hàng là các đỉnh bên trái và các cột là các đỉnh bên phải trong biểu đồ hai bên. Chúng tôi xây dựng chính xác$k$các cạnh trên mỗi hàng bằng cách sử dụng cửa sổ trượt qua các cột. 

1. Sửa chỉ mục hàng$i$. Chúng tôi chỉ định nó chính xác$k$cột. 
2. Đối với mỗi hàng$i$, kết nối nó với các cột$i, i+1, \dots, i+k-1$, sử dụng modulo$n$gói. 
3. Mỗi cặp như vậy$(i, j)$tương ứng với việc đặt một người bảo vệ ở ngã tư$(i, j)$. 
4. Xuất ra tất cả các cặp được xây dựng. 

Việc gói là cần thiết vì nếu không có số học modulo, các hàng gần cuối sẽ hết cột. Sự dịch chuyển theo chu kỳ đảm bảo tính đồng nhất trên tất cả các hàng. 

### Tại sao nó hoạt động 

Mỗi hàng tạo ra chính xác$k$các cột riêng biệt theo cách xây dựng, do đó các ràng buộc về hàng được đáp ứng ngay lập tức. Đối với các ràng buộc về cột, hãy quan sát cột đó$j$được bao gồm trong hàng$i$chính xác khi nào$j \in [i, i+k-1]$modulo$n$. Trên tất cả các hàng, mẫu khoảng này dịch chuyển đồng đều, do đó mỗi cột được bao phủ chính xác$k$lần. Điều này tương đương với việc nói rằng việc xây dựng là bất biến dưới sự dịch chuyển theo chu kỳ của các chỉ số, điều này buộc phải phân bố mức độ đồng đều ở cả hai bên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    res = []
    
    for i in range(n):
        for t in range(k):
            j = (i + t) % n
            res.append((i + 1, j + 1))
    
    out = [str(len(res))]
    for x, y in res:
        out.append(f"{x} {y}")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng theo chu kỳ. Hàng và cột được chuyển đổi thành chỉ mục dựa trên 1 tại thời điểm đầu ra. Vòng lặp lồng nhau$n$hàng và$k$phần bù tạo ra chính xác$n \cdot k$lính gác. 

Một lỗi triển khai phổ biến là quên modulo$n$, điều này phá vỡ tính chính xác của các hàng gần ranh giới. Một cách khác là lập chỉ mục từng cái một khi chuyển đổi giữa logic bên trong dựa trên 0 và định dạng đầu ra dựa trên 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
```Chúng tôi xây dựng một người bảo vệ mỗi hàng. 

| Hàng ngang$i$| Cột đã chọn$j$| Cặp đầu ra | 
| --- | --- | --- | 
| 1 | 1 | (1, 1) | 
| 2 | 2 | (2, 2) | 
| 3 | 3 | (3, 3) | 

Đầu ra:```
3
1 1
2 2
3 3
```Điều này thỏa mãn mỗi hàng và cột có đúng một người bảo vệ. 

### Ví dụ 2 

đầu vào:```
4 2
```| Hàng ngang$i$| Cột được chọn | Cặp đầu ra | 
| --- | --- | --- | 
| 1 | 1, 2 | (1,1), (1,2) | 
| 2 | 2, 3 | (2,2), (2,3) | 
| 3 | 3, 4 | (3,3), (3,4) | 
| 4 | 4, 1 | (4,4), (4,1) | 

Đầu ra:```
8
1 1
1 2
2 2
2 3
3 3
3 4
4 4
4 1
```Ví dụ thứ hai cho thấy cách bao bọc duy trì tính đồng nhất giữa các ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Chúng tôi tạo chính xác một mục đầu ra cho mỗi người bảo vệ | 
| Không gian |$O(nk)$| Lưu trữ tất cả các vị trí bảo vệ trước khi in | 

Ràng buộc$n \le 1000$đảm bảo$n^2 \le 10^6$, vì vậy ngay cả trường hợp xấu nhất$k = n$tạo ra khoảng một triệu cặp, dễ dàng nằm trong giới hạn trong Python khi sử dụng đầu ra được đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, sys.stdin.readline().split())
    
    res = []
    for i in range(n):
        for t in range(k):
            j = (i + t) % n
            res.append((i + 1, j + 1))
    
    out = [str(len(res))]
    for x, y in res:
        out.append(f"{x} {y}")
    return "\n".join(out)

# provided sample
assert run("3 1\n") == "3\n1 1\n2 2\n3 3"

# k = 0 edge case
assert run("5 0\n") == "0"

# full grid case
out = run("2 2\n").splitlines()
assert out[0] == "4"

# uniform small case
assert run("1 1\n") == "1\n1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | lựa chọn đường chéo | tính đúng đắn cơ bản | 
| 5 0 | 0 | xây dựng trống | 
| 2 2 | lưới đầy đủ | mật độ tối đa | 
| 1 1 | ô đơn | trường hợp tối thiểu khác không | 

## Vỏ cạnh 

cho$k = 0$, các vòng lặp không tạo ra cặp nào. Danh sách đầu ra vẫn trống và chương trình in chính xác số 0. Trường hợp này đảm bảo rằng việc xây dựng không vô tình phát ra tọa độ không hợp lệ khi vòng lặp bên trong bị bỏ qua. 

Vì$k = n$, mỗi hàng kết nối với tất cả các cột. Hoạt động modulo vẫn hoạt động, nhưng nó trở nên dư thừa vì không cần gói. Thuật toán tạo ra đầy đủ$n^2$lưới và mỗi hàng và cột có chính xác$n$vệ sĩ theo yêu cầu.
