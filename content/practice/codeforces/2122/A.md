---
title: "CF 2122A - Lưới tham lam"
description: "Chúng tôi đang làm việc trên một lưới trong đó mỗi ô chứa một số không âm và chúng tôi chỉ được phép di chuyển sang phải hoặc xuống bắt đầu từ góc trên cùng bên trái. Bất kỳ đường dẫn nào như vậy từ trên cùng bên trái đến dưới cùng bên phải đều có tổng điểm bằng tổng của tất cả các ô đã truy cập."
date: "2026-06-08T03:41:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2122
codeforces_index: "A"
codeforces_contest_name: "Order Capital Round 1 (Codeforces Round 1038, Div. 1 + Div. 2)"
rating: 800
weight: 2122
solve_time_s: 69
verified: true
draft: false
---

[CF 2122A - Lưới tham lam](https://codeforces.com/problemset/problem/2122/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới trong đó mỗi ô chứa một số không âm và chúng tôi chỉ được phép di chuyển sang phải hoặc xuống bắt đầu từ góc trên cùng bên trái. Bất kỳ đường dẫn nào như vậy từ trên cùng bên trái đến dưới cùng bên phải đều có tổng điểm bằng tổng của tất cả các ô đã truy cập. 

Trong số tất cả các đường dẫn từ phải xuống có thể, một số đường đạt được tổng tối đa có thể. Một cách độc lập, có một cách xác định để tạo đường dẫn được gọi là đường dẫn tham lam: ở mỗi bước, bạn nhìn vào ô bên phải và ô bên dưới, sau đó bạn di chuyển đến ô có giá trị lớn hơn hoặc chọn nếu chúng bằng nhau. 

Câu hỏi đặt ra là liệu có thể xây dựng một lưới n x m sao cho mọi đường đi tham lam đều dưới mức tối ưu hoàn toàn hay không, nghĩa là không có đường đi tham lam nào đạt được tổng đường đi tối đa có thể. 

Đầu vào bao gồm nhiều kích thước lưới độc lập. Với mỗi cặp n, m, chúng ta phải quyết định liệu một cấu trúc như vậy có tồn tại hay không. 

Các ràng buộc đủ nhỏ để mỗi bài kiểm tra có thể được trả lời theo lý luận về thời gian không đổi cho mỗi trường hợp. Với tối đa 5000 trường hợp thử nghiệm và n, m lên tới 100, bất kỳ giải pháp nào cố gắng xây dựng hoặc mô phỏng lưới một cách rõ ràng đều không cần thiết. Cấu trúc của bài toán gợi ý một điều kiện tổ hợp thuần túy chỉ phụ thuộc vào hình dạng của lưới. 

Trường hợp cạnh tinh tế là khi một trong các kích thước là 1. Trong lưới 1 x m hoặc n x 1, chỉ có một đường dẫn khả thi và nó đồng thời là tham lam và tối ưu. Vì vậy, bất kỳ công trình nào cố gắng tách đường tham lam ra khỏi đường dẫn tối ưu đều không thể thực hiện được. 

Một trường hợp cạnh quan trọng khác là khi cả hai chiều ít nhất là 2. Thật hấp dẫn khi cho rằng hành vi tham lam luôn gần với mức tối ưu, nhưng các lưới nhỏ như 2 x 2 đã cho phép các lựa chọn xung đột giữa mức tối ưu cục bộ và toàn cục. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ sẽ là thử tất cả các lưới có thể có kích thước hợp lý, liệt kê tất cả các đường dẫn bên phải, tính tổng đường dẫn tối đa và sau đó mô phỏng quy tắc đường dẫn tham lam để xem liệu nó có khớp với mức tối ưu hay không. Điều này ngay lập tức trở nên không khả thi vì ngay cả đối với lưới 5 x 5 cũng có nhiều đường dẫn nhị thức và số lượng lưới có thể là vô hạn trên các số nguyên không âm. 

Cái nhìn sâu sắc quan trọng là câu hỏi không phụ thuộc vào giá trị thực tế mà chỉ phụ thuộc vào việc liệu các quyết định tham lam của địa phương có thể gây hiểu lầm trên toàn cầu hay không. Điều này làm giảm vấn đề thành lý luận về việc liệu hình dạng lưới có tạo ra một đường dẫn duy nhất hay cho phép xung đột giữa hai hướng khi bắt đầu hay không. 

Nếu một trong hai chiều là 1 thì đường dẫn là duy nhất, do đó, mức độ tham lam là tối ưu một cách tầm thường. Nếu cả hai chiều đều ít nhất là 2, chúng ta có thể xây dựng một phản ví dụ đơn giản trong đó đường đi tối ưu ban đầu yêu cầu di chuyển theo hướng có vẻ tệ hơn cục bộ, trong khi quy tắc tham lam buộc phải chọn hàng xóm tốt hơn cục bộ và bị mắc kẹt vào một hành lang dưới mức tối ưu. Cấu trúc mẫu đã thể hiện hành vi này cho lưới 3 x 3 và ý tưởng tương tự sẽ mở rộng cho bất kỳ lưới lớn hơn nào bằng cách nhúng cấu trúc như vậy. 

Kết luận là câu trả lời chỉ phụ thuộc vào việc cả hai chiều có vượt quá 1 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Logic thời gian không đổi | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đọc cặp n và m để biết kích thước lưới. Chúng xác định hình dạng của tất cả các đường dẫn hợp lệ. 
2. Kiểm tra xem n bằng 1 hay m bằng 1. Điều này xác định một lưới suy biến trong đó chuyển động bị ép dọc theo một đường. 
3. Nếu một chiều là 1, xuất NO vì có chính xác một đường dẫn, do đó cần phải trùng khớp và tối ưu. 
4. Ngược lại, cả n và m ít nhất là 2. Trong trường hợp này, xuất CÓ vì chúng ta có thể xây dựng một lưới trong đó các quyết định tham lam ngay từ đầu buộc một quỹ đạo dưới mức tối ưu. 

Lý do đằng sau bước 4 là việc có ít nhất một cấu trúc con 2 x 2 là đủ để tạo ra xung đột cục bộ và toàn cầu. Quy tắc tham lam chỉ nhìn về phía trước một bước, trong khi đường đi tối ưu có thể phụ thuộc vào cấu trúc tầm xa hơn, do đó, một công trình tồn tại sẽ đánh lừa sự lựa chọn tham lam. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên sự phân đôi cấu trúc của biểu đồ lưới. Khi một trong hai chiều là 1, biểu đồ lưới sẽ thoái hóa thành biểu đồ đường dẫn với chính xác một tuyến đường từ nguồn đến đích, loại bỏ mọi lựa chọn và làm cho tham lam trở nên tối ưu một cách tối ưu. Khi cả hai chiều ít nhất là 2, lưới chứa một điểm nối có sẵn hai bước di chuyển đầu tiên khác nhau và có thể chỉ định các giá trị sao cho lân cận cục bộ tốt hơn dẫn đến một khu vực có tổng tổng kém hơn so với nhánh thay thế. Sự tách biệt giữa so sánh địa phương và tích lũy toàn cầu này đủ để đảm bảo rằng sự tham lam không phải lúc nào cũng đạt được sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n == 1 or m == 1:
        print("NO")
    else:
        print("YES")
```Giải pháp giảm toàn bộ vấn đề xuống còn một lần kiểm tra cấu trúc cho mỗi trường hợp thử nghiệm. Chi tiết triển khai chính là không cần mô phỏng hoặc xây dựng. Điều kiện này hoàn toàn dựa trên việc có thể phân nhánh trong biểu đồ lưới hay không. 

Sự tinh tế duy nhất là đảm bảo xử lý chính xác các trường hợp 1 chiều. Cả hai lưới 1 x m và n x 1 phải được xử lý đối xứng, vì trong cả hai trường hợp đều có chính xác một đường đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
```| Bước | n | m | Quyết định | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | CÓ | cả hai chiều đều vượt quá 1 | 

Trường hợp này xác nhận rằng khi một lưới có ít nhất một điểm lựa chọn bên trong, chúng ta có thể thiết kế các giá trị sao cho tham lam bị buộc vào một hành lang dưới mức tối ưu. 

### Ví dụ 2 

đầu vào:```
1 5
```| Bước | n | m | Quyết định | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | KHÔNG | chỉ có một con đường tồn tại | 

Trường hợp này cho thấy khi chuyển động tuyến tính, tham lam không thể đi chệch khỏi con đường tối ưu duy nhất nên việc tách rời là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | mỗi trường hợp thử nghiệm là một so sánh theo thời gian không đổi | 
| Không gian | O(1) | không sử dụng công trình phụ trợ | 

Giải pháp này sẽ thay đổi quy mô trực tiếp theo số lượng trường hợp thử nghiệm và thậm chí ở mức 5000 thử nghiệm, nó sẽ chạy ngay lập tức vì mỗi quyết định là một lần kiểm tra có điều kiện duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append("YES" if n > 1 and m > 1 else "NO")
    return "\n".join(out) + "\n"

# provided samples
assert run("""2
3 3
1 2
""") == """YES
NO
"""

# custom cases
assert run("""4
1 1
1 10
10 1
2 2
""") == """NO
NO
NO
YES
"""

assert run("""3
2 3
3 2
4 4
""") == """YES
YES
YES
"""

assert run("""1
100 100
""") == """YES
"""

assert run("""1
1 100
""") == """NO
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1, 1xN, Nx1, 2x2 | KHÔNG KHÔNG KHÔNG CÓ | tất cả các thoái hóa biên | 
| hỗn hợp 2x3, 3x2, 4x4 | CÓ CÓ CÓ | công trình xây dựng hợp lệ chung | 
| 100x100 | CÓ | hành vi kích thước lớn | 
| 1x100 | KHÔNG | trường hợp cạnh một hàng | 

## Vỏ cạnh 

Lưới 1 x 1 là trường hợp suy biến nhất. Thuật toán đọc n bằng 1 hoặc m bằng 1 và ngay lập tức xuất ra NO. Có chính xác một đường dẫn bao gồm một ô duy nhất, vì vậy tham lam và tối ưu là giống hệt nhau. 

Đối với lưới 1 x 5, việc kiểm tra sẽ kích hoạt điều kiện tương tự. Các trường hợp kiểm thử lặp lại thấy m bằng 5 và n bằng 1, do đó nó tạo ra NO mà không cần logic thêm. Điều này phù hợp với thực tế là không có điểm phân nhánh nào mà kẻ tham lam có thể đưa ra lựa chọn sai lầm. 

Đối với lưới 2 x 2, cả n và m đều vượt quá 1, do đó thuật toán đưa ra CÓ. Điều này tương ứng với sự tồn tại của một quyết định cục bộ ngay từ đầu và có thể bị lợi dụng bởi một mạng lưới được xây dựng để đánh lừa sự lựa chọn tham lam khỏi con đường tối ưu toàn cục.
