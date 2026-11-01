---
title: "CF 1064B - Phương trình phép thuật toán học"
description: "Phương trình liên kết một số a với một biến x bằng hai phép toán: phép trừ và XOR theo bit. Với mỗi giá trị cho trước của a, chúng ta được yêu cầu đếm xem có bao nhiêu số nguyên không âm x thỏa mãn đẳng thức a - (a XOR x) - x = 0. Đầu vào bao gồm một số giá trị độc lập của a."
date: "2026-06-15T08:28:25+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1064
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 516 (Div. 2, by Moscow Team Olympiad)"
rating: 1200
weight: 1064
solve_time_s: 185
verified: true
draft: false
---

[CF 1064B - Phương trình phép thuật toán học](https://codeforces.com/problemset/problem/1064/B) 

**Đánh giá:** 1200 
**Thẻ:** toán 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Phương trình liên kết một số`a`với một biến`x`sử dụng hai phép toán: phép trừ và XOR theo bit. Với mỗi giá trị đã cho của`a`, chúng ta được yêu cầu đếm có bao nhiêu số nguyên không âm`x`thỏa mãn danh tính`a - (a XOR x) - x = 0`. 

Đầu vào bao gồm một số giá trị độc lập của`a`. Đối với mỗi người, chúng ta phải xác định có bao nhiêu lựa chọn về`x`làm cho biểu thức đánh giá chính xác bằng 0. 

Một hạn chế quan trọng đó là`a`có thể lớn như ngay dưới`2^30`, có nghĩa là mọi số đều nằm trong phạm vi 30 bit nhị phân. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào lặp đi lặp lại trên tất cả các khả năng có thể`x`lên tới`a`là không khả thi, vì điều đó sẽ có thể lên tới một tỷ thao tác cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Ngay cả việc lặp lại tất cả các số 30 bit cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm nếu được thực hiện với mô phỏng bitwise nặng. 

Một cách tiếp cận ngây thơ sẽ thử mọi cách có thể`x`và trực tiếp đánh giá biểu thức. Điều này đúng về mặt logic, nhưng nó không thành công về mặt tính toán vì không gian tìm kiếm có số mũ theo số bit. 

Trường hợp cạnh xuất hiện khi`a`có rất ít bit được đặt hoặc khi nó được lấp đầy bằng các bit ở dạng nhị phân. Ví dụ, khi`a = 0`, sự lựa chọn hợp lệ duy nhất là`x = 0`. Khi`a = 2`, nhị phân`10`, có đúng hai lựa chọn hợp lệ,`x = 0`Và`x = 2`. Khi`a = 2^30 - 1`, mỗi bit là một và số lượng hợp lệ`x`trở nên cực kỳ lớn, đặc biệt là tất cả`2^30`mặt nạ phụ. Một giải pháp đúng đắn phải xử lý những thái cực này mà không cần liệt kê các ứng cử viên. 

## Phương pháp tiếp cận 

Chiến lược bạo lực xử lý vấn đề trực tiếp từ định nghĩa. Đối với mỗi`a`, chúng tôi cố gắng hết sức có thể`x`từ`0`đến một giới hạn lớn và kiểm tra xem`a - (a XOR x) - x`bằng không. Điều này hoạt động vì nó tuân theo chính xác câu lệnh và XOR rất dễ tính toán. Tuy nhiên, kể từ khi`a`có thể có tới 30 bit, không gian có thể`x`giá trị lên đến`2^30`. Ngay cả khi cắt tỉa, điều này vẫn vượt xa giới hạn khả thi. 

Quan sát chính xuất phát từ việc viết lại biểu thức bằng cách sử dụng nhận dạng XOR tiêu chuẩn:`a + x = (a XOR x) + 2 * (a AND x)`. 

Bằng cách sắp xếp lại phương trình ban đầu, chúng ta có thể loại bỏ hoàn toàn XOR và biểu thị điều kiện dưới dạng chồng chéo bit giữa`a`Và`x`. Việc đơn giản hóa cho thấy phương trình đúng khi và chỉ khi`x`không chứa bất kỳ bit nào ở đâu`a`có một bit 0. Nói cách khác, mỗi bit được đặt trong`x`cũng phải được đặt trong`a`. 

Điều này có nghĩa`x`chỉ đơn giản là một mặt nạ con của`a`. Mỗi bit được đặt trong`a`có thể được chọn hoặc không được chọn một cách độc lập`x`, trong khi tất cả các bit 0 trong`a`lực lượng`x`cũng có số 0 ở vị trí đó. Nếu như`a`có`k`đặt bit, sau đó có chính xác`2^k`những lựa chọn hợp lệ cho`x`. 

Điều này biến bài toán từ việc kiểm tra các ràng buộc trên các biểu thức số học thành một bài toán đếm tổ hợp đơn giản trên các tập con bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^30) mỗi bài kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(log a) mỗi bài kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng giá trị của`a`một cách độc lập. Mỗi trường hợp thử nghiệm đều bị cô lập nên không có trạng thái nào được chia sẻ. 
2. Đếm xem có bao nhiêu bit được đặt trong biểu diễn nhị phân của`a`. Giá trị này thể hiện có bao nhiêu vị trí có sẵn để lựa chọn độc lập trong việc hình thành`x`. 
3. Tính số hợp lệ`x`BẰNG`2^(popcount(a))`. Mỗi bit được đặt đóng góp một quyết định nhị phân: hoặc đưa nó vào`x`hay không. 
4. Xuất giá trị tính toán cho từng test. 

### Tại sao nó hoạt động 

Phương trình buộc tất cả các bit của`x`được chứa trong các bit được thiết lập của`a`. Không còn tương tác mang hoặc phụ thuộc bit chéo sau khi đơn giản hóa. Mỗi tập bit của`a`hoạt động độc lập vì các tương tác XOR và phép trừ hủy bỏ chính xác khi được viết lại bằng cách sử dụng nhận dạng bitwise. Tính độc lập này đảm bảo rằng mọi tập hợp con của các bit được đặt trong`a`tương ứng với chính xác một hợp lệ`x`và không có cấu hình không hợp lệ nào có thể thỏa mãn phương trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a = int(input())
        # number of valid x is 2^(number of set bits in a)
        print(1 << a.bit_count())

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên tính năng tích hợp sẵn của Python`bit_count()`tính toán trực tiếp số lượng bit được thiết lập một cách hiệu quả. Câu trả lời cuối cùng thu được bằng cách sử dụng phép dịch trái, phép tính lũy thừa của hai trong thời gian không đổi. 

Một cạm bẫy phổ biến là cố gắng mô phỏng phương trình một cách trực tiếp. Cách tiếp cận đó tạo ra độ phức tạp số học không cần thiết và che khuất cấu trúc tập hợp con đơn giản ẩn trong biểu diễn bit. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem quá trình chuyển đổi hoạt động như thế nào. 

### Ví dụ 1:`a = 2`(nhị phân`10`) 

| Bước | một (nhị phân) | số dân(a) | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 10 | - | - | 
| Đếm bit | 10 | 1 | 1 | 
| Tính 2^k | - | 1 | 2 | 

Các giá trị hợp lệ của`x`là`00`Và`10`. Cả hai đều thỏa mãn phương trình vì chúng chỉ sử dụng các bit được cho phép bởi`a`. 

### Ví dụ 2:`a = 0`| Bước | một (nhị phân) | số dân(a) | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 0 | - | - | 
| Đếm bit | 0 | 0 | 0 | 
| Tính 2^k | - | 0 | 1 | 

Chỉ một`x = 0`tồn tại vì không có bit sẵn có nào để lựa chọn. 

Những ví dụ này xác nhận rằng giải pháp rút gọn chính xác việc đếm các tập hợp con của các bit đã đặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · log a) | Mỗi trường hợp thử nghiệm yêu cầu đếm số bit trong`a`, tỷ lệ thuận với số bit (lên tới 30). | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ nào được sử dụng ngoài các biến cho từng trường hợp thử nghiệm. | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí 1000 trường hợp thử nghiệm chỉ yêu cầu các thao tác bit đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a = int(input())
        out.append(str(1 << a.bit_count()))
    return "\n".join(out)

# provided samples
assert run("3\n0\n2\n1073741823\n") == "1\n2\n1073741824"

# minimum case
assert run("1\n0\n") == "1"

# single bit
assert run("1\n2\n") == "2"

# all ones (3 bits)
assert run("1\n7\n") == "8"

# random case
assert run("1\n5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| trường hợp cơ sở không có bit thiết lập | 
|`2`|`2`| hành vi bit đơn | 
|`7`|`8`| vụ nổ tập hợp con đầy đủ | 
|`5`|`4`| độ chính xác của mẫu bit hỗn hợp | 

## Vỏ cạnh 

Khi nào`a = 0`, biểu diễn nhị phân không có bit nào được đặt. Thuật toán tính toán`popcount(a) = 0`, do đó kết quả trở thành`2^0 = 1`. Điều này phù hợp với thực tế là chỉ`x = 0`có thể tránh đưa ra các bit không hợp lệ. 

Khi`a`là sức mạnh của hai như`8`, chỉ có một bit được thiết lập. Thuật toán mang lại`2^1 = 2`, tương ứng chính xác với việc chọn tập con trống hoặc bit đơn đó. 

Khi`a = 2^30 - 1`, mọi bit đều được thiết lập. Thuật toán tính toán`2^30`, khớp với số lượng mặt nạ con hợp lệ tối đa có thể. Không có vấn đề tràn phát sinh vì số nguyên Python xử lý lũy thừa lớn của 2 một cách tự nhiên.
