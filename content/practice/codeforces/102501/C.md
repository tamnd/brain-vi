---
title: "CF 102501C - Kiến"
description: "Nhiệm vụ là khôi phục mã định danh tiếp theo mà chương trình nhận dạng kiến ​​sẽ gán. Đầu vào mô tả các mã định danh hiện được hệ thống nhận dạng nhìn thấy."
date: "2026-08-06T18:50:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 57
verified: true
draft: false
---

[CF 102501C - Kiến](https://codeforces.com/problemset/problem/102501/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là khôi phục mã định danh tiếp theo mà chương trình nhận dạng kiến sẽ gán. Đầu vào mô tả các mã định danh hiện được hệ thống nhận dạng nhìn thấy. Một số giá trị là số nhận dạng hợp lệ, trong khi các giá trị không đúng định dạng như số âm hoặc số nằm ngoài phạm vi hữu ích phải được bỏ qua. Trong số các định danh không âm còn lại, chúng ta cần tìm số nguyên nhỏ nhất bắt đầu từ 0 mà không xuất hiện. 

Đầu vào có thể chứa tối đa một triệu giá trị và mỗi giá trị có thể chứa gần một trăm chữ số. Độ dài chữ số lớn loại trừ việc dựa vào phân tích số nguyên thông thường cho mọi giá trị trong các ngôn ngữ có loại số nguyên có kích thước cố định. Quan trọng hơn, với một triệu mục nhập, bất kỳ giải pháp nào liên tục quét toàn bộ bộ sưu tập hoặc kiểm tra nhiều câu trả lời ứng viên sẽ thực hiện quá nhiều công việc. Chúng ta cần một cách tiếp cận gần tuyến tính về số lượng giá trị đầu vào. 

Quan sát quan trọng về phạm vi câu trả lời đến từ số lượng định danh. Nếu có N giá trị đầu vào, thì số nguyên không âm nhỏ nhất bị thiếu không bao giờ có thể lớn hơn N. Ngay cả khi mọi số từ 0 đến N-1 đều xuất hiện thì cũng chỉ có N số đó, vì vậy N là giá trị có thể thiếu đầu tiên. Điều này có nghĩa là các giá trị lớn hơn N không thể ảnh hưởng đến câu trả lời và có thể bị loại bỏ ngay lập tức. 

Một số trường hợp cạnh rất dễ xử lý sai. Khi không có giá trị nào, tập hợp này trống và câu trả lời là 0.```
Input:
0
```Đầu ra đúng là`0`. Giải pháp bắt đầu tìm kiếm từ một thay vì 0 sẽ thất bại ở đây. 

Trường hợp thứ hai liên quan đến các giá trị bị bỏ qua trông giống như số nhưng không thể là định danh hợp lệ.```
Input:
5
-3
999999999999999999999999
abc
0
1
```Đầu ra đúng là`2`. Chỉ có số không và một là có liên quan. Một giải pháp bất cẩn lưu trữ mọi giá trị được phân tích cú pháp hoặc giả sử mọi đầu vào khớp với một số nguyên bình thường có thể gây lãng phí bộ nhớ hoặc bị lỗi khi phân tích cú pháp. 

Các số 0 đứng đầu có thể là một nguồn lỗi khác.```
Input:
3
000
001
3
```Đầu ra đúng là`2`. Các giá trị`000`Và`001`đại diện cho số không và một. Nếu coi chúng như những chuỗi khác nhau sẽ nghĩ sai rằng không có một và một. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liên tục kiểm tra số lượng ứng viên. Chúng ta có thể bắt đầu từ số 0, kiểm tra xem nó có xuất hiện trong tập hợp đầu vào hay không, sau đó tiếp tục với một, hai, v.v. cho đến khi tìm thấy giá trị còn thiếu. Điều này đúng vì ứng cử viên đầu tiên vắng mặt chính xác là mã định danh bắt buộc. Với tập hợp băm, mỗi lần tra cứu được mong đợi là O(1), nhưng phương pháp này vẫn cần một cách để lưu trữ tất cả các giá trị hợp lệ và kiểm tra các ứng cử viên nhiều lần. Trong trường hợp xấu nhất, đầu vào chứa mọi số từ 0 đến N-1, vì vậy chúng tôi thực hiện khoảng N lần tra cứu sau khi đọc N giá trị. Điều này có thể chấp nhận được khi băm, nhưng việc triển khai đơn giản để quét mảng đầu vào cho từng ứng viên sẽ yêu cầu khoảng N2 thao tác, tức là khoảng 10¹² kiểm tra cho một triệu mục nhập. 

Cách tiếp cận tốt hơn đến từ việc giới hạn các giá trị duy nhất quan trọng. Vì câu trả lời nhiều nhất là N nên chúng ta chỉ cần biết có những số nào trong khoảng từ 0 đến N. Chúng ta có thể tạo một mảng boolean có kích thước N+1. Trong khi đọc từng giá trị đầu vào, chúng tôi bỏ qua các giá trị âm và giá trị lớn hơn N. Đối với các giá trị trong phạm vi hữu ích, chúng tôi đánh dấu vị trí tương ứng. Sau khi xử lý tất cả dữ liệu đầu vào, vị trí không được đánh dấu đầu tiên là mã định danh bị thiếu. 

Khó khăn là số đầu vào có thể có tới 100 chữ số, do đó việc chuyển đổi mọi thứ thành số nguyên là không cần thiết và có khả năng không an toàn. Thay vào đó, chúng tôi so sánh cách biểu diễn văn bản với N. Sau khi loại bỏ dấu và các số 0 ở đầu, một giá trị chỉ có thể sử dụng được nếu độ dài của nó tối đa bằng độ dài của N hoặc nếu nó có cùng độ dài và về mặt từ điển không lớn hơn N. 

Brute-force hoạt động vì nó kiểm tra trực tiếp định nghĩa của giá trị còn thiếu, nhưng nó lãng phí công sức đối với các giá trị nằm ngoài phạm vi câu trả lời và khi tìm kiếm lặp đi lặp lại. Nhận xét rằng câu trả lời bị giới hạn bởi N cho phép chúng ta giảm vấn đề xuống việc đánh dấu một tiền tố nhỏ của các định danh có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số định danh và tạo mảng`present`với vị trí N+1. Chức vụ`i`đại diện cho dù định danh`i`xuất hiện. Vị trí bổ sung cho N là cần thiết vì câu trả lời có thể bằng N. 
2. Đọc từng mã định danh dưới dạng một chuỗi thay vì chuyển đổi ngay thành số nguyên. Loại bỏ các số 0 đứng đầu khỏi các giá trị không âm để các dạng văn bản khác nhau của cùng một số trở nên giống hệt nhau. 
3. Bỏ qua các giá trị âm hoặc có giá trị chuẩn hóa lớn hơn N. Các giá trị như vậy không thể thay đổi mã định danh bị thiếu nhỏ nhất vì câu trả lời được đảm bảo tối đa là N. 
4. Đánh dấu mọi giá trị còn lại trong`present`mảng. Điều này ghi lại chính xác thông tin cần thiết cho tìm kiếm cuối cùng. 
5. Quét`present`từ chỉ số 0 trở lên và xuất chỉ mục đầu tiên có giá trị sai. Chỉ mục đó là mã định danh nhỏ nhất hiện chưa được chỉ định. 

Lý do điều này có tác dụng là vì mọi mã định danh có thể là câu trả lời đều được biểu thị trong mảng. Các giá trị nằm ngoài phạm vi này không thể là câu trả lời, vì vậy việc bỏ qua chúng sẽ không làm mất thông tin. 

Tại sao nó hoạt động: 

Sau khi xử lý tất cả dữ liệu đầu vào,`present[x]`đúng chính xác khi định danh`x`xuất hiện trong số các giá trị đầu vào có liên quan cho mọi`x`giữa 0 và N. Hãy xem xét chỉ số đầu tiên`k`điều đó vẫn sai. Mỗi mã định danh nhỏ hơn đều được đánh dấu, do đó mọi giá trị từ 0 đến`k-1`tồn tại. Mã định danh`k`không tồn tại, làm cho nó trở thành định danh bị thiếu nhỏ nhất. Nếu không thiếu giá trị nhỏ hơn thì quá trình quét không thể dừng sớm hơn, do đó câu trả lời được tạo ra luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize_number(s):
    if s[0] == '-':
        rest = s[1:].lstrip('0')
        if rest == '':
            return "0"
        return None
    s = s.lstrip('0')
    return s if s else "0"

def solve():
    line = input().strip()
    if not line:
        return
    n = int(line)

    present = [False] * (n + 1)
    limit = str(n)

    for _ in range(n):
        s = input().strip()
        value = normalize_number(s)
        if value is None:
            continue

        if len(value) < len(limit) or (len(value) == len(limit) and value <= limit):
            x = int(value)
            present[x] = True

    for i, exists in enumerate(present):
        if not exists:
            print(i)
            return

if __name__ == "__main__":
    solve()
```các`normalize_number`hàm xử lý định dạng đầu vào lớn mà không cần số học chính xác tùy ý. Nó loại bỏ các số 0 đứng đầu, xử lý số 0 một cách chính xác và loại bỏ các giá trị thực sự âm. 

Sự so sánh chống lại`n`xảy ra trước khi gọi`int`. Đây là sự tối ưu hóa quan trọng vì không nên chuyển đổi giá trị 100 chữ số trừ khi nó đã được chứng minh là đủ nhỏ để quan trọng. Khi độ dài chuỗi và so sánh từ điển cho thấy giá trị tối đa là N, thì việc chuyển đổi sẽ an toàn vì giá trị đó phù hợp với phạm vi của mảng boolean. 

các`present`mảng lưu trữ chính xác N+1 câu trả lời có thể. Vòng lặp cuối cùng bắt đầu từ số 0, điều này tránh được lỗi thường gặp là quên rằng số 0 là số nhận dạng hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào này:```
5
-1
0
1
4
100000000000000000000
```Dấu vết là: 

| Giá trị hiện tại | Giá trị chuẩn hóa | Hành động | Định danh được đánh dấu | 
| --- | --- | --- | --- | 
| -1 | bỏ qua | Bỏ qua giá trị âm | {} | 
| 0 | 0 | Đánh dấu số 0 | {0} | 
| 1 | 1 | Đánh dấu một | {0, 1} | 
| 4 | 4 | Đánh dấu bốn | {0, 1, 4} | 
| 1000000000000000000000 | quá lớn | Bỏ qua | {0, 1, 4} | 

Vị trí không được đánh dấu đầu tiên là hai, vì vậy đầu ra là:```
2
```Ví dụ này cho thấy các giá trị không hợp lệ và quá khổ không ảnh hưởng đến câu trả lời. 

Một ví dụ thứ hai:```
6
000
001
002
3
4
5
```Dấu vết là: 

| Giá trị hiện tại | Giá trị chuẩn hóa | Hành động | Định danh được đánh dấu | 
| --- | --- | --- | --- | 
| 000 | 0 | Đánh dấu số 0 | {0} | 
| 001 | 1 | Đánh dấu một | {0, 1} | 
| 002 | 2 | Đánh dấu hai | {0, 1, 2} | 
| 3 | 3 | Đánh dấu ba | {0, 1, 2, 3} | 
| 4 | 4 | Đánh dấu bốn | {0, 1, 2, 3, 4} | 
| 5 | 5 | Đánh dấu năm | {0, 1, 2, 3, 4, 5} | 

Mọi vị trí từ 0 đến 5 đều có mặt, vì vậy giá trị còn thiếu đầu tiên là 6. Đầu ra là:```
6
```Trường hợp này chứng minh tại sao việc so sánh các chuỗi thô sẽ không chính xác vì một số dạng văn bản biểu thị cùng một số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi mã định danh được đọc và xử lý một lần và lần quét cuối cùng sẽ kiểm tra tối đa N+1 vị trí. | 
| Không gian | O(N) | Mảng boolean lưu trữ xem mỗi câu trả lời có thể có từ 0 đến N có tồn tại hay không. | 

Thuật toán thực hiện một lượng công việc không đổi nhỏ trên mỗi dòng đầu vào và tránh xử lý các số nguyên lớn không cần thiết. Đối với một triệu mã định danh, yêu cầu về bộ nhớ tuyến tính và thời gian phù hợp với giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

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

# provided-style empty case
assert run("0\n") == "0\n", "empty input"

# consecutive identifiers starting at zero
assert run("3\n0\n1\n2\n") == "3\n", "all small values present"

# ignored negative and huge values
assert run("5\n-5\n0\n1\n999999999999999999999999\n2\n") == "3\n", "ignored values"

# leading zeroes
assert run("4\n000\n001\n003\n4\n") == "2\n", "leading zero handling"

# duplicate values
assert run("5\n0\n0\n1\n1\n3\n") == "2\n", "duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Kích thước đầu vào tối thiểu và số 0 là câu trả lời hợp lệ | 
|`0, 1, 2`|`3`| Câu trả lời có thể là N | 
| Các giá trị âm và lớn trộn lẫn với các giá trị nhỏ |`3`| Bỏ qua số nhận dạng không hợp lệ | 
| Giá trị có số 0 đứng đầu |`2`| Chuẩn hóa số đúng | 
| Số nhận dạng lặp đi lặp lại |`2`| Đặt hành vi thay vì đếm số lần xuất hiện | 

## Vỏ cạnh 

Đối với danh sách đầu vào trống, thuật toán tạo một mảng chỉ chứa trạng thái cho mã định danh 0. Quá trình quét ngay lập tức phát hiện ra rằng số 0 không được đánh dấu và in`0`, khớp với định nghĩa của mã định danh nhỏ nhất hiện có. 

Đối với số lượng rất lớn, hãy xem xét:```
5
0
1
999999999999999999999999999999
-2
3
```Giá trị lớn được chuẩn hóa có nhiều chữ số hơn N nên nó bị loại bỏ trước khi chuyển đổi. Giá trị âm cũng bị loại bỏ. Các mã định danh được đánh dấu là 0, một và ba, do đó quá trình quét sẽ trả về hai. 

Đối với các số 0 đứng đầu, hãy xem xét:```
4
000
001
003
4
```Các giá trị được chuẩn hóa thành 0, một, ba và bốn. Các vị trí 0, một, ba và bốn của mảng được đánh dấu, để lại vị trí thứ hai là mã định danh bị thiếu đầu tiên. Đầu ra là`2`. 

Đối với các số nhận dạng trùng lặp, hãy xem xét:```
5
2
2
0
1
1
```Mã định danh hai được đánh dấu hai lần, nhưng mảng boolean chỉ lưu trữ sự hiện diện chứ không lưu trữ tần số. Các vị trí 0, một và hai đều có mặt, do đó thuật toán trả về chính xác ba.
