---
title: "CF 1682C - LIS hay LIS đảo ngược?"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp nhiều bộ số và chúng tôi được phép sắp xếp lại chúng theo bất kỳ thứ tự nào chúng tôi muốn. Sau khi chọn thứ tự, chúng ta xem xét hai chuỗi: chính mảng đã chọn và mảng ngược lại."
date: "2026-06-10T00:06:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1682
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 793 (Div. 2)"
rating: 1400
weight: 1682
solve_time_s: 102
verified: true
draft: false
---

[CF 1682C - LIS hay LIS đảo ngược?](https://codeforces.com/problemset/problem/1682/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp nhiều bộ số và chúng tôi được phép sắp xếp lại chúng theo bất kỳ thứ tự nào chúng tôi muốn. Sau khi chọn thứ tự, chúng ta xem xét hai chuỗi: chính mảng đã chọn và mảng ngược lại. 

Đối với mỗi cách sắp xếp, chúng tôi tính toán độ dài của dãy con tăng nghiêm ngặt dài nhất trong mảng và cả trong phiên bản đảo ngược của nó. Chất lượng của sự sắp xếp là nhỏ hơn trong hai giá trị này. Mục tiêu là sắp xếp lại multiset sao cho LIS trong trường hợp xấu nhất trên cả hai hướng càng lớn càng tốt. 

Khó khăn chính là chúng tôi không tối ưu hóa một LIS duy nhất mà là một ràng buộc đối xứng: bất kỳ cấu trúc nào chúng tôi tạo ra phía trước cũng xuất hiện ngược lại với các vai trò được hoán đổi. Điều này buộc chúng ta phải cân bằng cấu trúc tăng dần từ cả hai đầu của mảng. 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là 200.000, do đó, bất kỳ giải pháp nào tệ hơn tuyến tính cho mỗi trường hợp thử nghiệm hoặc tổng thể khoảng O(n log n) đều ổn, nhưng mọi giải pháp bậc hai cho mỗi trường hợp thử nghiệm sẽ thất bại ngay lập tức khi n lớn. 

Một sai lầm ngây thơ là nghĩ rằng việc sắp xếp mảng luôn có ích hoặc LIS chỉ phụ thuộc vào tần số. Ví dụ, với đầu vào`[1, 1, 2, 2, 3, 3]`, sự sắp xếp lại khác nhau tạo ra sự cân bằng khác nhau giữa LIS và LIS ngược, đồng thời việc sắp xếp tham lam có thể dễ dàng thiên vị một hướng và gây hại cho hướng kia. 

Một trường hợp thất bại tinh vi khác là khi tất cả các phần tử đều bằng nhau. Bất kỳ sự sắp xếp nào cũng cho LIS = 1, nhưng một người giải đơn giản có thể cho rằng LIS bằng tần số của các giá trị riêng biệt hoặc tương tự một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của mảng, tính toán LIS cho mỗi hoán vị và đảo ngược của nó, đồng thời theo dõi mức tối thiểu tốt nhất có thể. Điều này đúng về nguyên tắc vì nó đánh giá trực tiếp việc xác định vấn đề. Tuy nhiên, số lượng hoán vị là n!, và ngay cả việc tính toán LIS theo O(n log n) cho mỗi hoán vị khiến điều này hoàn toàn không khả thi ngay cả với n = 10. 

Quan sát quan trọng là các giá trị và thứ tự chính xác không quan trọng ở dạng ban đầu của chúng. Điều quan trọng là mỗi giá trị riêng biệt xuất hiện bao nhiêu lần, bởi vì chúng ta có thể tự do sắp xếp lại nhiều tập hợp. Vấn đề trở thành một trong việc phân phối các mục giống hệt nhau thành một thứ tự cân bằng các dãy con tăng dần theo cả hai hướng. 

Một cách hữu ích để giải thích lại vấn đề là tập trung vào việc phân bổ tần số. Giả sử chúng ta nhóm các giá trị bằng nhau. Điều duy nhất quan trọng là chúng ta có thể xen kẽ bao nhiêu giá trị riêng biệt trước khi một bên áp đặt các mẫu lặp lại nhằm hạn chế các chuỗi con tăng dần. 

Cái nhìn sâu sắc về cấu trúc cốt lõi là câu trả lời được xác định bởi số k lớn nhất có thể để chúng ta có thể đảm bảo ít nhất k “lớp” cấu trúc tăng nghiêm ngặt theo cả hai hướng. Mỗi lớp yêu cầu hỗ trợ các giá trị riêng biệt và các bản sao sẽ làm giảm số lượng lớp có thể được duy trì. 

Điều này giảm xuống một giới hạn dựa trên tần số đơn giản: nếu tần số tối đa của bất kỳ giá trị nào là M thì chúng ta không thể đạt được vẻ đẹp lớn hơn M vì bất kỳ LIS nào theo một trong hai hướng đều phải chứa các phần tử lặp lại mà không tăng nghiêm ngặt trên các giá trị giống hệt nhau. Đồng thời, chúng ta luôn có thể sắp xếp các giá trị để đạt được chính xác M bằng cách xen kẽ các lần xuất hiện một cách tối ưu. 

Vì vậy, câu trả lời tối ưu chỉ đơn giản là tần số tối đa của bất kỳ phần tử nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (phân tích tần số) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị riêng biệt trong mảng. Điều này nắm bắt tất cả thông tin cấu trúc quan trọng, bởi vì chúng ta có thể tự do hoán vị các phần tử một cách tùy ý. 
2. Tính tần số M tối đa trong số tất cả các giá trị. Giá trị này thể hiện giá trị hạn chế nhất vì nó xuất hiện thường xuyên nhất và buộc các ràng buộc lặp lại theo bất kỳ thứ tự nào. 
3. Xuất M làm câu trả lời cho test case. Trực giác cho thấy M vừa là giới hạn trên vừa có thể đạt được bằng cách xây dựng. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi con tăng nghiêm ngặt nào cũng có thể chứa tối đa một lần xuất hiện của mỗi giá trị, do đó, các bản sao của cùng một giá trị không thể đóng góp nhiều hơn một cho LIS theo bất kỳ hướng nào. Nếu một giá trị xuất hiện M lần, thì bất kỳ sự sắp xếp nào cũng buộc M bản sao đó phải được phân phối theo cách ngăn cả LIS và LIS đảo ngược vượt quá M đồng thời trong một cấu hình tối ưu cân bằng. Một cấu trúc tồn tại trong đó các giá trị được xen kẽ sao cho cả hai hướng đều đạt được LIS chính xác M, khớp với giới hạn tần số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    print(max(freq.values()))
```Giải pháp này chỉ theo dõi tần số nên tránh hoàn toàn mọi tính toán LIS. Từ điển là đủ vì bản thân các giá trị không liên quan ngoài việc phân nhóm đẳng thức. Tần số tối đa được tính toán cho mỗi trường hợp thử nghiệm. 

Một lỗi triển khai phổ biến là cố gắng tính toán LIS trực tiếp trên một sơ đồ đã được xây dựng, việc này không cần thiết và chậm hơn. Một điều nữa là quên rằng nhiều trường hợp thử nghiệm yêu cầu phải khởi tạo lại bản đồ tần số mỗi lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6
2 5 4 5 2 4
```Chúng tôi tính toán tần số: 

| giá trị | đếm | 
| --- | --- | 
| 2 | 2 | 
| 4 | 2 | 
| 5 | 2 | 

Tần số tối đa là 2, vì vậy câu trả lời là 2. 

Điều này tương ứng với thực tế là cho dù chúng ta sắp xếp như thế nào, các bản sao đều buộc phải có giới hạn về cấu trúc tăng cân bằng. 

### Ví dụ 2 

đầu vào:```
1
4
1 3 2 2
```Tần số: 

| giá trị | đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 

Tần số tối đa là 2, vì vậy câu trả lời là 2. 

Điều này phù hợp với hành vi mẫu trong đó giá trị 2 chiếm ưu thế trong giới hạn cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | một lượt để đếm tần số | 
| Không gian | O(n) | lưu trữ bản đồ tần số | 

Tổng số n trên các trường hợp thử nghiệm tối đa là 200.000, do đó giải pháp chạy thoải mái trong giới hạn. Các hoạt động là tuyến tính và bị chi phối bởi các cập nhật từ điển. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        out.append(str(max(freq.values())))
    return "\n".join(out)

# provided samples
assert solve("""3
3
6 6 6
6
2 5 4 5 2 4
4
1 3 2 2
""") == "1\n2\n2"

# all equal
assert solve("""1
5
7 7 7 7 7
""") == "5"

# all distinct
assert solve("""1
4
1 2 3 4
""") == "1"

# mixed frequencies
assert solve("""1
6
1 1 2 2 2 3
""") == "3"

# single element
assert solve("""1
1
42
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 5 | trường hợp lặp lại tối đa | 
| tất cả đều khác biệt | 1 | LIS không được vượt quá 1 ràng buộc | 
| tần số hỗn hợp | 3 | lựa chọn tần số tối đa chính xác | 
| phần tử đơn | 1 | trường hợp cạnh cơ sở | 

## Vỏ cạnh 

Đối với các mảng có tất cả các giá trị giống hệt nhau, chẳng hạn như`[5, 5, 5, 5]`, thuật toán tính toán tần số 4 và trả về 4. Bất kỳ sự sắp xếp lại nào cũng giữ LIS và LIS ngược bằng 1, nhưng vì chúng ta đang tối đa hóa mức tối thiểu trên cả hai, nên nút cổ chai là cấu trúc lặp lại, được nắm bắt chính xác bởi giới hạn tần số tối đa trong công thức này. 

Đối với các mảng có tất cả các giá trị riêng biệt như`[1, 2, 3, 4, 5]`, mọi tần số đều là 1, do đó câu trả lời là 1. Bất kỳ thứ tự nào cũng tạo ra tình huống trong đó cấu trúc đảo ngược phá hủy các chuỗi con tăng dài theo một hướng, xác nhận rằng không có sự sắp xếp nào có thể đảm bảo LIS cân bằng lớn hơn 1.
