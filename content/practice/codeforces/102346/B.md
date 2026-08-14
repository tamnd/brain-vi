---
title: "CF 102346B - Thằng hề"
description: "Kết quả bầu cử được thể hiện bằng một loạt các cuộc kiểm phiếu, trong đó vị trí đầu tiên thuộc về Carlos và mọi vị trí sau đó thuộc về ứng cử viên đăng ký sau anh ta. Người chiến thắng là ứng cử viên có số phiếu bầu lớn nhất."
date: "2026-08-14T02:00:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 383
verified: true
draft: false
---

[CF 102346B - Thằng hề](https://codeforces.com/problemset/problem/102346/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kết quả bầu cử được thể hiện bằng một loạt các cuộc kiểm phiếu, trong đó vị trí đầu tiên thuộc về Carlos và mọi vị trí sau đó thuộc về ứng cử viên đăng ký sau anh ta. Người chiến thắng là ứng cử viên có số phiếu bầu lớn nhất. Nếu nhiều ứng cử viên có cùng số lượng lớn nhất thì ứng cử viên đăng ký sớm nhất sẽ thắng. Vì Carlos đã đăng ký trước nên anh ấy được bầu chính xác khi số phiếu bầu của anh ấy ít nhất bằng số phiếu bầu của mọi ứng cử viên khác. 

Đầu vào chứa một số nguyên`N`, theo sau là`N`số phiếu tích cực. Số đếm đầu tiên là kết quả của Carlos, số còn lại thuộc về các ứng cử viên khác. Đầu ra cần thiết là`S`nếu Carlos thắng và`N`nếu không thì. 

Ràng buộc`N <= 10^4`đủ nhỏ để ngay cả thuật toán bậc hai cũng có thể khả thi ở một số ngôn ngữ, nhưng ở đây nó không cần thiết. Một giải pháp bậc hai thực hiện khoảng`10^8`kiểm tra theo cặp ở kích thước tối đa, trong khi quét tuyến tính chỉ cần khoảng`10^4`so sánh. Tổng số phiếu bầu tối đa`100,000`, vì vậy bản thân các giá trị cũng nhỏ và dù sao thì số nguyên Python cũng không có vấn đề gì về tràn. 

Trường hợp cạnh chính là cà vạt. Ví dụ,```
2
10
10
```sản xuất```
S
```vì Carlos đã đăng ký trước. Một giải pháp bất cẩn đòi hỏi Carlos phải có nhiều phiếu bầu hơn sẽ in sai`N`. 

Một trường hợp cạnh khác là khi Carlos không phải là mức tối đa duy nhất:```
3
5
7
5
```Đầu ra đúng là```
N
```vì ứng cử viên thứ hai có nhiều phiếu hơn. Chỉ so sánh Carlos với ứng cử viên cuối cùng sẽ chấp nhận anh ta một cách sai lầm. 

Carlos cũng có thể hòa mức tối đa trong khi một số ứng cử viên sau này có cùng số phiếu bầu:```
4
8
8
8
3
```Đầu ra đúng là`S`. Luật tie-break luôn có lợi cho Carlos vì anh chiếm vị trí số một. 

## Phương pháp tiếp cận 

Một cách tiếp cận hoàn toàn trực tiếp bằng vũ lực là so sánh mọi ứng cử viên với mọi ứng cử viên khác và xác định xem số phiếu bầu của Carlos ít nhất có lớn bằng tất cả họ hay không. Điều này đúng vì Carlos thắng chính xác khi không có ứng cử viên nào có nhiều phiếu bầu hơn. Nếu chúng ta thực hiện mọi so sánh cặp có thứ tự có thể, thì có`N(N-1)`so sánh. Với`N = 10,000`, đó là`99,990,000`so sánh, điều này tốn kém một cách không cần thiết đối với một điều kiện đơn giản như vậy và có thể trở thành vấn đề tùy thuộc vào ngôn ngữ và giới hạn thời gian. 

Brute-force hoạt động vì nó kiểm tra rõ ràng định nghĩa của người chiến thắng, nhưng nó lặp lại cùng một thông tin nhiều lần. Nếu ứng cử viên số 7 có ít phiếu bầu hơn Carlos thì chúng ta chỉ cần xác minh điều đó một lần. Không có lý do gì để so sánh ứng viên số 7 với mọi ứng cử viên khác. 

Nhận xét quan trọng là vị trí của Carlos rất đặc biệt. Bởi vì anh ấy đã đăng ký trước nên tỷ số hòa đã có lợi cho anh ấy. Do đó, chúng tôi không cần xác định chính xác người chiến thắng hoặc sắp xếp tất cả số phiếu bầu. Chúng ta chỉ cần biết liệu có ứng cử viên nào sau này có nhiều phiếu bầu hơn Carlos hay không. Một lần đi qua phần còn lại`N - 1`giá trị là đủ. 

Một cách thậm chí còn tổng quát hơn để thể hiện ý tưởng tương tự là tính số phiếu bầu tối đa trong khi vẫn duy trì mức độ ưu tiên của ứng cử viên đầu tiên. Vì ứng cử viên đầu tiên là Carlos nên việc kiểm tra xem`votes[0]`lớn hơn hoặc bằng tổng số phiếu bầu tối đa là đủ. Điều này đưa ra một giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu theo cặp | O(N2) | O(1) | Chậm một cách không cần thiết | 
| Quét tuyến tính tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`và số phiếu bầu đầu tiên. Lưu số đếm đầu tiên dưới dạng tổng số phiếu bầu của Carlos vì ứng cử viên đầu tiên luôn là Carlos. 
2. Đọc từng phần còn lại`N - 1`số phiếu bầu. Nếu bất kỳ số nào lớn hơn số đếm của Carlos, ngay lập tức xác định rằng Carlos không thể thắng và xuất ra`N`. 
3. Nếu toàn bộ đầu vào đã được xử lý mà không tìm thấy số phiếu bầu lớn hơn, đầu ra`S`. Mọi ứng cử viên khác đều có nhiều nhất số phiếu của Carlos và sự bình đẳng là đủ để Carlos giành chiến thắng vì anh ấy đã đăng ký trước. 

Tại sao nó hoạt động: trong quá trình quét, điều bất biến là mọi ứng cử viên được xử lý cho đến nay đều có nhiều nhất số phiếu bầu của Carlos. Nếu một ứng cử viên vi phạm bất biến này bằng cách có nhiều phiếu bầu hơn, ứng cử viên đó phải đánh bại Carlos bất kể tất cả các ứng cử viên còn lại. Nếu không có vi phạm nào xảy ra sau khi xử lý tất cả mọi người, Carlos có ít nhất số phiếu bầu bằng mọi ứng cử viên, vì vậy quy tắc hòa sẽ khiến anh ta trở thành người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
carlos = int(input())

for _ in range(n - 1):
    votes = int(input())
    if votes > carlos:
        print("N")
        break
else:
    print("S")
```Hai lần đọc đầu tiên lấy số lượng ứng cử viên và số phiếu bầu của Carlos. Số lượng của Carlos được giữ riêng vì toàn bộ quyết định phụ thuộc vào việc liệu có ứng cử viên nào sau này vượt quá hay không. 

Vòng lặp xử lý chính xác`N - 1`ứng viên còn lại. Sự so sánh thật chặt chẽ`votes > carlos`, không`votes >= carlos`. Ứng cử viên đến sau có cùng số phiếu bầu không đánh bại Carlos vì Carlos đã đăng ký trước đó. 

các`break`là an toàn vì một khi đã tìm được ứng cử viên có nhiều phiếu bầu hơn thì kết quả bầu cử cuối cùng không thể thay đổi. các`for ... else`cấu trúc xử lý trường hợp ngược lại: nó`else`khối chỉ thực thi nếu vòng lặp kết thúc mà không gặp phải`break`, có nghĩa là không ai có nhiều phiếu bầu hơn Carlos. 

Không có vấn đề tràn số nguyên. Giá trị riêng lớn nhất lớn nhất là`100,000`và số nguyên Python có thể biểu diễn trực tiếp nó. 

## Ví dụ đã hoạt động 

Đoạn trích câu lệnh được cung cấp ở đây không chứa đầu vào số thực tế cho hai trường hợp mẫu của nó, mặc dù nó hiển thị kết quả đầu ra của chúng dưới dạng`S`Và`N`. Các dấu vết sau đây sử dụng hai đầu vào cụ thể để thực hiện hai kết quả đó. 

Đối với trường hợp Carlos thắng nhờ hòa:```
4
8
8
5
8
```| Ứng viên đã được xử lý | Carlos phiếu bầu | Phiếu bầu của ứng cử viên hiện tại |`votes > carlos`| Kết quả | 
| --- | --- | --- | --- | --- | 
| Carlos | 8 | 8 | Chưa được kiểm tra | Tiếp tục | 
| 2 | 8 | 8 | Sai | Tiếp tục | 
| 3 | 8 | 5 | Sai | Tiếp tục | 
| 4 | 8 | 8 | Sai | Tiếp tục | 
| Kết thúc | 8 | Không tìm thấy giá trị lớn hơn | Sai cho tất cả |`S`| 

Quá trình quét chứng minh tại sao sự bình đẳng không được gây ra sự từ chối. Ba ứng cử viên có tám phiếu, nhưng Carlos là người sớm nhất trong số đó nên anh ấy thắng. 

Đối với trường hợp ứng cử viên khác có nhiều phiếu hơn:```
3
5
7
5
```| Ứng viên đã được xử lý | Carlos phiếu bầu | Phiếu bầu của ứng cử viên hiện tại |`votes > carlos`| Kết quả | 
| --- | --- | --- | --- | --- | 
| Carlos | 5 | 5 | Chưa được kiểm tra | Tiếp tục | 
| 2 | 5 | 7 | Đúng |`N`| 
| 3 | 5 | 5 | Không cần thiết | Chưa được xử lý | 

Thí sinh thứ hai quyết định ngay kết quả. Đầu vào còn lại vẫn tồn tại, nhưng nó không thể thay đổi sự thật rằng Carlos đã thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phiếu bầu được đọc và so sánh nhiều nhất một lần. | 
| Không gian | O(1) | Chỉ một`N`, số phiếu bầu của Carlos và số phiếu bầu hiện tại được lưu trữ. | 

Với nhiều nhất`10,000`ứng cử viên, thuật toán chỉ thực hiện vài nghìn phép so sánh trong trường hợp xấu nhất. Nó cũng tránh lưu trữ toàn bộ mảng, do đó việc sử dụng bộ nhớ của nó không đổi bất kể`N`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    carlos = int(input())

    for _ in range(n - 1):
        votes = int(input())
        if votes > carlos:
            print("N")
            break
    else:
        print("S")

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

# The original statement excerpt does not include the numeric sample inputs.
# These two cases represent the shown sample outputs S and N.

assert run("4\n8\n8\n5\n8\n") == "S\n", "sample-style winning case"
assert run("3\n5\n7\n5\n") == "N\n", "sample-style losing case"

# Minimum-size input, Carlos wins through a tie.
assert run("2\n10\n10\n") == "S\n", "minimum size and tie"

# Minimum-size input, the second candidate wins.
assert run("2\n9\n10\n") == "N\n", "minimum size and Carlos loses"

# All candidates have the same number of votes.
assert run("5\n20\n20\n20\n20\n20\n") == "S\n", "all equal values"

# Carlos is the maximum, but the last candidate ties him.
assert run("6\n17\n3\n9\n17\n4\n17\n") == "S\n", "later ties must not defeat Carlos"

# Carlos loses to the first later candidate with more votes.
assert run("6\n12\n13\n100\n1\n1\n1\n") == "N\n", "larger candidate found early"

# Maximum-size input, Carlos wins and all values are equal.
n = 10000
inp = str(n) + "\n" + "\n".join(["1"] * n) + "\n"
assert run(inp) == "S\n", "maximum size"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 10 / 10`|`S`| Kích thước tối thiểu và sự ràng buộc | 
|`2 / 9 / 10`|`N`| Kích thước tối thiểu có thể bị lỗ ngay lập tức | 
|`5 / 20 / 20 / 20 / 20 / 20`|`S`| Tất cả các giá trị bằng nhau | 
|`6 / 17 / 3 / 9 / 17 / 4 / 17`|`S`| Nhiều mối quan hệ sau này vẫn nghiêng về Carlos | 
|`6 / 12 / 13 / 100 / 1 / 1 / 1`|`N`| Giá trị lớn hơn phải hạ gục Carlos ngay lập tức | 
| 10.000 ứng cử viên, mỗi người một phiếu bầu |`S`| Kích thước đầu vào tối đa | 

## Vỏ cạnh 

Trường hợp cạnh tranh đầu tiên là mối quan hệ ràng buộc trực tiếp giữa Carlos và ứng cử viên duy nhất còn lại:```
2
10
10
```Carlos bắt đầu với`10`. Ứng cử viên duy nhất sau này cũng có`10`, vậy điều kiện`votes > carlos`là sai. Vòng lặp kết thúc bình thường và in`S`. Một giải pháp sử dụng`votes >= carlos`sẽ từ chối Carlos một cách không chính xác. 

Trường hợp thứ hai là ứng cử viên muộn hơn với nhiều phiếu bầu hơn:```
3
5
7
5
```Carlos có`5`, và ứng cử viên tiếp theo có`7`. Từ`7 > 5`, thuật toán in`N`ngay lập tức. Ứng cử viên cuối cùng không quan trọng vì Carlos đã bị đánh bại rồi. 

Trường hợp cạnh thứ ba có một số ứng cử viên bị ràng buộc với Carlos:```
4
8
8
5
8
```Mỗi số phiếu bầu sau đó đều bằng hoặc nhỏ hơn`8`. Quá trình quét không bao giờ tìm thấy giá trị lớn hơn, do đó nó in`S`. Thuật toán không cần đếm xem có bao nhiêu ứng cử viên bị ràng buộc vì thứ tự đăng ký sẽ giải quyết mọi mối quan hệ như vậy theo hướng có lợi cho Carlos. 

Trường hợp kích thước tối đa có thể chứa 10.000 ứng cử viên có số phiếu bầu giống hệt nhau:```
5
1
1
1
1
1
```Logic tương tự được áp dụng bất kể có bao nhiêu ứng viên có mặt. Vì mỗi ứng cử viên có nhiều nhất một phiếu bầu của Carlos nên kết quả là`S`. Ở mức tối đa thực tế`N = 10,000`, việc triển khai vẫn chỉ thực hiện một so sánh cho mỗi ứng viên sau, do đó thời gian chạy vẫn tuyến tính.
