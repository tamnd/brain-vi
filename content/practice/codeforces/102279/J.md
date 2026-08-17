---
title: "CF 102279J - Chữ số tăng vọt"
description: "Giá trị được hiển thị là một chuỗi thập phân (X). Trục trặc ảnh hưởng đến chính xác hai vị trí chữ số, có nghĩa là có thể lấy được số dự định bằng cách hoán đổi các chữ số ở hai vị trí riêng biệt của (X)."
date: "2026-08-16T19:20:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "J"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 59
verified: true
draft: false
---

[CF 102279J - Chữ số tăng vọt](https://codeforces.com/problemset/problem/102279/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Giá trị được hiển thị là một chuỗi thập phân (X). Trục trặc ảnh hưởng đến chính xác hai vị trí chữ số, có nghĩa là có thể lấy được số dự định bằng cách hoán đổi các chữ số ở hai vị trí riêng biệt của (X). Trong số tất cả các số có thể đạt được bằng một phép hoán đổi như vậy, chúng ta cần số lớn nhất nhỏ hơn (X). Nếu không có trao đổi nào tạo ra số thập phân hợp lệ nhỏ hơn, câu trả lời là`-1`. 

Giá trị đầu vào có thể lớn bằng (10^{100}), do đó số có thể chứa tối đa 101 chữ số. Quy tắc này loại trừ việc coi giá trị là số nguyên máy có chiều rộng cố định trong các ngôn ngữ có loại số nguyên hạn chế, nhưng điều đó cũng có nghĩa là số chữ số rất nhỏ. Ngay cả một thuật toán thực hiện vài triệu thao tác trên chuỗi cũng dễ dàng đủ nhanh cho giới hạn một giây. Chúng ta có thể sử dụng cách liệt kê trực tiếp một cách an toàn trên các cặp vị trí. 

Có ba trường hợp nguy hiểm mà việc triển khai bất cẩn có thể xử lý sai. Vì`100`, hoán đổi`1`với một trong hai số 0 sẽ tạo ra một số bắt đầu bằng 0, do đó không có biểu diễn số nguyên dương nhỏ hơn hợp lệ và câu trả lời là`-1`. Giải pháp so sánh các chuỗi trước khi kiểm tra chữ số đầu có thể vô tình chấp nhận`010`hoặc`001`. 

Vì`999`, mỗi cặp đều chứa các chữ số bằng nhau. Hoán đổi bất kỳ cặp nào sẽ giữ nguyên số không thay đổi, do đó không có ứng cử viên nào nhỏ hơn hoàn toàn và câu trả lời là`-1`. Một giải pháp chấp nhận một ứng cử viên chỉ vì nó được tạo ra bởi một sự hoán đổi sẽ trả về không chính xác`999`. 

Vì`354`, hoán đổi chữ số thứ nhất và thứ ba cho`453`, lớn hơn, trong khi hoán đổi chữ số thứ hai và thứ ba sẽ cho`345`, cái nào nhỏ hơn Câu trả lời đúng là`345`. Điều này nắm bắt các triển khai chọn ứng cử viên nhỏ hơn đầu tiên thay vì ứng cử viên lớn nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng cặp vị trí chữ số. Đối với mỗi cặp, sao chép chuỗi gốc, hoán đổi hai chữ số đó, loại bỏ kết quả nếu nó bắt đầu bằng 0 và so sánh với (X). Nếu nó nhỏ hơn (X), hãy giữ nó bất cứ khi nào nó lớn hơn ứng cử viên tốt nhất được tìm thấy cho đến nay. 

Lực lượng mạnh mẽ này là chính xác bởi vì mọi kết quả có thể xảy ra của việc thay đổi chính xác hai vị trí chữ số đều tương ứng với một cặp (i<j) và thuật toán sẽ kiểm tra mọi cặp như vậy. Giữ ứng cử viên hợp lệ lớn nhất sau đó đưa ra câu trả lời chính xác được yêu cầu. 

Chi phí của phương pháp này vẫn còn rất nhỏ trong điều kiện hạn chế thực tế. Với (n) chữ số, có thể có (\binom n2) hoán đổi và việc xây dựng hoặc so sánh một ứng cử viên có thể thực hiện các thao tác ký tự (O(n)). Do đó độ phức tạp trong trường hợp xấu nhất là (O(n^3)). Ở độ dài tối đa có thể (n=101), chỉ có (\binom{101}{2}=5050) hoán đổi, mang lại khoảng (5050\cdot101) hoặc khoảng 510.000 thao tác ký tự. 

Chúng ta có thể thực hiện một quan sát nhỏ giúp giảm bớt những công việc không cần thiết. Giả sử (i<j). Nếu (X[i]\le X[j]), việc hoán đổi chúng không thể làm cho số nhỏ hơn. Vị trí thay đổi đầu tiên là (i) và chữ số của nó giữ nguyên hoặc trở nên lớn hơn. Kết quả nhỏ hơn chỉ có thể xảy ra khi (X[i]>X[j]). Chúng ta có thể bỏ qua tất cả các cặp khác. 

Phương pháp brute-force hoạt động vì đầu vào chỉ chứa khoảng một trăm chữ số. Quan sát cho thấy chỉ các cặp với (X[i]>X[j]) mới có thể giảm số lượng làm giảm số lượng ứng cử viên hơn nữa, nhưng lý do chính khiến phép liệt kê đơn giản được chấp nhận là số chữ số nhỏ bất thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n)) | Đã chấp nhận | 
| Ghép cặp lọc với (X[i]>X[j]) | (O(n^3)) trường hợp xấu nhất | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (X) dưới dạng chuỗi thay vì chuyển đổi nó thành số nguyên. Biểu diễn chuỗi trực tiếp cho phép truy cập vào mọi chữ số và tránh mọi sự phụ thuộc vào kích thước số nguyên. 
2. Khởi tạo câu trả lời tốt nhất là không tồn tại. Chúng tôi chỉ tạo câu trả lời sau khi tìm được hoán đổi tạo ra số hợp lệ nhỏ hơn (X). 
3. Liệt kê từng cặp vị trí (i<j). Các cặp này đại diện cho mọi lựa chọn có thể có của hai chữ số bị ảnh hưởng bởi trục trặc. 
4. Nếu (X[i]\le X[j]), bỏ qua cặp. Vị trí đầu tiên mà số được hoán đổi khác với (X) là (i) và chữ số mới sẽ là (X[j]). Vì (X[j]\ge X[i]), nên kết quả không thể nhỏ hơn hoàn toàn. 
5. Sao chép (X) và hoán đổi vị trí (i) và (j). Chúng tôi sao chép thay vì sửa đổi chuỗi gốc vì mọi ứng cử viên phải được tạo từ cùng một số ban đầu. 
6. Từ chối ứng viên nếu chữ số đầu tiên của nó là`0`. Số 0 đứng đầu sẽ không còn biểu thị số thập phân hợp lệ của số dự kiến. 
7. Vì (X[i]>X[j]), ứng viên được đảm bảo nhỏ hơn (X), vì vậy hãy so sánh nó với ứng viên tốt nhất hiện tại. Thay thế ứng viên tốt nhất khi ứng viên mới lớn hơn. 
8. Sau khi tất cả các cặp đã được kiểm tra, hãy in ra ứng cử viên tốt nhất nếu có. Nếu không thì in`-1`. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ câu trả lời hợp lệ nào (Y). Vì chính xác hai chữ số được thay đổi bằng cách hoán đổi, nên có một cặp vị trí duy nhất (i<j) có các chữ số tạo ra (Y). Đối với (Y<X), vị trí thay đổi đầu tiên (i) phải nhận được chữ số nhỏ hơn, do đó nhất thiết phải có (X[i]>X[j]). Thuật toán kiểm tra cặp này, xây dựng (Y) và chấp nhận nó trừ khi nó có số 0 đứng đầu không hợp lệ. Vì vậy, mọi ứng cử viên hợp lệ đều được xem xét. Thuật toán giữ mức tối đa trong số tất cả các ứng cử viên được chấp nhận, do đó kết quả cuối cùng chính xác là số lớn nhất nhỏ hơn (X) có thể đạt được sau một lần hoán đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    best = None

    for i in range(n):
        for j in range(i + 1, n):
            if s[i] <= s[j]:
                continue

            t = list(s)
            t[i], t[j] = t[j], t[i]

            if t[0] == '0':
                continue

            candidate = ''.join(t)

            if best is None or candidate > best:
                best = candidate

    print(best if best is not None else "-1")

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ trong`s`dưới dạng một chuỗi, do đó mỗi chữ số có thể được truy cập trực tiếp và so sánh giữa các chuỗi thập phân có độ dài bằng nhau có cùng thứ tự như so sánh giữa các giá trị số của chúng. 

Các vòng lặp lồng nhau tạo ra mọi cặp (i<j). điều kiện`s[i] <= s[j]`là một bước cắt tỉa hữu ích. Khi các chữ số bằng nhau, việc hoán đổi không thay đổi gì. Khi`s[i] < s[j]`, chữ số được thay đổi đầu tiên sẽ lớn hơn nên số thu được sẽ lớn hơn (X). 

Ứng viên được tạo từ một danh sách mới để việc hoán đổi không thể ảnh hưởng đến các lần lặp lại sau này. Đây là nguồn lỗi phổ biến trong kiểu hoán vị mạnh mẽ: sửa đổi mảng ban đầu mà không khôi phục nó sẽ khiến các ứng cử viên sau này phụ thuộc vào các lần hoán đổi trước đó. 

Việc kiểm tra số 0 đứng đầu diễn ra trước khi chấp nhận ứng viên. Ví dụ, việc hoán đổi`1`Và`0`TRONG`100`tạo ra`001`, không được coi là số hợp lệ`1`. 

Không có chuyển đổi số nguyên ở bất kỳ đâu trong giải pháp. Bản thân Python hỗ trợ các số nguyên có kích thước tùy ý, nhưng việc xử lý đầu vào dưới dạng một chuỗi làm cho biểu diễn dự định trở nên rõ ràng và giữ cho mọi thao tác tỷ lệ thuận với số chữ số. 

## Ví dụ đã hoạt động 

Ví dụ đầu tiên là`354`. 

| (i) | (j) | Chữ số gốc | Ứng viên | Hành động | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 |`3,5`| bỏ qua |`3 < 5`, vậy ứng viên sẽ lớn hơn | không | 
| 0 | 2 |`3,4`| bỏ qua |`3 < 4`, vậy ứng viên sẽ lớn hơn | không | 
| 1 | 2 |`5,4`|`345`| hợp lệ và nhỏ hơn |`345`| 

Câu trả lời cuối cùng là`345`. Dấu vết cho thấy tại sao chỉ những cặp có chữ số trước lớn hơn mới cần được xem xét. Trong số tất cả các giao dịch hoán đổi có thể xảy ra, chỉ có cặp cuối cùng mới có thể giảm số lượng. 

Ví dụ thứ hai là`100`. 

| (i) | (j) | Chữ số gốc | Ứng viên | Hành động | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 |`1,0`|`010`| từ chối số 0 đứng đầu | không | 
| 0 | 2 |`1,0`|`001`| từ chối số 0 đứng đầu | không | 
| 1 | 2 |`0,0`| bỏ qua | các chữ số bằng nhau không thể giảm số | không | 

Không còn ứng cử viên hợp lệ nào, vì vậy đầu ra là`-1`. Dấu vết này thực hiện ranh giới số 0 đứng đầu và cũng cho thấy lý do tại sao chỉ tìm một chuỗi nhỏ hơn về mặt từ điển là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) | Có các cặp (O(n^2)) và việc xây dựng/so sánh một ứng cử viên mất (O(n)) thời gian | 
| Không gian | (O(n)) | Một mảng chữ số tạm thời và một chuỗi ứng cử viên sử dụng bộ nhớ (O(n)) | 

Ở đây (n\le101), vì (X\le10^{100}). Do đó, trường hợp xấu nhất chỉ liên quan đến khoảng 5050 cặp vị trí và khoảng nửa triệu thao tác ở cấp độ ký tự, nằm trong giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()
    n = len(s)

    best = None

    for i in range(n):
        for j in range(i + 1, n):
            if s[i] <= s[j]:
                continue

            t = list(s)
            t[i], t[j] = t[j], t[i]

            if t[0] == '0':
                continue

            candidate = ''.join(t)

            if best is None or candidate > best:
                best = candidate

    print(best if best is not None else "-1")

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        output = io.StringIO()
        old_stdout = sys.stdout
        sys.stdout = output
        try:
            solve()
        finally:
            sys.stdout = old_stdout

        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("354\n") == "345", "sample 1"
assert run("999\n") == "-1", "sample 2"
assert run("100\n") == "-1", "sample 3"
assert run("11101\n") == "11011", "sample 4"
assert run("2\n") == "-1", "sample 5"

# Custom cases
assert run("10\n") == "-1", "minimum two-digit boundary"
assert run("21\n") == "12", "single possible decreasing swap"
assert run("54321\n") == "54312", "choose the greatest among many smaller swaps"
assert run("1234567890\n") == "1234567809", "swap the final decreasing pair"

# Maximum-length input
max_x = "1" + "0" * 99 + "1"
assert run(max_x + "\n") == "-1", "maximum-length leading-zero case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10`|`-1`| Trường hợp hai chữ số nhỏ nhất trong đó hoán đổi giảm duy nhất tạo ra số 0 đứng đầu | 
|`21`|`12`| Hoán đổi hợp lệ cơ bản | 
|`54321`|`54312`| Chọn ứng cử viên sáng giá nhất thay vì ứng viên đầu tiên | 
|`1234567890`|`1234567809`| Hành vi ranh giới gần cuối một số dài | 
|`1`theo sau là 99 số không và một số cuối cùng`1`|`-1`| Độ dài chữ số tối đa được phép và từ chối số 0 đứng đầu | 

## Vỏ cạnh 

cho`100`, thuật toán kiểm tra`(0,1)`Và`(0,2)`. Cả hai giao dịch hoán đổi sẽ làm giảm số lượng, nhưng chúng tạo ra`010`Và`001`, do đó việc kiểm tra số 0 đứng đầu sẽ bác bỏ cả hai. Cặp đôi`(1,2)`chứa các chữ số bằng nhau và được bỏ qua. Câu trả lời tốt nhất vẫn không tồn tại, do đó thuật toán sẽ in ra`-1`. 

Vì`999`, mọi cặp đều có chữ số bằng nhau. Mỗi cặp được bỏ qua bởi`s[i] <= s[j]`điều kiện, vì việc hoán đổi các chữ số bằng nhau không thể tạo ra số nhỏ hơn. Kết quả là`-1`, thay vì giá trị không thay đổi`999`. 

Vì`2`, chỉ có một chữ số, do đó các vòng lặp lồng nhau không chứa cặp nào cả. Không có hai chữ số riêng biệt nào có thể hoán đổi cho nhau, không để lại ứng cử viên nào và tạo ra`-1`. 

Vì`354`, cặp hữu ích duy nhất là vị trí 1 và 2, sử dụng chỉ mục dựa trên số 0. Từ`5 > 4`, hoán đổi chúng tạo ra`345`. Ứng viên hợp lệ, nhỏ hơn ban đầu và trở thành câu trả lời tốt nhất. Thuật toán in`345`. 

Vì`11101`, hoán đổi`1`ở vị trí 2 với`0`ở vị trí 3 cho`11011`. Các hoán đổi hữu ích khác hoặc tạo ra một ứng cử viên nhỏ hơn hoặc không cải thiện ứng cử viên này. Thuật toán giữ`11011`, đó là kết quả hợp lệ lớn nhất. 

Đối với đầu vào có độ dài tối đa, thuật toán không bao giờ chuyển đổi số thành số nguyên máy. Mọi thao tác đều hoạt động trực tiếp trên các ký tự thập phân của nó, do đó, ranh giới 101 chữ số được xử lý theo cách giống hệt như một đầu vào ngắn.
