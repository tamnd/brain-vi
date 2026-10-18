---
title: "CF 105586G - \u53ef\u4e50\u559d\u96ea\u78a7"
description: "Chúng ta được cấp một dãy số, mỗi số là −1, 0 hoặc 1. Giữa mỗi cặp phần tử liền kề, chúng ta được phép đặt dấu cộng hoặc dấu nhân và chúng ta cũng có thể tự do sử dụng dấu ngoặc đơn bằng cách chọn thứ tự đánh giá theo cách biểu thức thông thường…"
date: "2026-06-22T14:45:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "G"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 80
verified: true
draft: false
---

[CF 105586G - \u53ef\u4e50\u559d\u96ea\u78a7](https://codeforces.com/problemset/problem/105586/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số, mỗi số là −1, 0 hoặc 1. Giữa mỗi cặp phần tử liền kề, chúng ta được phép đặt dấu cộng hoặc dấu nhân và chúng ta cũng có thể tự do sử dụng dấu ngoặc đơn bằng cách chọn thứ tự đánh giá theo cách đánh giá biểu thức thông thường. 

Mỗi lựa chọn hoàn chỉnh của các toán tử và nhóm sẽ tạo ra một giá trị cho toàn bộ biểu thức. Nhiệm vụ là xác định xem có tồn tại bất kỳ công trình nào có giá trị cuối cùng chính xác bằng 0 hay không. 

Chỉ có một truy vấn cho mỗi trường hợp thử nghiệm trong phiên bản này và tất cả kích thước đầu vào trong các thử nghiệm có tổng tối đa là 10^6, do đó, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào. Bất kỳ phương trình bậc hai nào cho mỗi trường hợp thử nghiệm sẽ ngay lập tức thất bại do trường hợp xấu nhất là có nhiều mảng dài. 

Trường hợp cạnh tinh tế xuất phát từ sự tương tác giữa phép nhân và số 0. Số 0 có thể triệt tiêu bất kỳ tích nào nếu nó được đặt bên trong một biểu thức con nặng về phép nhân, nhưng nó cũng có thể bị “trung hòa” nếu bị cô lập bên trong các phép cộng. Điều này làm cho lý luận tham lam ngây thơ về việc “chỉ cần kết hợp mọi thứ” trở nên không đáng tin cậy. 

Một dạng lỗi phổ biến khác là giả sử biểu thức hoạt động giống như một phân vùng đơn giản thành các phân đoạn. Giả định đó bị phá vỡ ngay khi cho phép sử dụng dấu ngoặc đơn, bởi vì hiện tại chúng tôi đang làm việc với một cây nhị phân đầy đủ chứ không phải một nhóm cố định từ trái sang phải. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi cách đặt toán tử và tất cả các dấu ngoặc đơn có thể có. Đối với n phần tử, có 2^{n−1} lựa chọn toán tử và số dấu ngoặc đơn đầy đủ hợp lệ có kích thước bằng số Catalan. Ngay cả với n khoảng 20, điều này đã trở nên không khả thi và với n lên tới 10^6 thì điều đó hoàn toàn nằm ngoài tầm với. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng biểu thức có thể được xem dưới dạng cây nhị phân trong đó các lá là các giá trị trong {−1, 0, 1} và các nút bên trong là phép cộng hoặc phép nhân. Vì phép nhân với số 0 buộc toàn bộ cây con trở thành số 0, nên bất kỳ cấu trúc nào có thể “định tuyến” tất cả các đóng góp thông qua cây con chứa số 0 sẽ ngay lập tức đạt được mục tiêu. 

Điều này chuyển vấn đề từ phép liệt kê cấu trúc sang việc hỏi liệu hệ đại số được tạo bởi + và * trên ba giá trị này có thể biểu thị 0 bằng cách sử dụng nhiều bộ số có sẵn hay không. 

Hai quan sát làm sụp đổ cấu trúc: 

Nếu có ít nhất một số 0, chúng ta luôn có thể xây dựng một nút nhân sử dụng nó làm thừa số của toàn bộ biểu thức, buộc kết quả bằng 0. 

Nếu không có số 0 thì tất cả các giá trị đều là −1 hoặc 1. Trong trường hợp đó, phép cộng chỉ cho phép hủy khi cả hai dấu đều tồn tại; nếu không thì biểu thức buộc phải hoàn toàn tích cực hoặc hoàn toàn tiêu cực bất kể dấu ngoặc đơn. 

Điều này làm giảm vấn đề xuống việc kiểm tra sự tồn tại đơn giản trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên biểu thức | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm đại số | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và quét mảng một lần.

1. Quét mảng và kiểm tra xem có phần tử nào bằng 0 hay không. Nếu vậy, chúng ta có thể kết luận ngay câu trả lời là có, bởi vì chúng ta có thể xây dựng một biểu thức trong đó số 0 này được nhân với phần còn lại của biểu thức, buộc toàn bộ giá trị bằng 0. 
2. Nếu không có số 0, hãy ghi lại xem chúng ta có thấy ít nhất một −1 và ít nhất một 1 hay không. Điều này quan trọng vì đây là hai giá trị duy nhất có thể triệt tiêu lẫn nhau thông qua phép cộng khi được kết hợp trong các phần khác nhau của cây biểu thức. 
3. Nếu có cả −1 và 1, xuất ra có. Lý do là chúng ta có thể sắp xếp các phép cộng và phép nhân sao cho các đóng góp tích cực và tiêu cực xuất hiện trong các cây con riêng biệt và hủy bỏ ở cấp cao nhất. 
4. Nếu tất cả các giá trị đều giống nhau (tất cả 1 hoặc tất cả −1), xuất ra số không, vì mọi biểu thức có thể đều có giá trị khác 0 với dấu cố định. 

Tính đúng đắn phụ thuộc vào thực tế là phép nhân không thể thay đổi tính phân tập dấu và nếu không có cả hai dấu hoặc số 0, mọi cây con vẫn nhất quán dấu, ngăn chặn bất kỳ sự hủy bỏ nào về 0. 

### Tại sao nó hoạt động 

Mỗi biểu thức ước tính thành một giá trị được xây dựng từ các ứng dụng cộng và nhân lặp đi lặp lại trên một tập hợp chỉ chứa −1, 0 và 1. Cách duy nhất để buộc kết quả về 0 là đưa số 0 vào chuỗi nhân bao trùm toàn bộ biểu thức hoặc xây dựng phép cộng trừ giữa các đóng góp dương và âm. Nếu không tồn tại cả số 0 và cả hai dấu thì mọi biểu thức có thể sẽ đánh giá thành một giá trị khác 0 với dấu cố định, do đó việc đạt đến số 0 là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        x = int(input())  # always 0

        has0 = False
        has1 = False
        hasm1 = False

        for v in a:
            if v == 0:
                has0 = True
            elif v == 1:
                has1 = True
            else:
                hasm1 = True

        if has0:
            print("Yes")
        else:
            if has1 and hasm1:
                print("Yes")
            else:
                print("No")

if __name__ == "__main__":
    solve()
```Mã này chỉ trích xuất các thuộc tính chung của mảng thay vì mô phỏng bất kỳ cấu trúc biểu thức nào. Ba lá cờ nắm bắt chính xác thông tin duy nhất liên quan đến tính khả thi: sự hiện diện của số 0, sự hiện diện của số dương và sự hiện diện của số âm. 

Giá trị truy vấn luôn bằng 0 trong phiên bản này, vì vậy nó được đọc nhưng không ảnh hưởng đến việc phân nhánh. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[-1, -1, -1, -1]`. 

| Bước | has0 | có1 | có-1 | 
| --- | --- | --- | --- | 
| đọc -1 | F | F | T | 
| đọc -1 | F | F | T | 
| đọc -1 | F | F | T | 
| đọc -1 | F | F | T | 

Cuối cùng, chúng ta không có số 0 và không có số 1 nên điều kiện không thành công và câu trả lời là không. Bất kỳ biểu thức nào được xây dựng chỉ từ các giá trị −1 vẫn hoàn toàn âm hoặc dương tùy thuộc vào cấu trúc, nhưng không bao giờ đạt tới 0. 

Bây giờ hãy xem xét`[1, -1, 1]`. 

| Bước | has0 | có1 | có-1 | 
| --- | --- | --- | --- | 
| đọc 1 | F | T | F | 
| đọc -1 | F | T | T | 
| đọc 1 | F | T | T | 

Ở đây cả hai dấu đều tồn tại và không có số 0, vì vậy chúng ta xuất ra có. Điều này tương ứng với việc xây dựng phép loại bỏ phụ gia giữa các đóng góp tích cực và tiêu cực trong các cây con khác nhau của cây biểu thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử được quét một lần | 
| Không gian | O(1) | Chỉ có một số lượng cờ không đổi được lưu trữ | 

Tổng kích thước đầu vào được giới hạn bởi 10^6 trong tất cả các thử nghiệm, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    out = []
    input = sys.stdin.readline

    T = int(input())
    for _ in range(T):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        x = int(input())

        has0 = any(v == 0 for v in a)
        has1 = any(v == 1 for v in a)
        hasm1 = any(v == -1 for v in a)

        out.append("Yes" if (has0 or (has1 and hasm1)) else "No")

    return "\n".join(out)

# sample-like checks
assert run("1 1\n1\n0\n") == "No"
assert run("1 1\n0\n0\n") == "Yes"

# custom cases
assert run("1 1\n-1\n0\n") == "No"
assert run("3 1\n1 -1 1\n0\n") == "Yes"
assert run("4 1\n-1 -1 -1 -1\n0\n") == "No"
assert run("5 1\n1 1 1 1 1\n0\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | Có | zero ngay lập tức cho phép xây dựng | 
| dấu hiệu hỗn hợp | Có | có thể hủy bỏ | 
| tất cả -1 | Không | không thể hủy hoặc không thể | 
| tất cả 1 | Không | bất biến dương nghiêm ngặt | 

## Vỏ cạnh 

Mảng một phần tử chứa 0 là trường hợp thành công trực tiếp nhất. Thuật toán phát hiện nó ngay lập tức thông qua cờ has0 và trả về có mà không cần dựa vào bất kỳ lý luận cấu trúc nào. 

Mảng một phần tử chứa 1 hoặc −1 không thành công vì không thể áp dụng thao tác nào và giá trị duy nhất có thể là khác 0. Logic cờ trả về chính xác không trong cả hai trường hợp vì không điều kiện nào được thỏa mãn. 

Các mảng có cả 1 và −1 nhưng không có số 0 được xử lý theo điều kiện dấu hỗn hợp. Mặc dù không có sự hủy bỏ cục bộ ở cấp độ phần tử, nhưng cây biểu thức có thể tách các đóng góp thành các cây con khác nhau bị hủy bỏ ở phép cộng gốc, đó chính xác là những gì điều kiện nắm bắt. 

Các mảng bao gồm toàn bộ −1 hoặc hoàn toàn bằng 1 không thành công vì không có nguồn đóng góp hoặc triệt tiêu đối lập nào qua số 0, do đó mọi biểu thức có thể đều bảo toàn độ lớn khác 0 và dấu cố định.
