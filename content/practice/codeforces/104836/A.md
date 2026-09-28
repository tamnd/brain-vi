---
title: "CF 104836A - \u0427\u0438\u0441\u043b\u043e \u0431\u0435\u043b\u044b\u0445 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u043e\u0432"
description: "Chúng ta có một bàn cờ tiêu chuẩn $n nhân n$ trong đó hình vuông trên cùng bên trái có màu đen và các màu xen kẽ hoàn hảo theo cả chiều ngang và chiều dọc. Điều này tạo ra mẫu bàn cờ thông thường. Nhiệm vụ là xác định có bao nhiêu hình vuông có kích thước $1 nhân 1$ có màu trắng."
date: "2026-06-28T11:42:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104836
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0433\u043e\u0440\u043e\u0434\u0435 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u0440\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0435 \u041a\u0430\u0440\u0435\u043b\u0438\u044f 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441)"
rating: 0
weight: 104836
solve_time_s: 60
verified: true
draft: false
---

[CF 104836A - \u0427\u0438\u0441\u043b\u043e \u0431\u0435\u043b\u044b\u0445 \u043a\u0432\u0430\u0434\u0440\u0430\u0442\u043e\u0432](https://codeforces.com/problemset/problem/104836/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một tiêu chuẩn$n \times n$bàn cờ trong đó hình vuông trên cùng bên trái có màu đen và các màu xen kẽ hoàn hảo theo cả chiều ngang và chiều dọc. Điều này tạo ra mẫu bàn cờ thông thường. Nhiệm vụ là xác định có bao nhiêu hình vuông có kích thước$1 \times 1$có màu trắng. 

Kích thước bảng được điều khiển bởi một số nguyên duy nhất$n$. Đầu ra chỉ là số lượng ô trắng trong mẫu xen kẽ này. 

Những hạn chế quan trọng bởi vì$n$có thể lớn như$10^9$. Bất kỳ giải pháp nào cố gắng xây dựng lưới hoặc lặp lại trên tất cả các ô đều ngay lập tức không thể thực hiện được. Một mô phỏng đầy đủ sẽ yêu cầu$n^2$hoạt động đạt tới$10^{18}$trong trường hợp xấu nhất, vượt xa giới hạn khả thi ngay cả đối với$n \le 10^3$. 

Một cạm bẫy ngây thơ xuất hiện khi cố gắng tô màu rõ ràng các hàng và đếm các ô màu trắng theo từng hàng. Ví dụ, đối với$n = 4$, người ta có thể cố gắng xây dựng lưới: 

đầu vào:```
4
```Một mô phỏng thô bạo sẽ lấp đầy 16 ô, thay thế các màu và đếm số lượng màu trắng. Điều đó hiệu quả với nhỏ$n$, nhưng logic tương tự trở nên không khả thi đối với$n = 10^9$, nơi không có cấu trúc rõ ràng nào có thể tồn tại trong bộ nhớ hoặc thời gian. 

Một trường hợp tinh tế khác là những tấm ván có kích thước lẻ. Ví dụ: 

đầu vào:```
3
```Đầu ra:```
4
```Một giả định ngây thơ rằng một nửa số ô luôn có màu trắng sẽ cho$4.5$, điều đó không có ý nghĩa. Cấu trúc chẵn lẻ có vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại trên mọi ô$(i, j)$, xác định xem nó có màu trắng hay không dựa trên tính chẵn lẻ và tăng bộ đếm. Một ô có màu trắng nếu tổng tọa độ của nó có giá trị chẵn lẻ ngược lại với ô đen trên cùng bên trái. Vì màu đen bắt đầu lúc$(1,1)$, có tổng$2$, một ô có màu trắng khi$(i + j)$thật kỳ quặc. 

Phương pháp này đúng vì nó mã hóa trực tiếp quy tắc tô màu. Tuy nhiên, nó đòi hỏi$n^2$séc. Khi$n = 10^9$, điều này trở thành$10^{18}$hoạt động hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là bảng có tính tuần hoàn hoàn toàn. Mỗi khối 2 x 2 chứa chính xác hai ô trắng và hai ô đen. Điều này loại bỏ bất kỳ nhu cầu lặp lại. Thay vì đếm từng ô riêng lẻ, chúng ta chỉ cần đếm xem có bao nhiêu cặp hàng và cột đầy đủ và phần còn lại hoạt động như thế nào khi$n$thật kỳ quặc. 

Nếu như$n$chẵn, bảng chia thành$2 \times 2$các khối, mỗi khối đóng góp chính xác 2 ô trắng. Nếu như$n$là số lẻ, có thêm một phần hàng và cột giữ nguyên cấu trúc xen kẽ, thêm tổng cộng một ô trắng bổ sung so với trường hợp chẵn. 

Điều này dẫn trực tiếp đến một biểu thức dạng đóng dựa trên phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng màu của mỗi ô chỉ phụ thuộc vào độ chẵn lẻ của tọa độ của nó. Một ô màu trắng xuất hiện khi tổng tọa độ là số lẻ theo cấu hình ban đầu đã cho. Điều này loại bỏ sự cần thiết của bất kỳ mô phỏng nào. 
2. Chia bảng thành đầy đủ$2 \times 2$khối. Mỗi khối như vậy chứa đúng 2 ô màu trắng. Số khối hoàn chỉnh là$(n // 2)^2$. Điều này nắm bắt tất cả các cấu trúc được ghép nối đầy đủ. 
3. Xử lý các hàng, cột còn sót lại khi$n$thật kỳ quặc. Nếu có hàng cuối cùng hoặc cột cuối cùng, chúng sẽ đóng góp các ô bổ sung theo mẫu xen kẽ, dẫn đến kết quả chính xác$\lfloor n^2 / 2 \rfloor$tổng thể các tế bào trắng. 
4. Tính đáp án cuối cùng là phép chia số nguyên$n \cdot n // 2$, xử lý một cách tự nhiên cả trường hợp chẵn và lẻ mà không cần phân nhánh. 

### Tại sao nó hoạt động 

Việc tô màu tạo ra một phân vùng chẵn lẻ nghiêm ngặt của lưới: mỗi ô thuộc về chính xác một trong hai lớp dựa trên$(i + j) \bmod 2$. Bởi vì lưới bắt đầu bằng một ô màu đen tại$(1,1)$, các ô màu trắng tương ứng với một trong các lớp chẵn lẻ được phân bố đồng đều trên lưới. Trên bất kỳ vùng hình chữ nhật nào, tính chẵn lẻ xen kẽ hoàn hảo và sự mất cân bằng chỉ có thể xảy ra ở tối đa một ô khi vùng đó là số lẻ. Điều này đảm bảo rằng chính xác một nửa số ô được làm tròn xuống có màu trắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print((n * n) // 2)
```Giải pháp hoàn toàn dựa vào cấu trúc chẵn lẻ của bàn cờ. biểu hiện$n * n$tính tổng số ô. Chia cho 2 bằng phép chia số nguyên sẽ tự động xử lý cả trường hợp chẵn và lẻ bằng cách làm tròn xuống, khớp với số lượng ô trắng chính xác. 

Một điểm tinh tế là số nguyên Python xử lý các giá trị lớn một cách an toàn, vì vậy ngay cả khi$n = 10^9$, tính toán$n^2$vẫn còn hiệu lực. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, người ta cần đảm bảo số học 64 bit để tránh tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 2$Chúng tôi tính toán$n^2 = 4$, sau đó sàn chia cho 2. 

| Bước | Giá trị | 
| --- | --- | 
| n | 2 | 
| n² | 4 | 
| tế bào trắng | 2 | 

Điều này khớp với lưới rõ ràng: 

Đen, Trắng 

Trắng, Đen 

Vậy có 2 ô màu trắng. 

Dấu vết xác nhận rằng công thức xử lý chính xác hình vuông không tầm thường nhỏ nhất. 

### Ví dụ 2:$n = 3$| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| n² | 9 | 
| tế bào trắng | 4 | 

Chế độ xem thủ công của bảng hiển thị: 

Đen, Trắng, Đen 

Trắng, đen, trắng 

Đen, Trắng, Đen 

Đếm người da trắng cho 4. Công thức giải quyết chính xác sự mất cân bằng do lưới có kích thước lẻ tạo ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ các phép toán số học được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp thỏa mãn một cách tầm thường các ràng buộc lên đến$n = 10^9$, vì nó thực hiện một số lượng hoạt động không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    return str((n * n) // 2)

assert run("2\n") == "2", "sample 1"
assert run("3\n") == "4", "sample 2"
assert run("1\n") == "0", "minimum size"
assert run("4\n") == "8", "even square"
assert run("1000000000\n") == str((10**9 * 10**9)//2), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cạnh lưới nhỏ nhất | 
| 3 | 4 | hành vi chẵn lẻ có kích thước lẻ | 
| 10^9 | 5e17 | ổn định đầu vào lớn | 
| 4 | 8 | độ chính xác của lưới đồng đều | 

## Vỏ cạnh 

cho$n = 1$, lưới có một ô màu đen. Công thức cho$1 // 2 = 0$, phù hợp với thực tế là không có tế bào trắng nào tồn tại. Đối số chẵn lẻ đúng ngay cả trong trường hợp suy biến này. 

Đối với số lẻ$n$, chẳng hạn như$n = 5$, lưới chứa$25$tế bào. Công thức cho$12$, trong khi câu “một nửa là 12,5” ngây thơ sẽ không chính xác. Nửa ô bị thiếu được giải quyết bằng cách xếp sàn, tương ứng với sự phân bố chính xác của các lớp chẵn lẻ trong một hình vuông có kích thước lẻ. 

Đối với lớn$n$, chẳng hạn như$10^9$, không có sự thay đổi cấu trúc xảy ra. Việc tính toán vẫn là phép nhân và phép chia duy nhất, xác nhận rằng cấu trúc tổ hợp thay thế hoàn toàn mọi lý luận hình học.
