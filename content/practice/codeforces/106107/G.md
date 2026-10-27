---
title: "CF 106107G - Đếm các ô vuông"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một tập hợp các hình vuông, mỗi hình vuông có độ dài cạnh là số nguyên."
date: "2026-06-19T22:21:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106107
codeforces_index: "G"
codeforces_contest_name: "SCPC Teens 2025"
rating: 0
weight: 106107
solve_time_s: 55
verified: true
draft: false
---

[CF 106107G - Đếm các ô vuông](https://codeforces.com/problemset/problem/106107/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một tập hợp các hình vuông, mỗi hình vuông có độ dài cạnh là số nguyên. Từ bộ sưu tập này, chúng ta phải đếm có bao nhiêu cách để chọn ba hình vuông riêng biệt sao cho ba hình vuông này có thể được đặt thẳng hàng với nhau để xếp chính xác một hình chữ nhật không có khoảng trống hoặc chồng chéo. 

Mỗi hình vuông được chọn phải được sử dụng toàn bộ, không cần xoay vì các hình vuông có tính đối xứng và chúng ta không được phép phá vỡ hoặc hợp nhất các hình vuông. Câu hỏi hoàn toàn là liệu ba ô vuông đã cho có thể tạo thành một hình chữ nhật lớn hơn một cách hoàn hảo hay không. 

Các ràng buộc cho phép tối đa 200.000 ô vuông trong tất cả các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào thử trực tiếp tất cả các bộ ba đều không thể thực hiện được. Cách tiếp cận bậc ba hoặc thậm chí bậc hai cho mỗi trường hợp thử nghiệm sẽ không tồn tại. Giải pháp dự định phải giảm bớt vấn đề về việc đếm các mẫu theo tần số của độ dài các cạnh. 

Một điểm tinh tế là chúng ta không sắp xếp các hình vuông một cách tùy tiện trong mặt phẳng mà chỉ kiểm tra xem có sự sắp xếp nào đó hay không. Điều này có nghĩa là chúng tôi đang phân loại các ô xếp hình học hợp lệ chứ không phải mô phỏng các vị trí. 

Một cạm bẫy ngây thơ xuất hiện khi giả định rằng ba hình vuông bất kỳ có thể được kết hợp nếu diện tích của chúng có tổng diện tích là một hình chữ nhật. Ví dụ: các hình vuông có cạnh 1, 2, 3 có tổng diện tích là 14, đây không phải là diện tích hình chữ nhật hoàn hảo theo bất kỳ ý nghĩa ốp lát có ý nghĩa nào với các hình vuông thẳng hàng với trục, tuy nhiên chỉ suy luận dựa trên diện tích là không đủ và gây hiểu lầm. 

Một sai lầm phổ biến khác là cho rằng luôn có hai hình vuông bằng nhau là đủ. Ví dụ: các cạnh 2, 2, 3 không tạo thành một hình chữ nhật hợp lệ, mặc dù sự trùng lặp có thể gợi ý tính đối xứng. 

Khó khăn cốt lõi là xác định số lượng cấu hình ốp lát hợp lệ có giới hạn cho chính xác ba ô vuông. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử từng bộ ba chỉ số và kiểm tra xem ba hình vuông có thể tạo thành hình chữ nhật hay không. Đối với mỗi bộ ba, người ta sẽ thử tất cả các vị trí hình học hoặc kiểm tra tương đương tất cả các cấu hình ốp lát. Điều này dẫn đến khoảng$O(n^3)$gấp ba lần cho mỗi trường hợp thử nghiệm, vượt xa giới hạn khả thi khi$n$đạt tới$2 \cdot 10^5$. 

Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra hình học, nút cổ chai vẫn là số lượng bộ ba. Quan sát quan trọng là cấu trúc của các ô hợp lệ có đúng ba ô vuông là cực kỳ hạn chế. 

Nếu chúng ta tưởng tượng việc xây dựng một hình chữ nhật từ các hình vuông, chỉ có hai cách cơ bản để chia hình chữ nhật thành ba hình vuông thẳng hàng với nhau. Một là đặt ba hình vuông giống hệt nhau thành một hàng hoặc cột. Cách còn lại là đặt hai hình vuông nhỏ giống hệt nhau tạo thành một dải, với một hình vuông lớn hơn chiếm không gian còn lại ở chiều kia. 

Quan sát này chuyển đổi vấn đề từ hình học sang đếm tần số theo độ dài cạnh. Thay vì kiểm tra bộ ba, chúng ta chỉ cần đếm hai mẫu cụ thể trong nhiều tập hợp giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Đếm tần số |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đếm số lần mỗi cạnh hình vuông xuất hiện. Điều này làm giảm vấn đề từ các ô vuông riêng lẻ xuống các tần số có kích thước giống hệt nhau. 
2. Đếm các bộ ba trong đó cả ba hình vuông đều có cạnh bằng nhau. Nếu một giá trị$x$xuất hiện$f$nhiều lần, thì nó góp phần$\binom{f}{3}$bộ ba hợp lệ. Điều này tương ứng với việc tạo thành một hình chữ nhật dưới dạng dải hình vuông giống hệt nhau có kích thước 3 x 1. 
3. Đếm bộ ba khi hai hình vuông có cạnh dài$a$và một hình vuông có độ dài cạnh$2a$. Với mỗi giá trị$a$, nếu như$2a$tồn tại thì số cách chọn bộ ba đó là$\binom{f[a]}{2} \cdot f[2a]$. Điều này tương ứng với một$2 \times 1$ốp lát theo một chiều và một hình vuông duy nhất lấp đầy vùng còn lại. 
4. Tính tổng cả hai đóng góp trên tất cả các giá trị và đưa ra kết quả cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mọi cách xếp hình chữ nhật hợp lệ sử dụng chính xác ba hình vuông đều phải căn chỉnh các hình vuông theo cách tạo thành một hàng hoặc cột gồm ba hình vuông bằng nhau hoặc cấu trúc hai lớp trong đó một hình vuông trải dài toàn bộ chiều và hai hình còn lại lấp đầy vùng còn lại một cách đối xứng. Không có cách nào để phân chia một hình chữ nhật thành ba hình vuông có trục không bằng nhau mà không tạo ra các khoảng trống hoặc buộc các vùng không phải là hình vuông. Điều này hạn chế tất cả các cấu hình hợp lệ ở chính xác hai trường hợp được tính, giúp việc đếm hoàn chỉnh và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = Counter(a)
        ans = 0
        
        for x, fx in freq.items():
            if fx >= 3:
                ans += fx * (fx - 1) * (fx - 2) // 6
            
        for a_val, fa in freq.items():
            b_val = 2 * a_val
            if b_val in freq:
                ans += fa * (fa - 1) // 2 * freq[b_val]
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén đầu vào thành bản đồ tần số. Điều này rất cần thiết vì tất cả các cách xây dựng hợp lệ chỉ phụ thuộc vào số lần độ dài mỗi cạnh xuất hiện chứ không phụ thuộc vào vị trí của chúng. 

Công thức kết hợp cho các bộ ba giống hệt nhau sẽ tính trực tiếp các lựa chọn chỉ số không có thứ tự. Số hạng thứ hai cẩn thận cố định một giá trị thành hình vuông lớn hơn và chọn hai giá trị nhỏ hơn giống hệt nhau. Việc sử dụng kết hợp sẽ tránh được các vấn đề về thứ tự vì bộ ba được xác định bởi các bộ chỉ mục với$i < j < k$. 

Chi tiết triển khai tinh tế đang lặp lại trên bản đồ tần số thay vì mảng thô, đảm bảo hành vi tuyến tính cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
3 3 3 3
```Chúng tôi xây dựng tần số: 

| giá trị | tần số | 
| --- | --- | 
| 3 | 4 | 

Đóng góp bình đẳng: 

| bước | fx | đóng góp | 
| --- | --- | --- | 
| 3 giống hệt nhau | 4 | C(4,3) = 4 | 

Không có giá trị nào khác tồn tại nên câu trả lời là 4. 

Điều này thể hiện cấu hình “tất cả các ô vuông bằng nhau” tương ứng với ô xếp 1 x 3. 

### Ví dụ 2 

đầu vào:```
1
5
2 2 4 2 4
```Tần số: 

| giá trị | tần số | 
| --- | --- | 
| 2 | 3 | 
| 4 | 2 | 

Tất cả đều bình đẳng: 

C(3,3) = 1 từ giá trị 2. 

Trường hợp hỗn hợp: 

Với a = 2, b = 4: 

C(3,2) * 2 = 3 * 2 = 6. 

Câu trả lời cuối cùng là 7. 

Điều này cho thấy cả hai cấu hình cùng tồn tại trong một multiset. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được tính một lần và mỗi tần số được xử lý một lần | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ độ dài cạnh riêng biệt | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc xử lý tuyến tính trên tất cả các bài kiểm tra dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    import sys
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        ans = 0
        
        for x, fx in freq.items():
            if fx >= 3:
                ans += fx * (fx - 1) * (fx - 2) // 6
        
        for a_val, fa in freq.items():
            b_val = 2 * a_val
            if b_val in freq:
                ans += fa * (fa - 1) // 2 * freq[b_val]
        
        output.append(str(ans))
    
    return "\n".join(output)

# provided sample-style checks
assert run("1\n3\n3 3 3\n") == "1"

# custom cases

# minimum case
assert run("1\n3\n1 2 3\n") == "0"

# all equal large frequency
assert run("1\n5\n5 5 5 5 5\n") == "10"

# mixed valid (a,a,2a)
assert run("1\n3\n2 2 4\n") == "1"

# multiple test cases
assert run("2\n3\n1 1 1\n3\n1 1 2\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 1 2 3 | 0 | không có ô hợp lệ nào tồn tại | 
| 1 5 5 5 5 5 | 10 | tổ hợp cho mọi trường hợp bằng nhau | 
| 1 3 2 2 4 | 1 | trường hợp hỗn hợp (a,a,2a) | 
| 2 bài kiểm tra kết hợp | 1 1 | tính đúng đắn của nhiều bài kiểm tra | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi tất cả các hình vuông đều giống hệt nhau. Đối với đầu vào`1 1 1`, có chính xác một cách để chọn cả ba và nó tạo thành một hình chữ nhật hợp lệ. Thuật toán xử lý việc này hoàn toàn thông qua thuật ngữ kết hợp$\binom{f}{3}$, đếm chính xác tất cả các bộ ba chỉ số mà không cần thêm logic. 

Một trường hợp cạnh khác là khi chỉ tồn tại mẫu hỗn hợp, chẳng hạn như`2 2 4`. Ở đây thuật toán tránh tính bất kỳ sự sắp xếp không hợp lệ nào vì nó chỉ xem xét mối quan hệ chặt chẽ$b = 2a$, ngăn chặn việc vô tình đưa vào các giá trị không liên quan. 

Trường hợp tinh tế cuối cùng là tần số lớn, ví dụ như nhiều ô vuông giống hệt nhau. Công thức kết hợp phát triển nhanh chóng nhưng vẫn an toàn trong Python do số nguyên lớn và quá trình tính toán vẫn chạy theo thời gian tuyến tính vì nó tránh liệt kê các bộ ba một cách rõ ràng.
