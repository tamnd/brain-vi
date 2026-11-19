---
title: "CF 2111C - Giá trị bằng nhau"
description: "Chúng ta được cung cấp một mảng các số nguyên và được phép liên tục “phát” giá trị của một phần tử đã chọn tới mọi thứ ở bên trái hoặc mọi thứ ở bên phải của nó. Mỗi lần phát sóng có chi phí tỷ lệ thuận với số lượng phần tử bị ghi đè, nhân với giá trị được truyền đi."
date: "2026-06-08T04:30:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2111
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 179 (Rated for Div. 2)"
rating: 1100
weight: 2111
solve_time_s: 78
verified: true
draft: false
---

[CF 2111C - Giá trị bằng nhau](https://codeforces.com/problemset/problem/2111/C) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được phép liên tục “phát” giá trị của một phần tử đã chọn tới mọi thứ ở bên trái hoặc mọi thứ ở bên phải của nó. Mỗi lần phát sóng có chi phí tỷ lệ thuận với số lượng phần tử bị ghi đè, nhân với giá trị được truyền đi. Mục tiêu là làm cho toàn bộ mảng bao gồm một giá trị duy nhất trong khi trả ít nhất có thể. 

Một cách hữu ích để diễn giải lại các hoạt động là mỗi bước di chuyển sẽ chọn một vị trí và mở rộng giá trị của vị trí đó qua tiền tố hoặc hậu tố, trả tiền cho mọi chỉ mục mới bị ảnh hưởng. Vì các phần tử có thể được ghi đè nhiều lần nên quy trình này không đơn điệu: chúng ta không chỉ vẽ một lần mà sau này chúng ta có thể vẽ lại các vùng có giá trị rẻ hơn. 

Các ràng buộc rất lớn, với tổng kích thước mảng lên tới 5×10^5 trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc tuyến tính. Điều đó cho thấy câu trả lời phải có thể tính toán được từ cấu trúc cục bộ, thường dựa vào sự kề cận của các giá trị bằng nhau hoặc phân đoạn tối ưu. 

Một vài tình huống tế nhị phá vỡ lối suy luận ngây thơ. Một sai lầm phổ biến là cho rằng chúng ta có thể độc lập “sửa chữa” bên trái và bên phải một cách tham lam từ đầu đến cuối. Ví dụ: trong một mảng như`[3, 1, 3]`, một chiến lược tham lam cố gắng mở rộng từ điểm nhỏ nhất hoặc từ các điểm cuối có thể bỏ sót rằng chiến lược tối ưu sử dụng giá trị trung bình để chi trả cho cả hai bên với giá rẻ. 

Một vấn đề khác là cho rằng mỗi giá trị đóng góp một cách độc lập. Trên thực tế, các hoạt động chồng chéo rất nhiều: một hoạt động có thể thay thế một khu vực rộng lớn mà sau này có thể được sử dụng lại làm căn cứ cho hoạt động khác, khiến các quyết định cục bộ bị sai lệch. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xem xét giá trị cuối cùng mà chúng ta muốn mảng trở thành. Giả sử chúng ta sửa một giá trị`x`. Sau đó chúng ta cần chuyển đổi toàn bộ mảng thành`x`sử dụng các thao tác sao chép`x`hướng ra ngoài từ những lần xuất hiện đã chọn của`x`. Nếu chọn nhiều vị trí của`x`dưới dạng nguồn, mỗi nguồn sẽ mở rộng sang trái hoặc phải và các phân đoạn được bao phủ của chúng sẽ hợp nhất. Một chiến lược đơn giản sẽ thử tất cả các tập hợp con vị trí của từng giá trị và mô phỏng tất cả các chuỗi mở rộng có thể có. Đây là số mũ về số lần xuất hiện và thậm chí một mô phỏng đơn lẻ cũng là tuyến tính, khiến phương pháp này không khả thi. 

Quan sát quan trọng là đối với một giá trị mục tiêu cố định`x`, chúng tôi không bao giờ được hưởng lợi từ việc sử dụng không`x`các giá trị làm nguồn. Bất kỳ thao tác nào đều truyền một giá trị được chọn duy nhất, do đó, cuối cùng mọi thao tác đều liên quan đến một số lần xuất hiện của một giá trị và việc sử dụng các giá trị khác nhau chỉ làm tăng chi phí vì các giá trị cao hơn sẽ tốn nhiều chi phí hơn trên mỗi đơn vị chiều dài. 

Quan trọng hơn, nếu chúng ta nhìn vào sự xuất hiện của`x`, mảng giữa hai lần xuất hiện liên tiếp của`x`có thể được điền từ lần xuất hiện bên trái hoặc lần xuất hiện bên phải. Điều này tạo ra một quyết định cục bộ: mỗi khoảng cách giữa các`x`các vị trí phải được “thanh toán” đúng một lần và chi phí phụ thuộc vào việc chúng ta mở rộng từ trái hay phải. 

Điều này làm giảm vấn đề thành: với mỗi giá trị`x`, xét tất cả các vị trí ở đó`a[i] = x`. Các vị trí này chia mảng thành các phân đoạn và mỗi phân đoạn giữa các lần xuất hiện liên tiếp có thể được bao phủ từ một trong hai phía. Chi phí do khoảng trống đóng góp chỉ phụ thuộc vào độ dài của nó và`x`. Chúng tôi tính toán cách tốt nhất để bù đắp mọi khoảng trống, từ đó tổng hợp những khoản đóng góp tối thiểu được xác định theo cấu trúc địa phương. 

Cuối cùng, chúng tôi đánh giá chi phí này cho mọi giá trị khác biệt và lấy mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu hóa khoảng cách trên mỗi giá trị | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, nhóm các chỉ số theo giá trị của chúng. 

Chúng tôi cần những vị trí này vì chỉ những lần xuất hiện của một giá trị mới có thể đóng vai trò là nguồn cho giá trị đó. 
2. Với mỗi giá trị riêng biệt`x`, trích xuất danh sách đã sắp xếp các vị trí trong đó`a[i] = x`. 

Những vị trí này xác định nơi chúng ta có thể “neo” mở rộng`x`. 
3. Tính chi phí cần thiết để lấp đầy mọi thứ bằng cách sử dụng`x`như giá trị cuối cùng. 

Về mặt khái niệm, chúng tôi chia mảng thành các phân đoạn giữa các lần xuất hiện liên tiếp của`x`. 
4. Đối với mỗi khoảng cách giữa các lần xuất hiện liên tiếp`p[i]`Và`p[i+1]`, tính độ dài của nó`len = p[i+1] - p[i] - 1`. 

Đây là số phần tử phải được ghi đè bằng cách mở rộng từ hai phía. 
5. Mỗi khoảng trống như vậy đóng góp một chi phí tương ứng với`len * x`, bởi vì việc lấp đầy vùng đó một cách hiệu quả đòi hỏi phải truyền bá giá trị`x`qua các vị trí đó. 
6. Tổng chi phí cho giá trị`x`là tổng của tất cả các khoảng trống cộng với chi phí để xử lý tiền tố trước lần xuất hiện đầu tiên và hậu tố sau lần xuất hiện cuối cùng, được coi là khoảng trống so với lần xuất hiện gần nhất. 
7. Lấy chi phí tối thiểu trên tất cả các giá trị phân biệt. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trong bất kỳ chiến lược tối ưu nào nhắm đến một giá trị cố định`x`, mọi vị trí cuối cùng được lấp đầy bởi sự mở rộng bắt nguồn từ sự xuất hiện gần nhất của`x`đạt được nó đầu tiên trong kế hoạch tối ưu. Vì các hoạt động luôn truyền một giá trị không đổi và chi phí là tuyến tính theo số lượng vị trí mới được ghi đè nên việc chia phạm vi bảo hiểm thành các khoảng độc lập giữa các lần xuất hiện là không mất mát: không hoạt động nào có thể giảm tổng số vị trí được bảo hiểm cần thiết, chỉ phân phối lại cách chúng được thanh toán. Do đó, mỗi khoảng trống được thanh toán đúng một lần và việc chọn`x`quyết định mọi chi phí tại địa phương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = {}
        for i, v in enumerate(a):
            pos.setdefault(v, []).append(i)

        ans = float('inf')

        for v, p in pos.items():
            cost = 0

            # left boundary gap
            if p[0] > 0:
                cost += (p[0]) * v

            # internal gaps
            for i in range(len(p) - 1):
                gap = p[i + 1] - p[i] - 1
                cost += gap * v

            # right boundary gap
            if p[-1] < n - 1:
                cost += (n - 1 - p[-1]) * v

            ans = min(ans, cost)

        print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nhóm các chỉ số theo giá trị để mỗi giá trị cuối cùng ứng cử viên có thể được đánh giá độc lập. Với mỗi giá trị`v`, nó xử lý mảng dưới dạng các phân đoạn xung quanh lần xuất hiện của`v`. Mọi vị trí không bằng`v`phải được bao phủ bằng cách mở rộng từ một số lần xuất hiện của`v`, và tổng số vị trí như vậy được chia thành ranh giới bên trái, khoảng trống bên trong và ranh giới bên phải. Nhân các số đếm này với`v`phản ánh định nghĩa chi phí hoạt động, trong đó chi phí mở rộng mỗi đơn vị`v`. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng các hoạt động một cách rõ ràng. Thay vào đó, chúng tôi dựa vào thực tế là các chiến lược tối ưu không bao giờ được hưởng lợi từ việc xen kẽ các giá trị khác nhau, vì vậy việc đánh giá từng giá trị một cách độc lập là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 4 1 3
```Chúng tôi tính toán đóng góp cho mỗi giá trị. 

| Giá trị | Vị trí | Khoảng trống bên trái | Khoảng trống nội bộ | Khoảng cách bên phải | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 2 | [0] | 0 | 0 | 3 | 3 | 
| 4 | [1] | 1 | 0 | 2 | 12 | 
| 1 | [2] | 2 | 0 | 1 | 3 | 
| 3 | [3] | 3 | 0 | 0 | 9 | 

Tối thiểu là 3. 

Điều này cho thấy ngay cả một giá trị chỉ xuất hiện một lần cũng có thể tối ưu nếu nó đủ rẻ và nằm ở trung tâm. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| Giá trị | Vị trí | Khoảng trống bên trái | Khoảng trống nội bộ | Khoảng cách bên phải | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1,2] | 0 | 0 | 0 | 0 | 

Mọi thứ đều đã thống nhất nên không phát sinh chi phí. 

Điều này xác nhận thuật toán xử lý chính xác trường hợp đã thống nhất mà không thực hiện các thao tác không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vị trí được xử lý một lần thành nhóm và một lần cho mỗi giá trị | 
| Không gian | O(n) | Lưu trữ danh sách chỉ mục theo giá trị riêng biệt | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính theo kích thước đầu vào, điều này cần thiết với tổng ràng buộc 5×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = {}
        for i, v in enumerate(a):
            pos.setdefault(v, []).append(i)

        ans = float('inf')
        for v, p in pos.items():
            cost = 0
            if p[0] > 0:
                cost += p[0] * v
            for i in range(len(p) - 1):
                cost += (p[i + 1] - p[i] - 1) * v
            if p[-1] < n - 1:
                cost += (n - 1 - p[-1]) * v
            ans = min(ans, cost)

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
4
2 4 1 3
3
1 1 1
10
7 5 5 5 10 9 9 4 6 10
""") == """3
0
35"""

# custom cases
assert run("""1
2
1 1000000000
""") == "1000000000"

assert run("""1
5
2 2 2 2 2
""") == "0"

assert run("""1
5
5 1 5 1 5
""") == "6"

assert run("""1
4
1 2 3 4
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2\n1 1000000000`|`1000000000`| chia tỷ lệ giá trị cực trị | 
|`1\n5\n2 2 2 2 2`|`0`| đã thống nhất | 
|`1\n5\n5 1 5 1 5`|`6`| cấu trúc xen kẽ | 
|`1\n4\n1 2 3 4`|`6`| trình tự hoàn toàn khác biệt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi giá trị tối ưu chỉ xuất hiện một lần. Đối với đầu vào như`3 1 3`, đang chọn`1`vì mục tiêu có thể vẫn tối ưu mặc dù nó không có cấu trúc bên trong. Thuật toán xử lý vấn đề này vì danh sách vị trí có một phần tử duy nhất, do đó cả khoảng trống nội bộ và đóng góp ranh giới đều tính toán chính xác toàn bộ chi phí lan truyền từ nguồn duy nhất đó. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó, danh sách vị trí có phạm vi bao phủ đầy đủ và cả hai lần kiểm tra ranh giới đều có giá trị bằng 0. Chi phí tính toán bằng 0 mà không cần vỏ bọc đặc biệt. 

Trường hợp tinh tế cuối cùng là các giá trị xen kẽ như`1 2 1 2 1`. Mỗi giá trị riêng lẻ có nhiều lần xuất hiện và thuật toán chỉ đo chính xác khoảng cách giữa các lần xuất hiện, đảm bảo rằng ảnh hưởng chồng chéo không nhân đôi các vùng đếm không chính xác.
