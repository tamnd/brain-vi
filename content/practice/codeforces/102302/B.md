---
title: "CF 102302B - Phân chia"
description: "Chúng ta cần tìm mọi số nguyên dương đồng thời là ước của a và bội của b. Dữ liệu đầu vào cung cấp hai số nguyên a và b, với b <= a và cả hai đều có khả năng lớn bằng 10^12."
date: "2026-08-14T04:32:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "B"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 338
verified: false
draft: false
---

[CF 102302B - Divples](https://codeforces.com/problemset/problem/102302/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta cần tìm mọi số nguyên dương đồng thời là ước của`a`và bội số của`b`. Đầu vào cho hai số nguyên`a`Và`b`, với`b <= a`và cả hai đều có khả năng lớn như`10^12`. Đầu ra phải chứa tất cả các số nguyên thỏa mãn cả hai điều kiện, được sắp xếp từ nhỏ nhất đến lớn nhất. Nếu không có số nguyên như vậy tồn tại thì đầu ra được yêu cầu chỉ là một dòng trống. 

Hai điều kiện tương tác một cách hữu ích. Giả sử một số hợp lệ là`x`. Từ`x`là bội số của`b`, chúng ta có thể viết`x = b * k`với một số nguyên dương`k`. Từ`x`cũng phải chia`a`, chúng tôi có`b * k | a`. Điều này chỉ có thể thực hiện được khi`b`chính nó phân chia`a`. Khi`b | a`, viết`a = b * n`. Sau đó`b * k | b * n`chính xác khi nào`k | n`. Do đó, vấn đề ban đầu đã được rút gọn thành việc tìm tất cả các ước số`k`của`n = a / b`, sau đó nhân chúng với`b`. 

Giới hạn trên của`10^12`loại trừ bất kỳ phương pháp nào quét tất cả các số nguyên lên đến`a`. Trong trường hợp xấu nhất, quá trình quét như vậy thực hiện xung quanh`10^12`lặp đi lặp lại, vượt xa những gì giới hạn hai giây có thể hỗ trợ. Thậm chí chỉ quét nhiều lần`b`là không đủ, bởi vì`b`có thể`1`, để lại bao nhiêu là`10^12`ứng viên. Cấu trúc ước số cho phép chúng ta làm tốt hơn nhiều bằng cách liệt kê các ước số theo cặp cho đến căn bậc hai. Vì giá trị liên quan nhiều nhất là`10^12`, căn bậc hai của nó nhiều nhất là`10^6`, có thể dễ dàng quản lý được. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Nếu như`b`không chia`a`, không có câu trả lời. Ví dụ, với đầu vào`10 3`, không có bội số của`3`có thể chia`10`, vì vậy đầu ra đúng sẽ trống. Việc triển khai chỉ đơn giản là tạo ra bội số của`3`và dừng lại ở`10`sẽ sản xuất`3 6 9`, mặc dù không có con số nào trong số đó chia hết`10`. 

Trường hợp cạnh thứ hai xảy ra khi`a = b`. Đối với đầu vào`5 5`, số hợp lệ duy nhất là`5`, bởi vì`5`chia rẽ`5`và là bội số của`5`. Sau khi giảm đi`b`, chúng tôi nhận được`n = 1`, ước số duy nhất của nó là`1`. Câu trả lời tương ứng là`5 * 1 = 5`. 

Trường hợp cạnh thứ ba là một hình vuông hoàn hảo. Đối với đầu vào`36 2`, chúng tôi có`n = 18`, trong khi đối với đầu vào`49 7`, chúng tôi có`n = 7`. Tổng quát hơn, nếu`n`chính nó là một hình vuông, cặp số chia được tìm thấy tại`sqrt(n)`chứa cùng một ước số hai lần. Việc triển khai phải tránh thêm giá trị đó hai lần. Ví dụ,`a = 36, b = 1`có các ước số bao gồm`6`, Nhưng`6`chỉ được xuất hiện một lần ở đầu ra. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ nhất là kiểm tra mọi số nguyên có thể từ`1`bởi vì`a`, kiểm tra xem nó có chia hết cho không`b`và một số chia của`a`. Chúng ta có thể làm cho quá trình quét nhỏ hơn một chút bằng cách chỉ kiểm tra bội số của`b`, nhưng trong trường hợp xấu nhất`b = 1`, nên chúng tôi vẫn biểu diễn`10^12`séc. Ngay cả khi mỗi lần kiểm tra đều có thời gian không đổi thì thời gian này vẫn vượt xa thời gian sẵn có. Phương pháp vũ phu là đúng vì mọi câu trả lời có thể đều được kiểm tra, nhưng số lượng hoạt động của nó là vấn đề cơ bản. 

Quan sát quan trọng là mọi câu trả lời đều chứa`b`như một yếu tố. Nếu như`b`không chia`a`, không thể có câu trả lời nào được. Nếu như`b`chia rẽ`a`, bộ`n = a / b`. Mọi câu trả lời đều có dạng`b * k`, và điều kiện chia hết trở thành`k | n`. Chúng ta đã chuyển đổi bài toán từ việc tìm các ước số đặc biệt của`a`tìm mọi ước số thường của số nhỏ hơn`n`. 

Các ước số có thể được liệt kê theo cặp. Bất cứ khi nào`i`chia rẽ`n`, số`n / i`cũng là số chia. Chúng ta chỉ cần kiểm tra`i`lên tới`sqrt(n)`, bởi vì mọi ước số lớn hơn căn bậc hai đều được ghép với một ước số nhỏ hơn căn bậc hai. Vì vậy chúng tôi thực hiện nhiều nhất`10^6`kiểm tra tính chia hết. Chúng tôi thu thập cả hai thành viên của mỗi cặp ước số, nhân chúng với`b`, sắp xếp các giá trị kết quả và in chúng. 

Cách tiếp cận vũ phu có hiệu quả vì nó kiểm tra toàn bộ phạm vi câu trả lời có thể có, nhưng không thành công khi phạm vi đó đạt tới.`10^12`. Nhận xét rằng mọi số hợp lệ đều`b`nhân với số chia của`a / b`giảm không gian tìm kiếm xuống chỉ còn`O(sqrt(a / b))`ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) trong trường hợp xấu nhất | O(1) không bao gồm đầu ra | Quá chậm | 
| Tối ưu | O(sqrt(a / b) + d log d) | O(d) | Đã chấp nhận | 

Đây`d`là số ước của`a / b`. Đối với số lượng lên đến`10^12`,`d`nhỏ so với phạm vi tìm kiếm ban đầu. 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`b`. Trước khi thực hiện bất kỳ phép liệt kê số chia nào, hãy kiểm tra xem`a % b`là số không. Nếu không, hãy in một dòng trống và dừng lại, vì số đó là bội số của`b`không thể chia`a`Trừ khi`b`chính nó phân chia`a`. 
2. Tính toán`n = a // b`. Mọi câu trả lời hợp lệ bây giờ đều chính xác`b * k`, Ở đâu`k`là ước số của`n`. Điều này diễn ra từ`a = b * n`Và`b * k | b * n`, tương đương với`k | n`. 
3. Bắt đầu với danh sách câu trả lời trống và lặp lại`i`từ`1`trong khi`i * i <= n`. Chỉ kiểm tra phạm vi này là đủ vì mọi ước số bên dưới căn bậc hai đều có một ước số ghép đôi ở trên nó. 
4. Bất cứ khi nào`n % i == 0`, thêm vào`b * i`vào danh sách câu trả lời. Đồng thời thêm`b * (n // i)`, là thành viên còn lại của cặp số chia. 
5. Nếu`i * i == n`, không thêm giá trị thứ hai một cách riêng biệt. Trong tình huống đó`i`Và`n // i`là cùng một ước số, vì vậy việc thêm cả hai sẽ nhân đôi một câu trả lời. 
6. Sắp xếp các câu trả lời đã thu thập được. Các ước số được phát hiện theo thứ tự tăng dần đối với thành viên nhỏ của mỗi cặp nhưng các thành viên lớn được ghép đôi của chúng không được sắp xếp trên toàn cầu, do đó việc sắp xếp sẽ đưa ra thứ tự số cần thiết. 
7. In các câu trả lời cách nhau bằng dấu cách. Nếu danh sách trống, việc in chuỗi đã nối sẽ tự nhiên tạo ra dòng trống cần thiết. 

### Tại sao nó hoạt động 

Sau khi kiểm tra tính chia hết, ta biết`a = b * n`. một con số`x`là một câu trả lời hợp lệ chính xác khi nó có thể được viết là`x = b * k`Và`x | a`. Việc thay thế các biểu thức này sẽ cho`b * k | b * n`, tương đương với`k | n`. Do đó, có sự tương ứng một-một giữa câu trả lời hợp lệ và ước số của`n`, với mỗi ước số`k`ánh xạ tới`b * k`. 

Việc liệt kê số chia kiểm tra mọi`i <= sqrt(n)`. Nếu như`i`chia rẽ`n`, ước số ghép của nó`n / i`cũng được tìm thấy, do đó mọi ước số được tạo ra dưới dạng thành viên nhỏ hoặc thành viên lớn của một cặp. Trường hợp hình vuông được xử lý riêng nên ước số bằng`sqrt(n)`xuất hiện đúng một lần. Do đó, sau khi nhân với`b`và sắp xếp, đầu ra chứa mọi số nguyên hợp lệ chính xác một lần và không chứa số nguyên không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())

    if a % b != 0:
        print()
        return

    n = a // b
    ans = []

    i = 1
    while i * i <= n:
        if n % i == 0:
            ans.append(b * i)

            other = n // i
            if other != i:
                ans.append(b * other)

        i += 1

    ans.sort()
    print(*ans)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên kiểm tra tính chia hết cần thiết của`a`qua`b`. Không có nó, không có lý do gì để tìm kiếm các ước số vì mọi ứng cử viên sẽ phải chứa`b`như một yếu tố. 

Biến`n`là kích thước bài toán giảm đi,`a / b`. Vòng lặp kiểm tra các ước số nhỏ có thể có từ`1`bởi vì`sqrt(n)`. điều kiện`i * i <= n`tránh các căn bậc hai có dấu phẩy động và an toàn cho các số nguyên có độ chính xác tùy ý của Python. 

Khi`i`chia rẽ`n`, cả hai`i`Và`n // i`là các ước số. Nhân từng cái với`b`chuyển đổi chúng trở lại những con số thực tế mà bài toán ban đầu yêu cầu. các`other != i`kiểm tra xử lý các hình vuông hoàn hảo, ngăn không cho căn bậc hai bị chèn hai lần. 

Việc sắp xếp cuối cùng là cần thiết vì các cặp số chia được phát hiện theo thứ tự hỗn hợp. Ví dụ, khi`n = 12`, phép lặp tìm thấy`1, 12`, sau đó`2, 6`, sau đó`3, 4`. Trình tự cụ thể này gần như được sắp xếp theo thứ tự, nhưng nói chung các giá trị được chuyển đổi không cần phải đạt đến thứ tự chung được yêu cầu. Việc sắp xếp làm cho điều kiện đầu ra trở nên rõ ràng. 

Số nguyên Python không bị tràn, vì vậy các giá trị như`b * (n // i)`được an toàn ngay cả khi chúng lớn như`10^12`. 

## Ví dụ đã hoạt động 

### Mẫu 1:`12 3`Đây`b`chia rẽ`a`, do đó bài toán quy về việc tìm các ước của`n = 12 / 3 = 4`. 

|`i`|`i * i <= n`|`n % i`| Số chia nhỏ | Số chia ghép đôi | Câu trả lời được thu thập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đúng | 0 | 1 | 4 | 3, 12 | 
| 2 | đúng | 0 | 2 | 2 | 3, 12, 6 | 
| 3 | sai | chưa được kiểm tra | | | 3, 12, 6 | 

Sau khi sắp xếp sẽ có câu trả lời`3 6 12`. 

Dấu vết hiển thị trực tiếp bất biến cặp số chia. Các ước số của`4`là`1`,`2`, Và`4`, và nhân chúng với`3`tạo ra chính xác những con số cần thiết. giá trị`2`là căn bậc hai của`4`, vì vậy nó chỉ được thêm một lần. 

### Mẫu 2:`10 3`Phép thử tính chia hết đầu tiên đã giải quyết được vấn đề vì`10 % 3 = 1`. 

|`a`|`b`|`a % b`|`n`| Đáp án | 
| --- | --- | --- | --- | --- | 
| 10 | 3 | 1 | không được tính toán | trống | 

Thuật toán in một dòng trống. Đây là trường hợp bắt được một giải pháp không chính xác mà chỉ tạo ra bội số của`b`. Các giá trị như`3`,`6`, Và`9`là bội số của`3`, nhưng không có gì chia được`10`, vì vậy không có câu trả lời nào hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(a / b) + d log d) | Chúng tôi kiểm tra các ước số lên đến`sqrt(a / b)`, phát ra`d`câu trả lời, sau đó sắp xếp chúng. | 
| Không gian | O(d) | Danh sách câu trả lời lưu trữ các giá trị hợp lệ. | 

Từ`a / b <= 10^12`, việc tìm kiếm số chia thực hiện tối đa khoảng`10^6`các lần lặp vòng lặp. Số lượng ước số`d`nhỏ hơn nhiều so với`10^6`, nên chi phí phân loại cũng khiêm tốn. Thuật toán phù hợp thoải mái với giới hạn thời gian hai giây và duy trì tốt trong giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    a, b = map(int, input().split())

    if a % b != 0:
        print()
        return

    n = a // b
    ans = []

    i = 1
    while i * i <= n:
        if n % i == 0:
            ans.append(b * i)

            other = n // i
            if other != i:
                ans.append(b * other)

        i += 1

    ans.sort()
    print(*ans)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        from io import StringIO

        old_stdout = sys.stdout
        sys.stdout = StringIO()

        try:
            solve()
            return sys.stdout.getvalue()
        finally:
            sys.stdout = old_stdout
    finally:
        sys.stdin = old_stdin
        input = old_input

assert run("12 3\n") == "3 6 12\n", "sample 1"
assert run("10 3\n") == "\n", "sample 2"
assert run("128 2\n") == "2 4 8 16 32 64 128\n", "sample 3"

assert run("1 1\n") == "1\n", "minimum-size input"
assert run("5 5\n") == "5\n", "all-equal values"
assert run("36 1\n") == "1 2 3 4 6 9 12 18 36\n", "perfect square"
assert run("1000000000000 1000000000000\n") == "1000000000000\n", "maximum-size equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Các giá trị tối thiểu và`n = 1`trường hợp | 
|`5 5`|`5`| Bình đẳng`a`Và`b`| 
|`36 1`|`1 2 3 4 6 9 12 18 36`| Ghép nối số chia bình phương hoàn hảo và đầu ra được sắp xếp | 
|`1000000000000 1000000000000`|`1000000000000`| Kích thước đầu vào tối đa và số học ranh giới | 

## Vỏ cạnh 

Khi nào`b`không chia`a`, thuật toán thoát trước khi liệt kê số chia. Đối với đầu vào`10 3`, phép tính đầu tiên là`10 % 3 = 1`, do đó danh sách câu trả lời vẫn trống và chương trình sẽ in ra một dòng trống. Điều này ngăn ngừa sai lầm phổ biến khi xử lý mọi bội số của`b`như một câu trả lời mà không kiểm tra xem nó có thực sự chia`a`. 

Khi`a = b`, giá trị giảm là`n = 1`. Đối với đầu vào`5 5`, vòng lặp bắt đầu bằng`i = 1`, Và`1 * 1 <= 1`là đúng. Từ`1`chia rẽ`1`, thuật toán thêm`5 * 1 = 5`. Số chia ghép đôi cũng là`1`, Vì thế`other != i`là sai và không có bản sao nào được chèn vào. Vòng lặp sau đó kết thúc, tạo ra`5`. 

Khi`n`là một hình vuông hoàn hảo, việc bảo vệ trùng lặp là điều cần thiết. Coi như`36 1`. Số giảm tương ứng là`36`, và khi nào`i = 6`, cả hai biểu thức ước đều cho`6`. Thuật toán bổ sung`6`một lần và bỏ qua lần chèn thứ hai vì`other == i`. Đầu ra được sắp xếp hoàn chỉnh là`1 2 3 4 6 9 12 18 36`. 

Ở các giá trị lớn nhất có thể, chẳng hạn như`1000000000000 1000000000000`, số giảm chỉ còn`1`. Thuật toán không lặp lại ở bất cứ đâu gần`10^12`lần. Nó thực hiện kiểm tra ước số duy nhất và xuất ra giá trị ban đầu. Điều này minh họa tại sao giảm vấn đề bằng cách`b`rất hữu ích ngay cả trước khi áp dụng phép liệt kê số chia căn bậc hai.
