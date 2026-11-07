---
title: "CF 1337A - Ichihime và Tam giác"
description: "Chúng ta có bốn số nguyên theo thứ tự không giảm và chúng ta cần chọn ba độ dài từ ba khoảng riêng biệt. Độ dài đầu tiên phải đến từ khoảng đầu tiên, độ dài thứ hai từ khoảng thứ hai và độ dài thứ ba từ khoảng thứ ba."
date: "2026-06-16T09:06:27+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1337
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 635 (Div. 2)"
rating: 800
weight: 1337
solve_time_s: 385
verified: false
draft: false
---

[CF 1337A - Ichihime và Tam giác](https://codeforces.com/problemset/problem/1337/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 6 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn số nguyên theo thứ tự không giảm và chúng ta cần chọn ba độ dài từ ba khoảng riêng biệt. Độ dài đầu tiên phải đến từ khoảng đầu tiên, độ dài thứ hai từ khoảng thứ hai và độ dài thứ ba từ khoảng thứ ba. Sau khi chọn ba giá trị này, chúng ta phải đảm bảo chúng có thể tạo thành một hình tam giác có diện tích khác 0. 

Một tam giác có độ dài các cạnh$x, y, z$tồn tại với diện tích dương khi và chỉ nếu các bất đẳng thức tam giác là nghiêm ngặt:$x + y > z$,$x + z > y$, Và$y + z > x$. Bởi vì các giá trị được chọn trong các khoảng được sắp xếp trong đó$x \le y \le z$luôn có thể đạt được bằng cách xây dựng, ràng buộc có ý nghĩa duy nhất trở thành$x + y > z$. 

Các ràng buộc nhỏ về kích thước đầu vào, nhiều nhất là 1000 trường hợp thử nghiệm, do đó, việc xây dựng thời gian không đổi cho mỗi trường hợp thử nghiệm là đủ. Mỗi giá trị có thể lớn bằng$10^9$, loại trừ bất kỳ cách tiếp cận nào phụ thuộc vào việc liệt kê các ứng cử viên trong các khoảng thời gian. 

Một nỗ lực ngây thơ sẽ thử tất cả các bộ ba trong các khoảng thời gian. Đối với mỗi trường hợp thử nghiệm có liên quan đến$(b-a+1)(c-b+1)(d-c+1)$sự kết hợp, có thể rất lớn, lên đến$10^{27}$trong trường hợp xấu nhất. Điều này rõ ràng là không thể. 

Một cạm bẫy tinh tế hơn là giả định rằng việc chọn các giá trị biên tùy ý luôn hoạt động, ví dụ như chọn$x=a$,$y=b$,$z=c$. Điều này có thể thất bại khi$a + b \le c$, tạo ra một tam giác suy biến hoặc không hợp lệ. Cấu trúc mẫu cho thấy cần có sự linh hoạt trong các khoảng thời gian. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ liệt kê tất cả các bộ ba hợp lệ$x, y, z$trong phạm vi tương ứng của chúng và kiểm tra tình trạng tam giác. Điều này hoạt động về mặt khái niệm vì mọi kết hợp hợp lệ đều được kiểm tra, nhưng số lượng kết hợp tăng theo cấp số nhân với kích thước khoảng. Vì mỗi khoảng có thể kéo dài tới$10^9$, không gian tìm kiếm trở nên lớn về mặt thiên văn và ngay lập tức không khả thi. 

Nhận xét quan trọng là điều kiện của tam giác chỉ phụ thuộc vào tổng hai cạnh nhỏ hơn so với cạnh lớn nhất. Bởi vì$y$được kẹp giữa hai khoảng còn lại, điều chỉnh$y$cung cấp sự kiểm soát trực tiếp nhất để thỏa mãn bất đẳng thức. Mục tiêu là làm cho$x + y$chỉ đủ lớn để vượt quá$z$và vì chúng ta có thể tự do lựa chọn trong các khoảng thời gian nên việc chọn các giá trị gần nhau gần ranh giới là đủ. 

Một chiến lược mang tính xây dựng xuất hiện: chọn$x$càng lớn càng tốt trong vòng$[a, b]$, nhặt$y$càng lớn càng tốt trong vòng$[b, c]$, và chọn$z$càng nhỏ càng tốt trong vòng$[c, d]$. Điều này đẩy tổng$x + y$hướng lên trong khi giữ$z$tối thiểu, tối đa hóa cơ hội thỏa mãn bất đẳng thức nghiêm ngặt. Nếu lựa chọn này không thành công, việc dịch chuyển các giá trị một chút trong vùng chồng lấp giữa các khoảng luôn cho phép điều chỉnh và vấn đề đảm bảo tồn tại giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((b-a)(c-b)(d-c))$|$O(1)$| Quá chậm | 
| Xây dựng tham lam |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên$a, b, c, d$. Chúng xác định ba phạm vi độc lập cho$x, y, z$. 
2. Xây dựng$x = b$. Sự lựa chọn này đặt$x$ở đầu trên của phạm vi của nó, tối đa hóa sự đóng góp tiềm năng vào bất đẳng thức tam giác. 
3. Xây dựng$y = c$. Điều này tương tự sẽ tối đa hóa cạnh thứ hai trong khi vẫn tôn trọng khoảng cho phép của nó. 
4. Xây dựng$z = c$. Điều này giúp giảm thiểu cạnh thứ ba trong khi vẫn nằm trong khoảng của nó, giúp thỏa mãn$x + y > z$. 
5. Đầu ra$x, y, z$. 

Ý tưởng tinh tế là chúng ta không cần phải tìm kiếm sự cân bằng mong manh. Cấu trúc của các khoảng đã đảm bảo rằng việc đẩy hai biến đầu tiên lên trên và biến cuối cùng đi xuống sẽ tạo ra một cấu hình hợp lệ. 

### Tại sao nó hoạt động 

Việc đảm bảo xây dựng$x = b \le c = y = z$hoặc ít nhất$x \le y \le z$cho đến các trường hợp bình đẳng, nhưng điều kiện quan trọng là$x + y > z$. Từ$x \ge b$Và$y \ge b$, tổng của chúng ít nhất là$2b$, trong khi$z \le d$nhưng chúng tôi đặc biệt neo nó ở$c$, ranh giới trung điểm giữa các phạm vi. Sự đảm bảo của vấn đề đảm bảo rằng trong các khoảng này luôn tồn tại một cấu hình trong đó tổng của hai phân đoạn trước vượt quá phân đoạn thứ ba. Bằng cách căn chỉnh hai giá trị ở ranh giới trên và giá trị thứ ba ở ranh giới dưới trong phạm vi của nó, chúng ta buộc giữ nguyên bất đẳng thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, d = map(int, input().split())
    
    x = b
    y = c
    z = c
    
    print(x, y, z)
```Mã trực tiếp thực hiện việc xây dựng tham lam. Mỗi trường hợp thử nghiệm được xử lý độc lập và không cần bộ nhớ phụ trợ. Sự lựa chọn của`b, c, c`căn chỉnh các biến với các điểm dừng tự nhiên của các khoảng. 

Một điểm tinh tế là mặc dù điều này trông có vẻ tầm thường nhưng nó dựa trên thực tế là các ranh giới chồng chéo đảm bảo tính khả thi. Bất kỳ cấu trúc hợp lệ thay thế nào cũng sẽ hoạt động, nhưng cấu trúc này ổn định và tránh được lý luận cạnh bên trong mã. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3 5 7
```Chúng tôi tính toán: 

| Bước | x | y | z | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | khoảng thời gian được xác định | 
| Chỉ định | 3 | 5 | 5 | thi công áp dụng | 
| Xác minh | 3 | 5 | 5 | 3 + 5 > 5 | 

Bất đẳng thức xảy ra vì$3 + 5 = 8 > 5$. Tam giác là hợp lệ và không suy biến. 

### Ví dụ 2 

đầu vào:```
1 1 1 10
```| Bước | x | y | z | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Chỉ định | 1 | 1 | 1 | xây dựng ranh giới | 
| Xác minh | 1 | 1 | 1 | 1 + 1 > 1 | 

Ngay cả trong trường hợp cực đoan khi tất cả các khoảng đều thu gọn ở bên trái, việc xây dựng vẫn tạo ra một tam giác hợp lệ vì bất kỳ cạnh dương bằng nhau nào tạo thành một điều kiện tam giác diện tích dương có vẻ suy biến nhưng hợp lệ được thỏa mãn bởi sự bất đẳng thức nghiêm ngặt khi so sánh một phía. 

Điều này chứng tỏ rằng ngay cả khi phạm vi là tối thiểu, việc căn chỉnh các giá trị tại các ranh giới chung vẫn duy trì tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| công việc liên tục cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| không có dung lượng bổ sung ngoài các biến | 

Giải pháp thực hiện một số phép tính số học cố định cho mỗi trường hợp thử nghiệm, tối ưu cho$t \le 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        x, y, z = b, c, c
        out.append(f"{x} {y} {z}")
    return "\n".join(out)

# provided samples
assert run("4\n1 3 5 7\n1 5 5 7\n100000 200000 300000 400000\n1 1 977539810 977539810") == \
"3 5 5\n5 5 5\n200000 300000 300000\n1 977539810 977539810"

# custom cases
assert run("1\n1 2 3 4") == "2 3 3"
assert run("1\n5 5 5 5") == "5 5 5"
assert run("1\n1 10 10 10") == "10 10 10"
assert run("1\n2 3 5 9") == "3 5 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | 2 3 3 | phạm vi tăng tối thiểu | 
| 5 5 5 5 | 5 5 5 | khoảng suy biến | 
| 1 10 10 10 | 10 10 10 | ranh giới chặt chẽ bên phải | 
| 2 3 5 9 | 3 5 5 | khoảng cách không đồng đều | 

## Vỏ cạnh 

Hãy xem xét trường hợp cực đoan trong đó tất cả các khoảng đều thu gọn tại một điểm duy nhất, chẳng hạn như:```
1 1 1 1
```Đầu ra của thuật toán$1, 1, 1$. Điều kiện tam giác đúng vì$1 + 1 > 1$. Không có quyền tự do lựa chọn các giá trị khác nhau và việc xây dựng vẫn thỏa mãn tính giá trị. 

Một trường hợp khác là khi các khoảng cách đều nhau:```
1 2 100 1000
```Thuật toán tạo ra$2, 100, 100$. Đây$2 + 100 > 100$, do đó tồn tại một tam giác hợp lệ mặc dù khoảng cuối cùng ở rất xa. Việc xây dựng hoạt động hiệu quả vì các giá trị ở giữa và bên phải được căn chỉnh theo ranh giới khả thi nhỏ nhất, ngăn không cho khoảng cách phá vỡ sự bất bình đẳng.
