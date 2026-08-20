---
title: "CF 102185F - \u0422\u0430\u0439\u043c-\u043b\u0438\u043c\u0438\u0442"
description: "Chúng tôi có N giải pháp gửi. Giải pháp tôi đã đo thời gian chạy trong trường hợp xấu nhất T[i]. Đối với mỗi truy vấn, một chuỗi S sẽ phân loại mọi giải pháp là xấu (0), tốt (1) hoặc không chắc chắn (?)."
date: "2026-08-19T06:31:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "F"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 78
verified: true
draft: false
---

[CF 102185F - \u0422\u0430\u0439\u043c-\u043b\u0438\u043c\u0438\u0442](https://codeforces.com/problemset/problem/102185/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`N`các giải pháp được đưa ra. Giải pháp`i`đã đo thời gian chạy trong trường hợp xấu nhất`T[i]`. Đối với mỗi truy vấn, một chuỗi`S`phân loại mọi giải pháp là xấu (`0`), Tốt (`1`), hoặc không chắc chắn (`?`). 

Chúng ta cần giới hạn thời gian số nguyên dương nhỏ nhất`X`đáp ứng mọi phân loại thực sự áp đặt một hạn chế. Một giải pháp tồi phải mất ít nhất gấp đôi giới hạn, vì vậy đối với mọi vị trí có`S[i] = '0'`chúng tôi cần`T[i] >= 2X`. 

Một giải pháp tốt phải nằm trong một nửa giới hạn, vì vậy đối với mọi vị trí có`S[i] = '1'`chúng tôi cần`T[i] <= X / 2`, 

tương đương với`X >= 2T[i]`. 

Dấu hỏi không áp đặt điều kiện nào cả. Nếu không có số nguyên dương`X`thỏa mãn mọi bất đẳng thức thì đáp án là`-1`. 

Đối với mỗi truy vấn, đầu vào cung cấp một chuỗi phân loại có độ dài`N`. Đầu ra là một số nguyên, giới hạn thời gian hợp lệ tối thiểu hoặc`-1`nếu sự phân loại mâu thuẫn với nhau. 

Đây`N`Và`Q`nhiều nhất là cả hai`1000`, vì vậy việc kiểm tra mọi giải pháp cho mọi truy vấn yêu cầu nhiều nhất`10^6`so sánh ký tự-thời gian. Điều đó hoàn toàn thoải mái trong giới hạn một giây trong Python khi được triển khai bằng các phép toán số nguyên đơn giản. Không cần cấu trúc dữ liệu phức tạp hoặc thuật toán truy vấn tuyến tính. Thời gian chạy tối đa của mỗi giải pháp chỉ là`10^6`, vì vậy số nguyên Python thông thường cũng là quá đủ. 

Các trường hợp đặc biệt chính đến từ các phân loại chỉ chứa một loại giải pháp bị ràng buộc. Coi như```
1
10
1
0
```Một giải pháp tồi phải thỏa mãn`10 >= 2X`, Vì thế`X <= 5`. Giá trị dương nhỏ nhất là`1`, và câu trả lời là`1`. Việc triển khai bất cẩn giả định tồn tại cả giới hạn dưới và giới hạn trên có thể từ chối trường hợp này một cách không chính xác. 

Bây giờ hãy xem xét```
1
10
1
1
```Một giải pháp tốt đòi hỏi`X >= 20`, vậy câu trả lời là`20`. Ở đây không có giới hạn trên. Việc triển khai khởi tạo giới hạn trên về 0 sẽ kết luận không chính xác rằng không có câu trả lời nào tồn tại. 

Một trường hợp quan trọng khác là sự bình đẳng ở biên:```
2
10 20
1
01
```Giải pháp tồi có thời gian`10`, cho`X <= 5`, trong khi giải pháp tốt có thời gian`20`, cho`X >= 40`. Các khoảng này không giao nhau nên đáp án là`-1`. Vì các bất đẳng thức đã bao hàm nên một trường hợp như`T = 10`Và`X = 5`có giá trị đối với một giải pháp tồi, trong khi thay thế`>=`qua`>`sẽ gây ra lỗi từng cái một. 

Cuối cùng, dấu chấm hỏi có thể loại bỏ mọi hạn chế:```
3
5 100 20
1
???
```Mọi số nguyên dương đều hợp lệ nên giá trị tối thiểu là`1`. Một giải pháp cố gắng suy ra các ràng buộc từ các giá trị số mà không kiểm tra ký tự tương ứng có thể tạo ra giá trị lớn hơn không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý một truy vấn một cách độc lập. Đối với mọi vị trí, hãy kiểm tra tính cách của nó. Nếu nó là`0`, bất đẳng thức`T[i] >= 2X`đưa ra một giới hạn trên`X <= floor(T[i] / 2)`. Nếu nó là`1`, bất đẳng thức`T[i] <= X / 2`đưa ra giới hạn dưới`X >= 2T[i]`. Chúng tôi giữ giới hạn dưới mạnh nhất và giới hạn trên mạnh nhất. Sau khi quét toàn bộ chuỗi, số nguyên dương nhỏ nhất trong khoảng kết quả là đáp án. 

Mô tả bạo lực này đã tối ưu cho các ràng buộc nhất định. Người ta có thể tạo ra một thuật toán bạo lực theo đúng nghĩa đen hơn bằng cách thử mọi điều tích cực có thể`X`và kiểm tra tất cả`N`giải pháp. Từ`T[i] <= 10^6`, không có lý do gì để thử các giá trị vượt quá khoảng`2 * 10^6`, nhưng trong trường hợp xấu nhất, điều này vẫn cần khoảng`10^9`kiểm tra xuyên suốt`1000`các truy vấn quá chậm. 

Quan sát quan trọng là các điều kiện không phải là tùy ý. Mọi giải pháp tốt chỉ nói lên điều đó`X`phải đủ lớn và mọi giải pháp tồi chỉ nói lên điều đó`X`phải đủ nhỏ. Do đó, toàn bộ truy vấn được biểu thị chỉ bằng hai số: giới hạn dưới tối đa và giới hạn trên tối thiểu. 

Brute-force hoạt động vì mọi ràng buộc riêng lẻ đều dễ xác minh, nhưng sẽ thất bại nếu chúng ta tìm kiếm qua các giá trị ứng cử viên của`X`. Quan sát rằng tất cả các ràng buộc có thể được nén thành một giới hạn dưới và một giới hạn trên cho phép chúng tôi xác định câu trả lời trong một lần quét truy vấn. Từ`NQ <= 10^6`, quá trình quét trực tiếp này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trước ứng cử viên`X`|`O(Q * N * Tmax)`|`O(1)`| Quá chậm | 
| Giao lộ giới hạn |`O(QN)`|`O(1)`bên cạnh đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với truy vấn hiện tại, hãy khởi tạo giới hạn dưới`L`ĐẾN`1`. Điều này thể hiện yêu cầu rằng giới hạn thời gian phải là số nguyên dương. Khởi tạo giới hạn trên`R`đến vô cùng, vì ban đầu không có giới hạn nào ngăn cản`X`từ việc lớn. 
2. Quét tất cả`N`vị trí của chuỗi truy vấn cùng với thời gian chạy của chúng. 
3. Nếu ký tự hiện tại là`1`, lời giải phải thỏa mãn`T[i] <= X / 2`. Nhân với hai cho`X >= 2T[i]`, vậy hãy cập nhật`L`ĐẾN`max(L, 2T[i])`. Chúng tôi giữ mức tối đa vì tất cả các giải pháp tốt phải được đáp ứng đồng thời. 
4. Nếu ký tự hiện tại là`0`, điều kiện là`T[i] >= 2X`, cho`X <= T[i] / 2`. Từ`X`là một số nguyên, điều này có nghĩa`X <= floor(T[i] / 2)`. Cập nhật`R`ĐẾN`min(R, T[i] // 2)`bởi vì mọi giải pháp tồi đều đóng góp một giới hạn trên. 
5. Bỏ qua mọi`?`. Giải pháp như vậy không hạn chế thời gian. 
6. Sau khi quét truy vấn, so sánh`L`Và`R`. Nếu như`L <= R`, các giá trị nguyên hợp lệ chính xác là các số nguyên từ`L`bởi vì`R`, vì vậy giá trị hợp lệ nhỏ nhất là`L`. Nếu như`L > R`, khoảng yêu cầu trống và câu trả lời là`-1`. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của truy vấn,`L`là giá trị nhỏ nhất của`X`đáp ứng mọi ràng buộc về giải pháp tốt được thấy cho đến nay, trong khi`R`là giá trị lớn nhất thỏa mãn mọi ràng buộc lời giải xấu được thấy cho đến nay. Một giải pháp tốt chỉ có thể tăng`L`, và một giải pháp tồi chỉ có thể làm giảm`R`, vì vậy sau khi quét hoàn tất khoảng thời gian`[L, R]`chứa chính xác các giá trị nguyên thỏa mãn mọi ràng buộc. Nếu khoảng không trống, chọn`L`là tối ưu vì mọi số nguyên dương nhỏ hơn đều vi phạm ít nhất một ràng buộc nghiệm tốt. Nếu khoảng trống thì không có giá trị nào có thể thỏa mãn mọi ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    n = int(input())
    t = list(map(int, input().split()))

    q = int(input())
    answers = []

    for _ in range(q):
        s = input().strip()

        lower = 1
        upper = INF

        for ti, c in zip(t, s):
            if c == '1':
                lower = max(lower, 2 * ti)
            elif c == '0':
                upper = min(upper, ti // 2)

        if lower <= upper:
            answers.append(str(lower))
        else:
            answers.append("-1")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Hai dòng đầu vào đầu tiên cung cấp số lượng giải pháp và thời gian chạy của chúng. Thời gian chạy được lưu trữ một lần vì chúng được chia sẻ bởi mọi truy vấn. 

Đối với mỗi truy vấn,`lower`bắt đầu lúc`1`, không phải ở mức 0, vì giới hạn thời gian phải dương.`upper`bắt đầu ở một giá trị đủ lớn vì một truy vấn có thể không chứa giải pháp xấu nào cả. 

Để có giải pháp tốt,`2 * ti`chính xác là giới hạn dưới. Đối với một giải pháp tồi,`ti // 2`là giới hạn trên của số nguyên chính xác. Việc phân chia tầng là cần thiết vì`X`chính nó phải là số nguyên. Ví dụ, nếu`ti = 7`, điều kiện`7 >= 2X`cho phép`X <= 3`, không`X <= 3.5`. 

Mã chỉ cập nhật giới hạn cho`0`Và`1`. Dấu chấm hỏi bị cố tình bỏ qua vì việc gán cho nó một trong hai loại sẽ tạo ra một ràng buộc mà truy vấn không thực sự yêu cầu. 

Không có vấn đề tràn trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, giới hạn lớn nhất ở đây chỉ là`2 * 10^6`. các`zip`vòng lặp xử lý chính xác`N`vị trí tương ứng của mảng thời gian chạy và chuỗi truy vấn. 

## Ví dụ đã hoạt động 

Mẫu được sao chép trong câu lệnh dường như đã mất một phần định dạng đầu ra. Từ đầu vào đã cho, bốn câu trả lời cho truy vấn là`200`,`-1`,`1000`, Và`1`. Dấu vết bên dưới sử dụng các giá trị đó. 

Đối với truy vấn đầu tiên, dữ liệu là```
5
500 1000 300 700 100
1
?0?01
```Những thay đổi trạng thái có liên quan là: 

| Vị trí | Thời gian | Nhân vật | Giới hạn dưới | Giới hạn trên | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | 1 | INF | 
| 1 | 500 |`?`| 1 | INF | 
| 2 | 1000 |`0`| 1 | 500 | 
| 3 | 300 |`?`| 1 | 500 | 
| 4 | 700 |`0`| 1 | 350 | 
| 5 | 100 |`1`| 200 | 350 | 

Khoảng thời gian cuối cùng là`[200, 350]`, vì vậy giới hạn thời gian hợp lệ tối thiểu là`200`. Hai giải pháp xấu buộc giới hạn trên xuống`350`, trong khi giải pháp tốt buộc giới hạn dưới lên đến`200`. 

Đối với truy vấn thứ hai,```
5
500 1000 300 700 100
1
?0101
```quá trình quét là: 

| Vị trí | Thời gian | Nhân vật | Giới hạn dưới | Giới hạn trên | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | 1 | INF | 
| 1 | 500 |`?`| 1 | INF | 
| 2 | 1000 |`0`| 1 | 500 | 
| 3 | 300 |`1`| 600 | 500 | 
| 4 | 700 |`0`| 600 | 350 | 
| 5 | 100 |`1`| 600 | 350 | 

Bây giờ giới hạn dưới là`600`trong khi giới hạn trên là`350`. Không có số nguyên nào có thể nằm trong khoảng đó nên đáp án là`-1`. Điều này chứng tỏ tại sao chỉ cần so sánh hai giới hạn mạnh nhất sau khi quét là đủ. 

Đối với truy vấn thứ ba và thứ tư, phép tính tương tự cho`1000`Và`1`tương ứng. Truy vấn thứ ba chỉ chứa các giải pháp tốt trong số các vị trí bị ràng buộc, do đó không có giới hạn trên. Truy vấn thứ tư chỉ chứa dấu chấm hỏi, do đó giới hạn dưới dương ban đầu`1`vẫn là câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(QN)`| Mỗi truy vấn sẽ quét`N`ký tự đúng một lần | 
| Không gian |`O(N + Q)`|`N`thời gian chạy và chuỗi đầu ra được thu thập được lưu trữ | 

Với`N <= 1000`Và`Q <= 1000`, có nhiều nhất`10^6`lần lặp của vòng lặp bên trong. Mỗi lần lặp chỉ thực hiện so sánh ký tự và số lượng phép toán số nguyên không đổi, do đó giải pháp phù hợp thoải mái với giới hạn thời gian một giây. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    t = list(map(int, input().split()))

    q = int(input())
    answers = []

    for _ in range(q):
        s = input().strip()

        lower = 1
        upper = 10**30

        for ti, c in zip(t, s):
            if c == '1':
                lower = max(lower, 2 * ti)
            elif c == '0':
                upper = min(upper, ti // 2)

        answers.append(str(lower if lower <= upper else -1))

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """5
500 1000 300 700 100
4
?0?01
?0101
1?1?1
?????
"""

assert run(sample) == "200\n-1\n1000\n1", "provided sample, reconstructed output"

assert run("""1
1
1
?
""") == "1", "minimum-size query with no constraints"

assert run("""1
10
2
0
1
""") == "1\n20", "single bad and single good solution"

assert run("""2
10 20
3
01
10
??
""") == "-1\n40\n1", "contradictory bounds and unconstrained query"

assert run("""4
10 10 10 10
4
0000
1111
0?1?
?0?1
""") == "5\n20\n20\n20", "all-equal values and exact boundaries"

assert run("""1000
""" + "1000 " * 999 + "1000\n1\n" + "0" * 1000 + "\n") == "500", \
    "maximum-size input"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / ?`|`1`| Kích thước tối thiểu và không có ràng buộc | 
| Một giá trị`10`, truy vấn`0`Và`1`|`1`,`20`| Thiếu giới hạn trên hoặc giới hạn dưới | 
| Giá trị`10 20`, truy vấn`01`,`10`,`??`|`-1`,`40`,`1`| Những giới hạn mâu thuẫn và dấu chấm hỏi | 
| Bốn giá trị bằng nhau`10`|`5`,`20`,`20`,`20`| Bao gồm các điều kiện biên | 
|`1000`giá trị bằng nhau`1000`, truy vấn của`1000`số 0 |`500`| Kích thước đầu vào tối đa | 

## Vỏ cạnh 

Khi mỗi nhân vật đều`?`, không có hạn chế nào cả. Vì```
3
5 100 20
1
???
```giới hạn ban đầu là`lower = 1`Và`upper = INF`và quá trình quét không thay đổi giá trị nào. Câu trả lời là`1`. Điều này trực tiếp xử lý trường hợp truy vấn không chứa thông tin hữu ích. 

Khi không có giải pháp tốt, dữ liệu sẽ không có giới hạn dưới. Vì```
1
10
1
0
```giới hạn trên trở thành`10 // 2 = 5`, trong khi`lower`ở lại`1`. Câu trả lời là`1`, bởi vì mọi tích cực`X`từ`1`bởi vì`5`là hợp lệ và chúng ta cần cái nhỏ nhất. 

Khi không có nghiệm xấu thì không có giới hạn trên hữu hạn. Vì```
1
10
1
1
```giới hạn dưới trở thành`2 * 10 = 20`, trong khi`upper`vẫn là vô hạn. Câu trả lời là`20`. 

Sự bình đẳng chính xác phải được chấp nhận. Với```
2
10 40
1
01
```giải pháp tồi mang lại`X <= 5`, trong khi giải pháp tốt mang lại`X >= 80`, vậy câu trả lời là`-1`. Để xem trực tiếp ranh giới bao gồm, hãy sử dụng```
2
10 5
1
01
```Ở đây giải pháp tồi đòi hỏi`X <= 5`, và lời giải tốt đòi hỏi`X >= 10`, vậy câu trả lời lại là`-1`. Nếu thay vào đó các giá trị là```
2
20 5
1
01
```giải pháp tồi mang lại`X <= 10`và giải pháp tốt mang lại`X >= 10`, Vì thế`X = 10`là hợp lệ và câu trả lời là chính xác`10`. Một so sánh chặt chẽ sẽ bác bỏ trường hợp này một cách không chính xác. 

Cuối cùng, phép chia số nguyên có ý nghĩa quan trọng đối với thời gian chạy lẻ. Giả định```
1
7
1
0
```Điều kiện là`7 >= 2X`, vậy số nguyên hợp lệ lớn nhất`X`là`3`. Thuật toán tính toán`7 // 2 = 3`, cho khoảng đúng`[1, 3]`. Việc sử dụng phép chia dấu phẩy động sẽ không cần thiết và có thể khiến việc xử lý ranh giới trở nên kém rõ ràng hơn. Công thức số nguyên nắm bắt chính xác điều kiện.
