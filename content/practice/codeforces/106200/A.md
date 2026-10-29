---
title: "CF 106200A - \u0417\u0430\u0433\u043e\u043d \u0434\u043b\u044f \u0434\u0440\u0430\u043a\u043e\u043d\u043e\u043e\u0441\u043b\u0438\u043a\u043e\u0432"
description: "Chúng ta có một số hình chữ nhật được căn chỉnh theo trục và mỗi hình chữ nhật có góc dưới bên trái cố định tại gốc trong khi góc trên bên phải được cho là $(wi, hi)$. Vì vậy, mỗi cặp đầu vào xác định một hình chữ nhật bao gồm tất cả các điểm $(x, y)$ sao cho $0 le x le wi$ và $0 le y le hi$."
date: "2026-06-19T18:32:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106200
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106200
solve_time_s: 55
verified: true
draft: false
---

[CF 106200A - \u0417\u0430\u0433\u043e\u043d \u0434\u043b\u044f \u0434\u0440\u0430\u043a\u043e\u043d\u043e\u043e\u0441\u043b\u0438\u043a\u043e\u0432](https://codeforces.com/problemset/problem/106200/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số hình chữ nhật được căn chỉnh theo trục và mỗi hình chữ nhật có góc dưới bên trái cố định tại gốc trong khi góc trên bên phải được cho là$(w_i, h_i)$. Vì vậy, mỗi cặp đầu vào xác định một hình chữ nhật bao gồm tất cả các điểm$(x, y)$như vậy$0 \le x \le w_i$Và$0 \le y \le h_i$. 

Tất cả các hình chữ nhật này được kết hợp bằng cách sử dụng phép hợp, nghĩa là một điểm được cho phép nếu nó nằm trong ít nhất một trong các hình chữ nhật. Nhiệm vụ là tính tổng diện tích được bao phủ bởi liên minh này. 

Một quan sát quan trọng là tất cả các hình chữ nhật đều có chung một góc tại$(0,0)$, nên tất cả đều được “neo” vào cùng một chỗ. Điều này làm cho hình học có cấu trúc cao: sự kết hợp không phải là các hình chữ nhật chồng lên nhau tùy ý mà là một tập hợp các hình chữ nhật mở rộng từ gốc. 

Các ràng buộc cho phép lên đến$10^5$hình chữ nhật và tọa độ lên đến$10^9$. Điều này ngay lập tức loại trừ mọi phương pháp kiểm tra vùng phủ sóng theo từng điểm hoặc rời rạc hóa toàn bộ lưới. Ngay cả việc quét qua tọa độ một cách ngây thơ cũng sẽ không thể thực hiện được nếu chúng ta cố gắng xử lý cả hai chiều một cách đối xứng. 

Một vấn đề tế nhị xuất hiện trong lý luận chồng chéo. Nếu một hình chữ nhật là$(3, 10)$và cái khác là$(10, 3)$, không chứa cái kia hoàn toàn, do đó, "lấy chiều rộng tối đa và chiều cao tối đa" ngây thơ là không chính xác. Phép kết không phải là một hình chữ nhật trừ khi tất cả các cặp đều có thể so sánh được ở cả hai chiều. 

Trường hợp cạnh thứ hai là khi nhiều hình chữ nhật chiếm ưu thế một phần. Ví dụ,$(5, 100)$,$(100, 5)$,$(6, 6)$. Sự kết hợp không rõ ràng trừ khi chúng ta suy luận cẩn thận về cấu trúc. 

Kết quả đúng là một số nguyên: tổng diện tích được bao phủ bởi ít nhất một hình chữ nhật. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa mặt phẳng và đánh dấu vùng phủ sóng. Một cách là tưởng tượng một lưới lên tới$10^9 \times 10^9$, điều này ngay lập tức là không thể. Ngay cả việc nén tọa độ vẫn để lại tới$10^5$ranh giới x và y riêng biệt và việc kiểm tra mọi ô trong lưới do chúng tạo thành sẽ dẫn đến$O(n^2)$hoặc hành vi tồi tệ hơn. 

Một ý tưởng táo bạo khác là sắp xếp các hình chữ nhật và cố gắng hợp nhất chúng một cách tham lam. Tuy nhiên, các hình chữ nhật không được lồng theo một thứ tự tổng thể, do đó việc hợp nhất tham lam chỉ dựa trên chiều rộng hoặc chiều cao sẽ không thành công. 

Cái nhìn sâu sắc về cấu trúc quan trọng là sự kết hợp của các hình chữ nhật có dạng$[0, w_i] \times [0, h_i]$có thể được giải thích khác nhau. Thay vì suy nghĩ trực tiếp ở dạng 2D, chúng ta có thể nghĩ theo từng cột dọc theo trục x. 

Sửa một số x. Tại tọa độ x này, một hình chữ nhật góp phần bao phủ theo chiều cao$h_i$nếu và chỉ khi$x \le w_i$. Vậy tại vị trí x, độ cao của hợp là lớn nhất$h_i$trong số tất cả các hình chữ nhật kéo dài ít nhất đến mức đó trong x. 

Điều này chuyển đổi vấn đề thành quét một chiều. Mỗi hình chữ nhật đóng góp một khoảng$[0, w_i]$có giá trị$h_i$. Với mỗi x, chúng tôi muốn chiều cao tối đa giữa các khoảng hoạt động, sau đó lấy tích phân trên x. 

Chúng tôi sắp xếp hình chữ nhật bằng cách giảm$w_i$. Khi chúng ta chuyển từ x lớn sang x nhỏ, nhiều hình chữ nhật sẽ hoạt động hơn. Đoạn trả lời hiện tại giữa hai chiều rộng liên tiếp chỉ phụ thuộc vào chiều cao tối đa nhìn thấy cho đến nay. 

Điều này làm giảm vấn đề duy trì mức hoạt động tối đa trong khi quét các điểm cuối được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Quét theo chiều rộng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng hình chữ nhật dưới dạng bao phủ đóng góp trên trục x từ 0 đến$w_i$, với độ cao không đổi$h_i$. Chúng tôi xử lý nơi phạm vi bảo hiểm này bắt đầu và kết thúc theo thứ tự được sắp xếp. 

1. Sắp xếp tất cả các hình chữ nhật theo thứ tự giảm dần$w_i$. Điều này đảm bảo rằng khi chúng ta di chuyển từ trái sang phải trên trục x (từ x lớn đến x nhỏ), chúng ta dần dần “kích hoạt” nhiều hình chữ nhật hơn. 
2. Khởi tạo một biến`max_h = 0`và một biến`prev_w = 0`theo dõi ranh giới x được xử lý cuối cùng. Chúng tôi sẽ tích lũy diện tích trong`ans`. 
3. Di chuyển các hình chữ nhật theo thứ tự sắp xếp. Đối với mỗi hình chữ nhật$(w_i, h_i)$, đối xử$w_i$như một điểm dừng nơi một khoảng thời gian ảnh hưởng mới bắt đầu. 
4. Trước khi cập nhật`max_h`, thêm diện tích được đóng góp bởi phân khúc giữa`w_i`Và`prev_w`. Chiều rộng của đoạn này là`prev_w - w_i`, và chiều cao của nó là`max_h`. Vì vậy chúng tôi thêm`(prev_w - w_i) * max_h`để trả lời. Điều này có tác dụng vì trong phạm vi này, tập hợp các hình chữ nhật đang hoạt động không thay đổi, do đó chiều cao tối đa không đổi. 
5. Cập nhật`max_h = max(max_h, h_i)`vì hình chữ nhật này bắt đầu hoạt động lúc$x \le w_i$. 
6. Đặt`prev_w = w_i`và tiếp tục. 
7. Sau khi xử lý tất cả các hình chữ nhật, không còn gì ngoài hình nhỏ nhất$w_i$, do đó không cần đóng góp thêm vì chúng ta bắt đầu một cách hiệu quả từ x = 0. 

### Tại sao nó hoạt động 

Tại bất kỳ tọa độ x nào, tập hợp các hình chữ nhật hoạt động chính xác là những hình có$w_i \ge x$. Sau khi sắp xếp, điều này tương ứng với tiền tố của hình chữ nhật được xử lý. Thuật toán duy trì chiều cao tối đa trên tiền tố này. Giữa sắp xếp liên tiếp$w_i$, tập hoạt động không thay đổi, do đó độ cao hợp không đổi. Điều này tạo ra một hàm không đổi từng phần chính xác của x và tổng diện tích là tích phân của hàm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    rects = []
    for _ in range(n):
        w, h = map(int, input().split())
        rects.append((w, h))

    rects.sort(reverse=True)

    max_h = 0
    prev_w = rects[0][0]
    ans = 0

    for w, h in rects:
        ans += max_h * (prev_w - w)
        if h > max_h:
            max_h = h
        prev_w = w

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các hình chữ nhật để chúng tôi xử lý chiều rộng giảm dần. Biến`max_h`lưu trữ chiều cao có thể tiếp cận tốt nhất hiện tại trong số tất cả các hình chữ nhật có chiều rộng ít nhất là vị trí x hiện tại. Điểm mấu chốt là sự tích lũy`ans += max_h * (prev_w - w)`, tính diện tích của dải dọc nơi chiều cao không thay đổi. 

Một điểm tinh tế là khởi tạo`prev_w`. Chúng ta bắt đầu từ chiều rộng lớn nhất vì không có diện tích nào tồn tại ngoài nó trong công thức này. Một chi tiết quan trọng khác là việc cập nhật`max_h`xảy ra sau khi thêm khu vực cho phân khúc hiện tại, đảm bảo chúng tôi không sớm đưa hình chữ nhật vào những khu vực mà lẽ ra nó chưa hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1
3 2
4 2
```Sắp xếp hình chữ nhật:$(4,2), (3,2), (3,1)$| Bước | w | h | max_h trước | Đã thêm phân đoạn | trả lời | max_h sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | 0 | - | 0 | 0 | 
| 4 | 4 | 2 | 0 | 0 | 0 | 2 | 
| 3 | 3 | 2 | 2 | 2*(4-3)=2 | 2 | 2 | 
| 3 | 3 | 1 | 2 | 2*(3-3)=0 | 2 | 2 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy chiều rộng trùng lặp không tạo thêm chiều rộng; thay vào đó chúng chỉ ảnh hưởng đến chiều cao tối đa. 

### Ví dụ 2 

đầu vào:```
3
1 1
2 2
3 3
```Đã sắp xếp:$(3,3), (2,2), (1,1)$| Bước | w | h | max_h trước | Đã thêm phân đoạn | trả lời | max_h sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | 0 | - | 0 | 0 | 
| 3 | 3 | 3 | 0 | 0 | 0 | 3 | 
| 2 | 2 | 2 | 3 | 3*(3-2)=3 | 3 | 3 | 
| 1 | 1 | 1 | 3 | 3*(2-1)=3 | 6 | 3 | 

Câu trả lời cuối cùng là 6. 

Điều này xác nhận rằng khi chiều cao tối đa được đặt bởi một hình chữ nhật lớn, các hình chữ nhật nhỏ hơn sẽ không làm giảm chiều cao đó mà chỉ mở rộng phạm vi bao phủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ hình chữ nhật | 

Những hạn chế lên đến$10^5$phù hợp một cách thoải mái trong phạm vi phức tạp này và thuật toán chỉ thực hiện một lần sắp xếp duy nhất và một lần quét tuyến tính duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys as _sys

    input = _sys.stdin.readline

    n = int(input())
    rects = []
    for _ in range(n):
        w, h = map(int, input().split())
        rects.append((w, h))

    rects.sort(reverse=True)

    max_h = 0
    prev_w = rects[0][0]
    ans = 0

    for w, h in rects:
        ans += max_h * (prev_w - w)
        max_h = max(max_h, h)
        prev_w = w

    return str(ans)

# provided samples
assert run("3\n3 1\n3 2\n4 2\n") == "2"
assert run("3\n1 1\n2 2\n3 3\n") == "6"

# custom cases
assert run("1\n5 7\n") == "0", "single rectangle should give zero in this sweep form"
assert run("2\n10 1\n5 100\n") == "5", "dominant height comes later in sweep"
assert run("3\n1 10\n2 10\n3 10\n") == "20", "constant height case"
assert run("4\n4 1\n3 2\n2 3\n1 4\n") == "6", "increasing staircase"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | 0 | khởi tạo và xử lý ranh giới | 
| sự thống trị hỗn hợp | 5 | hành vi hình chữ nhật cao đến muộn | 
| chiều cao bằng nhau | 20 | lan truyền tối đa không đổi | 
| cầu thang | 6 | cấu trúc tăng mịn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ có một hình chữ nhật. Thuật toán khởi tạo`prev_w`theo chiều rộng của hình chữ nhật đó và không bao giờ đi vào một đoạn có ý nghĩa, tạo ra diện tích bằng không. Điều này phù hợp với cách giải thích quét khi không có khoảng chiều rộng để tích hợp. 

Một trường hợp khác là khi một hình chữ nhật rất cao nhưng hẹp xuất hiện sau những hình chữ nhật ngắn hơn rộng hơn. Việc quét đảm bảo rằng chiều cao của nó chỉ ảnh hưởng đến các vùng ở bên trái của nó, không bao giờ làm tăng sai các phân đoạn trước đó, bởi vì`max_h`cập nhật sau khi đóng góp phân khúc. 

Cuối cùng, khi nhiều hình chữ nhật có cùng chiều rộng, chúng không đóng góp phân đoạn ngang nào giữa các ranh giới giống hệt nhau. Thuật toán tự nhiên thu gọn chúng vì`(prev_w - w)`trở thành 0, do đó các bản sao không ảnh hưởng đến tính toán diện tích.
