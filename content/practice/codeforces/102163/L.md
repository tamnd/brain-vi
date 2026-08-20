---
title: "CF 102163L - Đề thi Hóa học"
description: "Bài thi của mỗi học sinh được mã hóa dưới dạng số nguyên. Biểu diễn nhị phân của số nguyên đó ghi lại câu trả lời của học sinh, với 1 nghĩa là câu trả lời cho câu hỏi đó là đúng và 0 nghĩa là câu trả lời sai."
date: "2026-08-19T15:07:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "L"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 613
verified: false
draft: false
---

[CF 102163L - Kỳ thi Hóa học](https://codeforces.com/problemset/problem/102163/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài thi của mỗi học sinh được mã hóa dưới dạng số nguyên. Biểu diễn nhị phân của số nguyên đó ghi lại câu trả lời của học sinh, với một`1`có nghĩa là câu trả lời cho câu hỏi đó là chính xác và`0`nghĩa là nó đã sai. Nhiệm vụ là xác định số câu trả lời đúng cho mỗi học sinh, sao cho mỗi giá trị`A_i`chúng tôi cần số lượng`1`các bit trong biểu diễn nhị phân của nó. 

Ví dụ,`5`là`101`ở dạng nhị phân, vì vậy nó chứa hai`1`bit và học sinh tương ứng nhận được`2`dấu vết. Tương tự,`8`là`1000`, vậy đáp án của nó là`1`. 

Số lượng học sinh trong một ca kiểm tra có thể lên tới`10^5`và mỗi tờ giấy được mã hóa có thể lớn bằng`10^9`. Từ`10^9 < 2^30`, mỗi số có tối đa 30 bit nhị phân liên quan. Một cách tiếp cận thực hiện một lượng công việc không đổi trên mỗi bit đã có hiệu quả tuyến tính trong`N`, bởi vì nó thực hiện tối đa khoảng ba triệu bit hoạt động cho`10^5`sinh viên. Một cách tiếp cận so sánh từng cặp học sinh sẽ yêu cầu khoảng`10^10`so sánh và ngay lập tức là không phù hợp. Vì đầu vào chứa nhiều trường hợp thử nghiệm, nên sử dụng trực tiếp các phép toán bit số nguyên của Python sẽ tốt hơn là mô phỏng rõ ràng các bit. 

Có một vài trường hợp nhỏ trong đó việc triển khai có thể gặp trục trặc. Giấy tờ hợp lệ nhỏ nhất là`1`, có dạng nhị phân là`1`, vậy câu trả lời là`1`. Việc triển khai vô tình bắt đầu kiểm tra các bit từ vị trí`1`thay vì vị trí`0`sẽ tạo ra số không không chính xác. 

Một trường hợp ranh giới khác là lũy thừa của hai. Ví dụ, đầu vào```
1
4
1 2 4 8
```phải sản xuất```
1 1 1 1
```Mọi lũy thừa của hai đều có chính xác một bit được đặt. Mã cố gắng suy ra số điểm từ độ lớn của số thay vì biểu diễn nhị phân của nó có thể dễ dàng mắc lỗi này. 

Trường hợp ngược lại là một số có nhiều bit được đặt. Vì```
1
1
7
```đầu ra là```
3
```bởi vì`7 = 111₂`. Một lỗi phổ biến là đếm số chữ số nhị phân thay vì số`1`các chữ số, điều này cũng có tác dụng với`7`nhưng không đạt được giá trị như`8`, câu trả lời đúng là ở đâu`1`trong khi độ dài nhị phân là`4`. 

Cuối cùng, ranh giới trên quan trọng vì`10^9`không phải là một giá trị nhị phân đặc biệt. Câu trả lời của nó là số lượng bit được đặt trong biểu diễn nhị phân thông thường của nó và số nguyên Python xử lý trực tiếp giá trị này mà không bị tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra biểu diễn nhị phân của số học sinh và đếm số đó.`1`bit. Một cách để làm điều này mà không cần xây dựng chuỗi là kiểm tra liên tục bit có trọng số thấp nhất bằng`x & 1`, thêm nó vào câu trả lời và dịch chuyển`x`đúng một vị trí. Phương pháp này đúng vì mỗi lần lặp sẽ loại bỏ chính xác một chữ số nhị phân và mọi chữ số gốc`1`đóng góp chính xác một lần vào số đếm. 

Vì mọi`A_i`nhiều nhất là`10^9`, cần tối đa 30 lần lặp cho một số. Vì`N = 10^5`, điều đó có nghĩa nhiều nhất là khoảng`3 × 10^6`kiểm tra bit cho một trường hợp thử nghiệm. Điều đó không tệ về mặt tiệm cận, nhưng Python có thể thực hiện thao tác tương tự hiệu quả hơn bằng cách sử dụng phép toán số nguyên tích hợp của nó`int.bit_count()`, thực hiện số lượng popcount trong mã gốc được tối ưu hóa. 

Quan sát quan trọng là dấu bắt buộc chính xác là số lượng tổng thể của số nguyên. Không có sự tương tác giữa các học sinh và không cần phải tự xây dựng lại các câu hỏi. Mỗi`A_i`có thể được xử lý độc lập và`A_i.bit_count()`trả về trực tiếp số bit đã đặt trong biểu diễn nhị phân của nó. 

Phương pháp brute-force hoạt động vì số lượng bit bị giới hạn, nhưng nó vẫn thực hiện vòng lặp trong Python cho từng bit của mỗi số. Quan sát thấy rằng câu trả lời chính xác là một số nguyên cho phép chúng tôi giảm bài tập của mỗi học sinh thành một phép toán được tối ưu hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N log A) | O(1) phụ trợ | Được chấp nhận nhưng vòng lặp cấp độ Python không cần thiết | 
| Tối ưu | O(N log A) có độ phức tạp bit, với số lượng gốc trên mỗi số nguyên | O(N) cho đầu vào/đầu ra được lưu trữ | Đã chấp nhận | 

Đây`A`biểu thị giá trị giấy được mã hóa tối đa. Với`A ≤ 10^9`,`log A`tối đa là khoảng 30. Ưu điểm thực tế của phiên bản tối ưu là việc đếm bit được thực hiện bởi bộ máy số nguyên của Python chứ không phải là một vòng lặp Python rõ ràng. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và xử lý từng test case một cách độc lập. Kết quả của mỗi học sinh chỉ phụ thuộc vào bài viết được mã hóa của học sinh đó nên không có trạng thái nào cần phải chia sẻ giữa các ca kiểm tra. 
2. Đọc`N`bài thi được mã hóa thành một mảng. Bản thân các giá trị đã là thông tin đầy đủ cần thiết để tính điểm. 
3. Với mọi giá trị`x`, tính toán`x.bit_count()`. Điều này trả về chính xác số lượng vị trí chứa`1`trong biểu diễn nhị phân của`x`. 
4. Lưu trữ số lượng kết quả theo thứ tự giống như số học sinh xuất hiện trong đầu vào. Việc duy trì thứ tự này là cần thiết vì đầu ra tương ứng từng vị trí với mảng đầu vào. 
5. In tất cả số lượng của ca kiểm thử hiện tại trên một dòng. 

### Tại sao nó hoạt động 

Với mọi số nguyên dương`x`, biểu diễn nhị phân của nó chứa một chữ số nhị phân cho mỗi câu hỏi được biểu thị bằng số nguyên. Một câu trả lời đúng tương ứng với một`1`, vậy điểm của học sinh chính xác là số`1`chữ số. của Python`x.bit_count()`trả về chính xác số lượng đó. Vì hoạt động được áp dụng độc lập cho mọi`A_i`, mọi giá trị đầu ra là điểm chính xác cho học sinh tương ứng và việc giữ nguyên thứ tự đầu vào sẽ duy trì thứ tự học sinh được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = [x.bit_count() for x in a]
        print(*ans)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên của`solve`đọc số lượng ca kiểm thử. Sau đó, vòng lặp bên ngoài sẽ tách biệt quá trình xử lý từng bài kiểm tra. 

Đối với mỗi trường hợp thử nghiệm,`map(int, input().split())`chuyển đổi các giấy tờ được mã hóa thành số nguyên Python. Việc hiểu danh sách được áp dụng`bit_count()`độc lập với mọi giá trị và tạo ra các dấu theo cùng một thứ tự. 

Không cần xử lý đặc biệt lũy thừa của hai, các số có nhiều bit được đặt hoặc giới hạn trên`10^9`. Số nguyên có độ chính xác tùy ý của Python cũng có nghĩa là không có vấn đề tràn số nguyên. 

Mã giả định đầu vào tuân theo định dạng đã nêu, trong đó tất cả`N`các giá trị cho một trường hợp thử nghiệm xảy ra ở dòng tiếp theo. Điều này là đủ cho vấn đề nhất định. Đầu ra được tạo ra với`print(*ans)`, đặt dấu cách giữa các câu trả lời và kết thúc dòng một cách chính xác. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm đầu tiên của mẫu, các giá trị đầu vào là`1, 2, 3, 4, 5`. Trạng thái quan trọng là số nguyên hiện tại và biểu diễn nhị phân của nó. 

| Sinh viên | Giá trị | Nhị phân |`bit_count()`| Đánh dấu | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`1`| 1 | 1 | 
| 2 | 2 |`10`| 1 | 1 | 
| 3 | 3 |`11`| 2 | 2 | 
| 4 | 4 |`100`| 1 | 1 | 
| 5 | 5 |`101`| 2 | 2 | 

Dòng kết quả là`1 1 2 1 2`. Điều này chứng tỏ rằng bản thân giá trị chỉ đơn giản là mã hóa các câu trả lời và chỉ các bit được đặt của nó mới quan trọng. 

Đối với trường hợp thử nghiệm thứ hai, các giá trị là lũy thừa của hai. 

| Sinh viên | Giá trị | Nhị phân |`bit_count()`| Đánh dấu | 
| --- | --- | --- | --- | --- | 
| 1 | 2 |`10`| 1 | 1 | 
| 2 | 4 |`100`| 1 | 1 | 
| 3 | 8 |`1000`| 1 | 1 | 
| 4 | 16 |`10000`| 1 | 1 | 

Đầu ra là`1 1 1 1`. Đây là dấu vết ranh giới hữu ích vì mỗi giá trị có chính xác một bit được đặt mặc dù biểu diễn nhị phân có độ dài khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Độ phức tạp bit O(N log A) | Mỗi giá trị có nhiều nhất`log₂(A) + 1`chữ số nhị phân và mỗi chữ số được tính một lần | 
| Không gian | O(N) | Mảng đầu vào và mảng đầu ra chứa`N`số nguyên | 

Với`A ≤ 10^9`, mỗi số có tối đa 30 chữ số nhị phân. Như vậy khối lượng công việc tăng trưởng tuyến tính với số lượng học sinh, với hệ số hằng số rất nhỏ. Việc thực hiện sử dụng bản địa`bit_count()`hoạt động, do đó nó tránh được vòng lặp cấp Python trên 30 bit đó. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB cho`N = 10^5`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(" ".join(str(x.bit_count()) for x in a))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided sample
assert run(
    """2
5
1 2 3 4 5
4
2 4 8 16
"""
) == """1 1 2 1 2
1 1 1 1
""", "sample 1"

# Minimum-size case
assert run(
    """1
1
1
"""
) == """1
""", "minimum-size input"

# All values equal
assert run(
    """1
5
15 15 15 15 15
"""
) == """4 4 4 4 4
""", "all-equal values"

# Boundary values, including the largest allowed value
assert run(
    """1
6
1 2 3 4 5 1000000000
"""
) == """1 1 2 1 2 13
""", "boundary values"

# Maximum-size case
max_n = 100000
inp = "1\n" + str(max_n) + "\n" + ("1 " * (max_n - 1)) + "1\n"
expected = " ".join(["1"] * max_n) + "\n"

assert run(inp) == expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Đầu vào hợp lệ tối thiểu và giấy được mã hóa thấp nhất có thể | 
|`5 / 15 15 15 15 15`|`4 4 4 4 4`| Các giá trị lặp lại và một số có nhiều bit được đặt | 
|`1 2 3 4 5 1000000000`|`1 1 2 1 2 13`| lũy thừa của hai, giá trị liền kề và ranh giới số tối đa | 
|`100000`bản sao của`1`|`100000`bản sao của`1`| Tối đa`N`và bảo quản trật tự đầu ra | 

## Vỏ cạnh 

Giá trị tối thiểu`1`được biểu diễn dưới dạng`1₂`, do đó đầu vào```
1
1
1
```sản xuất```
1
```Thuật toán gọi`1.bit_count()`, trả về`1`. Không có giá trị 0 trong các ràng buộc đầu vào, do đó không cần xử lý biểu diễn nhị phân trống. 

lũy thừa của hai chứa chính xác một bit được đặt. Ví dụ,```
1
4
1 2 4 8
```cho```
1 1 1 1
```Các giá trị có độ dài nhị phân khác nhau, nhưng mỗi biểu diễn chứa chính xác một`1`. Điều này phát hiện các triển khai vô tình đếm các chữ số nhị phân thay vì đặt bit. 

Một giá trị với các bit được đặt liên tiếp sẽ thực hiện phía bên kia của biểu diễn. Vì```
1
1
15
```chúng tôi có`15 = 1111₂`, vậy câu trả lời là```
4
```cuộc gọi`15.bit_count()`đếm trực tiếp tất cả bốn vị trí đã đặt. 

Giá trị tối đa được phép cũng không cần số học đặc biệt. Vì```
1
1
1000000000
```biểu diễn nhị phân chứa 13 bit được đặt, do đó đầu ra là```
13
```Python xử lý chính xác giá trị này và`bit_count()`hoạt động mà không có bất kỳ mối lo ngại nào về tràn hoặc số nguyên có dấu. 

Cuối cùng, một trường hợp thử nghiệm có chứa`100000`học sinh kiểm tra thang đo của thuật toán. Nếu mỗi tờ giấy đều`1`, mọi câu trả lời đều`1`, vì vậy đầu ra chứa`100000`những cái đó. Mỗi học sinh được xử lý độc lập và thuật toán không vô tình hợp nhất các giá trị liền kề hoặc làm mất thứ tự của chúng.
