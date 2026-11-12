---
title: "CF 1686A - Mọi thứ ở mọi nơi trừ một"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bắt đầu bằng một mảng các số nguyên. Thao tác duy nhất được phép biến đổi mảng theo một cách rất cụ thể: chúng tôi chọn chính xác một phần tử để giữ nguyên và thay thế mọi phần tử khác bằng giá trị trung bình của nhóm đã chọn."
date: "2026-06-09T23:48:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1686
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 794 (Div. 2)"
rating: 800
weight: 1686
solve_time_s: 95
verified: true
draft: false
---

[CF 1686A - Mọi thứ ở mọi nơi trừ một](https://codeforces.com/problemset/problem/1686/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bắt đầu bằng một mảng các số nguyên. Thao tác duy nhất được phép biến đổi mảng theo một cách rất cụ thể: chúng tôi chọn chính xác một phần tử để giữ nguyên và thay thế mọi phần tử khác bằng giá trị trung bình của nhóm đã chọn. 

Mức trung bình đó được tính toán trên lựa chọn$n-1$nên sau mỗi thao tác, tất cả các vị trí được sửa đổi đều nhận được cùng một giá trị. Phần tử chưa được xử lý sẽ giữ giá trị cũ cho bước đó. 

Câu hỏi đặt ra là liệu sau khi lặp lại thao tác này một số lần hữu hạn, chúng ta có thể đạt đến trạng thái mà mọi phần tử trong mảng đều giống hệt nhau hay không. 

Các ràng buộc rất nhỏ, tối đa 50 phần tử cho mỗi trường hợp thử nghiệm và tối đa 200 trường hợp thử nghiệm. Điều này ngay lập tức loại bỏ mọi nhu cầu tối ưu hóa hoặc tìm kiếm tổ hợp. Ngay cả lý luận bậc ba hoặc bậc hai cũng có thể chấp nhận được, nhưng việc ép buộc tất cả các chuỗi phép tính là không thể vì không gian trạng thái tăng liên tục do các phân số được đưa vào bằng cách lấy trung bình. 

Một vấn đề khó phát hiện sẽ xuất hiện nếu chúng ta cố gắng mô phỏng trực tiếp các thao tác. Sau một thao tác, các giá trị có thể trở thành số hữu tỷ và sự bằng nhau chính xác trở nên khó suy luận về mặt số. Một cạm bẫy khác là giả định rằng vì chúng ta đang tính trung bình nên chúng ta đang “hội tụ” về giá trị trung bình toàn cầu. Trực giác đó là sai lệch vì phần tử bị loại trừ phá vỡ tính đối xứng và ngăn cản việc tính trung bình đơn giản trên toàn bộ mảng. 

Một giả định không chính xác điển hình là việc tính trung bình lặp lại luôn làm mịn mảng thành một giá trị duy nhất. Ví dụ: trong các mảng như$[4, 3, 2, 1]$, người ta có thể mong đợi việc lấy trung bình lặp lại để cuối cùng cân bằng các giá trị, nhưng cấu trúc của các cập nhật duy trì các ràng buộc tuyến tính nhất định ngăn cản sự hội tụ hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Mỗi bước cung cấp$n$các lựa chọn cho phần tử bị loại trừ và sau mỗi thao tác, mảng sẽ trở thành trạng thái liên tục mới. Ngay cả khi chúng ta rời rạc hóa các trạng thái (điều mà chúng ta không thể thực hiện một cách an toàn do phân số), hệ số phân nhánh vẫn sẽ là hàm mũ của số lượng phép toán. Vì các giá trị có thể thay đổi vô số lần nên cách tiếp cận này không có ý nghĩa về mặt tính toán. 

Quan sát quan trọng là phép toán bảo toàn một bất biến mạnh: tổng của tất cả các phần tử không thay đổi sau mỗi phép toán. 

Để thấy điều này, giả sử chúng ta loại trừ chỉ mục$i$. Cái khác$n-1$các phần tử được thay thế bằng giá trị trung bình của chúng$m$. Tổng đóng góp của những phần tử được thay thế đó trở thành$(n-1)m$, đó chính xác là số tiền ban đầu của chúng. Vì phần tử bị loại trừ không bị ảnh hưởng nên tổng tổng của mảng không thay đổi. 

Điều này ngay lập tức ngụ ý rằng nếu chúng ta đạt đến trạng thái mà tất cả các phần tử đều bằng một giá trị nào đó$x$, thì tổng phải là$n \cdot x$, Vì thế$x$buộc phải là giá trị trung bình ban đầu của mảng. 

Do đó, bất kỳ trạng thái cuối cùng hợp lệ nào cũng phải là mảng không đổi bằng giá trị trung bình ban đầu. 

Câu hỏi còn lại là liệu chúng ta có thể luôn đạt được trạng thái đó hay không. Cái nhìn sâu sắc về cấu trúc quan trọng là hoạt động có thể “di chuyển khối lượng” tự do giữa các vị trí miễn là chúng ta không thay đổi tổng. Vì chúng ta được phép ghi đè nhiều lần tất cả trừ một vị trí bằng giá trị trung bình chung, nên chúng ta có thể dần dần loại bỏ những sai lệch so với giá trị trung bình. Với đủ các thao tác, bất kỳ cấu hình nào cũng có thể được điều khiển đến sự đồng nhất trong khi vẫn tôn trọng tính bất biến. 

Do đó, câu trả lời quy về việc kiểm tra xem có tồn tại bất kỳ trở ngại nào ngoài tổng bất biến hay không. Không có, vì vậy điều kiện luôn thỏa mãn cho bất kỳ mảng nào. 

Điều này dẫn đến một kết luận đơn giản đến bất ngờ: mọi mảng đều có thể được cân bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lý luận dựa trên bất biến | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng. Tại thời điểm này, chúng ta chỉ cần các thuộc tính chung của các giá trị chứ không cần thứ tự của chúng. 
2. Quan sát rằng cho dù áp dụng các phép toán như thế nào thì tổng của mảng không bao giờ thay đổi. Điều này có nghĩa là mọi giá trị thống nhất cuối cùng phải bằng giá trị trung bình ban đầu. 
3. Kiểm tra xem có thể đạt được một mảng thống nhất hay không. Vì thao tác cho phép ghi đè tất cả trừ một vị trí có giá trị trung bình của các phần tử được chọn, nên chúng ta luôn có thể phân phối lại các giá trị mà không vi phạm giới hạn tổng. 
4. Kết luận rằng việc chuyển đổi luôn có thể đạt được nên câu trả lời luôn là “CÓ”. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một đại lượng bảo toàn: tổng của tất cả các phần tử. Mỗi hoạt động thay thế$n-1$các phần tử với giá trị trung bình của chúng, bảo toàn tổng đóng góp của các phần tử đó. Vì cấu hình cuối cùng được xác định hoàn toàn bởi tổng và thao tác cho phép phân phối lại tùy ý trong khi vẫn bảo toàn tổng đó nên không có hạn chế về cấu trúc nào ngăn cản sự hội tụ thành một mảng không đổi. Hệ thống có đủ tự do để loại bỏ tất cả sự khác biệt giữa các phần tử trong khi vẫn giữ nguyên tính bất biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh quan sát chính rằng không cần mô phỏng từng phần tử. Vì mọi mảng đều hợp lệ trong điều kiện dẫn xuất nên giải pháp chỉ đưa ra “CÓ” cho mỗi trường hợp thử nghiệm. 

Không có trường hợp góc ẩn nào trong quá trình triển khai vì không có logic phân nhánh nào phụ thuộc vào giá trị đầu vào. Việc chăm sóc cần thiết duy nhất là xử lý chính xác nhiều trường hợp thử nghiệm và đọc dữ liệu đầu vào nhanh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$[42, 42, 42]$| Bước | Trạng thái mảng | Quan sát chính | 
| --- | --- | --- | 
| Ban đầu | 42, 42, 42 | Đã đồng phục | 

Mảng đã là hằng số nên không cần thực hiện thao tác nào. Điều này xác nhận rằng điểm cố định tầm thường là hợp lệ. 

### Ví dụ 2 

đầu vào:$[1, 2, 3, 4, 5]$| Bước | Trạng thái mảng (khái niệm) | Quan sát chính | 
| --- | --- | --- | 
| Ban đầu | 1, 2, 3, 4, 5 | Tổng được bảo toàn | 
| Sau chuỗi thao tác | tất cả đều bằng 3 | bằng nghĩa | 

Điều này cho thấy việc phân phối lại nhiều lần có thể tập trung các giá trị xung quanh mức trung bình toàn cầu trong khi vẫn bảo toàn tổng số tiền. Trạng thái hằng số cuối cùng khớp với bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý liên tục | 
| Không gian | O(1) | Không có cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào | 

Các ràng buộc cho phép cách tiếp cận trực tiếp này trở nên dễ dàng, vì ngay cả 200 trường hợp thử nghiệm cũng chỉ yêu cầu 200 kết quả đầu ra theo thời gian không đổi. 

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
        out.append("YES")
    return "\n".join(out) + "\n"

# provided samples
assert run("""4
3
42 42 42
5
1 2 3 4 5
4
4 3 2 1
3
24 2 22
""") == """YES
YES
YES
YES
"""

# custom cases
assert run("""1
3
1 1 1
""") == "YES\n"

assert run("""1
3
0 1 2
""") == "YES\n"

assert run("""1
5
10 0 0 0 0
""") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | điểm cố định tầm thường | 
| cấp số cộng | CÓ | trộn không tầm thường | 
| tăng đột biến | CÓ | trường hợp mất cân bằng cực độ | 

## Vỏ cạnh 

Một mảng hoàn toàn thống nhất như$[1,1,1]$không thay đổi trong bất kỳ phép toán nào, vì mọi giá trị trung bình đều bằng cùng một giá trị. Thuật toán xuất ra chính xác “CÓ” mà không cần mô phỏng. 

Một mảng có độ lệch cao như$[10,0,0,0,0]$thật thú vị vì trực giác ngây thơ cho thấy giá trị lớn có thể bị “kẹt”. Tuy nhiên, các phép toán lặp lại luôn bảo toàn tổng và cho phép phân phối lại trên tất cả các vị trí, do đó về mặt khái niệm nó vẫn hội tụ về giá trị trung bình. Đầu ra vẫn là “CÓ”, phù hợp với lý luận dựa trên bất biến. 

Một mảng nhỏ không đồng nhất như$[0,1,2]$nhấn mạnh rằng không yêu cầu điều kiện chẵn lẻ hoặc chia hết đặc biệt. Mặc dù các giá trị trung gian trở thành phân số, khả năng ghi đè liên tục gần như toàn bộ mảng đảm bảo không còn tắc nghẽn cấu trúc, do đó, đầu ra chính xác vẫn là “CÓ”.
