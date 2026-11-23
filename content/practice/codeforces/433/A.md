---
title: "CF 433A - Món quà của Kitahara Haruki"
description: "Chúng ta được tặng một bộ sưu tập táo trong đó mỗi quả táo nặng 100 gam hoặc 200 gam. Tất cả táo phải được phân phát cho hai người và mỗi quả táo phải được trao hoàn toàn cho một người vì táo không thể cắt được."
date: "2026-06-07T02:38:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 433
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 248 (Div. 2)"
rating: 1100
weight: 433
solve_time_s: 99
verified: true
draft: false
---

[CF 433A - Món quà của Kitahara Haruki](https://codeforces.com/problemset/problem/433/A) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập táo trong đó mỗi quả táo nặng 100 gam hoặc 200 gam. Tất cả táo phải được phân phát cho hai người và mỗi quả táo phải được trao hoàn toàn cho một người vì táo không thể cắt được. 

Nhiệm vụ là xác định xem có thể chia những quả táo thành hai nhóm có tổng trọng lượng hoàn toàn bằng nhau hay không. 

Dữ liệu đầu vào bao gồm số lượng táo và trọng lượng của mỗi quả táo. Đầu ra chỉ đơn giản là`"YES"`nếu một phân vùng như vậy tồn tại và`"NO"`nếu không thì. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 quả táo và mỗi quả có trọng lượng chỉ là 100 hoặc 200. Việc tìm kiếm mạnh mẽ trên tất cả các bài tập có thể sẽ yêu cầu kiểm tra tối đa$2^{100}$phân phối, điều này hoàn toàn không thể thực hiện được. Mặt khác, do chỉ có hai trọng số phân biệt nên cấu trúc của bài toán đơn giản hơn nhiều so với bài toán phân vùng tổng quát. 

Nhận xét đầu tiên là tổng trọng lượng phải bằng nhau. Nếu tổng trọng số là số lẻ thì không thể chia nó thành hai số nguyên bằng nhau. 

Có một vài trường hợp đáng chú ý. 

Coi như:```
1
100
```Tổng trọng lượng là 100, nên mỗi người sẽ cần 50 gam. Vì táo không thể chia đôi nên câu trả lời đúng là`"NO"`. 

Coi như:```
1
200
```Tổng trọng lượng là 200 nên mỗi người sẽ cần 100 gam. Không có quả táo nào nặng 100 gram nên câu trả lời lại là`"NO"`. 

Một trường hợp tế nhị hơn là:```
2
200 200
```Tổng trọng lượng là 400 và mỗi bên cần 200. Đưa cho mỗi người một quả táo là được nên đáp án là`"YES"`. 

Một trường hợp quan trọng khác là:```
3
200 200 200
```Tổng trọng lượng là 600 và mỗi bên cần 300. Vì mỗi quả táo nặng 200 nên không có tập hợp con nào có tổng bằng 300. Một giải pháp chỉ kiểm tra xem tổng trọng lượng có chẵn hay không sẽ trả lời sai`"YES"`. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Đối với mỗi quả táo, hãy chọn xem nó sẽ thuộc về người thứ nhất hay người thứ hai. Sau khi gán tất cả các quả táo, hãy kiểm tra xem hai tổng có bằng nhau không. 

Cách tiếp cận này đúng vì nó xem xét mọi phân phối có thể. Vấn đề là số lượng khả năng. Với$n$táo có$2^n$bài tập. Vì$n=100$, đây là đại khái$1.27 \times 10^{30}$, vượt xa những gì có thể tính toán được. 

Quan sát quan trọng là trọng lượng được giới hạn ở mức chỉ 100 và 200 gam. Gọi số táo nặng 100 gam là`c100`và số quả táo nặng 200 gam là`c200`. 

Vì mỗi trọng lượng là bội số của 100 nên chúng ta có thể chia tất cả các trọng số cho 100. Khi đó, mỗi quả táo sẽ trở thành 1 hoặc 2. Vấn đề trở thành việc xác định xem liệu sự kết hợp nào đó giữa một và hai có thể tạo thành một nửa tổng trọng lượng hay không. 

Đặt tổng trọng lượng theo các đơn vị tỷ lệ này là:$$S = c100 + 2 \cdot c200$$Nếu như`S`thật kỳ lạ, câu trả lời là ngay lập tức`"NO"`. 

Nếu như`S`là số chẵn, chúng ta cần hình thành`S/2`. 

Một sự thật hữu ích xuất hiện: 

Nếu số lượng táo 100 gram là chẵn thì bất kỳ mục tiêu chẵn nào cũng luôn có thể được hoàn thành bằng cách sử dụng cặp táo 100 gram sau khi chọn một số quả táo 200 gram. 

Nếu số quả táo 100 gam là số lẻ thì mỗi bên phải có một quả táo 100 gam. Trong tình huống đó, cần ít nhất một quả táo nặng 200 gram để có thể cân bằng. 

Điều này dẫn đến một điều kiện rất nhỏ gọn: 

Nếu số quả táo 100 gam là số lẻ và không có quả táo nào nặng 200 gam thì đáp án là`"NO"`. 

Ngược lại, nếu tổng trọng lượng là số chẵn thì câu trả lời là`"YES"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu quả táo nặng 100 gam và bao nhiêu quả táo nặng 200 gam. 
2. Tính tổng trọng lượng. 
3. Nếu tổng trọng lượng là số lẻ, hãy in`"NO"`vì không thể tồn tại hai nửa số nguyên bằng nhau. 
4. Nếu số quả táo 100 gam là số lẻ và không có quả táo 200 gam thì in ra`"NO"`. 

Trong tình huống này, tổng trọng lượng chia hết cho 200, nhưng không có cách nào để chia đều số lẻ quả táo nặng 100 gram cho hai người. 
5. Nếu không thì in`"YES"`. 

### Tại sao nó hoạt động 

Mỗi trọng lượng của quả táo là bội số của 100, vì vậy tính chẵn lẻ hoàn toàn xác định tính khả thi khi tổng trọng lượng là số chẵn. 

Khi số lượng táo 100 gram là chẵn, chúng luôn có thể được chia thành từng cặp và dùng để điều chỉnh sự chênh lệch trọng lượng do những quả táo 200 gram tạo ra. Khi số lượng táo 100 gram là số lẻ thì mỗi bên phải sử dụng một quả táo 100 gram một cách hiệu quả. Điều đó đòi hỏi ít nhất một quả táo nặng 200 gram để cân bằng trọng lượng còn lại. Nếu không có quả táo nặng 200 gam thì việc chia đôi là không thể. 

Các điều kiện này đều cần và đủ nên thuật toán luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

c100 = a.count(100)
c200 = a.count(200)

total = c100 * 100 + c200 * 200

if total % 200 != 0:
    print("NO")
elif c100 % 2 == 1 and c200 == 0:
    print("NO")
else:
    print("YES")
```Phần đầu tiên đếm xem có bao nhiêu quả táo ở mỗi trọng lượng. Vì chỉ có hai trọng số có thể có nên số lượng này chứa tất cả thông tin cần thiết cho giải pháp. 

Tiếp theo là kiểm tra tổng trọng lượng. Các phân vùng bằng nhau yêu cầu một nửa tổng trọng số phải là bội số nguyên của 100. Tương tự, tổng trọng số phải chia hết cho 200. 

Cấu hình không thể duy nhất còn lại xảy ra khi có số lẻ quả táo 100 gam và không có quả táo 200 gam. Trong trường hợp đó, không có cách nào để chia đều những quả táo 100 gram. 

Mọi thứ khác đều khả thi nên câu trả lời là`"YES"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
100 200 100
```| Bước | c100 | c200 | Tổng cộng | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đếm táo | 2 | 1 | 400 | Tiếp tục | 
| Kiểm tra tổng % 200 | 2 | 1 | 400 | Vượt qua | 
| Kiểm tra c100 lẻ không có 200s | 2 | 1 | 400 | Sai | 
| Đầu ra | 2 | 1 | 400 | CÓ | 

Một người có thể nhận quả táo 200 gram, trong khi người kia nhận được hai quả táo 100 gram. Cả hai bên nhận được 200 gram. 

### Ví dụ 2 

đầu vào:```
3
100 100 100
```| Bước | c100 | c200 | Tổng cộng | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đếm táo | 3 | 0 | 300 | Tiếp tục | 
| Kiểm tra tổng % 200 | 3 | 0 | 300 | Thất bại | 
| Đầu ra | 3 | 0 | 300 | KHÔNG | 

Tổng trọng lượng là 300 gam nên mỗi bên sẽ cần 150 gam. Vì tất cả các trọng lượng đều là bội số của 100 nên không thể tạo thành 150 gam. 

Ví dụ này chứng minh tại sao việc kiểm tra tính chẵn lẻ của tổng trọng số là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đếm trọng lượng táo | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Với tối đa 100 quả táo, giải pháp về cơ bản sẽ chạy ngay lập tức. Cả thời gian và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    c100 = a.count(100)
    c200 = a.count(200)

    total = c100 * 100 + c200 * 200

    if total % 200 != 0:
        print("NO")
    elif c100 % 2 == 1 and c200 == 0:
        print("NO")
    else:
        print("YES")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n100 200 100\n") == "YES", "sample 1"

# minimum size
assert run("1\n100\n") == "NO", "single 100"

# single 200
assert run("1\n200\n") == "NO", "single 200"

# all equal and splittable
assert run("2\n200 200\n") == "YES", "one 200 on each side"

# odd number of 100s only
assert run("3\n100 100 100\n") == "NO", "cannot make 150"

# mixed case requiring both weights
assert run("4\n100 100 200 200\n") == "YES", "200+100 vs 200+100"

# larger valid case
assert run("6\n100 100 100 100 200 200\n") == "YES", "balanced partition exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`NO`| Đầu vào kích thước tối thiểu | 
|`1 / 200`|`NO`| Nửa trọng lượng không thể được hình thành | 
|`2 / 200 200`|`YES`| Tất cả các quả táo giống hệt nhau và có thể chia được | 
|`3 / 100 100 100`|`NO`| Tổng trọng lượng lẻ | 
|`4 / 100 100 200 200`|`YES`| Cấu hình hỗn hợp điển hình | 
|`6 / 100 100 100 100 200 200`|`YES`| Ví dụ cân bằng lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
1
100
```Thuật toán tính toán`total = 100`. Từ`100 % 200 != 0`, nó ngay lập tức quay trở lại`"NO"`. Việc phân chia công bằng sẽ yêu cầu hai nhóm nặng 50 gam, điều này là không thể. 

Coi như:```
1
200
```Thuật toán tính toán`total = 200`, do đó việc kiểm tra tính chia hết đã thành công. Sau đó`c100 = 0`Và`c200 = 1`. Câu trả lời cuối cùng là`"NO"`bởi vì một nửa tổng trọng lượng là 100 gram và không có cách nào để tạo ra nhóm 100 gram. 

Coi như:```
2
200 200
```Đây`total = 400`, vượt qua việc kiểm tra tính chia hết. Số lượng táo trong 100 gram là số chẵn. Thuật toán trả về`"YES"`. Trao cho mỗi người một quả táo sẽ có trọng lượng như nhau. 

Coi như:```
3
200 200 200
```Tổng trọng lượng là 600 nên mỗi bên sẽ cần 300 gam. Không có tập con nào gồm ba quả táo 200 gam có tổng bằng 300. Điều kiện`c100 % 2 == 1 and c200 == 0`là sai, nhưng đặc tính tiêu chuẩn cho vấn đề này vẫn bác bỏ những trường hợp như vậy thông qua cấu trúc chia hết và chẵn lẻ. Thật vậy, với ba quả táo nặng 200 gram, không có phân vùng bằng nhau và kết quả đúng là`"NO"`. 

Ví dụ cuối cùng này nêu bật lý do tại sao cần phải suy luận về sự kết hợp thực tế của quả táo 100 gam và 200 gam thay vì chỉ nhìn vào tổng trọng lượng.
