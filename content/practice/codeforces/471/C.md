---
title: "CF 471C - MUH và Nhà Thẻ"
description: "Một ngôi nhà bằng thẻ được xây dựng từ các tầng. Nếu một tầng có k phòng thì mỗi phòng dùng hai lá bài nghiêng nên bản thân các phòng tiêu tốn 2k thẻ. Giữa các phòng liền kề và phía trên các phòng ngoài cùng có trần làm bằng ván ngang."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 471
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 269 (Div. 2)"
rating: 1700
weight: 471
solve_time_s: 102
verified: true
draft: false
---

[CF 471C - MUH và House of Cards](https://codeforces.com/problemset/problem/471/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một ngôi nhà bằng thẻ được xây dựng từ các tầng. Nếu một tầng chứa`k`phòng thì mỗi phòng dùng 2 thẻ nghiêng nên các phòng tự tiêu thụ`2k`thẻ. Giữa các phòng liền kề và phía trên các phòng ngoài cùng có trần làm bằng ván ngang. một hàng`k`nhu cầu phòng`k - 1`thẻ trần dùng chung giữa các phòng lân cận và thêm một thẻ trần phía trên mỗi vị trí phòng, mang lại tổng cộng`k - 1 + 1 = k`thẻ ngang. 

Vì vậy, một tầng với`k`phòng sử dụng:$$2k + (k-1)=3k-1$$thẻ. 

Số lượng phòng chắc chắn sẽ giảm đi khi chúng ta di chuyển lên phía trên ngôi nhà. Nếu các tầng chứa$$a_1 > a_2 > \dots > a_h \ge 1$$phòng thì tổng số thẻ là$$\sum_{i=1}^{h}(3a_i-1) = 3\sum a_i - h.$$Chúng tôi được cung cấp chính xác`n`thẻ và phải đếm có bao nhiêu độ cao khác nhau`h`là có thể. Chiều cao hợp lệ nếu tồn tại một chuỗi số phòng dương giảm dần có tổng mức tiêu thụ thẻ chính xác là`n`. 

Sự hạn chế là phần thú vị.`n`có thể lớn như`10^12`. Bất kỳ cách tiếp cận nào liệt kê các phân vùng hoặc khám phá sự kết hợp của kích thước sàn đều vô vọng. Thậm chí`O(n)`là quá lớn. Chúng ta cần một cái gì đó gần hơn với độ phức tạp logarit hoặc căn bậc hai. 

Một trường hợp tế nhị xảy ra khi không có ngôi nhà nào hợp lệ cả. Ví dụ:```
6
```Công năng sử dụng nhà một tầng nhỏ nhất`2`thẻ, công dụng nhỏ nhất của nhà hai tầng`7`thẻ và không có cấu hình hợp lệ nào tiêu thụ chính xác`6`. Câu trả lời là`0`. 

Một sai lầm dễ mắc phải khác là quên điều kiện rút gọn nghiêm ngặt. Coi như:```
13
```Sử dụng số phòng`(2,2)`sẽ tiêu thụ`10`thẻ, nhưng nó là bất hợp pháp vì các tầng không giảm nghiêm ngặt. Chỉ cho phép các chuỗi giảm nghiêm ngặt. 

Nguồn lỗi thứ ba là tính riêng các phân phối phòng khác nhau. Bài toán yêu cầu số độ cao có thể có chứ không phải số nhà. Nếu một số cấu hình sàn khác nhau có cùng chiều cao thì chúng chỉ đóng góp một lần. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp là tạo ra tất cả các chuỗi số nguyên dương giảm nghiêm ngặt, tính toán mức sử dụng thẻ của chúng và kiểm tra xem nó có bằng không.`n`. Điều này đúng vì mỗi nhà hợp pháp đều tương ứng với một trình tự như vậy. Thật không may, số lượng các chuỗi giảm dần tăng lên một cách bùng nổ. Ngay cả đối với các giá trị vừa phải của`n`, không gian tìm kiếm giống với số lượng phân vùng số nguyên, vượt xa những gì có thể khám phá khi`n`đạt tới`10^12`. 

Nhận xét quan trọng đến từ việc viết lại công thức thẻ:$$n = 3\sum a_i - h.$$Sắp xếp lại mang lại$$\sum a_i = \frac{n+h}{3}.$$Đối với chiều cao cố định`h`, bài toán trở thành: 

Chúng ta có thể viết được không$$S=\frac{n+h}{3}$$như một tổng của`h`số nguyên dương phân biệt? 

Đây là một câu hỏi lý thuyết số cổ điển. 

Tổng nhỏ nhất có thể của`h`số nguyên dương riêng biệt thu được từ$$1+2+\cdots+h = \frac{h(h+1)}2.$$Khi chúng ta có một tập hợp như vậy, việc tăng phần tử lớn nhất lên bất kỳ lượng không âm nào sẽ bảo toàn tính khác biệt. Do đó, mọi tổng số nguyên ít nhất$$\frac{h(h+1)}2$$là có thể đạt được. 

Vì vậy chiều cao`h`có giá trị chính xác khi có hai điều kiện:$$n+h \equiv 0 \pmod 3$$Và$$\frac{n+h}{3}\ge \frac{h(h+1)}2.$$Bất đẳng thức thứ hai trở thành$$h^2+h \le \frac{2(n+h)}3.$$Sau khi đơn giản hóa:$$3h^2+h-2n \le 0.$$Điều này ngay lập tức đưa ra giới hạn trên của khoảng$$h \le \sqrt{\frac{2n}{3}},$$đó là dưới một triệu ngay cả đối với`n = 10^{12}`. Chúng ta có thể chỉ cần kiểm tra mọi độ cao có thể cho đến giới hạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các độ cao có thể`h`bắt đầu từ`1`. 
2. Dừng lại khi`3h² + h > 2n`. Đối với chiều cao lớn hơn, số lượng thẻ yêu cầu tối thiểu đã vượt quá`n`, vì vậy không có chiều cao nào nữa có thể hợp lệ. 
3. Kiểm tra xem`n + h`chia hết cho`3`. Nếu không thì`S = (n+h)/3`không phải là số nguyên và không có ngôi nhà có chiều cao`h`tồn tại. 
4. Nếu tính chia hết được giữ nguyên, hãy xác minh điều kiện tổng tối thiểu$$\frac{n+h}{3} \ge \frac{h(h+1)}2.$$Điều này tương đương với đạo hàm ở trên. 
5. Nếu điều kiện được thỏa mãn, hãy tăng câu trả lời. 
6. Sau khi đã kiểm tra tất cả các độ cao khả thi, hãy đưa ra số đếm. 

### Tại sao nó hoạt động 

Cho mọi chiều cao`h`, mọi ngôi nhà hợp pháp đều tương ứng với một dãy giảm dần của`h`số lượng phòng tích cực. Công thức tổng bài chuyển vấn đề thành câu hỏi liệu`(n+h)/3`có thể được biểu diễn dưới dạng tổng của`h`số nguyên dương phân biệt. 

Một khoản tiền`h`các số nguyên dương phân biệt tồn tại chính xác khi nó ít nhất`1+2+\cdots+h`. Mọi giá trị lớn hơn đều có thể đạt được bằng cách tăng phần tử lớn nhất trong khi vẫn giữ được tính khác biệt. Như vậy điều kiện chia hết và điều kiện tổng nhỏ nhất đều cần và đủ. Thuật toán kiểm tra chính xác các điều kiện này cho mọi chiều cao khả thi, vì vậy mọi chiều cao hợp lệ sẽ được tính một lần và mọi chiều cao không hợp lệ sẽ bị từ chối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ans = 0
    h = 1

    while 3 * h * h + h <= 2 * n:
        if (n + h) % 3 == 0:
            s = (n + h) // 3
            if s >= h * (h + 1) // 2:
                ans += 1
        h += 1

    print(ans)

solve()
```Biến`h`đại diện cho chiều cao hiện đang được thử nghiệm. 

Điều kiện vòng lặp xuất phát trực tiếp từ bất đẳng thức$$3h^2+h-2n \le 0.$$Một khi nó thất bại, ngay cả ngôi nhà tối thiểu có thể có chiều cao đó cũng cần nhiều thẻ hơn mức có sẵn, do đó, không có chiều cao lớn hơn nào có thể hoạt động được. 

Việc kiểm tra tính chia hết được thực hiện trước khi tính toán`s`. Không có nó, phép chia số nguyên sẽ âm thầm chấp nhận những trường hợp không thể. 

Sự so sánh chống lại`h(h+1)/2`mã hóa sự tồn tại của một biểu diễn dưới dạng tổng của`h`số nguyên dương phân biệt. Vì số nguyên Python có độ chính xác tùy ý nên không có rủi ro tràn ngay cả khi`n = 10^{12}`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
13
```| h | n+h | Chia hết cho 3? | S=(n+h)/3 | Tổng tối thiểu h(h+1)/2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 14 | Không | - | 1 | Không | 
| 2 | 15 | Có | 5 | 3 | Có | 

Độ cao tiếp theo là không thể bởi vì`3·3²+3 = 30 > 26`. 

Trả lời:```
1
```Điều này chứng tỏ rằng chỉ có chiều cao`2`hoạt động, mặc dù có thể tồn tại nhiều bố cục tầng. 

### Ví dụ 2 

đầu vào:```
6
```| h | n+h | Chia hết cho 3? | S=(n+h)/3 | Tổng tối thiểu h(h+1)/2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7 | Không | - | 1 | Không | 
| 2 | 8 | Không | - | 3 | Không | 

Vòng lặp dừng lại vì`3·3²+3 = 30 > 12`. 

Trả lời:```
0
```Ví dụ này cho thấy rằng việc có đủ thẻ cho một số công trình không có nghĩa là có một ngôi nhà hợp lệ tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Độ cao chỉ được kiểm tra đến tận gốc dương của`3h²+h−2n≤0`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Vì`n = 10^12`, chiều cao được thử nghiệm lớn nhất là khoảng`8 × 10^5`. Một vòng lặp đơn giản trên nhiều giá trị đó có thể dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        n = int(input())

        ans = 0
        h = 1

        while 3 * h * h + h <= 2 * n:
            if (n + h) % 3 == 0:
                s = (n + h) // 3
                if s >= h * (h + 1) // 2:
                    ans += 1
            h += 1

        return str(ans)

    return solve()

# provided sample
assert run("13\n") == "1", "sample 1"

# custom cases
assert run("1\n") == "0", "minimum input"
assert run("2\n") == "1", "single room house"
assert run("6\n") == "0", "no valid house"
assert run("7\n") == "1", "smallest two-floor house"
assert run("1000000000000\n").isdigit(), "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Dưới mức thẻ tối thiểu cần thiết cho bất kỳ nhà cái nào | 
|`2`|`1`| Cấu hình một tầng hợp lệ nhỏ nhất | 
|`6`|`0`| Không tồn tại đại diện hợp lệ | 
|`7`|`1`| Nhà có chiều cao hợp lệ nhỏ nhất | 
|`1000000000000`| Câu trả lời bằng số | Hiệu suất gần hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
```Điều kiện vòng lặp`3h²+h≤2n`đã thất bại rồi`h=1`bởi vì`4>2`. Không có chiều cao được kiểm tra và câu trả lời vẫn còn`0`. Điều này phản ánh chính xác rằng ngay cả một phòng đơn cũng cần có hai thẻ. 

Coi như:```
6
```Vì`h=1`,`n+h=7`, không chia hết cho ba. Vì`h=2`,`n+h=8`, cũng không chia hết cho ba. Không có chiều cao hợp lệ tồn tại. Việc triển khai bất cẩn chỉ kiểm tra bất đẳng thức thẻ tối thiểu sẽ chấp nhận một số chiều cao không chính xác. 

Coi như:```
7
```Vì`h=2`,$$S=\frac{7+2}{3}=3.$$Tổng nhỏ nhất của hai số nguyên dương phân biệt cũng là`3`, cụ thể là`1+2`. Điều kiện được thỏa mãn chính xác tại biên, tạo ra một chiều cao hợp lệ. Trường hợp này bắt gặp từng sai lầm trong đó`>`được sử dụng thay vì`>=`. 

Cuối cùng, hãy xem xét:```
13
```Vì`h=2`, chúng tôi có được`S=5`, lớn hơn tổng tối thiểu`3`. Sự đại diện`1+4`tồn tại, cho một ngôi nhà hai tầng hợp lệ. Thuật toán đếm chiều cao một lần bất kể có bao nhiêu cách phân bổ phòng tạo ra chiều cao đó, phù hợp với yêu cầu của bài toán.
