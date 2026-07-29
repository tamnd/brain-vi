---
title: "CF 102766F - Bàn phím Singhal và Broken (phiên bản dễ dàng)"
description: "Chúng ta có một bàn phím chỉ có hai phím có thể là a và b. Khi nhấn một phím, bàn phím không in ký tự một lần. Thay vào đó, nó in hai hoặc ba bản sao của ký tự đó."
date: "2026-07-28T23:40:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "F"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 75
verified: true
draft: false
---

[CF 102766F - Bàn phím Singhal và bị hỏng (phiên bản dễ dàng)](https://codeforces.com/problemset/problem/102766/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn phím chỉ có hai phím,`a`Và`b`. Khi nhấn một phím, bàn phím không in ký tự một lần. Thay vào đó, nó in hai hoặc ba bản sao của ký tự đó. Với trình tự các phím mà Singhal nhấn, chúng ta cần đếm xem có bao nhiêu chuỗi cuối cùng khác nhau có thể xuất hiện. 

Đầu vào chứa một số chuỗi gốc. Mỗi chuỗi chỉ chứa`a`Và`b`và đầu ra của mỗi chuỗi là số chuỗi riêng biệt có thể được tạo ra, modulo`10^9 + 7`. 

Khó khăn chính là các lựa chọn mở rộng khác nhau có thể dẫn đến cùng một chuỗi cuối cùng. Ví dụ như nhấn`a`hai lần có thể sản xuất`aaaa`,`aaaaa`, hoặc`aaaaaa`. kết quả`aaaaa`có thể được tạo bằng cách mở rộng đầu tiên`a`chiều dài hai và chiều thứ hai là chiều dài ba, hoặc ngược lại. Chúng tôi chỉ đếm chuỗi kết quả một lần. 

Tổng chiều dài của tất cả các chuỗi đầu vào tối đa là`10^5`. Điều này loại trừ việc tạo ra tất cả các kết quả đầu ra có thể vì số lượng kết hợp tăng theo cấp số nhân. Nếu một chuỗi có độ dài`n`, đã có rồi`2^n`các lựa chọn có thể có về việc mỗi lần nhấn phím sẽ trở thành hai hay ba ký tự. Ngay cả đối với mức độ vừa phải`n`, điều này vượt xa những gì giải pháp một giây có thể xử lý được. Chúng ta cần tìm một công thức đếm xử lý mỗi ký tự một số lần không đổi. 

Các trường hợp phức tạp đến từ các ký tự bằng nhau liên tiếp. Một cách tiếp cận đơn giản có thể xử lý mọi lần nhấn phím một cách độc lập và nhân với hai lựa chọn cho mỗi ký tự. Điều đó vượt quá số lượng vì các ký tự bằng nhau lân cận tạo thành một khối liên tục trong chuỗi cuối cùng. 

Ví dụ:```
Input:
aa

Correct output:
3
```Các đầu ra có thể là`aaaa`,`aaaaa`, Và`aaaaaa`. Nhân hai lựa chọn độc lập sẽ cho ra bốn khả năng, nhưng hai lựa chọn mở rộng khác nhau sẽ tạo ra cùng một chuỗi ở giữa. 

Một trường hợp quan trọng khác là khi tất cả các ký tự đều khác với các ký tự lân cận của chúng.```
Input:
aba

Correct output:
8
```Mỗi ký tự tạo khối riêng biệt nên mỗi lựa chọn sẽ thay đổi chuỗi cuối cùng. Độ dài khối có thể được chọn độc lập là hai hoặc ba, cho`2 * 2 * 2 = 8`kết quả. 

Trường hợp cạnh cuối cùng là một ký tự đơn.```
Input:
b

Correct output:
2
```Khả năng duy nhất là`bb`Và`bbb`. Thuật toán vẫn phải xử lý một chuỗi chỉ có một lần chạy chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi hành vi có thể có của bàn phím. Đối với mỗi ký tự, hãy chọn xem nó có mở rộng thành độ dài hai hoặc ba hay không, xây dựng chuỗi kết quả và lưu trữ tất cả kết quả trong một bộ. Điều này đúng vì tập hợp này sẽ tự động loại bỏ các bản sao. 

Tuy nhiên, đối với một chuỗi có độ dài`n`, có`2^n`các lựa chọn mở rộng. Một chuỗi có độ dài`50`sẽ tạo ra hơn một triệu triệu khả năng, vì vậy phương pháp này không thể hoạt động trong điều kiện hạn chế. 

Quan sát hữu ích đến từ việc xem xét các chuỗi ký tự bằng nhau. Xét một khối liên tiếp có cùng ký tự có độ dài`k`. Mỗi ký tự trong khối này mở rộng thành hai hoặc ba bản sao, do đó tổng chiều dài của khối có thể là bất kỳ giá trị nào từ`2k`ĐẾN`3k`. Mọi độ dài trong khoảng này đều có thể thực hiện được vì mỗi ký tự đóng góp hai hoặc hai cộng với một ký tự phụ. 

Số độ dài cuối cùng có thể có của khối này là:```
3k - 2k + 1 = k + 1
```Các khối ký tự khác nhau không thể can thiệp lẫn nhau vì các khối lân cận chứa các chữ cái khác nhau. Chuỗi cuối cùng được xác định hoàn toàn bởi độ dài mở rộng của mỗi khối. Do đó, số chuỗi có thể có là tích của số lựa chọn cho mỗi lần chạy. 

Phương pháp brute-force hoạt động vì mọi lựa chọn mở rộng được xem xét riêng biệt, nhưng nó không thành công vì nhiều lựa chọn thu gọn vào cùng một thời lượng chạy. Việc nén chuỗi thành các lần chạy sẽ loại bỏ sự trùng lặp này và giảm vấn đề thành một phép nhân đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(2^n * n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải và xác định các nhóm ký tự liên tiếp bằng nhau. Một nhóm đại diện cho một khối sẽ vẫn là một khối ký tự duy nhất sau khi mở rộng. 
2. Với mọi nhóm có độ dài`k`, nhân câu trả lời với`k + 1`. Giá trị này đại diện cho tất cả độ dài cuối cùng có thể có của khối này, từ`2k`bởi vì`3k`. 
3. Tiếp tục cho đến khi mọi nhóm đều được xử lý. Giữ phép nhân theo modulo`10^9 + 7`vì số lượng chuỗi có thể có có thể trở nên rất lớn. 

Tại sao nó hoạt động: 

Chuỗi cuối cùng có thể được mô tả một cách duy nhất bằng độ dài mở rộng của lần chạy ban đầu. Các ký tự của các lần chạy khác nhau thay thế nhau, vì vậy việc biết độ dài của mỗi lần chạy sẽ cho chúng ta biết chính xác vị trí của mỗi ký tự thay đổi. Một đoạn dài`k`có chính xác`k + 1`chiều dài mở rộng có thể. Vì mỗi lần chạy có thể chọn độ dài một cách độc lập nên việc nhân các giá trị này sẽ tính mọi chuỗi cuối cùng có thể có chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()

        result = 1
        current = 1

        for i in range(1, len(s)):
            if s[i] == s[i - 1]:
                current += 1
            else:
                result = (result * (current + 1)) % MOD
                current = 1

        result = (result * (current + 1)) % MOD
        ans.append(str(result))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`current`lưu trữ kích thước của lần chạy hiện tại. Bất cứ khi nào nhân vật thay đổi, quá trình chạy đó sẽ kết thúc và đóng góp hệ số của nó vào`current + 1`. 

Lần chạy cuối cùng cần xử lý riêng sau vòng lặp vì không có ký tự tiếp theo nào sẽ kích hoạt phép nhân. Việc quên bước này là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Hoạt động modulo được áp dụng sau mỗi lần nhân. Số nguyên Python không bị tràn, nhưng việc giảm giá trị sẽ ngăn chặn sự tăng trưởng không cần thiết và khớp với đầu ra được yêu cầu. 

Thuật toán chỉ tính số lần chạy nên không cần lưu trữ các chuỗi mở rộng hoặc thậm chí cả cấu trúc lần chạy. Nó sử dụng bộ nhớ bổ sung liên tục ngoài bộ lưu trữ đầu vào và đầu ra. 

## Ví dụ đã hoạt động 

cho`S = "aba"`: 

| Bước | Nhân vật hiện tại | Độ dài chạy hiện tại | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | một | 1 | 1 | 
| Kết thúc một | một | 1 | 2 | 
| Kết thúc b | b | 1 | 4 | 
| Kết thúc một | một | 1 | 8 | 

Ba lần chạy độc lập. Mỗi lần chạy có thể dài hai hoặc ba lần, vì vậy câu trả lời là`2 * 2 * 2 = 8`. 

Vì`S = "aa"`: 

| Bước | Nhân vật hiện tại | Độ dài chạy hiện tại | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | một | 1 | 1 | 
| Kết thúc chuỗi | một | 2 | 3 | 

Hai lần nhấn phím là một phần của một lần chạy. Độ dài chuỗi có thể trở thành bốn, năm hoặc sáu, tạo thành ba chuỗi riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập một lần trong khi hình thành các lượt chạy. | 
| Không gian | O(1) | Chỉ bộ đếm và câu trả lời hiện tại được lưu trữ. | 

Tổng chiều dài của tất cả các chuỗi là`10^5`, do đó quét tuyến tính dễ dàng nằm trong giới hạn. Giải pháp tránh được số lượng lựa chọn mở rộng theo cấp số nhân bằng cách đếm trực tiếp các khoảng thời gian chạy có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()

        ans = 1
        run = 1

        for i in range(1, len(s)):
            if s[i] == s[i - 1]:
                run += 1
            else:
                ans = ans * (run + 1) % MOD
                run = 1

        ans = ans * (run + 1) % MOD
        out.append(str(ans))

    return "\n".join(out)

assert solution("""4
a
ab
aba
aa
""") == """2
4
8
3""", "provided samples"

assert solution("""1
b
""") == "2", "single character"

assert solution("""1
aaaa
""") == "5", "single long run"

assert solution("""1
ababab
""") == "64", "all separate runs"

assert solution("""1
aabbaa
""") == "27", "multiple equal blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`b`|`2`| Chuỗi kích thước tối thiểu với một lần chạy | 
|`aaaa`|`5`| Một khối lớn duy nhất nơi các lựa chọn hợp nhất | 
|`ababab`|`64`| Mỗi ký tự tạo thành một khối độc lập | 
|`aabbaa`|`27`| Một số lần chạy với độ dài khác nhau | 

## Vỏ cạnh 

Đối với đầu vào:```
aa
```Thuật toán nhìn thấy một chiều dài`2`. Nó nhân câu trả lời với`2 + 1`, sản xuất`3`. Ba chuỗi cuối cùng có thể là`aaaa`,`aaaaa`, Và`aaaaaa`. Nó không tính riêng các thứ tự mở rộng khác nhau vì chỉ có tổng thời lượng chạy mới quan trọng. 

Đối với đầu vào:```
aba
```Thuật toán xử lý ba lần chạy có độ dài`1`. Mỗi thứ đều đóng góp một yếu tố`2 +?`Sửa chữa: mỗi lần chạy độ dài`1`đóng góp`1 + 1 = 2`, vì vậy câu trả lời trở thành`2 * 2 * 2 = 8`. Vì các lần chạy chứa các ký tự khác nhau nên không có hai lựa chọn nào có thể hợp nhất thành cùng một chuỗi cuối cùng. 

Đối với đầu vào:```
b
```Có một lần chạy với chiều dài`1`. Độ dài có thể có của nó là`2`Và`3`, đưa ra hệ số`1 + 1 = 2`. Thuật toán xử lý trường hợp này vì lần chạy cuối cùng được nhân lên sau khi quá trình quét kết thúc.
