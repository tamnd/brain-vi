---
title: "CF 1547D - Trình tự cùng phát triển"
description: "Chúng ta được cấp một dãy $x1, x2, dấu chấm, xn$. Nhiệm vụ của chúng ta là xây dựng một chuỗi $y1, y2, dots, yn$ khác sao cho khi chúng ta XOR chúng theo từng phần tử, chuỗi kết quả $$ai = xi oplus yi$$ có cấu trúc bit đơn điệu: mọi bit được đặt trong $ai$ cũng phải được đặt trong $a{i+1}$."
date: "2026-06-14T19:58:01+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 1300
weight: 1547
solve_time_s: 503
verified: false
draft: false
---

[CF 1547D - Trình tự cùng phát triển](https://codeforces.com/problemset/problem/1547/D) 

**Đánh giá:** 1300 
**Tags:** bitmasks, thuật toán xây dựng, tham lam 
**Thời gian giải:** 8m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một trình tự$x_1, x_2, \dots, x_n$. Nhiệm vụ của chúng ta là xây dựng một chuỗi khác$y_1, y_2, \dots, y_n$sao cho khi chúng ta XOR chúng theo từng phần tử, chuỗi kết quả$$a_i = x_i \oplus y_i$$có cấu trúc bit đơn điệu: mọi bit được đặt trong$a_i$cũng phải được đặt trong$a_{i+1}$. Theo thuật ngữ bitwise, điều này có nghĩa$$a_i \& a_{i+1} = a_i.$$Vì vậy trình tự$a_i$không giảm theo nghĩa các bit được đặt: một khi một bit trở thành 1, nó không bao giờ có thể trở thành 0 sau này. 

Chúng tôi không xây dựng$a_i$trực tiếp. Thay vào đó chúng ta phải chọn$y_i$và trong số tất cả các lựa chọn hợp lệ, chúng ta phải giảm thiểu trình tự theo từ điển. 

Sự tương tác chính là mỗi$y_i$điều khiển$a_i$thông qua XOR. Vì XOR lật các bit một cách độc lập nên mỗi vị trí$i$có thể được coi là việc lựa chọn$a_i$, và sau đó suy ra$y_i = a_i \oplus x_i$, nhưng với hạn chế là kết quả$a$-Trình tự phải thỏa mãn điều kiện sinh trưởng. 

Các ràng buộc lớn: tổng cộng$n$qua các bài kiểm tra lên đến$2 \cdot 10^5$. Điều này buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra. Bất cứ điều gì cố gắng xem xét tất cả các mẫu bit có thể có hoặc quay lại các lựa chọn đều quá chậm. 

Một cách tiếp cận ngây thơ sẽ cố gắng xây dựng$y_i$tham lam nhưng hãy kiểm tra tính hợp lệ của tiền tố đầy đủ mỗi lần hoặc thậm chí cố gắng chọn từng bit một cách độc lập trong khi vẫn duy trì tính nhất quán. Vấn đề là ràng buộc kết hợp tất cả các vị trí thông qua tính bền vững của bit, do đó các quyết định cục bộ có thể phá vỡ tính khả thi trong tương lai. 

Một trường hợp thất bại đơn giản xuất hiện khi giảm thiểu một cách tham lam$y_i$độc lập:```
x = [1, 2]
```Nếu chúng ta chọn$y_1 = 0$, sau đó$a_1 = 1$. Tại$i=2$, nếu chúng ta chọn lại$y_2 = 0$, sau đó$a_2 = 2$, và chúng tôi nhận được$1 \& 2 = 0 \neq 1$, không hợp lệ. Việc khắc phục điều này đòi hỏi phải dự đoán những hạn chế từ các vị trí trong tương lai. 

Vì vậy khó khăn chính là mỗi$a_i$phải là siêu bộ (theo nghĩa bit) của các bộ trước đó, trong khi vẫn giữ$y_i$nhỏ về mặt từ điển. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là cố gắng hết sức có thể$y_1$, tính toán$a_1$, sau đó thực thi mỗi lần tiếp theo$a_i$phải là siêu tập hợp của tất cả các tập trước đó$a$'S. Ở bước$i$, chúng tôi sẽ thử tất cả$y_i$như vậy$a_i = x_i \oplus y_i$thỏa mãn ràng buộc ngày càng tăng với$a_{i-1}$. Điều này có nghĩa là liệt kê tất cả các mặt nạ bit cho từng vị trí và kiểm tra tính tương thích của từng ứng viên. 

Vì mỗi$x_i < 2^{30}$, mỗi$y_i$cũng nằm trong không gian 30 bit, bằng$2^{30}$khả năng cho mỗi vị trí. Ngay cả việc cắt tỉa bằng các ràng buộc vẫn để lại một tìm kiếm theo cấp số nhân. Điều này rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là sự hạn chế về$a_i$là đơn điệu về mặt bao gồm bit: một khi một bit trở thành 1 trong một số$a_i$, nó phải ở lại 1 mãi mãi. Điều này gợi ý việc duy trì bitmask toàn cầu$S$, đại diện cho tất cả các bit đã bị ép lên 1 trong lần trước$a$-giá trị. 

Tại vị trí$i$, chúng ta phải đảm bảo rằng$a_i$chứa tất cả các bit trong$S$. Sự lựa chọn tối thiểu về mặt từ điển cho$y_i$tương ứng với việc làm$a_i$càng nhỏ càng tốt trong khi vẫn bao gồm$S$. Từ$a_i = x_i \oplus y_i$, thay vào đó chúng ta có thể quyết định$a_i$như: 

- tất cả các bit trong$S$phải là 1 
- các bit bổ sung có thể là 1 tùy chọn nếu chúng hỗ trợ các ràng buộc trong tương lai, nhưng bất kỳ bit 1 bổ sung nào cũng chỉ gây tổn hại đến các lựa chọn sau này về mặt từ điển 

Vì vậy, chiến lược tối ưu là thực thi$a_i = x_i \,|\, S$. Bất kỳ bit nào đã được các bước trước yêu cầu đều phải xuất hiện và việc giới thiệu các bit mới chỉ làm tăng thêm$S$, điều này sẽ làm trầm trọng thêm các hạn chế trong tương lai và có khả năng buộc các giá trị lớn hơn theo thứ tự từ điển của$y$. 

Một lần$a_i$đã được sửa, chúng tôi phục hồi$y_i = a_i \oplus x_i$, và cập nhật$S = S \,|\, a_i$. 

Điều này làm giảm vấn đề thành một đường chuyền từ trái sang phải duy nhất để duy trì mặt nạ bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mặt nạ bit$S = 0$. Điều này đại diện cho các bit phải tồn tại trong tất cả các tương lai$a_i$. 
2. Lặp lại trình tự từ trái sang phải. Tại vị trí$i$, tính toán một ứng cử viên$$a_i = x_i \,|\, S.$$Điều này đảm bảo rằng tất cả các bit được yêu cầu trước đó đều được giữ nguyên. 
3. Tính toán$y_i = a_i \oplus x_i$. Đây là giá trị tối thiểu của$y_i$điều đó biến đổi$x_i$vào trong$a_i$, vì XOR lật chính xác các bit ở đó$x_i$Và$a_i$khác nhau. 
4. Cập nhật ràng buộc toàn cục:$$S = S \,|\, a_i.$$Điều này mở rộng tập hợp các bit phải tồn tại trong tương lai. 
5. Đầu ra$y_i$ngay lập tức vì các quyết định trong tương lai không ảnh hưởng đến giá trị trong quá khứ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí$i$, bộ hiện tại$S$bằng với sự kết hợp bitwise của tất cả được xây dựng$a_1, \dots, a_i$. Bất kỳ tương lai hợp lệ nào$a_j$phải bao gồm tất cả các bit trong$S$, nếu không nó sẽ vi phạm điều kiện tăng trưởng với một số chỉ mục trước đó nơi bit đó xuất hiện lần đầu tiên. 

Tại mỗi bước lựa chọn$a_i = x_i | S$cho giá trị nhỏ nhất có thể$a_i$dưới sự ràng buộc này. Bất kỳ sự lựa chọn nhỏ hơn sẽ bỏ lỡ một chút trong$S$và phá vỡ hiệu lực. Bất kỳ sự lựa chọn nào lớn hơn sẽ đưa ra những bit không cần thiết, điều này sẽ làm tăng$y_i$hoặc buộc phải cập nhật trong tương lai chỉ có thể tăng thứ tự từ điển. Vì so sánh từ điển phụ thuộc vào vị trí khác nhau sớm nhất nên giảm thiểu mỗi$y_i$dưới ràng buộc hợp lệ chặt chẽ nhất mang lại mức tối thiểu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        
        S = 0
        res = []
        
        for xi in x:
            ai = xi | S
            yi = ai ^ xi
            res.append(str(yi))
            S |= ai
        
        out.append(" ".join(res))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì mặt nạ ràng buộc đang phát triển`S`. Tại mỗi bước, chúng ta xây dựng giá trị hợp lệ nhỏ nhất$a_i$bằng cách OR-ing với`S`, sau đó rút ra$y_i$thông qua XOR. bản cập nhật`S |= a_i`đảm bảo rằng tất cả các bit đã từng xuất hiện trong bất kỳ$a_i$được bảo toàn cho phần còn lại của chuỗi. 

Một điểm tinh tế là chúng ta không bao giờ cần kiểm tra rõ ràng điều kiện sinh trưởng, bởi vì việc xây dựng`S`đảm bảo nó theo cách quy nạp. Mỗi`a_i`đã chứa tất cả các bit trước đó, vì vậy`a_{i-1} & a_i = a_{i-1}`giữ tự động. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 4
x = [1, 2, 3, 4]
```Chúng tôi theo dõi`S`,`a_i`, Và`y_i`. 

| tôi | x_i | S trước | a_i = x_i | S trước HOẶC x_i | y_i | S sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 0 | 1 | 
| 2 | 2 | 1 | 3 | 3 | 1 | 3 | 
| 3 | 3 | 3 | 3 | 3 | 0 | 3 | 
| 4 | 4 | 3 | 7 | 7 | 3 | 7 | 

Trình tự cuối cùng là$y = [0, 1, 0, 3]$. Mỗi bước đảm bảo rằng tất cả các bit được kích hoạt trước đó vẫn tồn tại, xác nhận điều kiện phát triển được duy trì. 

Bây giờ hãy xem xét:```
x = [5, 4, 3]
```| tôi | x_i | S trước | a_i | y_i | S sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 5 | 0 | 5 | 
| 2 | 4 | 5 | 5 | 1 | 5 | 
| 3 | 3 | 5 | 7 | 4 | 7 | 

Ở đây, bước thứ ba buộc các bit mới phải đáp ứng các ràng buộc trước đó, chứng tỏ mặt nạ chỉ có thể phát triển như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các phép toán bit có thời gian không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được duy trì bất kể kích thước đầu vào | 

Tổng độ phức tạp của tất cả các trường hợp thử nghiệm vẫn tuyến tính ở kích thước đầu vào, phù hợp với ràng buộc$\sum n \le 2 \cdot 10^5$. Điều này tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        x = list(map(int, sys.stdin.readline().split()))
        S = 0
        res = []
        for xi in x:
            ai = xi | S
            yi = ai ^ xi
            res.append(str(yi))
            S |= ai
        out.append(" ".join(res))
    return "\n".join(out)

# provided samples
assert run("""5
4
1 3 7 15
4
1 2 4 8
5
1 2 3 4 5
4
11 13 15 1
1
0
""") == """0 0 0 0
0 1 3 7
0 1 0 3 2
0 2 0 14
0""", "sample tests"

# custom cases
assert run("""1
1
0
""") == "0", "single zero"

assert run("""1
3
0 0 0
""") == "0 0 0", "all zeros"

assert run("""1
3
7 0 0
""") == "0 7 7", "decreasing x forces accumulation"

assert run("""1
4
1 2 4 8
""") == "0 1 3 7", "powers of two case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | trường hợp cơ bản tầm thường | 
| tất cả số không | 0 0 0 | ổn định không có bit | 
| 7 0 0 | 0 7 7 | truyền bá các bit cưỡng bức | 
| sức mạnh của hai | 0 1 3 7 | tăng trưởng bit tích lũy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các bit ban đầu bằng 0. Đối với đầu vào`x = [0, 0, 0]`, thuật toán giữ`S = 0`, vì vậy mỗi`a_i = 0`và mọi`y_i = 0`. Điều kiện phát triển diễn ra bình thường vì tất cả các mặt nạ đều giống hệt nhau. 

Một trường hợp cạnh khác xảy ra khi các phần tử đầu giới thiệu các bit cao mà các phần tử sau không chứa. Vì`x = [1, 2, 4]`, mặt nạ phát triển như sau: sau bước đầu tiên`S = 1`, buộc thứ hai`a_2 = 3`, sau đó`S = 3`, và cuối cùng`a_3 = 7`. Thuật toán tăng chính xác các giá trị sau để bảo toàn các bit trước đó, đảm bảo bao gồm bit đơn điệu ngay cả khi giá trị ban đầu giảm. 

Trường hợp tinh tế cuối cùng là một chuỗi phần tử đơn. Vì không có ràng buộc nào giữa các phần tử nên câu trả lời đơn giản là$y_1 = 0$, bởi vì chọn$a_1 = x_1$luôn hợp lệ và giảm thiểu XOR.
