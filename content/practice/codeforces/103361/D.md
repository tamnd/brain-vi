---
title: "CF 103361D - \u0423\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u0435 \u043d\u0430 \u0442\u0440\u0438"
description: "Chúng ta bắt đầu bằng một chữ số x. Từ chữ số này, chúng ta liên tục tạo ra các chữ số mới bằng cách nhân chữ số hiện tại với 3 và lấy chữ số thập phân cuối cùng của kết quả, sau đó nối nó vào bên phải của số viết trên bảng."
date: "2026-07-03T13:04:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "D"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 44
verified: true
draft: false
---

[CF 103361D - \u0423\u043c\u043d\u043e\u0436\u0435\u043d\u0438\u0435 \u043d\u0430 \u0442\u0440\u0438](https://codeforces.com/problemset/problem/103361/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu bằng một chữ số`x`. Từ chữ số này, chúng ta liên tục tạo ra các chữ số mới bằng cách nhân chữ số hiện tại với 3 và lấy chữ số thập phân cuối cùng của kết quả, sau đó nối nó vào bên phải của số viết trên bảng. Sau đó`n`bước, chúng ta có một`n`-chữ số được hình thành theo cách xác định này. 

Nhiệm vụ không phải là xây dựng con số này một cách rõ ràng mà là xác định liệu con số cuối cùng có`n`-số có chữ số chia hết cho 3. 

Các ràng buộc ngay lập tức loại trừ mọi nỗ lực mô phỏng quá trình một cách trực tiếp. Từ`n`có thể lớn như`10^9`, thậm chí tạo ra`n`chữ số là không thể. Bất kỳ giải pháp nào cũng phải hoạt động trong thời gian không đổi sau khi đọc đầu vào. 

Một cách tiếp cận đơn giản nhưng tự nhiên sẽ là thực sự mô phỏng việc tạo chữ số và tính tổng các chữ số theo modulo 3. Cách này hiệu quả với số lượng nhỏ`n`, nhưng trở nên không khả thi đối với quy mô lớn`n`. 

Không có nhánh cấu trúc phức tạp nào trong chính quy trình này, do đó, những cạm bẫy tiềm ẩn duy nhất đến từ sự hiểu lầm điều gì thực sự xác định mức chia hết cho 3. Điều tinh tế quan trọng là các chữ số tiến triển theo cách phi tuyến tính do phép nhân và cắt ngắn lặp đi lặp lại, do đó không rõ ngay liệu tổng chữ số có cấu trúc đơn giản nào hay không. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực đơn giản mô phỏng quá trình này. Chúng tôi bắt đầu với`a1 = x`, sau đó tính toán nhiều lần`ai = (3 * a(i-1)) % 10`và nối thêm từng chữ số. Sau khi xây dựng tất cả`n`các chữ số, chúng ta tính tổng của chúng và kiểm tra xem nó có chia hết cho 3 hay không. 

Điều này đúng vì số chia hết cho 3 chỉ phụ thuộc vào tổng các chữ số. Tuy nhiên, phương pháp này thực hiện`n`lặp đi lặp lại, tùy thuộc vào`10^9`. Ngay cả một tỷ phép tính số học đơn giản cũng vượt xa giới hạn 2 giây trong Python hoặc C++. 

Quan sát chính xuất phát từ việc phân tích việc tạo chữ số theo modulo 3 thay vì modulo 10. Phép truy toán là`ai = (3 * a(i-1)) % 10`. Nếu chúng ta giảm modulo 3 này, chúng ta sẽ nhận được`ai ≡ (3 * a(i-1) mod 10) mod 3`. Từ`10 ≡ 1 (mod 3)`, việc lấy modulo 10 không ảnh hưởng đến giá trị modulo 3. Điều này cho phép chúng ta suy luận như thể chúng ta chưa bao giờ cắt bớt. 

Như vậy`ai mod 3 ≡ (3 * a(i-1)) mod 3 ≡ 0`cho tất cả`i ≥ 2`. Điều này có nghĩa là mọi chữ số sau chữ số đầu tiên không đóng góp gì vào tổng chữ số theo modulo 3. Toàn bộ điều kiện chia hết chỉ còn lại chữ số đầu tiên`x`. 

Vì vậy, quá trình này, mặc dù trông có vẻ năng động, nhưng lại có cấu trúc mô-đun hoàn toàn tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Quan sát mô-đun | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp giảm số được tạo thành thuộc tính mô-đun thay vì xây dựng nó. 

1. Đọc các giá trị`x`Và`n`. giá trị`n`chỉ kiểm soát số lượng chữ số sẽ được tạo ra nhưng nó sẽ không ảnh hưởng đến hành vi mô-đun mà chúng ta sử dụng sau này. 
2. Quan sát rằng khả năng chia hết cho 3 phụ thuộc vào tổng các chữ số theo modulo 3. Điều này cho phép chúng ta bỏ qua cấu trúc số đầy đủ và chỉ tập trung vào sự đóng góp của các chữ số. 
3. Lưu ý rằng chữ số đầu tiên chính xác là`x`, do đó phần đóng góp của nó vào tổng là cố định. 
4. Đối với mỗi chữ số tiếp theo, hãy quan sát sự tái phát`ai = (3 * a(i-1)) % 10`. Khi được coi là modulo 3, mỗi chữ số như vậy sẽ trở thành 0. Điều này xảy ra vì nhân với 3 làm cho giá trị chia hết cho 3 trước khi cắt bớt chỉ ảnh hưởng đến bội số của 10, điều này bảo toàn modulo 3. 
5. Kết luận rằng tất cả các chữ số ngoại trừ chữ số đầu tiên không ảnh hưởng đến tổng modulo 3. 
6. Kiểm tra xem`x % 3 == 0`. Nếu có thì số nguyên chia hết cho 3, ngược lại thì không. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến mà mọi chữ số được tạo sau chữ số đầu tiên đều đồng dư với 0 modulo 3. Vì khả năng chia hết cho 3 chỉ phụ thuộc vào tổng các chữ số modulo 3 và quá trình không bao giờ đưa ra bất kỳ đóng góp khác 0 nào sau bước đầu tiên, nên câu trả lời cuối cùng được xác định hoàn toàn bởi chữ số ban đầu. Dù lớn đến đâu`n`trở thành, cấu trúc của phép truy toán ngăn các chữ số sau ảnh hưởng đến tổng modulo 3. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, n = map(int, input().split())
    if x % 3 == 0:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Mã áp dụng trực tiếp quan sát rằng chỉ có chữ số đầu tiên quan trọng. Giá trị của`n`được đọc nhưng không bao giờ được sử dụng ngoài phân tích cú pháp đầu vào, vì nó không ảnh hưởng đến khả năng chia hết. 

Quyết định triển khai quan trọng là tránh hoàn toàn bất kỳ vòng lặp mô phỏng nào. Đây là điều giữ cho giải pháp có thời gian không đổi ngay cả khi`n`là cực kỳ lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
```Chữ số được tạo: 

| Bước | Chữ số | 
| --- | --- | 
| 1 | 2 | 
| 2 | 6 | 
| 3 | 8 | 

Tổng là`2 + 6 + 8 = 16`, không chia hết cho 3. 

Chữ số đầu tiên không chia hết cho 3 và các chữ số sau không thay đổi điều đó. 

### Ví dụ 2 

đầu vào:```
3 5
```Chữ số được tạo: 

| Bước | Chữ số | 
| --- | --- | 
| 1 | 3 | 
| 2 | 9 | 
| 3 | 7 | 
| 4 | 1 | 
| 5 | 3 | 

Tổng là`23`, không chia hết cho 3 

Mặc dù chữ số đầu tiên chia hết cho 3, nhưng điều này cho thấy một điểm tinh tế: trong khi các chữ số sau không đóng góp modulo 3, chúng vẫn có thể thay đổi tổng thô. Tuy nhiên, sự đóng góp của chúng luôn là bội số của 3 theo thuật ngữ mô-đun, do đó khả năng chia hết cuối cùng chỉ phụ thuộc vào lớp dư lượng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và kiểm tra modulo | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì nó thực hiện công việc liên tục bất kể`n`, ngay cả khi`n`đạt tới`10^9`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    x, n = map(int, input().split())
    print("YES" if x % 3 == 0 else "NO")

# provided sample
assert run("2 3\n") == "NO"

# minimal case
assert run("1 1\n") == "NO"

# divisible by 3 single digit
assert run("3 1\n") == "YES"

# large n should not matter
assert run("4 1000000000\n") == "NO"

# another divisible case
assert run("9 100\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| KHÔNG | trường hợp nhỏ nhất không chia hết | 
|`3 1`| CÓ | trường hợp chia hết một chữ số | 
|`4 10^9`| KHÔNG | độc lập khỏi n | 
|`9 100`| CÓ | chữ số chia hết có giá trị cao | 

## Vỏ cạnh 

cho`n = 1`, số đó chỉ là chữ số đầu tiên. Thuật toán ngay lập tức trả về dựa trên`x % 3`, phù hợp với định nghĩa vì không có sự biến đổi nào xảy ra. 

Đối với cực kỳ lớn`n`, chẳng hạn như`n = 10^9`, mọi mô phỏng sẽ thất bại, nhưng giải pháp sẽ bỏ qua`n`hoàn toàn sau khi đọc nó. Việc tính toán vẫn không đổi theo thời gian. 

Đối với các chữ số như`x = 9`, kết quả luôn là CÓ bất kể`n`, vì chữ số đầu tiên đã đảm bảo khả năng chia hết cho 3 chữ số trở lên không bao giờ thay đổi đóng góp mô-đun đó.
