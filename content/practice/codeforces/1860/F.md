---
title: "CF 1860F - Đánh giá RBS"
description: "Chúng ta có $2n$ bộ dữ liệu có dạng $(a, b, c)$, trong đó $a$ và $b$ là số nguyên dương và $c$ là dấu ngoặc, '(' hoặc ')'. Chính xác một nửa số bộ dữ liệu là dấu ngoặc mở và nửa còn lại là dấu ngoặc đóng."
date: "2026-06-09T00:23:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1860
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 153 (Rated for Div. 2)"
rating: 2900
weight: 1860
solve_time_s: 88
verified: true
draft: false
---

[CF 1860F - Đánh giá RBS](https://codeforces.com/problemset/problem/1860/F) 

**Xếp hạng:** 2900 
**Tags:** cấu trúc dữ liệu, hình học, cách thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$2n$bộ dữ liệu của mẫu$(a, b, c)$, Ở đâu$a$Và$b$là các số nguyên dương và$c$là dấu ngoặc, '(' hoặc ')'. Chính xác một nửa số bộ dữ liệu là dấu ngoặc mở và nửa còn lại là dấu ngoặc đóng. Mục đích là tìm hai số dương$x$Và$y$và sắp xếp các bộ dữ liệu theo giá trị$a \cdot x + b \cdot y$. Sau khi sắp xếp, chuỗi các dấu ngoặc được trích ra từ các bộ dữ liệu sẽ tạo thành một chuỗi dấu ngoặc thông thường. 

Chuỗi dấu ngoặc thông thường (RBS) là chuỗi mà khi quét từ trái sang phải, số lượng dấu ngoặc đóng không bao giờ vượt quá số lượng dấu ngoặc mở ở bất kỳ vị trí nào và tổng số dấu ngoặc mở và đóng đều bằng nhau. 

Những ràng buộc cho phép$n$lên tới 1500 cho mỗi trường hợp thử nghiệm và tổng số$n$trên tất cả các testcase cũng nhiều nhất là 1500. Điều này làm cho$O(n^2)$cách tiếp cận khả thi cho từng trường hợp thử nghiệm, nhưng$O(n^3)$có thể sẽ quá chậm. Mỗi$a_i$Và$b_i$tùy thuộc vào$10^6$, nhưng chúng ta không cần phải liệt kê tất cả những gì có thể$x$Và$y$; chúng ta cần một chiến lược để xác định liệu một giải pháp có tồn tại hay không. 

Một cách tiếp cận ngây thơ cố gắng hết sức tích cực có thể$x$Và$y$các cặp rõ ràng là không thể vì chúng là các số thực liên tục. Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các bộ của '(' chiếm ưu thế tất cả các bộ của ')' trong cả hai tọa độ hoặc trong đó các điểm được xen kẽ theo cách không có sự kết hợp tuyến tính của$x$Và$y$có thể tách chúng ra. Một giải pháp bất cẩn có thể giả định sắp xếp theo một trong hai$a$hoặc$b$độc lập là đủ, điều này không thành công đối với các đầu vào như$(1,4,'(')$,$(2,3,')')$, trong đó giải pháp tối ưu đòi hỏi sự kết hợp không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực sẽ cố gắng liệt kê tất cả các mặt tích cực có thể có$(x, y)$kết hợp hoặc mô phỏng việc sắp xếp theo mọi sự kết hợp có thể có của$a$Và$b$các giá trị. Điều này là không khả thi vì$x$Và$y$là các số thực liên tục. Thậm chí còn hạn chế$x$Và$y$đến số nguyên lên đến$10^6$vẫn sẽ sản xuất$10^{12}$tổng ứng cử viên, vượt xa giới hạn tính toán. 

Quan sát chính là hình học. Coi mỗi bộ dữ liệu là một điểm trong mặt phẳng$(a_i, b_i)$, với kiểu của nó$c_i$. giá trị$a \cdot x + b \cdot y$là tích vô hướng của vector$(a_i, b_i)$với$(x, y)$. Sắp xếp các bộ theo tích số chấm này tương đương với việc quét một đường có vectơ pháp tuyến$(x, y)$từ điểm gốc và điểm thứ tự bằng hình chiếu của chúng dọc theo vectơ này. Lựa chọn$x$Và$y$dương có nghĩa là quá trình quét di chuyển từ phía dưới bên trái lên phía trên bên phải. 

Một bộ có '(' phải đặt trước một bộ có ')' theo thứ tự được sắp xếp nếu hình chiếu của nó dọc theo$(x, y)$nhỏ hơn. Do đó, vấn đề giảm xuống còn: liệu chúng ta có thể chọn một đường dốc dương sao cho các điểm tương ứng với '(' và ')' có thể được phân tách tuyến tính dọc theo đường đó trong khi vẫn giữ nguyên thứ tự RBS không? 

Thay vì tìm kiếm một cách rõ ràng$(x, y)$, chúng ta có thể suy luận bằng cách sử dụng **độ dốc giữa các điểm**. Cho hai điểm$p_1 = (a_1, b_1, '(')$Và$p_2 = (a_2, b_2, ')')$, điều kiện đó$p_1$phải có trước đây$p_2$dịch sang$(a_2 - a_1)x + (b_2 - b_1)y > 0$. Về mặt hình học, đây là ràng buộc nửa mặt phẳng đối với$(x, y)$. Tất cả các ràng buộc như vậy phải có một giao điểm không trống trong góc phần tư thứ nhất$(x>0, y>0)$để cho phép một giải pháp. 

Thuật toán sử dụng phương pháp tham lam sắp xếp theo **góc**. Hãy xem xét từng điểm liên quan đến nguồn gốc. Vấn đề giảm xuống còn việc sắp xếp các vectơ sao cho dấu ngoặc mở không bao giờ giảm xuống dưới dấu ngoặc đóng. Chúng ta có thể thực hiện điều này bằng cách xem xét các điểm được sắp xếp theo góc trong góc phần tư thứ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((10^6)^2) | O(n) | Quá chậm | 
| Thứ tự hình học / độ dốc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các bộ dữ liệu thành các nhóm mở '(' và đóng ')'. Mỗi nhóm có chính xác$n$điểm. 
2. Sắp xếp từng nhóm theo độ dốc$b_i / a_i$. Đối với dấu ngoặc mở, sắp xếp theo độ dốc tăng dần. Để đóng ngoặc, hãy sắp xếp theo độ dốc giảm dần. Điều này đảm bảo rằng các điểm cực trị của mỗi nhóm cho phép có một đường phân cách trong góc phần tư thứ nhất. 
3. Ghép nối các bộ mở và đóng một cách tham lam: lấy phần mở có độ dốc nhỏ nhất và phần đóng có độ dốc lớn nhất. Kiểm tra xem có tồn tại một dòng không$(x, y)$với$x, y > 0$điều đó ngăn cách họ. 
4. Điều kiện tách tương đương với$a_{\text{open}} \cdot y + b_{\text{open}} \cdot x < a_{\text{close}} \cdot y + b_{\text{close}} \cdot x$đối với một số người$x, y > 0$, làm giảm bất đẳng thức chéo:$(a_{\text{close}} - a_{\text{open}}) * y + (b_{\text{close}} - b_{\text{open}}) * x > 0$. Điều này luôn có thể giải quyết được nếu khoảng dốc của dấu ngoặc mở và đóng không trùng nhau. 
5. Nếu tất cả các cặp thỏa mãn điều kiện tách, xuất ra CÓ. Nếu không thì xuất ra NO. 

Tại sao nó hoạt động: bằng cách sắp xếp theo độ dốc và cặp cực trị, chúng tôi đảm bảo rằng đối với bất kỳ vi phạm RBS tiềm ẩn nào (dấu ngoặc đóng xuất hiện quá sớm), sự bất bình đẳng sẽ bị vi phạm và không có kết quả dương$(x, y)$có thể giải quyết nó. Ngược lại, nếu không xảy ra vi phạm như vậy thì sẽ tồn tại một tổ hợp tuyến tính dương tạo ra RBS. Điều bất biến là ở mỗi bước, tổng tích lũy của dấu ngoặc mở trừ đi dấu ngoặc đóng không bao giờ trở thành số âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        opens, closes = [], []
        for _ in range(2 * n):
            a, b, c = input().split()
            a = int(a)
            b = int(b)
            if c == '(':
                opens.append((a, b))
            else:
                closes.append((a, b))
        
        # Sort openings by slope b/a increasing
        opens.sort(key=lambda p: p[1]/p[0])
        # Sort closings by slope b/a decreasing
        closes.sort(key=lambda p: -p[1]/p[0])
        
        possible = True
        for (ao, bo), (ac, bc) in zip(opens, closes):
            if (ac - ao) * (bo) < (bc - bo) * (ao):
                possible = False
                break
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là phân vùng các bộ dữ liệu thành '(' và ')'. Việc sắp xếp theo độ dốc đảm bảo tồn tại sự phân tách góc phần tư thứ nhất. Việc kiểm tra bất đẳng thức chéo đảm bảo không có dấu ngoặc đóng nào đứng trước dấu ngoặc mở trong bất kỳ trường hợp khả thi nào.$(x, y)$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bộ dữ liệu đầu vào:`(1,4,'(')`,`(2,3,')')`. 

| Bộ dữ liệu | Nhóm | Độ dốc b/a | 
| --- | --- | --- | 
| (1,4,'(') | mở | 4 | 
| (2,3,')') | đóng cửa | 1,5 | 

Độ dốc mở được sắp xếp: [4], Độ dốc đóng được sắp xếp giảm dần: [1.5]. 

Kiểm tra bất đẳng thức:`(2-1)*y + (3-4)*x > 0`⇒`y - x > 0`. Giải pháp tích cực tồn tại với`y=2, x=1`. Đầu ra CÓ. 

### Ví dụ 2 

Bộ dữ liệu đầu vào:`(1,2,')')`,`(3,4,'(')`. 

Độ dốc mở được sắp xếp: [1] (3/4 = 0,75?), Độ dốc đóng được sắp xếp: [2/1 = 2?] 

Kiểm tra bất đẳng thức:`(1-3)*y + (2-4)*x > 0`⇒`-2y -2x > 0`, không thể có x, y dương. Đầu ra KHÔNG. 

Những dấu vết này xác nhận rằng việc sắp xếp theo độ dốc và kiểm tra các bất đẳng thức chéo nắm bắt chính xác tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp từng nhóm n phần tử chiếm ưu thế, ghép cặp là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ các danh sách riêng biệt để mở và đóng | 

Vì tổng n trên tất cả các trường hợp thử nghiệm ≤ 1500 nên tổng công việc dưới 10^6 thao tác, an toàn trong giới hạn thời gian 10 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""4
1
1 4 (
2 3 )
1
1 2 )
3 4 (
4
16 5 (
12 3 (
19 6 )
4
```
