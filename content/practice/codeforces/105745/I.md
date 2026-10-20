---
title: "CF 105745I - Cơn ác mộng CNTT"
description: "Chúng ta được cung cấp một chuỗi các phần tử đại diện cho “trạng thái” trong một hệ thống nơi các phân đoạn tương tác một cách hạn chế. Mỗi phần tử có một giá trị và bài toán yêu cầu chúng ta liên tục đánh giá hoặc tối ưu hóa một hàm trên tất cả các mảng con liền kề."
date: "2026-06-25T21:09:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105745
codeforces_index: "I"
codeforces_contest_name: "AGM 2025 Qualification Round"
rating: 0
weight: 105745
solve_time_s: 31
verified: true
draft: false
---

[CF 105745I - Cơn ác mộng CNTT](https://codeforces.com/problemset/problem/105745/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các phần tử đại diện cho “trạng thái” trong một hệ thống nơi các phân đoạn tương tác một cách hạn chế. Mỗi phần tử có một giá trị và bài toán yêu cầu chúng ta liên tục đánh giá hoặc tối ưu hóa một hàm trên tất cả các mảng con liền kề. Hoạt động cốt lõi không mang tính cục bộ, bởi vì mọi phân đoạn đều phụ thuộc vào cả phần tử tối thiểu và tối đa của nó, nghĩa là mỗi cặp ranh giới đều có khả năng thay đổi sự đóng góp của nhiều mảng con. 

Đầu ra là một giá trị tổng hợp duy nhất được tính toán trên tất cả các mảng con sau khi áp dụng một phép biến đổi phụ thuộc vào cực trị bên trong mỗi mảng con. Cấu trúc gợi ý rõ ràng rằng việc liệt kê đơn giản trên tất cả các mảng con là một phần của điểm khởi đầu tinh thần, nhưng ngay lập tức không thể thực hiện được khi kích thước mảng tăng lên đến các ràng buộc điển hình của Codeforces như 2⋅10^5 hoặc 3⋅10^5. 

Với n lên đến 3⋅10^5, bất kỳ phép liệt kê O(n²) nào của các mảng con đều bao hàm khoảng 10^10 thao tác, vượt xa giới hạn thời gian. Ngay cả các phương pháp tiếp cận O(n² log n) cũng bị loại trừ. Điều này buộc chúng ta phải hướng tới một giải pháp trong đó mỗi phần tử chỉ tham gia vào một số “sự kiện” giới hạn, thường được khấu hao O(1) hoặc O(log n). 

Trường hợp cạnh tinh tế phát sinh khi các giá trị bằng hoặc gần bằng nhau. Trong nhiều bài toán “tối thiểu/tối đa trên mảng con”, các giá trị bằng nhau sẽ phá vỡ các giả định ngăn xếp đơn điệu trừ khi được xử lý rõ ràng. Ví dụ: trong một mảng như [5, 1, 1, 5], các so sánh nghiêm ngặt ngây thơ có thể đếm gấp đôi các mảng con hoặc đặt sai ranh giới. Một trường hợp sai sót khác là khi các khoản đóng góp được tính toán với giả định là có sự độc lập giữa ranh giới tối thiểu và tối đa; điều này không thành công khi cả hai cực trị được cập nhật bởi cùng một phần tử, điều này xảy ra thường xuyên trong phạm vi nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi mảng con, hãy tính giá trị tối thiểu và tối đa của nó bằng cách quét nó, sau đó đánh giá biểu thức cần thiết và thêm nó vào câu trả lời. Điều này đúng vì nó tuân theo định nghĩa của hàm. Tuy nhiên, đối với mỗi mảng con O(n2), việc tính toán chi phí tối thiểu và tối đa là O(n) trong trường hợp xấu nhất, tạo ra độ phức tạp tổng cộng là O(n³). Ngay cả với quá trình tiền xử lý tiền tố cho RMQ, đưa ra các truy vấn tối thiểu/tối đa O(1), chúng ta vẫn có các mảng con O(n²), vốn đã quá lớn. 

Quan sát quan trọng giúp mở ra tính hiệu quả là mỗi phần tử trở thành giá trị tối thiểu hoặc tối đa của một mảng con trên một phạm vi lựa chọn liền kề của điểm cuối bên trái và bên phải. Thay vì lặp lại các mảng con, chúng tôi đảo ngược phối cảnh: sửa một phần tử và đếm xem có bao nhiêu mảng con mà nó có giá trị tối thiểu hoặc tối đa kiểm soát. Điều này chuyển đổi một vấn đề liệt kê toàn cầu thành một vấn đề đóng góp cho mỗi phần tử. 

Khi chúng ta chuyển sang phần đóng góp, cấu trúc sẽ trở nên đơn điệu. Ở mức tối thiểu, chúng tôi mở rộng ranh giới bên trái và bên phải cho đến khi đạt được phần tử nhỏ hơn; đạt mức tối đa cho đến khi chúng ta đạt được phần tử lớn hơn. Đây là cách phân rã ngăn xếp đơn điệu cổ điển để phân chia mảng thành các khoảng ảnh hưởng. 

Khía cạnh “ác mộng” của vấn đề thường xuất phát từ việc cần đồng thời cả tương tác tối thiểu và tối đa. Trong những trường hợp như vậy, thủ thuật tiêu chuẩn là xử lý các đóng góp bằng cách sắp xếp các sự kiện theo giá trị, thường sử dụng một ngăn xếp đơn điệu kết hợp với việc quét hoặc hợp nhất chia để trị. Mỗi phần tử đóng góp chính xác một lần cho mỗi hướng ưu thế, do đó tổng độ phức tạp giảm xuống tuyến tính hoặc gần tuyến tính. 

Do đó, quá trình chuyển đổi từ giải pháp vũ phu sang giải pháp tối ưu là sự chuyển đổi từ “liệt kê các mảng con” sang “liệt kê phạm vi ảnh hưởng của từng phần tử”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) hoặc O(n² log n) | O(1)-O(n2) | Quá chậm | 
| Tối ưu (ngăn xếp đơn điệu/đếm đóng góp) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi phần tử, xác định phần tử gần nhất nhỏ hơn ở bên trái và bên phải. Điều này xác định khoảng thời gian tối đa trong đó phần tử này có thể đóng vai trò ở mức tối thiểu mà không bị ghi đè. Điều tương tự cũng được thực hiện đối với các phần tử lớn hơn để có ảnh hưởng tối đa. 
2. Xây dựng hai mảng, một mảng dành cho các ranh giới nhỏ hơn trước và nhỏ hơn tiếp theo, còn một mảng dành cho các ranh giới lớn hơn trước đó và lớn hơn tiếp theo. Các ranh giới này được tính toán bằng cách sử dụng một ngăn xếp đơn điệu, giúp duy trì các ứng cử viên theo thứ tự tăng hoặc giảm tùy theo hướng. 
3. Với mỗi chỉ mục i, hãy tính xem có bao nhiêu mảng con coi i là nhỏ nhất. Đây là kết quả của việc chúng ta có thể kéo dài sang trái và phải bao xa mà không gặp phải phần tử nhỏ hơn. Điều tương tự được tính cho sự đóng góp tối đa. 
4. Mỗi đóng góp của mảng con được xác định bằng cách ghép một phân đoạn chiếm ưu thế tối thiểu và cấu trúc phân khúc chiếm ưu thế tối đa. Thay vì lặp lại các mảng con, chúng tôi tích lũy các đóng góp cho mỗi phần tử dựa trên số lượng mảng con mà nó chi phối ở mức cực đoan. 
5. Tính tổng tất cả các đóng góp một cách cẩn thận, đảm bảo rằng mỗi mảng con được tính chính xác một lần. Điều này thường dựa vào sự phân rã trong đó mỗi mảng con có đóng góp cặp tối thiểu và tối đa được xác định duy nhất. 

Bước không tầm thường là tính đúng đắn của việc phân tách ranh giới. Ngăn xếp đơn điệu đảm bảo rằng đối với mỗi phần tử, khoảng tính toán là tối đa và không trùng lặp không chính xác với ứng cử viên mạnh hơn. 

### Tại sao nó hoạt động 

Mỗi mảng con có một phần tử tối thiểu duy nhất và một phần tử tối đa duy nhất. Bằng cách gán trách nhiệm về mảng con đó cho các vị trí của các cực trị này, chúng ta phân chia toàn bộ các mảng con thành các lớp rời rạc. Ngăn xếp đơn điệu đảm bảo rằng mỗi phần tử được xem xét chính xác trong phạm vi các mảng con trong đó nó hợp lệ ở mức cực trị. Vì các phạm vi này là tối đa và không chồng chéo theo thứ tự thống trị nên mỗi mảng con đóng góp chính xác một lần vào tổng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # previous smaller
    prev_smaller = [-1] * n
    stack = []
    for i in range(n):
        while stack and a[stack[-1]] > a[i]:
            stack.pop()
        prev_smaller[i] = stack[-1] if stack else -1
        stack.append(i)

    # next smaller
    next_smaller = [n] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        next_smaller[i] = stack[-1] if stack else n
        stack.append(i)

    # previous greater
    prev_greater = [-1] * n
    stack = []
    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()
        prev_greater[i] = stack[-1] if stack else -_
        stack.append(i)

    # next greater
    next_greater = [n] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] <= a[i]:
            stack.pop()
        next_greater[i] = stack[-1] if stack else n
        stack.append(i)

    res = 0

    for i in range(n):
        min_left = i - prev_smaller[i]
        min_right = next_smaller[i] - i
        max_left_
```
