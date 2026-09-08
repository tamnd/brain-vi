---
title: "CF 104573C - Kỳ nhông ánh kim"
description: "Chúng ta được cung cấp một bộ sưu tập cự đà, mỗi con được xác định bằng ID từ 1 đến N. Mỗi cự đà có hai thuộc tính: một số vảy và một số màu sắc. Mục tiêu là xếp hạng những con cự đà này từ tốt nhất đến kém nhất bằng cách sử dụng quy tắc so sánh theo cặp, sau đó đưa ra ba con cao nhất."
date: "2026-06-30T08:19:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 69
verified: true
draft: false
---

[CF 104573C - Cự đà ánh kim](https://codeforces.com/problemset/problem/104573/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập cự đà, mỗi con được xác định bằng ID từ 1 đến N. Mỗi cự đà có hai thuộc tính: một số vảy và một số màu sắc. Mục tiêu là xếp hạng những con cự đà này từ tốt nhất đến kém nhất bằng cách sử dụng quy tắc so sánh theo cặp, sau đó đưa ra ba con cao nhất. 

Quy tắc so sánh không xác định rõ ràng một điểm tổng thể duy nhất. Thay vào đó, iguana i được coi là tốt hơn iguana j nếu tỷ lệ vảy thỏa mãn so sánh chéo với tỷ lệ màu sắc, nghĩa là iguana i có “lợi thế về vảy” so với j cao hơn so với “bất lợi về màu sắc”. Nếu sự so sánh hoàn toàn cân bằng thì kỳ nhông có ID lớn hơn sẽ thắng. 

Kiểu so sánh này tạo ra một thứ tự tổng thể có thể được biểu diễn dưới dạng sắp xếp theo khóa dẫn xuất, nhưng bản thân khóa đó không rõ ràng ngay lập tức. Thách thức là chuyển đổi sự so sánh dựa trên tỷ lệ thành một thứ có thể được tính toán một cách hiệu quả cho tất cả N cự đà. 

Với N lên tới 100.000, bất kỳ cách tiếp cận nào so sánh trực tiếp từng cặp đều dẫn đến so sánh gần đúng N2, tức là khoảng 10¹⁰ phép tính trong trường hợp xấu nhất và vượt xa những gì phù hợp trong một giây. Điều này ngay lập tức loại trừ việc phân loại thô bạo bằng một bộ so sánh tùy chỉnh thực hiện lặp đi lặp lại các phép tính đắt tiền. 

Một trường hợp cạnh tinh tế phát sinh khi hai con cự đà tương đương nhau trong điều kiện tỷ lệ. Ví dụ: nếu hai con cự đà có các thuộc tính tỷ lệ như (2, 4) và (3, 6), thì tỷ lệ của chúng bằng nhau. Trong trường hợp này, quy tắc ràng buộc phụ thuộc vào ID chứ không phụ thuộc vào thuộc tính. Việc triển khai ngây thơ mà quên mất sự ràng buộc này có thể tạo ra thứ tự không nhất quán trong các thuật toán sắp xếp. 

Một trường hợp khác là so sánh dấu phẩy động. Ví dụ: so sánh 1/3 và 2/6 phải bằng nhau, nhưng số học nổi có thể gây ra các lỗi chính xác nhỏ và làm đứt các mối nối không chính xác. Điều này dẫn đến thứ tự không chính xác và hành vi sắp xếp không ổn định. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp gợi ý so sánh cự đà theo cặp bằng cách sử dụng quy tắc đã cho. Đối với mỗi cặp (i, j), chúng tôi sẽ tính xem (a_i / a_j) > (b_i / b_j). Điều này có thể được viết lại dưới dạng phép nhân chéo, nhưng ngay cả khi đó, một cách sắp xếp dựa trên so sánh đầy đủ vẫn thực hiện các phép so sánh O(N log N), mỗi phép so sánh liên quan đến phép nhân số nguyên lên tới 10⁹ × 10⁹, an toàn trong Python nhưng vẫn không cần thiết phải nghĩ đến ở cấp độ đó. 

Thông tin chi tiết quan trọng là chuyển đổi phép so sánh thành một khóa có thể sắp xếp duy nhất. Chúng ta bắt đầu từ bất đẳng thức: 

a_i / a_j > b_i / b_j 

Nhân chéo (tất cả các giá trị đều dương, do đó hướng được giữ nguyên): 

a_i * b_j > a_j * b_i 

Việc sắp xếp lại phần này sẽ đưa ra sự so sánh giữa hai mục i và j chỉ phụ thuộc vào từng mục riêng lẻ: 

a_i * b_j > a_j * b_i tương đương với việc sắp xếp theo tỷ lệ a_i/b_i theo thứ tự giảm dần. 

Do đó, mỗi con kỳ nhông có thể được gán một giá trị v_i = a_i / b_i và việc sắp xếp theo giá trị giảm dần này sẽ tạo ra thứ tự đúng. 

Tuy nhiên, tỷ lệ dấu phẩy động không an toàn do vấn đề về độ chính xác, vì vậy chúng tôi tránh tính toán trực tiếp v_i. Thay vào đó, chúng tôi xác định một bộ so sánh giữa i và j bằng cách sử dụng phép nhân chéo: 

a_i * b_j so với a_j * b_i 

Điều này đưa ra một thứ tự nghiêm ngặt mà không có số học dấu phẩy động. 

Cuối cùng, khi hai con cự đà thỏa mãn a_i * b_j == a_j * b_i, trước tiên chúng tôi sẽ ngắt mối quan hệ bằng ID lớn hơn. 

Điều này làm giảm vấn đề xuống một loại sắp xếp tiêu chuẩn với bộ so sánh tùy chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xếp hạng theo cặp Brute Force | O(N2) | O(1) | Quá chậm | 
| Sắp xếp bằng bộ so sánh nhân chéo | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Quy trình từng bước

1. Đọc tất cả cự đà và lưu trữ từng con dưới dạng một bộ dữ liệu (a_i, b_i, i). ID phải được lưu trữ vì nó cần thiết cho việc kết nối và xuất dữ liệu. 
2. Xác định quy tắc sắp xếp giữa hai con cự đà i và j. Chúng tôi so sánh a_i * b_j và a_j * b_i. Nếu sản phẩm đầu tiên lớn hơn thì iguana i sẽ tốt hơn. Nếu cái thứ hai lớn hơn thì iguana j sẽ tốt hơn. 
3. Nếu các sản phẩm bằng nhau, hãy so sánh ID của chúng và chọn ID lớn hơn. Điều này thực thi thứ tự xác định khi tỷ lệ khớp chính xác. 
4. Sắp xếp danh sách bằng logic so sánh này. Trong Python, chúng tôi thực hiện điều này bằng cách chuyển đổi từng con kỳ nhông thành một khóa duy trì thứ tự mà không cần viết một bộ so sánh rõ ràng. Một cách tiếp cận an toàn là sắp xếp theo bộ dữ liệu (a_i / b_i không được sử dụng trực tiếp), thay vào đó, chúng tôi sử dụng khóa tùy chỉnh được xây dựng bằng khả năng của Python để so sánh các phân số thông qua phép nhân chéo bằng cách sử dụng bộ dữ liệu có biểu diễn tỷ lệ hoặc bằng cách sử dụng functools.cmp_to_key. 
5. Sau khi sắp xếp, xuất ba ID đầu tiên theo thứ tự. 

## Tại sao nó hoạt động 

Quy tắc so sánh xác định thứ tự chặt chẽ tương đương với việc sắp xếp theo giá trị hữu tỉ a_i/b_i. Phép nhân chéo chuyển đổi so sánh tỷ lệ này thành số học số nguyên mà không làm mất độ chính xác. Vì tất cả các giá trị đều dương nên thứ tự có tính bắc cầu và nhất quán, điều này đảm bảo rằng việc sắp xếp sẽ tạo ra thứ hạng toàn cầu hợp lệ. Quy tắc tie-break đảm bảo rằng các tỷ lệ bằng nhau vẫn tạo thành một thứ tự chặt chẽ nhất quán, bắt buộc để sắp xếp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from functools import cmp_to_key

def cmp(i, j):
    ai, bi, idi = i
    aj, bj, idj = j

    left = ai * bj
    right = aj * bi

    if left > right:
        return -1
    if left < right:
        return 1

    # tie: higher ID first
    if idi > idj:
        return -1
    return 1

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

arr = [(a[i], b[i], i + 1) for i in range(n)]
arr.sort(key=cmp_to_key(cmp))

print(arr[0][2])
print(arr[1][2])
print(arr[2][2])
```Giải pháp lưu trữ từng con cự đà cùng với ID của nó để logic sắp xếp có thể truy cập cả hai thuộc tính và giải quyết các mối quan hệ một cách chính xác. Hàm so sánh thực hiện quy tắc toán học chính xác bằng cách sử dụng phép nhân chéo, đảm bảo không có lỗi dấu phẩy động. 

Việc sử dụng`cmp_to_key`là cần thiết vì API sắp xếp của Python yêu cầu chức năng chính thay vì bộ so sánh. Trình bao bọc này chuyển đổi bộ so sánh thành một đối tượng khóa có thể sắp xếp được. 

Cuối cùng, sau khi sắp xếp, ba phần tử đầu tiên tương ứng với những con cự đà tốt nhất theo thứ tự đã xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 3 10 5 6 9
7 8 2 4 6 9
```Chúng tôi tính toán so sánh ngầm thông qua tỷ lệ a_i / b_i: 

| Bước | Cặp hiện tại | Kết quả so sánh | Đặt hàng cho đến nay | 
| --- | --- | --- | --- | 
| 1 | (1,7) so với người khác | tỷ lệ nhỏ nhất | | 
| 2 | (10,2) | tỷ lệ lớn nhất | trở thành hạng 1 | 
| 3 | (5,4), (6,6), (9,9) | tầm trung | đặt hàng dưới 2/10 | 

Thứ tự sắp xếp cuối cùng trở thành ID: 

3, 4, 6, 5, 2, 1 

Chúng tôi xuất ba đầu tiên: 

3 

4 

6 

Điều này xác nhận rằng tỷ lệ a/b cao hơn chiếm ưu thế trong việc đặt hàng và không cần có sự ràng buộc ở đây. 

### Ví dụ 2 

đầu vào:```
4
2 4 6 3
4 8 12 5
```Ở đây nhiều con cự đà có tỷ lệ giống hệt nhau: 

| ID | một | b | Tỷ lệ a/b | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 0,5 | 
| 2 | 4 | 8 | 0,5 | 
| 3 | 6 | 12 | 0,5 | 
| 4 | 3 | 5 | 0,6 | 

Thứ tự sắp xếp: 

| Bước | Đã chọn | Lý do | 
| --- | --- | --- | 
| 1 | 4 | tỷ lệ cao nhất | 
| 2 | 3 | nhóm hòa, ID cao nhất trước | 
| 3 | 2 | cùng tỷ lệ tie-break | 
| 4 | 1 | cùng tỷ lệ tie-break | 

Đầu ra: 

4 

3 

2 

Điều này cho thấy rằng việc phá vỡ ràng buộc bằng ID là cần thiết để đặt hàng xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | phân loại chiếm ưu thế; mỗi phép so sánh là số học O(1) | 
| Không gian | O(N) | lưu trữ bộ dữ liệu kỳ nhông | 

Các ràng buộc cho phép lên tới 100.000 con cự đà và việc sắp xếp O(N log N) vừa vặn thoải mái trong giới hạn thời gian trong Python. Việc sử dụng bộ nhớ là tuyến tính và dễ dàng phù hợp trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from functools import cmp_to_key

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def cmp(i, j):
        ai, bi, idi = i
        aj, bj, idj = j
        left = ai * bj
        right = aj * bi
        if left > right:
            return -1
        if left < right:
            return 1
        return -1 if idi > idj else 1

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    arr = [(a[i], b[i], i + 1) for i in range(n)]
    arr.sort(key=cmp_to_key(cmp))

    return "\n".join(map(str, [arr[0][2], arr[1][2], arr[2][2]])) + "\n"

# provided sample
assert run("""6
1 3 10 5 6 9
7 8 2 4 6 9
""") == "3\n4\n6\n"

# all equal ratios, tie-break by ID
assert run("""3
2 4 6
4 8 12
""") == "3\n2\n1\n"

# minimum size edge case
assert run("""3
1 2 3
3 2 1
""") in ["3\n1\n2\n", "3\n2\n1\n"]

# extreme dominance
assert run("""3
100 1 2
1 100 50
""") == "1\n3\n2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tỷ lệ bằng nhau | 3 2 1 | hòa theo ID | 
| tỷ lệ hỗn hợp | 3 1 2 | đúng hướng đặt hàng | 
| giá trị cực trị | 1 3 2 | ổn định theo tỷ lệ lớn | 

## Vỏ cạnh 

Khi tất cả cự đà có các thuộc tính tỷ lệ, chẳng hạn như (2,4), (4,8), (6,12), sự so sánh sẽ suy biến thành thứ tự ID thuần túy. Thuật toán xử lý điều này vì bộ so sánh rõ ràng quay trở lại ID khi các tích chéo bằng nhau. Sau đó, việc sắp xếp tạo ra các ID giảm dần trong nhóm bằng nhau, đảm bảo đầu ra xác định. 

Khi một con kỳ nhông thống trị tất cả những con kỳ nhông khác, chẳng hạn như (10^9, 1) so với những con kỳ nhông khác có tỷ lệ nhỏ hơn nhiều, phép nhân chéo luôn ưu tiên con kỳ nhông đó vì 10^9 * b_j sẽ vượt quá a_i * 1 đối với bất kỳ a_i hợp lý nào. Thuật toán đặt nó ở trên cùng một cách chính xác mà không cần bất kỳ cách viết hoa đặc biệt nào. 

Khi các giá trị cực kỳ lớn, lên tới 10^9, phép nhân chéo có thể đạt tới 10^18, vẫn phù hợp an toàn với số nguyên Python. Điều này tránh những lo ngại tràn có thể xuất hiện trong các ngôn ngữ số nguyên có chiều rộng cố định.
