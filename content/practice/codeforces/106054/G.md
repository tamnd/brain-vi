---
title: "CF 106054G - Đi ki-ốt"
description: "Chúng tôi đang mô hình hóa một hệ thống thanh toán rất nhỏ. Một khách hàng muốn mua một mặt hàng có giá $A$ peso. Anh ta thanh toán bằng một tờ tiền trị giá $B$ peso, vì vậy ki-ốt phải trả lại chính xác $B - A$ peso tiền lẻ."
date: "2026-06-21T07:43:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106054
codeforces_index: "G"
codeforces_contest_name: "2025 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 106054
solve_time_s: 34
verified: true
draft: false
---

[CF 106054G - Đi đến ki-ốt](https://codeforces.com/problemset/problem/106054/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô hình hóa một hệ thống thanh toán rất nhỏ. Một khách hàng muốn mua một món hàng có giá$A$peso. Anh ta thanh toán bằng một tờ tiền có giá trị$B$peso nên ki-ốt phải trả lại chính xác$B - A$peso thay đổi. Điều khó khăn là ki-ốt không trả lại tiền xu hoặc hóa đơn mà chỉ trả lại kẹo và mỗi viên kẹo có một giá trị cố định$C$peso. 

Câu hỏi đặt ra là liệu có thể biểu diễn chính xác số tiền thay đổi chỉ bằng cách sử dụng toàn bộ số kẹo có giá trị hay không?$C$. Nói cách khác, chúng ta cần xác định xem sự khác biệt$B - A$có thể được viết là$k \cdot C$đối với một số nguyên$k \ge 0$. 

Các ràng buộc cực kỳ nhỏ, với tất cả các giá trị lên tới 1000. Điều này ngay lập tức loại bỏ mọi lo ngại về hiệu suất hoặc tràn, vì ngay cả việc kiểm tra số học trực tiếp cũng là thời gian không đổi. 

Một vấn đề tế nhị là hiểu được yêu cầu “thay đổi chính xác”. Kiosk đến gần thôi là chưa đủ. Bất kỳ phần còn lại hoặc thiếu hụt nào đều làm mất hiệu lực của giải pháp. Ví dụ, nếu$A = 6$,$B = 10$, Và$C = 3$, thay đổi là 4. Hai viên kẹo cho 6, quá nhiều và một viên kẹo cho 3, quá ít. Đây là một ví dụ trực tiếp trong đó khả năng chia hết không thành công mặc dù giá trị nhỏ. 

Các trường hợp biên chủ yếu liên quan đến logic chia hết hơn là tính toán. Từ$A < B$, sự thay đổi luôn dương, vì vậy chúng ta không bao giờ cần xử lý thay đổi bằng 0 hoặc âm. Điều đó loại bỏ sự mơ hồ tiềm tàng duy nhất trong việc giải thích. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là thử tất cả số kẹo có thể có. Nếu chúng ta chọn$k$kẹo, chúng tôi trả lại$k \cdot C$peso. Chúng ta có thể lặp lại$k$từ 0 đến$\lfloor (B-A)/C \rfloor$và kiểm tra xem bất kỳ lựa chọn nào có khớp chính xác với thay đổi được yêu cầu hay không. 

Điều này đúng vì các giao dịch hợp lệ duy nhất là bội số nguyên của$C$. Tuy nhiên, mặc dù phạm vi ở đây đủ nhỏ nhưng cách tiếp cận này không hiệu quả về mặt cấu trúc và không cần thiết. Trong một biến thể lớn hơn của vấn đề, việc lặp lại tất cả$k$sẽ trở nên tuyến tính theo kích thước của lượng thay đổi. 

Quan sát quan trọng là chúng ta không tìm kiếm bất kỳ cấu trúc đặc biệt nào ngoài khả năng chia hết. Chúng ta chỉ đơn giản kiểm tra xem sự khác biệt$B - A$chia hết cho$C$. Nếu đúng như vậy thì chúng ta có thể tạo ra sự thay đổi chính xác bằng cách sử dụng kẹo; nếu không thì không thể được. 

Điều này làm giảm toàn bộ vấn đề thành một phép toán modulo duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(B-A)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số tiền thay đổi$D = B - A$. Giá trị này thể hiện giá trị chính xác mà ki-ốt phải trả về. 
2. Kiểm tra xem$D$chia hết cho$C$, nghĩa là liệu$D \bmod C = 0$. Nếu điều này được giữ, sự thay đổi có thể được hình thành chính xác bằng cách sử dụng kẹo. 
3. Nếu điều kiện chia hết được thỏa mãn thì xuất ra “S”, nếu không thì xuất ra “N”. 

Lý do việc kiểm tra này là đủ là vì mỗi viên kẹo đóng góp chính xác$C$peso và không có hạn chế nào về số lượng kẹo ngoài tính không âm. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ đều phải thể hiện sự thay đổi dưới dạng tổng của các đơn vị kích thước giống hệt nhau$C$. Điều đó có nghĩa là sự thay đổi phải nằm trong tập hợp$\{0, C, 2C, 3C, \dots\}$. Tập hợp này chính xác là tập hợp bội số của$C$. Tư cách thành viên trong tập hợp này tương đương với việc có số dư bằng 0 khi chia cho$C$. Từ$D = B - A$là cố định và mang tính quyết định, việc kiểm tra khả năng chia hết sẽ mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A, B, C = map(int, input().split())

change = B - A

if change % C == 0:
    print("S")
else:
    print("N")
```Giải pháp đọc ba số nguyên, tính toán thay đổi cần thiết và áp dụng kiểm tra modulo. Chi tiết triển khai chính là tính toán sự khác biệt trước tiên và chỉ sau đó áp dụng modulo, giúp giữ logic rõ ràng và tránh suy luận về nhiều biểu thức. 

Không cần có vòng lặp hoặc nhánh trường hợp cạnh vì cấu trúc của các câu trả lời hợp lệ hoàn toàn là số học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 20 5
```Thay đổi là 10. 

| Bước | Thay đổi (B-A) | C | thay đổi %C | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 5 | - | - | 
| Kiểm tra | 10 | 5 | 0 | S | 

Vì 10 chính xác là 2 nhân 5 nên ki-ốt có thể trả lại hai chiếc kẹo. 

Điều này xác nhận tính bất biến rằng biểu diễn chính xác tồn tại bất cứ khi nào tính chia hết được giữ nguyên. 

### Ví dụ 2 

đầu vào:```
6 10 3
```Thay đổi là 4. 

| Bước | Thay đổi (B-A) | C | thay đổi %C | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 3 | - | - | 
| Kiểm tra | 4 | 3 | 1 | N | 

Ở đây, không có số nguyên viên kẹo nào có tổng bằng 4. Một viên kẹo quá nhỏ, hai viên kẹo vượt quá số lượng yêu cầu. Điều này trực tiếp cho thấy tại sao sự phân chia là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một vài phép tính số học và một lần kiểm tra modulo | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép mọi giải pháp số học theo thời gian không đổi một cách thoải mái trong giới hạn. Ngay cả một vòng lặp brute-force cũng sẽ đủ nhanh ở mức$A, B, C \le 1000$, nhưng kiểm tra modulo là đặc tính toán học trực tiếp của điều kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    A, B, C = map(int, input().split())
    change = B - A
    return "S" if change % C == 0 else "N"

# provided samples
assert run("10 20 5") == "S"
assert run("6 10 3") == "N"

# custom cases
assert run("1 2 1") == "S"   # exact 1 candy
assert run("1 3 2") == "S"   # 2 change with 1 candy
assert run("1 4 2") == "S"   # multiple candies exact fit
assert run("1 5 2") == "N"   # remainder case
assert run("999 1000 7") == "N"  # small edge non-divisible
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 | S | khả năng chia hết hợp lệ tối thiểu | 
| 1 3 2 | S | trường hợp kẹo duy nhất chính xác | 
| 1 5 2 | N | phần dư không chia hết | 
| 999 1000 7 | N | ranh giới gần giá trị tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi thay đổi chính xác bằng một giá trị kẹo. Đối với đầu vào`A = 1, B = 4, C = 3`, thay đổi là 3. Thuật toán tính toán$3 \bmod 3 = 0$, do đó nó xuất ra “S” một cách chính xác. Điều này tương ứng với việc đưa ra chính xác một viên kẹo. 

Một trường hợp khác là khi tiền lẻ nhỏ hơn một viên kẹo. Đối với đầu vào`A = 6, B = 10, C = 7`, thay đổi là 4. Vì 4 không chia hết cho 7 nên việc kiểm tra modulo không thành công và đầu ra là “N”. Điều này cho thấy không thể biểu diễn một lượng nhỏ hơn bằng cách sử dụng các đơn vị cố định lớn hơn. 

Trường hợp cạnh cuối cùng là khi thay đổi bằng 0, điều này thực sự không thể xảy ra dưới ràng buộc$A < B$. Nếu được phép, thuật toán vẫn hoạt động chính xác vì số 0 chia hết cho bất kỳ số dương nào.$C$, tạo ra chữ “S”, phù hợp với thực tế là không cho kẹo là hợp lệ.
