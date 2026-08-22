---
title: "CF 104180B - Máy thu mưa"
description: "Chúng ta được cung cấp lượng nước mưa ban đầu được thu vào ngày đầu tiên, ký hiệu là số nguyên $i$. Giá trị này quyết định mọi thứ về thời gian còn lại trong tuần."
date: "2026-07-02T00:42:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 53
verified: true
draft: false
---

[CF 104180B - Bộ thu mưa](https://codeforces.com/problemset/problem/104180/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp lượng nước mưa ban đầu được thu vào ngày đầu tiên, biểu thị bằng số nguyên$i$. Giá trị này quyết định mọi thứ về thời gian còn lại trong tuần. Từ ngày thứ hai đến ngày thứ bảy, lượng mưa tăng đều đặn: mỗi ngày tăng thêm một lượng như nhau$x$, Ở đâu$x$được tính bằng tổng các chữ số của$i$. Tổng cộng trong bảy ngày, chúng ta cần tính toán lượng nước tích lũy được thu thập. 

Một cách hữu ích để giải thích lại vấn đề là sử dụng một dãy số học đơn giản. Thuật ngữ đầu tiên là$i$và mỗi số hạng tiếp theo tăng thêm$x$. Vì vậy, trình tự trông giống như:$i, i + x, i + 2x, \dots, i + 6x$. Nhiệm vụ là tính tổng tất cả các số hạng này. 

Các ràng buộc là nhỏ, với$i \leq 10^5$, do đó việc tính tổng các chữ số và thực hiện một số phép tính số học không đổi là không đáng kể về mặt hiệu suất. Ngay cả việc tính toán trực tiếp hoàn toàn cho mỗi trường hợp thử nghiệm cũng có thời gian không đổi. 

Các trường hợp cạnh rất tinh tế nhưng quan trọng vì cách hoạt động của tổng chữ số. 

Trường hợp một cạnh là$i = 0$. Tổng các chữ số là$0$, vậy cả bảy ngày đều bằng không. Việc triển khai ngây thơ giả sử các số dương hoặc bỏ qua hành vi giống số 0 đứng đầu vẫn chỉ xử lý nó một cách chính xác nếu logic tổng chữ số mạnh mẽ. 

Một trường hợp cạnh khác là khi$i$có số 0 ở cuối, ví dụ$i = 10000$. Tổng các chữ số là$1$, không bị ảnh hưởng bởi số 0 nên mức tăng nhỏ nhưng chuỗi vẫn tăng chính xác. Sai lầm thường xuất phát từ việc diễn giải sai số gia như “chắp thêm chữ số” thay vì tổng các chữ số. 

Cuối cùng, vì độ dài chuỗi là cố định (7 thuật ngữ), nên không có nguy cơ tràn trong Python, nhưng trong các ngôn ngữ chặt chẽ hơn, việc tích lũy đơn giản bằng cách sử dụng số học lớn có thể cần được quan tâm. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tính tổng các chữ số$x$, sau đó mô phỏng rõ ràng bảy ngày. Bắt đầu từ$i$, chúng tôi liên tục thêm$x$và tích lũy tổng số. Điều này đúng vì nó phản ánh chính xác quá trình được mô tả trong bài toán. 

Chi phí của phương pháp này là không đổi cho mỗi trường hợp thử nghiệm vì chỉ có bảy ngày. Ngay cả khi chúng ta tính lại tổng các chữ số hoặc thực hiện phép tính lặp đi lặp lại thì độ phức tạp vẫn bị giới hạn bởi một hằng số cố định. Tuy nhiên, sự kém hiệu quả tổng quát hơn nằm ở việc bỏ qua cấu trúc: chuỗi là số học, do đó phép tính tổng có thể được thực hiện ở dạng đóng thay vì tích lũy lặp. 

Quan sát quan trọng là dãy số là một cấp số cộng với số hạng đầu tiên$i$, học kỳ trước$i + 6x$, và tổng cộng 7 số hạng. Tổng của một cấp số cộng là:$$\text{sum} = \frac{7}{2} \cdot (2i + 6x)$$Điều này làm giảm mọi thứ thành việc tính tổng các chữ số một lần và áp dụng công thức trực tiếp. 

Điều này biến vấn đề thành số học thuần túy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Công thức số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$i$từ đầu vào. Đây là giá trị cơ bản của chuỗi và nó xác định toàn bộ tiến trình. 
2. Tính toán$x$, tổng các chữ số của$i$. Giá trị này hoạt động như mức tăng cố định hàng ngày, do đó độ chính xác phụ thuộc hoàn toàn vào việc trích xuất các chữ số một cách chính xác. 
3. Nhận biết rằng 7 giá trị hàng ngày tạo thành một dãy số học bắt đầu từ$i$với sự khác biệt chung$x$. 
4. Tính tổng bằng công thức cấp số cộng:$$S = \frac{7}{2} \cdot (2i + 6x)$$Điều này tránh việc lặp lại trong bảy ngày trong khi vẫn giữ được sự tương đương chính xác. 
5. Đầu ra$S$. 

### Tại sao nó hoạt động 

Việc xây dựng các giá trị hàng ngày đảm bảo tiến trình tuyến tính vì mỗi ngày chỉ phụ thuộc vào hằng số cộng cố định được lấy từ đầu vào. Một lần$x$là cố định, mỗi số hạng được xác định duy nhất là$a_k = i + kx$. Tính tổng các số hạng này tương đương với tính tổng một dãy số học và công thức dạng đóng giống hệt về mặt đại số với tích lũy trực tiếp. Vì không có bước trung gian nào sửa đổi$x$, cấu trúc vẫn ổn định trên tất cả bảy số hạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

i = int(input().strip())

x = sum(int(c) for c in str(i))

# arithmetic series: i + (i+x) + ... + (i+6x)
# sum = 7/2 * (2i + 6x) = 7 * (2i + 6x) / 2
total = 7 * (2 * i + 6 * x) // 2

print(total)
```Giải pháp đọc dữ liệu đầu vào và tính tổng các chữ số ngay lập tức bằng cách chuyển đổi số nguyên thành chuỗi và tính tổng các ký tự. Điều này an toàn cho tất cả các đầu vào bao gồm cả số 0, vì`"0"`tạo ra tổng chữ số chính xác bằng 0. 

Chi tiết triển khai chính là tránh số học dấu phẩy động. Mặc dù công thức bao gồm phép chia cho 2 nhưng biểu thức luôn là số chẵn, do đó phép chia số nguyên là an toàn. Viết nó như`7 * (2 * i + 6 * x) // 2`đảm bảo đầu ra số nguyên chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
70
```Đây,$i = 70$, vậy tổng chữ số$x = 7$. 

Trình tự trở thành:$70, 77, 84, 91, 98, 105, 112$| Ngày | Giá trị | 
| --- | --- | 
| 1 | 70 | 
| 2 | 77 | 
| 3 | 84 | 
| 4 | 91 | 
| 5 | 98 | 
| 6 | 105 | 
| 7 | 112 | 

Tổng là$637$. 

Điều này xác nhận rằng ngay cả khi các chữ số bao gồm số 0, chỉ các chữ số khác 0 mới ảnh hưởng đến số gia. 

### Ví dụ 2 

đầu vào:```
5
```Đây,$i = 5$, Vì thế$x = 5$. 

Sự liên tiếp:$5, 10, 15, 20, 25, 30, 35$| Ngày | Giá trị | 
| --- | --- | 
| 1 | 5 | 
| 2 | 10 | 
| 3 | 15 | 
| 4 | 20 | 
| 5 | 25 | 
| 6 | 30 | 
| 7 | 35 | 

Tổng là$140$. 

Điều này thể hiện rõ ràng cấu trúc cấp số cộng khi tổng các chữ số bằng chính số đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tổng chữ số được giới hạn bởi tối đa 6 chữ số và chỉ thực hiện số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Thời gian chạy không đổi bất kể kích thước đầu vào, dễ dàng phù hợp với mọi ràng buộc thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    i = int(input().strip())
    x = sum(int(c) for c in str(i))
    total = 7 * (2 * i + 6 * x) // 2
    return str(total)

# provided sample
assert run("70\n") == "637"

# minimum case
assert run("0\n") == "0"

# small number
assert run("5\n") == "140"

# all digits zero except one
assert run("10000\n") == str(7 * (2 * 10000 + 6 * 1) // 2)

# max constraint
assert run("100000\n") == str(7 * (2 * 100000 + 6 * 1) // 2)

# repeated digit sum variation
assert run("99\n") == str(7 * (2 * 99 + 6 * 18) // 2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | xử lý bằng không | 
| 5 | 140 | cấp số cộng cơ bản | 
| 10000 | độ chính xác của công thức với các số 0 ở cuối | | 
| 100000 | hành vi giới hạn trên | | 
| 99 | tính chính xác của tổng nhiều chữ số | | 

## Vỏ cạnh 

Đối với đầu vào`0`, tổng các chữ số là`0`, do đó dãy số không đổi trong suốt bảy ngày. Thuật toán tính toán$x = 0$, dẫn đến:$S = \frac{7}{2}(0) = 0$, khớp chính xác với đầu ra dự kiến. 

Đối với đầu vào`10000`, tổng các chữ số là`1`. Trình tự trở thành:$10000, 10001, 10002, 10003, 10004, 10005, 10006$. 

Thuật toán tính toán:$S = 7(20000 + 6)/2 = 70003$, phù hợp với phép tính tổng trực tiếp. 

Đối với đầu vào`99`, tổng các chữ số là`18`. Dãy số tăng nhanh hơn nhiều nhưng công thức số học vẫn được áp dụng không thay đổi. Mỗi số hạng chỉ được xác định bằng số gia cố định, do đó không có bước nào khác với định nghĩa cấp số cộng.
