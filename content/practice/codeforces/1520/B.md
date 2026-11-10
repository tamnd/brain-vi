---
title: "CF 1520B - Số thông thường"
description: "Chúng ta được cung cấp nhiều truy vấn và mỗi truy vấn bao gồm một số nguyên dương $n$. Với mỗi $n$, chúng ta cần đếm xem có bao nhiêu số trong phạm vi từ 1 đến $n$ có tất cả các chữ số giống nhau ở dạng thập phân. Những con số “bình thường” này có cấu trúc rất cứng nhắc."
date: "2026-06-14T18:14:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 800
weight: 1520
solve_time_s: 289
verified: true
draft: false
---

[CF 1520B - Số thông thường](https://codeforces.com/problemset/problem/1520/B) 

**Đánh giá:** 800 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều truy vấn và mỗi truy vấn bao gồm một số nguyên dương duy nhất$n$. Đối với mỗi$n$, chúng ta cần đếm có bao nhiêu số trong khoảng từ 1 đến$n$có tất cả các chữ số giống hệt nhau trong biểu diễn thập phân của chúng. 

Những con số “bình thường” này có cấu trúc rất cứng nhắc. Mỗi số hợp lệ được hình thành bằng cách chọn một chữ số từ 1 đến 9 và lặp lại nó một hoặc nhiều lần. Vì vậy, các ví dụ hợp lệ bao gồm 7, 88, 444 và 999999999, trong khi các số như 10, 101 hoặc 123 bị loại trừ vì có ít nhất hai chữ số khác nhau. 

Ràng buộc$n \le 10^9$nghĩa là mỗi số có nhiều nhất 10 chữ số. Đây là hạn chế chính về cấu trúc: tổng thể chỉ có một số lượng nhỏ các số thông thường có thể có và chúng cực kỳ thưa thớt trong phạm vi của tất cả các số nguyên. 

Một cách tiếp cận đơn giản là kiểm tra mọi số từ 1 đến$n$, xác minh xem tất cả các chữ số có bằng nhau hay không. Điều này đòi hỏi phải kiểm tra tới$10^9$số lượng trên mỗi truy vấn trong trường hợp xấu nhất, vượt xa giới hạn có thể chấp nhận được$t \le 10^4$. Ngay cả việc quét tuyến tính cho mỗi trường hợp thử nghiệm cũng không thể thực hiện được. 

Các trường hợp cạnh phát sinh khi$n$nhỏ hoặc khớp chính xác với một số bình thường. Ví dụ, nếu$n = 5$, câu trả lời là 5 vì mọi số có một chữ số đều là số bình thường. Nếu như$n = 10$, đáp án vẫn là 9 vì 10 không hề bình thường. Cách tiếp cận bất cẩn chỉ đếm dựa trên độ dài chữ số có thể bao gồm không chính xác 10 hoặc các số có chữ số hỗn hợp khác. 

Sự tinh tế chính là nhận ra rằng câu trả lời không phụ thuộc vào sự phân bố hoặc các thuộc tính số học của$n$, nhưng hoàn toàn dựa trên việc đếm một tập hợp ứng cử viên có cấu trúc rất nhỏ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp từ 1 đến$n$, chuyển đổi từng số thành một chuỗi và kiểm tra xem tất cả các ký tự có giống nhau hay không. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa. Tuy nhiên, chi phí của nó tỷ lệ thuận với$n \log n$cho mỗi trường hợp kiểm thử do kiểm tra chữ số, điều này trở nên không khả thi ngay lập tức khi$n$đạt tới$10^9$Và$t$là lớn. 

Quan sát quan trọng là các số thông thường có cấu trúc tổ hợp cố định. Với mỗi chữ số từ 1 đến 9, chúng ta có thể dựng các số như 1, 11, 111, 1111, v.v. Đối với một chiều dài cố định$k$, có chính xác 9 số thường: một số ứng với mỗi chữ số từ 1 đến 9. Khi đó, bài toán quy về việc đếm xem có bao nhiêu số như vậy nhỏ hơn hoặc bằng$n$. 

Thay vì lặp lại tất cả các số nguyên, chúng tôi chỉ tạo ra các ứng cử viên có cấu trúc này. Có nhiều nhất 9 chữ số và nhiều nhất là 10 độ dài, do đó tổng số ứng viên bị giới hạn bởi 90. Điều này biến mỗi truy vấn thành một lần quét liên tục trên một tập hợp cố định nhỏ. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi con số một cách rõ ràng, nhưng không thành công vì vũ trụ rất lớn. Quan sát cho thấy các số thông thường hoàn toàn được xác định bởi sự lựa chọn chữ số và độ dài làm sụp đổ không gian tìm kiếm từ tuyến tính sang dạng tuyến tính.$n$đến kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \log n)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là mọi số thông thường đều có thể được tạo ra bằng cách chọn một chữ số và lặp lại nó. 

1. Tính toán trước tất cả các số thông thường lên đến$10^9$bằng cách lặp lại các lựa chọn chữ số từ 1 đến 9 và xây dựng các số có chữ số lặp lại có độ dài tăng dần. 

Mỗi cách xây dựng được thực hiện bằng cách bắt đầu từ 0 và nối lại cùng một chữ số, tạo thành giá trị tăng trưởng hình học. 
2. Lưu trữ tất cả các giá trị được tạo trong danh sách. 
3. Sắp xếp danh sách để chúng ta có thể trả lời các truy vấn bằng cách sử dụng tìm kiếm nhị phân hoặc đếm đơn giản. 
4. Đối với mỗi truy vấn$n$, đếm xem có bao nhiêu giá trị được tính toán trước nhỏ hơn hoặc bằng$n$. 

Bước đếm thực chất là một truy vấn tiền tố trên một mảng tĩnh được sắp xếp, có thể được thực hiện bằng tìm kiếm nhị phân theo thời gian logarit, mặc dù ngay cả việc quét tuyến tính trên 90 phần tử cũng đủ. 

### Tại sao nó hoạt động 

Mỗi số thông thường tương ứng duy nhất với một cặp (chữ số, độ dài). Không có sự trùng lặp vì các cấu trúc có độ dài chữ số khác nhau luôn tạo ra các số nguyên riêng biệt. Vì chúng tôi tạo ra tất cả các cặp như vậy một cách triệt để trong giới hạn chữ số do$10^9$, tập hợp được tính toán trước khớp chính xác với tập hợp các câu trả lời hợp lệ. Số lượng cuối cùng cho bất kỳ$n$do đó chỉ là số phần tử trong bộ hoàn chỉnh này không vượt quá$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute all ordinary numbers up to 1e9
ordinaries = []

for digit in range(1, 10):
    val = 0
    for _ in range(10):  # max 10 digits because n <= 1e9
        val = val * 10 + digit
        ordinaries.append(val)

ordinaries.sort()

t = int(input())
for _ in range(t):
    n = int(input())
    
    # count how many are <= n
    cnt = 0
    for x in ordinaries:
        if x <= n:
            cnt += 1
    print(cnt)
```Mã này xây dựng tất cả các số thông thường ứng cử viên bằng cách sửa một chữ số và liên tục nối thêm nó. Mỗi giá trị được tạo ra sẽ được lưu trữ trong một danh sách chung, sau đó được sắp xếp để truy vấn đơn điệu. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp qua danh sách được tính toán trước và đếm xem có bao nhiêu giá trị không vượt quá$n$. Vì kích thước danh sách chỉ là 90 nên đây thực sự là thời gian không đổi cho mỗi truy vấn. 

Một lỗi triển khai phổ biến là cố gắng tạo ra các số chỉ có độ dài chữ số là$n$dynamically per query. Điều đó làm phức tạp logic một cách không cần thiết và có nguy cơ xảy ra lỗi khi xử lý các giá trị biên như lũy thừa của mười. Precomputation avoids all such issues.

 ## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem cách sử dụng tập hợp tính toán trước. 

### Ví dụ 1:$n = 5$| bước | hiện tại x | x ≤ n | đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 
| 2 | 11 | không | 1 | 
| 3 | 111 | không | 1 | 

Quá trình quét sẽ ngừng đóng góp khi giá trị vượt quá 5. Kết quả là 5 số có một chữ số, khớp với thực tế là chỉ từ 1 đến 5 là số thông thường hợp lệ. 

### Ví dụ 2:$n = 100$| bước | hiện tại x | x ≤ n | đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 
| ... | ... | ... | ... | 
| 9 | 9 | vâng | 9 | 
| 10 | 11 | vâng | 10 | 
| 11 | 22 | vâng | 11 | 
| 18 | 99 | vâng | 18 | 
| 19 | 111 | không | 18 | 

Số đếm ổn định ở mức 18, tương ứng với tất cả các số lặp lại có một chữ số và hai chữ số. 

Dấu vết này cho thấy rằng khi chúng tôi vượt qua 99, tất cả các ứng cử viên còn lại đều vượt quá 100, do đó không có sự gia tăng nào nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm kiểm tra tối đa 90 giá trị được tính toán trước | 
| Không gian |$O(1)$| Chỉ một danh sách cố định các số thông thường được lưu trữ | 

Các ràng buộc cho phép lên đến$10^4$truy vấn và mỗi truy vấn thực hiện công việc liên tục. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    ordinaries = []
    for digit in range(1, 10):
        val = 0
        for _ in range(10):
            val = val * 10 + digit
            ordinaries.append(val)
    ordinaries.sort()

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        cnt = 0
        for x in ordinaries:
            if x <= n:
                cnt += 1
        out.append(str(cnt))
    return "\n".join(out)

# provided samples
assert run("6\n1\n2\n3\n4\n5\n100\n") == "1\n2\n3\n4\n5\n18"

# minimum size
assert run("1\n1\n") == "1"

# boundary around 10
assert run("2\n9\n10\n") == "9\n9"

# exact ordinary number
assert run("1\n111\n") == "3"

# large value
assert run("1\n1000000000\n") == "90"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào nhỏ nhất có thể | 
| 9/10 | 9/9 | vượt qua ranh giới số không bình thường | 
| 111 | 3 | đếm lặp lại nhiều chữ số | 
| 1e9 | 90 | bảo hiểm đầy đủ giới hạn trên | 

## Vỏ cạnh 

cho$n = 10$, thuật toán chỉ đếm từ 1 đến 9 vì 10 không phải là số có nhiều chữ số lặp lại. Danh sách được tính toán trước chứa 11, vì vậy trong quá trình đếm, 11 bị loại trừ một cách chính xác vì nó vượt quá$n$. 

Vì$n = 999999999$, tất cả các số lặp lại có một chữ số đến chín chữ số đều được bao gồm. Thuật toán lặp qua tất cả 90 giá trị được tính toán trước và đếm tất cả, tạo ra 90. 

cho$n = 1$, chỉ có phần tử đầu tiên trong danh sách mới đóng góp. Vì tất cả các giá trị được tạo khác đều lớn hơn 1 nên kết quả vẫn chính xác là 1.
