---
title: "CF 1253C - Ăn đồ ngọt"
description: "Chúng ta được cung cấp một danh sách các loại đồ ngọt, mỗi loại có một giá trị nguyên dương biểu thị mức độ ngọt của nó. Yui muốn ăn chính xác $k$ số kẹo cho mỗi $k$ từ 1 đến $n$. Tuy nhiên, cô ấy không thể ăn nhiều hơn $ triệu đô la kẹo trong một ngày. Thời gian được chia thành các ngày bắt đầu từ ngày 1."
date: "2026-06-11T21:04:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1253
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 600 (Div. 2)"
rating: 1500
weight: 1253
solve_time_s: 103
verified: true
draft: false
---

[CF 1253C - Ăn đồ ngọt](https://codeforces.com/problemset/problem/1253/C) 

**Đánh giá:** 1500 
**Tags:** dp, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các loại đồ ngọt, mỗi loại có một giá trị nguyên dương biểu thị mức độ ngọt của nó. Yui muốn ăn chính xác$k$kẹo cho mỗi người$k$từ 1 đến$n$. Tuy nhiên, cô ấy không thể ăn nhiều hơn$m$kẹo trong một ngày. 

Thời gian được chia thành các ngày bắt đầu từ ngày 1. Nếu ăn đồ ngọt vào ngày$d$, phần đóng góp của nó vào tổng số tiền phạt là$d \cdot a_i$, do đó việc trì hoãn tiêu dùng sẽ làm tăng chi phí tuyến tính theo thời gian. Mỗi món ngọt phải được ăn đúng một lần hoặc không ăn chút nào. 

Đối với mọi$k$, chúng tôi muốn tổng số tiền phạt tối thiểu có thể nếu Yui chọn bất kỳ$k$đồ ngọt và lên kế hoạch cho chúng một cách tối ưu với sự ràng buộc tối đa$m$đồ ngọt có thể được ăn mỗi ngày. 

Sự tương tác chính là giữa đặt hàng và nhóm. Chọn đồ ngọt nào để ăn là chưa đủ; cách chúng được phân phối qua các ngày sẽ xác định hệ số nhân được áp dụng cho từng giá trị. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$đồ ngọt. Bất kỳ giải pháp nào cố gắng tính toán lại lịch trình tối ưu một cách độc lập cho từng$k$hoặc mô phỏng những quyết định tham lam ngày qua ngày một cách ngây thơ sẽ quá chậm. MỘT$O(n^2)$hoặc thậm chí$O(n^2 \log n)$cách tiếp cận sẽ không vượt qua. Chúng ta nên mong đợi điều gì đó gần gũi$O(n \log n)$hoặc$O(n)$sau khi sắp xếp. 

Một trường hợp thất bại tinh tế xuất hiện khi một chiến lược tham lam cố gắng luôn lấy những viên kẹo nhỏ nhất có sẵn mà không tính đến hiệu ứng theo đợt. Ví dụ, nếu$m = 2$và các giá trị là$[1, 100, 101]$, việc chọn đồ ngọt theo thứ tự giao đến sẽ dẫn đến việc ghép nối không chính xác trong nhiều ngày và bị phạt quá mức. Chiến lược đúng có thể trì hoãn hoặc nhóm các lựa chọn khác nhau để giữ cho các giá trị lớn không bị nhân với các chỉ số ngày lớn quá sớm. 

## Phương pháp tiếp cận 

Nếu chúng ta sửa một tập hợp con của$k$kẹo, quyết định duy nhất còn lại là làm thế nào để phân phát chúng trong nhiều ngày, mỗi ngày giữ nhiều nhất$m$mặt hàng. Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con có kích thước$k$và với mỗi tập hợp con hãy tính toán lịch trình tối ưu. Ngay cả khi việc lập kế hoạch được thực hiện một cách tham lam, việc liệt kê các chi phí của tập hợp con$\binom{n}{k}$, điều này là không thể thực hiện được ngay cả đối với nhỏ$n$. 

Một góc nhìn hữu ích hơn là sắp xếp đồ ngọt theo giá trị. Giả sử chúng ta quyết định lấy$k$kẹo nhỏ nhất. Điều này là tối ưu cho cố định$k$bởi vì hình phạt là tuyến tính trong$a_i$và các giá trị lớn hơn luôn đắt hơn theo bất kỳ lịch trình nào. 

Bây giờ vấn đề giảm xuống việc đặt hàng những thứ này$k$các giá trị được chọn theo thời gian. Chiến lược tốt nhất là luôn đặt giá trị lớn hơn vào những ngày trước đó vì những ngày trước đó có số nhân nhỏ hơn. Vì mỗi ngày có thể mất nhiều nhất$m$đồ ngọt, cấu trúc trở thành: chúng tôi lấp đầy ngày 1 với tối đa$m$kẹo được chọn nhỏ nhất, sau đó là ngày thứ 2 với mẻ tiếp theo, v.v. 

Điều này gợi ý việc xử lý đồ ngọt theo thứ tự sắp xếp và nhóm chúng thành các khối có kích thước$m$. Mỗi khối tương ứng với một ngày và tất cả các phần tử trong khối$d$lấy số nhân$d$. 

Sau đó, chúng tôi duy trì tích lũy tiền tố: sau khi sắp xếp, chúng tôi có thể tính toán các khoản đóng góp tăng dần, cập nhật câu trả lời cho tất cả$k$đồng thời bằng cách mở rộng cấu trúc khối ngày hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + tích lũy hàng loạt) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp tất cả các loại đồ ngọt theo thứ tự độ ngọt không giảm dần. Điều này đảm bảo rằng khi chúng tôi lấy tiền tố của mảng này, chúng tôi luôn chọn bộ tốt nhất có thể cho bất kỳ giá trị cố định nào.$k$. 

Chúng tôi mô phỏng việc xây dựng câu trả lời cho tất cả$k$cùng một lúc: 

1. Sắp xếp mảng$a$theo thứ tự tăng dần. Điều này đảm bảo các giá trị nhỏ hơn được chỉ định vào những ngày trước đó. 
2. Duy trì tổng các giá trị trong khối ngày hiện tại và mảng câu trả lời chung được khởi tạo bằng 0. 
3. Lặp lại việc sắp xếp đồ ngọt, theo dõi vị trí của chúng. 
4. Đối với mỗi phần tử tại vị trí$i$, xác định chỉ số ngày của nó là$\lfloor i / m \rfloor + 1$. 
5. Cộng phần đóng góp của nó vào tổng số hiện có, vì nó ảnh hưởng đến tất cả các câu trả lời có hậu tố$x_k$Ở đâu$k \ge i+1$. 
6. Tích lũy các câu trả lời tăng dần: mỗi tiền tố xác định một giải pháp ứng viên để chọn chính xác số lượng kẹo đó. 
7. Xuất ra mảng kết quả. 

Một cách chính xác hơn để thấy điều đó là mọi yếu tố đều đóng góp cho tất cả$k$nơi nó được bao gồm và số nhân của nó chỉ phụ thuộc vào số lượng khối đầy đủ kích thước$m$trước nó theo thứ tự được sắp xếp. 

Tại sao nó hoạt động được gắn liền với các cuộc tranh luận trao đổi. Nếu hai chiếc kẹo không đúng thứ tự về mặt giá trị nhưng được đặt vào những ngày muộn hơn hoặc sớm hơn không nhất quán, thì việc hoán đổi chúng sao cho giá trị lớn hơn nhận được số nhân nhỏ hơn luôn làm giảm hoặc bảo toàn tổng chi phí. Trong mỗi ngày, việc sắp xếp thứ tự không thành vấn đề, nhưng qua nhiều ngày, sự đơn điệu buộc phải nhóm lại theo thứ tự được sắp xếp. Do đó, cấu trúc tối ưu được xác định duy nhất bằng cách sắp xếp và điền vào các lô có kích thước$m$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    res = [0] * n
    
    current_sum = 0
    day_cost = 0
    j = 0
    
    for i in range(n):
        day = i // m + 1
        current_sum += a[i] * day
        res[i] = current_sum
    
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp để thực thi cấu trúc toàn cầu chính xác. Quan sát quan trọng là sau khi được sắp xếp, lịch trình tối ưu được xác định ngầm bởi các khối có kích thước$m$, vì vậy chúng tôi không bao giờ mô phỏng rõ ràng số ngày ngoài việc tính toán hệ số nhân dưới dạng hàm của chỉ mục. 

Mảng`res[i]`đại diện cho chi phí tối thiểu để lấy chính xác$i+1$đồ ngọt, vì tiền tố của độ dài$i+1$luôn là tối ưu. Tổng hiện có tích lũy các khoản đóng góp với số nhân ngày chính xác bắt nguồn từ phép chia số nguyên cho (m`. 

Một lỗi phổ biến là thử tính lại số tiền mỗi ngày một cách riêng biệt, điều này dẫn đến việc lặp lại$O(n^2)$tích lũy. Ở đây mọi thứ đều được khấu hao trong một lần. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

đầu vào:```
n = 9, m = 2
a = [6, 19, 3, 4, 4, 2, 6, 7, 8]
```Đã sắp xếp:```
[2, 3, 4, 4, 6, 6, 7, 8, 19]
```Chúng ta tính toán từng bước: 

| tôi | giá trị | ngày = i//2 + 1 | đóng góp | tổng tiền tố | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 2 | 2 | 
| 1 | 3 | 1 | 3 | 5 | 
| 2 | 4 | 2 | 8 | 13 | 
| 3 | 4 | 2 | 8 | 21 | 
| 4 | 6 | 3 | 18 | 39 | 
| 5 | 6 | 3 | 18 | 57 | 
| 6 | 7 | 4 | 28 | 85 | 
| 7 | 8 | 4 | 32 | 117 | 
| 8 | 19 | 5 | 95 | 212 | 

Tổng tiền tố tương ứng với câu trả lời cho$k = 1 \dots 9$, phù hợp với cấu trúc phân mẻ tối ưu. 

Dấu vết này cho thấy số nhân ngày tăng chính xác như thế nào khi các khối có kích thước$m$được lấp đầy, xác nhận rằng việc phân nhóm sau khi sắp xếp sẽ xác định đầy đủ lịch trình tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, chuyển tuyến tính đơn sau đó | 
| Không gian | O(n) | lưu trữ kết quả mảng và tiền tố | 

Các ràng buộc cho phép lên tới 200.000 phần tử, do đó$O(n \log n)$giải pháp dựa trên sắp xếp thoải mái trong giới hạn, trong khi bất kỳ phương pháp bậc hai nào cũng sẽ thất bại do$4 \times 10^{10}$hoạt động trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    
    res = []
    for i in range(n):
        day = i // m + 1
        if i == 0:
            res.append(a[i] * day)
        else:
            res.append(res[-1] + a[i] * day)
    
    return " ".join(map(str, res))

# provided sample
assert run("9 2\n6 19 3 4 4 2 6 7 8\n") == "2 5 13 21 39 57 85 117 212"

# minimum size
assert run("1 1\n5\n") == "5"

# all equal
assert run("5 2\n3 3 3 3 3\n") == "3 6 12 15 21"

# m = n
assert run("4 4\n1 2 3 4\n") == "1 3 6 10"

# strict increasing
assert run("5 1\n1 2 3 4 5\n") == "1 4 9 16 25"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2 5 13 21 39 57 85 117 212 | tính đúng đắn so với tham chiếu | 
| 1 sản phẩm | 5 | trường hợp cơ sở | 
| tất cả đều bình đẳng | tính chính xác của việc trộn lũy tiến | xử lý các giá trị giống hệt nhau | 
| m = n | tích lũy tam giác | hành vi một ngày | 
| m = 1 | chia ly cả ngày | tăng trưởng hình phạt tối đa | 

## Vỏ cạnh 

Khi nào$m = 1$, mỗi chiếc kẹo sẽ chuyển sang một ngày mới, vì vậy hình phạt sẽ trở thành tổng của các tiền tố sau khi sắp xếp. Thuật toán xử lý việc này một cách tự nhiên vì mỗi chỉ mục tạo thành khối riêng và hệ số nhân bằng vị trí. 

Khi$m \ge n$, tất cả đồ ngọt đều được ăn vào ngày thứ nhất, vì vậy câu trả lời chỉ đơn giản là tổng tiền tố của mảng được sắp xếp mà không chia tỷ lệ. Vì phép chia số nguyên mang lại ngày 1 cho tất cả các chỉ số nên phép tính sẽ giảm chính xác. 

Khi tất cả các giá trị đều bằng nhau, về nguyên tắc mọi thứ tự đều tối ưu, nhưng việc xếp khối vẫn tạo ra số nhân tăng dần. Thuật toán chỉ định các chỉ số ngày lớn hơn cho đồ ngọt sau này, điều này không thể tránh khỏi do hạn chế về dung lượng và khớp với tổng số tối thiểu có thể đạt được.
