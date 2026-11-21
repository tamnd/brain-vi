---
title: "CF 2195A - Sàng Erato67henes"
description: "Chúng ta được cho một số tập hợp số nguyên rất nhỏ, mỗi tập hợp chứa tối đa năm số và chúng ta được hỏi liệu chúng ta có thể chọn một tập hợp con không trống nào có tích chính xác là 67 hay không. Mỗi trường hợp thử nghiệm là độc lập."
date: "2026-06-07T20:37:13+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2195
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1080 (Div. 3)"
rating: 800
weight: 2195
solve_time_s: 71
verified: true
draft: false
---

[CF 2195A - Sàng Erato67henes](https://codeforces.com/problemset/problem/2195/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số tập hợp số nguyên rất nhỏ, mỗi tập hợp chứa tối đa năm số và chúng ta được hỏi liệu chúng ta có thể chọn một tập hợp con không trống nào có tích chính xác là 67 hay không. 

Mỗi trường hợp thử nghiệm là độc lập. Trong trường hợp thử nghiệm, chúng tôi được phép chọn bất kỳ tập hợp con nào của các số đã cho, miễn là chúng tôi chọn ít nhất một phần tử và chúng tôi nhân tất cả các giá trị đã chọn với nhau. Câu hỏi đặt ra là liệu có tồn tại tập hợp con nào có tích chính xác bằng 67 hay không. 

Ràng buộc mỗi test có tối đa năm số sẽ thay đổi hoàn toàn bản chất của vấn đề. Một bộ kích thước năm chỉ có$2^5 - 1 = 31$các tập hợp con không trống, do đó, việc liệt kê tất cả các tập hợp con một cách thô bạo cũng không đáng kể$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu về lý thuyết số phức tạp hoặc các thủ thuật tối ưu hóa. 

Một vấn đề tế nhị nảy sinh từ thực tế là giá trị mục tiêu 67 là số nguyên tố. Điều này có nghĩa là bất kỳ lựa chọn hợp lệ nào cũng phải nhân chính xác đến 67, vì vậy cách duy nhất để thành công là chọn các số có tích không chứa thừa số nguyên tố và khớp chính xác với 67. Vì tất cả các giá trị đầu vào nằm trong khoảng từ 1 đến 67 nên các số duy nhất có thể đóng góp có ý nghĩa là 1 và 67, vì mọi số nguyên khác đều đưa ra các thừa số nguyên tố không tương thích với 67. 

Một sai lầm ngây thơ là cho rằng các kết hợp như 7 và 9 hoặc 3 và 23 bằng cách nào đó có thể kết hợp thành 67. Điều này là không thể vì 67 không có hệ số phân tích không tầm thường đối với các số nguyên lớn hơn 1. 

Một cạm bẫy tiềm ẩn khác là quên rằng không được phép chọn phần tử nào. Ví dụ: nếu không có phần tử nào bằng 67 và tất cả các phần tử đều bằng 1, thì việc triển khai bất cẩn có thể kết thúc thành công một cách không chính xác bằng cách coi sản phẩm trống là 1 rồi "mở rộng quy mô", nhưng điều đó không được phép. 

Các trường hợp cạnh bao gồm: 

Nếu mảng chứa một phần tử bằng 67, câu trả lời rõ ràng là CÓ. 

đầu vào:```
1
1
67
```Đầu ra:```
YES
```Nếu mảng chỉ chứa các số đơn vị và các số tổng hợp nhỏ như 2, 3, 5, 7 thì không có tổ hợp nào có thể tạo thành 67. 

đầu vào:```
1
5
1 2 3 4 5
```Đầu ra:```
NO
```Điều này là do bất kỳ lựa chọn nào không phải là một đều đưa ra các yếu tố không liên quan đến 67. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta thử mọi tập con không rỗng của các số đã cho, tính tích của nó và kiểm tra xem nó có bằng 67 hay không. Vì$n \le 5$, chúng ta có nhiều nhất là 31 tập con, vì vậy đây là tối đa 31 phép nhân cho mỗi trường hợp thử nghiệm. Sang$10^4$trường hợp thử nghiệm, đây là khoảng$3.1 \times 10^5$hoạt động thoải mái trong giới hạn. 

Quan sát quan trọng là chúng ta thậm chí không cần tính tích đầy đủ. Mục tiêu là một số cố định, 67, là số nguyên tố. Bất kỳ thừa số nào ngoài 1 hoặc 67 ngay lập tức khiến không thể đạt tới 67. Điều này có nghĩa là chúng ta chỉ cần kiểm tra xem liệu chúng ta có thể chọn một số 67 hay chỉ kết hợp 1 và 67. Vì nhân với 1 không có ý nghĩa gì nên yêu cầu có ý nghĩa duy nhất là sự hiện diện của ít nhất một số 67 trong mảng. Nếu số 67 tồn tại, chúng ta có thể bỏ qua tất cả các số khác và chỉ chọn phần tử đó. 

Vì vậy, vấn đề giảm từ việc liệt kê tập hợp con sang kiểm tra sự tồn tại đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con) |$O(t \cdot 2^n)$|$O(1)$| Đã chấp nhận | 
| Tối ưu (kiểm tra sự tồn tại của 67) |$O(t \cdot n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc danh sách các số. Chúng ta chỉ cần xác định xem 67 có xuất hiện ít nhất một lần hay không, bởi vì bất kỳ tập hợp con hợp lệ nào cũng phải bao gồm 67 và không thể bao gồm bất kỳ số nào lớn hơn 1 mà không phải là 67. 
2. Quét qua mảng và kiểm tra xem có phần tử nào bằng 67 hay không. Khi tìm thấy nó, chúng ta biết tồn tại một tập hợp con hợp lệ bằng cách chỉ chọn phần tử đó. 
3. Nếu chúng tôi quét xong mà không tìm thấy 67, hãy kết luận rằng không có tập hợp con nào có thể tạo ra 67, vì tất cả các số khác đều là 1 hoặc có các thừa số tối giản không thể tạo thành 67. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ cấu trúc của 67. Vì 67 là số nguyên tố nên bất kỳ tích nào bằng 67 đều phải bao gồm chính xác một thừa số bằng 67 và tất cả các thừa số khác bằng 1. Không có sự kết hợp nào của các số nguyên lớn hơn 1 ngoài chính 67 có thể tránh được việc đưa ra thêm các thừa số nguyên tố. Vì vậy, sự tồn tại của số 67 trong mảng vừa cần vừa đủ để câu trả lời là CÓ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    
    ok = False
    for x in arr:
        if x == 67:
            ok = True
            break
    
    print("YES" if ok else "NO")
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập và thực hiện quét tuyến tính trên tối đa năm số. Việc nghỉ sớm đảm bảo công việc tối thiểu trong trường hợp 67 xuất hiện sớm. 

Chi tiết triển khai chính là chúng tôi không hề cố gắng tính toán sản phẩm. Điều đó tránh được những lo ngại về tăng trưởng số nguyên không cần thiết và giữ cho logic phù hợp với cấu trúc toán học của giá trị đích. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 7 6 7 67
```Chúng tôi theo dõi xem 67 có xuất hiện hay không. 

| Bước | Giá trị hiện tại | Tìm thấy 67? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 7 | Không | 
| 3 | 6 | Không | 
| 4 | 7 | Không | 
| 5 | 67 | Có | 

Khi gặp số 67, chúng ta có thể dừng lại ngay lập tức. Bất kỳ tập hợp con nào chỉ chứa phần tử này đều cho kết quả 67. 

Đầu ra là CÓ. 

Dấu vết này cho thấy các giá trị trung gian như 7 hoặc 6 hoàn toàn không quan trọng, xác nhận rằng chỉ có sự hiện diện chính xác của 67 mới có liên quan. 

### Ví dụ 2 

đầu vào:```
5
1 3 5 7 8
```| Bước | Giá trị hiện tại | Tìm thấy 67? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 3 | Không | 
| 3 | 5 | Không | 
| 4 | 7 | Không | 
| 5 | 8 | Không | 

Không tìm thấy 67 nên không có tập hợp con nào có thể tạo ra sản phẩm được yêu cầu. 

Đầu ra là KHÔNG. 

Dấu vết này chứng tỏ rằng mặc dù có nhiều số tồn tại nhưng không có số nào đóng góp vào việc xây dựng 67, vì chúng đưa ra các thừa số nguyên tố không tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot n)$| Mỗi test case quét tối đa 5 phần tử một lần | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài vài biến | 

Các ràng buộc là cực kỳ nhỏ đối với mỗi trường hợp thử nghiệm, do đó, ngay cả việc liệt kê tập hợp con mạnh mẽ cũng sẽ vượt qua, nhưng việc kiểm tra sự tồn tại được tối ưu hóa sẽ làm giảm giải pháp thành công việc không đổi cho mỗi trường hợp thử nghiệm trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        ok = any(x == 67 for x in arr)
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("""2
5
1 7 6 7 67
5
1 3 5 7 8
""") == """YES
NO"""

# single element success
assert run("""1
1
67
""") == "YES"

# single element failure
assert run("""1
1
5
""") == "NO"

# all ones
assert run("""1
5
1 1 1 1 1
""") == "NO"

# mixed small numbers
assert run("""1
5
2 3 4 5 6
""") == "NO"

# multiple test cases
assert run("""3
3
1 1 67
2
7 11
4
67 67 1 1
""") == """YES
NO
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 67 | CÓ | trường hợp tích cực tối thiểu | 
| đơn phi 67 | KHÔNG | trường hợp tiêu cực tối thiểu | 
| tất cả những cái | KHÔNG | 1s không tạo được 67 | 
| hỗn hợp tổng hợp | KHÔNG | yếu tố không liên quan không thể giúp được | 
| nhiều bài kiểm tra trùng lặp | CÓ/KHÔNG trộn | tính đúng đắn trong các trường hợp | 

## Vỏ cạnh 

Mảng một phần tử chứa 67 là trường hợp thành công trực tiếp nhất. Thuật toán đọc một giá trị, phát hiện sự bằng nhau ngay lập tức và trả về CÓ mà không cần thực hiện thêm thao tác nào. 

Một mảng duy nhất là trường hợp trực giác nhân có thể đánh lừa. Mặc dù mọi tích của tập hợp con đều là 1 nhưng không có tập hợp con nào có thể đạt tới 67, do đó quá trình quét hoàn tất mà không tìm thấy 67 và trả về KHÔNG. 

Một trường hợp có nhiều 67, chẳng hạn như`[67, 67, 1, 1]`, vẫn hoạt động chính xác vì thuật toán không quan tâm đến số lượng. Lần xuất hiện đầu tiên kích hoạt CÓ và phần còn lại không liên quan.
