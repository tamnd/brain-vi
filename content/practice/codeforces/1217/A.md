---
title: "CF 1217A - Tạo nhân vật"
description: "Chúng ta được cấp cho một nhân vật với hai thuộc tính cơ bản: sức mạnh và trí thông minh. Chúng tôi cũng được cung cấp một lượng điểm kinh nghiệm bổ sung mà tất cả đều phải được phân phối. Mỗi điểm có thể tăng sức mạnh hoặc trí thông minh thêm đúng một đơn vị."
date: "2026-06-15T18:52:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1217
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 72 (Rated for Div. 2)"
rating: 1300
weight: 1217
solve_time_s: 330
verified: true
draft: false
---

[CF 1217A - Tạo nhân vật](https://codeforces.com/problemset/problem/1217/A) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho một nhân vật với hai thuộc tính cơ bản: sức mạnh và trí thông minh. Chúng tôi cũng được cung cấp một lượng điểm kinh nghiệm bổ sung mà tất cả đều phải được phân phối. Mỗi điểm có thể tăng sức mạnh hoặc trí thông minh thêm đúng một đơn vị. Sau khi phân phối tất cả các điểm, chúng ta thu được cặp giá trị cuối cùng. 

Nhiệm vụ không phải là tính toán một bản dựng tối ưu duy nhất mà là đếm xem có bao nhiêu cấu hình cuối cùng khác nhau hợp lệ trong một ràng buộc: sau khi tiêu hết tất cả điểm kinh nghiệm, sức mạnh thu được phải lớn hơn nhiều so với trí thông minh thu được. Hai cấu hình được coi là khác nhau nếu sức mạnh cuối cùng hoặc trí thông minh cuối cùng khác nhau. 

Cấu trúc chính là mọi kết quả hợp lệ đều được xác định đầy đủ bằng số điểm chúng ta gán cho điểm mạnh. Khi điều đó đã được khắc phục, trí thông minh sẽ được xác định tự động vì tất cả các điểm phải được sử dụng. 

Các ràng buộc lên tới 10^8 cho tất cả các tham số, với tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân phối một cách rõ ràng, vì ngay cả việc lặp lại exp cho mỗi trường hợp thử nghiệm cũng có thể đạt tới 10^10 thao tác trong trường hợp xấu nhất. Giải pháp phải là O(1) cho mỗi truy vấn. 

Trường hợp cạnh tinh tế xuất hiện khi không có phân phối nào hợp lệ. Điều này xảy ra khi trí thông minh bắt đầu quá lớn so với sức mạnh, do đó, ngay cả việc dồn toàn bộ kinh nghiệm vào sức mạnh cũng không làm cho sức mạnh trở nên lớn hơn. Một trường hợp biên khác là khi exp bằng 0, trong đó có chính xác một cách dựng có thể và nó chỉ hợp lệ nếu bất đẳng thức ban đầu đã đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi cách phân chia điểm kinh nghiệm có thể. Nếu chúng ta gán x điểm cho sức mạnh và điểm exp - x cho trí thông minh, thì giá trị cuối cùng sẽ trở thành: 

sức mạnh = str + x 

trí tuệ = int + (exp - x) 

Chúng ta có thể lặp x từ 0 đến exp và kiểm tra bất đẳng thức cho từng trường hợp. Điều này đúng vì nó liệt kê tất cả các phân phối có thể có, nhưng nó là tuyến tính trong exp. Với exp lên tới 10^8 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là ràng buộc bất đẳng thức có thể được viết lại thành bất đẳng thức tuyến tính theo x. Điều này biến bài toán thành việc đếm các nghiệm nguyên trong một khoảng bị chặn. Khi làm như vậy, chúng ta chỉ cần tìm phạm vi của x thỏa mãn ràng buộc, sau đó đếm xem có bao nhiêu số nguyên nằm trong đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(exp) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta gọi x là số điểm kinh nghiệm được gán cho sức mạnh. Sau đó trí thông minh nhận được điểm kinh nghiệm - x. 

1. Biểu diễn các thuộc tính cuối cùng dưới dạng phương trình: 

sức mạnh = str + x, trí thông minh = int + exp - x. 
2. Chuyển điều kiện Strength > Intelligence thành bất đẳng thức: 

str + x > int + exp - x. 
3. Sắp xếp lại mọi thứ liên quan đến x ở một bên: 

2x > int + exp - str. 
4. Giải x dưới dạng số nguyên cận dưới: 

x > (int + exp - str) / 2. 
5. Chuyển bất đẳng thức nghiêm ngặt về số nguyên nhỏ nhất x: 

x_min = tầng((int + exp - str) / 2) + 1. 
6. Thực thi các ràng buộc về tính khả thi xuất phát từ cách xác định x: 

0 ≤ x ≤ kinh nghiệm. 
7. Số lượng bản dựng hợp lệ là kích thước của khoảng [x_min, exp], nếu nó tồn tại. 

Tại sao nó hoạt động: mọi bản dựng hợp lệ tương ứng với chính xác một số nguyên x trong [0, exp]. Bất đẳng thức giảm xuống còn một ràng buộc tuyến tính duy nhất, do đó tập hợp các bản dựng hợp lệ sẽ trở thành một khoảng số nguyên liền kề. Việc đếm các giải pháp sẽ trở thành việc đếm các điểm nguyên trong khoảng đó và không có hai giá trị x khác nhau tạo ra cặp (sức mạnh, trí thông minh) giống nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        str_val, int_val, exp = map(int, input().split())
        
        # Solve inequality:
        # str + x > int + (exp - x)
        # 2x > int + exp - str
        
        rhs = int_val + exp - str_val
        
        # minimal integer x satisfying strict inequality
        # x > rhs / 2
        x_min = rhs // 2 + 1
        
        if x_min < 0:
            x_min = 0
        
        if x_min > exp:
            out.append("0")
        else:
            out.append(str(exp - x_min + 1))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là tính toán giá trị hợp lệ tối thiểu của x. Bất đẳng thức tạo ra một nửa giới hạn chặt chẽ, do đó chúng ta chuyển đổi nó một cách cẩn thận bằng cách sử dụng phép chia số nguyên. Bởi vì tầng Python hướng tới âm vô cực, sử dụng`rhs // 2 + 1`xử lý chính xác cả trường hợp rhs dương và âm. 

Sau khi tính toán`x_min`, chúng tôi kẹp nó vào miền hợp lệ [0, exp]. Nếu vượt quá exp thì không tồn tại cấu hình hợp lệ. Mặt khác, mọi số nguyên từ x_min đến exp đều tương ứng với một bản dựng hợp lệ, vì vậy chúng tôi đếm chúng trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
str = 5, int = 3, exp = 4
```Chúng tôi tính toán rhs = 3 + 4 - 5 = 2. 

| x | sức mạnh | trí thông minh | có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | 5 | 7 | không | 
| 1 | 6 | 6 | không | 
| 2 | 7 | 5 | vâng | 
| 3 | 8 | 4 | vâng | 
| 4 | 9 | 3 | vâng | 

Giá trị x hợp lệ là 2, 3, 4, cho 3 cấu hình. 

Điều này xác nhận rằng các nghiệm hợp lệ tạo thành một hậu tố liền kề của dãy. 

### Ví dụ 2 

đầu vào:```
str = 2, int = 1, exp = 0
```rhs = 1 + 0 - 2 = -1, vậy bất đẳng thức đã được thỏa mãn. 

| x | sức mạnh | trí thông minh | có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | vâng | 

Chỉ tồn tại một cấu hình phù hợp với thực tế là exp tạo ra x = 0. 

Điều này cho thấy thuật toán xử lý chính xác trường hợp ranh giới không có kinh nghiệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm được giải bằng cách sử dụng số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với các giá trị lên tới 10^8, do đó, giải pháp truy vấn có thời gian không đổi là đủ và cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    res = []
    for _ in range(T):
        a, b, c = map(int, input().split())
        rhs = b + c - a
        x_min = rhs // 2 + 1
        if x_min < 0:
            x_min = 0
        if x_min > c:
            res.append("0")
        else:
            res.append(str(c - x_min + 1))
    return "\n".join(res)

# provided samples
assert run("""4
5 3 4
2 1 0
3 5 5
4 10 6
""") == """3
1
2
0"""

# minimum exp
assert run("""1
5 1 0
""") == "1"

# no valid configuration
assert run("""1
1 10 0
""") == "0"

# all allocations valid
assert run("""1
10 1 5
""") == "6"

# boundary equality case
assert run("""1
5 3 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ mẫu | đưa ra | tính đúng đắn trong các trường hợp hỗn hợp | 
| 5 1 0 | 1 | exp = 0 ranh giới | 
| 1 10 0 | 0 | bất bình đẳng không thể | 
| 10 1 5 | 6 | tất cả x trong phạm vi đầy đủ hợp lệ | 
| 5 3 0 | 1 | xử lý bình đẳng không linh hoạt | 

## Vỏ cạnh 

Khi exp bằng 0, x buộc phải bằng 0. Thuật toán đặt x_min dựa trên bất đẳng thức và sau đó kẹp nó vào [0, exp]. Nếu ràng buộc cho phép x = 0, chính xác một bản dựng sẽ được tính; mặt khác, x_min vượt quá exp và câu trả lời trở thành 0. 

Khi trí thông minh lớn hơn sức mạnh rất nhiều, rhs trở nên lớn và dương, đẩy x_min vượt xa exp. Trong trường hợp đó, khoảng [x_min, exp] trống và thuật toán trả về 0 một cách chính xác mà không cần lặp lại. 

Khi cường độ đã lớn, rhs trở thành âm và x_min đánh giá thành số âm. Việc đặt nó ở mức 0 đảm bảo chúng tôi tính tất cả các phân bổ có thể có, phù hợp với thực tế là mọi sự phân chia đều duy trì sự thống trị về sức mạnh.
