---
title: "CF 1915A - Lẻ Một Ra"
description: "Chúng tôi được giao một loạt nhiệm vụ độc lập rất nhỏ. Mỗi nhiệm vụ bao gồm chính xác ba chữ số và chúng ta được hứa hẹn một cấu trúc cụ thể: trong số ba giá trị, hai giá trị giống hệt nhau và một giá trị khác nhau."
date: "2026-06-08T19:55:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1915
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 918 (Div. 4)"
rating: 800
weight: 1915
solve_time_s: 81
verified: true
draft: false
---

[CF 1915A - Odd One Out](https://codeforces.com/problemset/problem/1915/A) 

**Đánh giá:** 800 
**Thẻ:** bitmask, triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một loạt nhiệm vụ độc lập rất nhỏ. Mỗi nhiệm vụ bao gồm chính xác ba chữ số và chúng ta được hứa hẹn một cấu trúc cụ thể: trong số ba giá trị, hai giá trị giống hệt nhau và một giá trị khác nhau. Công việc của chúng ta là xác định số ngoại lệ, chữ số chỉ xuất hiện một lần trong bộ ba đó. 

Mặc dù kích thước đầu vào cho mỗi trường hợp thử nghiệm là rất nhỏ, nhưng vấn đề này được thiết kế để kiểm tra lý luận rõ ràng về các mẫu đẳng thức hơn là bất kỳ tính toán nặng nề nào. Mỗi trường hợp thử nghiệm đều độc lập nên không có trạng thái chia sẻ hoặc cấu trúc tích lũy để khai thác. 

Các ràng buộc đủ nhỏ để bất kỳ thời gian không đổi cho mỗi giải pháp thử nghiệm là đủ. Với tối đa 270 trường hợp thử nghiệm và chỉ có ba số nguyên cho mỗi trường hợp, ngay cả một phương pháp thực hiện nhiều so sánh hoặc sử dụng các cấu trúc đếm đơn giản cũng nhanh chóng trong giới hạn thời gian. Áp lực hạn chế thực tế là không tồn tại; thay vào đó, tính đúng đắn và cách xử lý mẫu đẳng thức mới là vấn đề. 

Sự tinh tế duy nhất đến từ việc giả định cấu trúc luôn hợp lệ. Việc triển khai bất cẩn có thể cố gắng khái quát hóa hoặc sắp xếp mà không xem xét tính ổn định của logic, nhưng vì chính xác một giá trị được đảm bảo là duy nhất nên chúng ta có thể dựa trực tiếp vào bất biến đó. 

Các trường hợp cạnh hầu hết là các dạng thoái hóa của cùng một mẫu. Ví dụ: các đầu vào như`5 5 6`,`6 5 5`, hoặc`5 6 5`tất cả đều yêu cầu cùng một đầu ra, nhưng lý luận vị trí ngây thơ có thể thất bại nếu giả sử giá trị duy nhất luôn ở một vị trí cố định. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ là đếm số lần xuất hiện của mỗi chữ số trong bộ ba bằng cách sử dụng cấu trúc tần số như từ điển hoặc mảng có kích thước 10. Chúng tôi tăng số lần đếm cho từng giá trị trong số ba giá trị, sau đó quét tìm giá trị có tần số bằng một. Điều này đúng và chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Hơn 270 test case, tổng công việc không đáng kể. 

Tuy nhiên, cách tiếp cận này không hiệu quả vì cấu trúc của đầu vào bị hạn chế nhiều. Chúng ta thực sự không cần một bảng tần số đầy đủ; chúng ta chỉ cần phân biệt vị trí nào trong ba vị trí này khác với vị trí còn lại. Quan sát quan trọng là sự bằng nhau giữa ba phần tử có chính xác một phần tử riêng biệt có nghĩa là phần tử riêng biệt phải khác với ít nhất một trong hai phần tử còn lại, trong khi cặp bằng nhau phải khớp với nhau. 

Điều này cho phép trích xuất logic trực tiếp: so sánh các cặp. Nếu như`a == b`, sau đó`c`là câu trả lời. Ngược lại, nếu`a == c`, sau đó`b`là câu trả lời. Nếu không thì,`a`phải là duy nhất. Điều này làm giảm vấn đề xuống một số lượng so sánh không đổi mà không có bộ nhớ phụ trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đếm) | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| So sánh theo cặp | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Điều này xác định có bao nhiêu bộ ba độc lập mà chúng tôi sẽ xử lý. 
2. Với mỗi test case, hãy đọc ba chữ số`a`,`b`, Và`c`. 
3. So sánh`a`Và`b`. Nếu chúng bằng nhau thì cặp lặp lại đã được xác định, do đó giá trị thứ ba`c`phải là duy nhất. 
4. Nếu`a`Và`b`không bằng nhau, hãy so sánh`a`Và`c`. Nếu những điều này bằng nhau thì`b`là giá trị duy nhất không khớp với cặp của nó nên nó là chữ số duy nhất. 
5. Nếu không có điều kiện nào ở trên xảy ra thì`b`phải bằng`c`, Và`a`là chữ số duy nhất. 

### Tại sao nó hoạt động 

Việc đảm bảo rằng chính xác hai giá trị bằng nhau sẽ thực thi việc phân chia nghiêm ngặt ba phần tử thành một cặp và một phần tử. Mọi so sánh đều xác định trực tiếp cặp đó hoặc loại bỏ một ứng cử viên. Vì đẳng thức có tính bắc cầu nên khi chúng ta tìm thấy bất kỳ cặp khớp nào, phần tử còn lại không thể bằng chúng, vì vậy nó phải là phần tử duy nhất. Không có cấu hình thay thế nào phù hợp với ràng buộc đầu vào, điều này đảm bảo tính chính xác của chuỗi điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    
    if a == b:
        print(c)
    elif a == c:
        print(b)
    else:
        print(a)
```Giải pháp dựa hoàn toàn vào so sánh trực tiếp. Điều kiện đầu tiên kiểm tra xem hai giá trị đầu tiên đã tạo thành cặp lặp lại hay chưa, điều này ngay lập tức tách biệt câu trả lời. Điều kiện thứ hai chỉ đạt được nếu lần so sánh đầu tiên thất bại, vì vậy chúng ta biết`a != b`. Lúc đó, việc kiểm tra`a == c`xác định liệu`a`là một phần của cặp lặp lại. Nếu cả hai điều kiện đều không được thỏa mãn thì cấu trúc nhất quán duy nhất còn lại là`b == c`, làm`a`yếu tố độc đáo. 

Không có vòng lặp hoặc cấu trúc dữ liệu nào được yêu cầu ngoài việc phân tích cú pháp đầu vào. Thứ tự kiểm tra rất quan trọng vì nó tránh được sự so sánh dư thừa và đảm bảo chúng ta luôn xác định được đơn vị chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
1 2 2
```| Bước | một | b | c | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | a == b | Sai | 
| 2 | 1 | 2 | 2 | a == c | Sai | 
| 3 | 1 | 2 | 2 | dự phòng | xuất ra | 

Dấu vết này cho thấy trường hợp cặp lặp lại là`(2, 2)`, nhưng nó không được căn chỉnh ngay từ đầu. Logic tránh giả định cấu trúc vị trí một cách chính xác và thay vào đó suy ra các mối quan hệ bình đẳng. 

### Ví dụ Dấu vết 2 

đầu vào:```
5 5 6
```| Bước | một | b | c | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 6 | a == b | Đúng | 
| 2 | - | - | - | đầu ra c | 6 | 

Điều này thể hiện hành vi thoát sớm khi hai phần tử đầu tiên đã tạo thành cặp bằng nhau. Không cần so sánh thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng số lượng so sánh không đổi và in một kết quả | 

| Không gian | O(1) | Chỉ có ba biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép tối đa 270 trường hợp thử nghiệm, do đó, giải pháp thời gian không đổi cho mỗi trường hợp nằm trong giới hạn. Ngay cả khi được mở rộng sang kích thước đầu vào lớn hơn nhiều, cách tiếp cận tương tự sẽ mở rộng tuyến tính mà không có rủi ro. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        if a == b:
            out.append(str(c))
        elif a == c:
            out.append(str(b))
        else:
            out.append(str(a))
    return "\n".join(out)

# provided samples
assert run("""10
1 2 2
4 3 4
5 5 6
7 8 8
9 0 9
3 6 3
2 8 2
5 7 7
7 7 5
5 7 5""") == """1
3
6
7
0
6
8
5
5
7"""

# all equal pair in different positions
assert run("1\n2 3 3") == "2"

# pair at start
assert run("1\n9 9 1") == "1"

# pair in middle
assert run("1\n4 7 4") == "7"

# alternating pattern
assert run("3\n0 1 1\n2 2 3\n8 9 8") == "0\n3\n9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 3`|`2`| cặp ở cuối | 
|`9 9 1`|`1`| cặp lúc bắt đầu | 
|`4 7 4`|`7`| cặp chia xung quanh giữa | 
| lô hỗn hợp | khác nhau | nhiều trường hợp độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cặp bằng nhau không liền kề theo thứ tự đầu vào. Ví dụ, trong`4 7 4`, cặp`(4, 4)`được chia xung quanh vị trí giữa. Thuật toán xử lý việc này bằng cách kiểm tra đầu tiên`a == b`, không thành công, sau đó kiểm tra`a == c`, thành công, xác định chính xác`b`như là yếu tố duy nhất. 

Một trường hợp khác là khi cặp bằng nhau xuất hiện ở hai vị trí đầu tiên, chẳng hạn như`9 9 1`. Đây là điều kiện đầu tiên`a == b`kích hoạt ngay lập tức và đầu ra thuật toán`c`mà không cần bất kỳ kiểm tra thêm. Điều này xác nhận rằng hành vi thoát sớm là an toàn và không bỏ qua bất kỳ lý do cần thiết nào. 

Cuối cùng, trong`7 7 5`, cấu trúc giống hệt nhau nhưng có giá trị khác nhau. Bất biến tương tự giữ nguyên: tồn tại chính xác một cặp đẳng thức, do đó phép so sánh khớp đầu tiên luôn xác định phân vùng chính xác của bộ ba, đảm bảo phần tử duy nhất được trả về một cách nhất quán.
