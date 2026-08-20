---
title: "CF 102218A - Sinh Nhật Alan"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và chúng ta có thể sắp xếp lại các ký tự của nó theo bất kỳ thứ tự nào. Alan tìm kiếm từ điển của mình từ chuỗi nhỏ nhất về mặt từ điển cho đến chuỗi lớn nhất, vì vậy món quà tuyệt vời nhất là sự sắp xếp lại xuất hiện càng sớm càng tốt trong…"
date: "2026-08-18T22:31:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "A"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 593
verified: false
draft: false
---

[CF 102218A - Sinh nhật của Alan](https://codeforces.com/problemset/problem/102218/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 53 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và chúng ta có thể sắp xếp lại các ký tự của nó theo bất kỳ thứ tự nào. Alan tìm kiếm từ điển của mình từ chuỗi nhỏ nhất về mặt từ điển đến chuỗi lớn nhất, vì vậy món quà tốt nhất là sự sắp xếp lại xuất hiện càng sớm càng tốt theo thứ tự đó. 

Sự sắp xếp lại nhỏ nhất về mặt từ điển chỉ đơn giản là chuỗi có tất cả các ký tự được đặt theo thứ tự bảng chữ cái tăng dần. Ví dụ như các nhân vật của`mac`chỉ có thể được sắp xếp lại thành các chuỗi như`mac`,`mca`,`amc`, vân vân, và`acm`là cái đầu tiên trong số chúng về mặt từ điển. 

Độ dài có thể đạt tới (10^7), lớn hơn nhiều so với kích thước mà các thuật toán đắt tiền có thể sử dụng thoải mái trong giới hạn thời gian khoảng một giây. Thuật toán (O(N^2)) sẽ yêu cầu khoảng (10^{14}) thao tác ở giới hạn trên và phép liệt kê (O(N!)) còn vượt xa điều đó. Ngay cả cách sắp xếp dựa trên so sánh (O(N\log N)) cũng thực hiện theo thứ tự hàng trăm triệu so sánh khi (N=10^7). Thông tin bổ sung quan trọng là mỗi ký tự thuộc về một bảng chữ cái chỉ có 26 chữ cái viết thường. 

Trường hợp cạnh đầu tiên là một chuỗi một ký tự. Đối với đầu vào`1`theo sau là`z`, câu trả lời vẫn là`z`. Không có gì để sắp xếp lại và việc triển khai giả định ít nhất hai ký tự có thể gây ra lỗi ranh giới không cần thiết. 

Trường hợp cạnh thứ hai là các ký tự lặp lại. Đối với đầu vào`5`theo sau là`aaaaa`, câu trả lời là`aaaaa`. Việc triển khai bất cẩn coi các ký tự bằng nhau như các đối tượng riêng biệt có thể thực hiện công việc dư thừa, mặc dù kết quả phải chứa chính xác năm bản sao giống nhau. 

Trường hợp cạnh thứ ba là sự kết hợp trong đó ký tự nhỏ nhất xuất hiện nhiều lần. Đối với đầu vào`6`theo sau là`zzabca`, câu trả lời là`aabcz z`, hoặc`aabczz`không có không gian. hai`a`cả hai ký tự đều phải đứng trước mọi ký tự lớn hơn. Việc triển khai chỉ di chuyển ký tự nhỏ nhất một lần sẽ tạo ra thứ tự sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra mọi khả năng sắp xếp lại chuỗi, so sánh chúng và giữ chuỗi nhỏ nhất về mặt từ điển. Có (N!) hoán vị khi tất cả các ký tự đều khác biệt. So sánh hai chuỗi có thể kiểm tra tối đa (N) ký tự, vì vậy trường hợp xấu nhất là (O(N\cdot N!)). Tại (N=10^7), ngay cả việc viết ra phần nhỏ đầu tiên của các hoán vị này cũng là không thể. Các ký tự lặp lại làm giảm số lượng hoán vị riêng biệt, nhưng trường hợp xấu nhất về nguyên tắc vẫn có tất cả các ký tự khác biệt và hạn chế bảng chữ cái đầu vào không giải cứu được phép liệt kê hoán vị cho lớn (N). 

Một cách tiếp cận có mục đích chung hợp lý hơn là sắp xếp so sánh. Nếu chúng ta sắp xếp các ký tự theo thứ tự tăng dần thì kết quả chính xác là sự sắp xếp lại nhỏ nhất về mặt từ điển. Việc đó thực hiện các phép so sánh (O(N\log N)) với cách sắp xếp so sánh tiêu chuẩn. Điều này đúng, nhưng ràng buộc (N\le 10^7) làm cho hệ số logarit bổ sung trở nên đắt đỏ một cách không cần thiết, đặc biệt là dưới giới hạn 1,25 giây được sử dụng bởi bài toán ban đầu. 

Quan sát quan trọng là chỉ có 26 ký tự có thể. Chúng ta không cần phải khám phá thứ tự tương đối của hàng triệu ký tự riêng lẻ vì thứ tự bảng chữ cái của chúng đã được biết trước. Chúng ta chỉ cần đếm có bao nhiêu`a`ký tự tồn tại thì có bao nhiêu`b`các ký tự tồn tại, v.v. thông qua`z`. Khi đã biết 26 tần số đó, câu trả lời sẽ có được bằng cách viết từng ký tự theo số lần đếm được theo thứ tự bảng chữ cái. 

Đây là cách sắp xếp đếm ở dạng đơn giản nhất có thể. Thay vì trả tiền (O(N\log N)) để so sánh các ký tự có thứ tự đã biết trước, chúng tôi quét đầu vào một lần và sử dụng một mảng cố định gồm 26 bộ đếm. Việc xây dựng câu trả lời cũng cần (O(N)), do đó toàn bộ thuật toán là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các sắp xếp lại | (O(N\cdot N!)) | (O(N)) | Quá chậm | 
| Sắp xếp so sánh | (O(N\log N)) | (O(N)) | Đắt không cần thiết | 
| Đếm ký tự | (O(N)) | (O(N)) cho đầu vào/đầu ra, (O(26)) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N) và chuỗi (S). Giá trị của (N) cho chúng ta biết số lượng ký tự dự kiến, trong khi chuỗi thực tế chứa các ký tự mà chúng ta phải sắp xếp lại. 
2. Tạo 26 bộ đếm, một bộ đếm cho mỗi chữ cái viết thường. Với mỗi ký tự trong (S), tăng bộ đếm tương ứng với vị trí bảng chữ cái của nó. Vì chỉ có 26 vị trí có thể nên mỗi ký tự có thể được phân loại theo thời gian không đổi. 
3. Đi qua 26 quầy từ`a`bởi vì`z`. Đối với một ký tự có tần số (f), hãy thêm ký tự đó chính xác (f) lần vào kết quả. Tất cả các bản sao của ký tự nhỏ hơn phải đứng trước mỗi bản sao của ký tự lớn hơn trong một chuỗi tối thiểu về mặt từ điển. 
4. Viết chuỗi kết quả. Mỗi ký tự đầu vào được tính chính xác một lần và sau đó được phát ra chính xác một lần, do đó đầu ra là sự sắp xếp lại của chuỗi gốc. 

### Tại sao nó hoạt động 

Sau khi đếm, thuật toán biết chính xác có bao nhiêu bản sao của mỗi chữ cái xuất hiện trong đầu vào. Giả sử chuỗi kết quả có ký tự lớn hơn trước ký tự nhỏ hơn. Hoán đổi hai vị trí đó sẽ làm cho chuỗi nhỏ hơn về mặt từ điển trong khi vẫn giữ nguyên tất cả tần số ký tự. Do đó sự đảo ngược như vậy không thể xảy ra trong sự sắp xếp lại nhỏ nhất về mặt từ điển. Sự sắp xếp duy nhất không có sự đảo ngược như vậy là sự sắp xếp mà tất cả`a`ký tự đến trước, theo sau là tất cả`b`các ký tự, v.v. thông qua`z`. Thuật toán xây dựng chính xác sự sắp xếp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    count = [0] * 26

    for ch in s:
        count[ord(ch) - ord('a')] += 1

    result = []
    for i, freq in enumerate(count):
        if freq:
            result.append(chr(ord('a') + i) * freq)

    sys.stdout.write(''.join(result))

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc độ dài đã khai báo, mặc dù thuật toán không cần sử dụng nó sau khi đọc xong dữ liệu đầu vào. Việc giữ giá trị này rất hữu ích để khớp định dạng đầu vào, đồng thời lặp lại chuỗi thực tế sẽ tránh được các giả định về ranh giới dòng ngoài sự đảm bảo của câu lệnh. 

biểu hiện`ord(ch) - ord('a')`chuyển đổi một chữ cái viết thường thành một chỉ số từ 0 đến 25. Ví dụ:`a`trở thành 0,`b`trở thành 1, và`z`trở thành 25. Do đó, mảng bộ đếm chỉ cần 26 mục bất kể chuỗi chứa mười ký tự hay mười triệu. 

Kết quả được xây dựng từ các khối như`a * 3`hoặc`m * 2`. Điều này tốt hơn so với việc nối liên tục từng ký tự một, vì việc nối chuỗi lặp lại có thể tạo ra sự sao chép không cần thiết. Danh sách chứa tối đa 26 chuỗi và`''.join(result)`kết hợp chúng vào đầu ra cuối cùng một lần. 

Không có số nguyên nào có thể tràn trong Python. Bộ đếm lớn nhất chỉ là (10^7), được biểu thị dễ dàng bằng số nguyên Python. 

các`.strip()`cuộc gọi loại bỏ dòng mới được tạo bởi`readline()`. Vì đầu vào chỉ chứa các chữ cái viết thường nên không có khoảng trắng có ý nghĩa nào cần được giữ lại. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, chuỗi đầu vào là`mac`. 

| Đọc ký tự | Trạng thái truy cập cho các chữ cái khác 0 | Kết quả | 
| --- | --- | --- | 
|`m`|`m: 1`| | 
|`a`|`a: 1, m: 1`| | 
|`c`|`a: 1, c: 1, m: 1`| | 
| phát ra`a`|`a: 1, c: 1, m: 1`|`a`| 
| phát ra`c`|`a: 1, c: 1, m: 1`|`ac`| 
| phát ra`m`|`a: 1, c: 1, m: 1`|`acm`| 

Mảng tần số bảo toàn chính xác một bản sao của mỗi ký tự đầu vào. Đi qua các vị trí trong bảng chữ cái`a`trước`c`Và`c`trước`m`, cho`acm`, đây là sự sắp xếp lại đầu tiên có thể có theo thứ tự từ điển. 

Đối với Mẫu 2, chuỗi đầu vào là`geso`. 

| Đọc ký tự | Trạng thái truy cập cho các chữ cái khác 0 | Kết quả | 
| --- | --- | --- | 
|`g`|`g: 1`| | 
|`e`|`e: 1, g: 1`| | 
|`s`|`e: 1, g: 1, s: 1`| | 
|`o`|`e: 1, g: 1, o: 1, s: 1`| | 
| phát ra`e`|`e: 1, g: 1, o: 1, s: 1`|`e`| 
| phát ra`g`|`e: 1, g: 1, o: 1, s: 1`|`eg`| 
| phát ra`o`|`e: 1, g: 1, o: 1, s: 1`|`ego`| 
| phát ra`s`|`e: 1, g: 1, o: 1, s: 1`|`egos`| 

Một lần nữa, chuỗi cuối cùng chứa chính xác nhiều bộ ký tự giống như đầu vào. Các chữ cái của nó theo thứ tự bảng chữ cái không giảm dần, do đó không có cặp vị trí nào có thể hoán đổi để có được một chuỗi nhỏ hơn về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Đầu vào được quét một lần và 26 bộ đếm được phát ra với kích thước bảng chữ cái không đổi. | 
| Không gian | (O(N)) | Chuỗi đầu vào và kết quả đầu ra chiếm không gian tuyến tính, trong khi mảng tần số chỉ sử dụng 26 bộ đếm. | 

Với (N) lớn bằng (10^7), xử lý tuyến tính là mục tiêu thích hợp. Thuật toán thực hiện một lượng công việc không đổi trên mỗi ký tự đầu vào và không bao giờ phân bổ cấu trúc dữ liệu tỷ lệ thuận với số lượng hoán vị có thể có. Trạng thái đếm phụ vẫn cố định ở 26 mục, đặc biệt phù hợp với giới hạn bộ nhớ 64 MB ban đầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    count = [0] * 26

    for ch in s:
        count[ord(ch) - ord('a')] += 1

    result = []
    for i, freq in enumerate(count):
        if freq:
            result.append(chr(ord('a') + i) * freq)

    sys.stdout.write(''.join(result))

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

# Provided samples
assert run("3\nmac\n") == "acm", "sample 1"
assert run("4\ngeso\n") == "egos", "sample 2"

# Minimum-size input
assert run("1\nz\n") == "z", "single character"

# All characters are equal
assert run("7\naaaaaaa\n") == "aaaaaaa", "all equal"

# Boundary alphabet characters and repeated letters
assert run("8\nzzzzaaaa\n") == "aaaazzzz", "alphabet boundaries"

# Maximum-size input
s = "a" * 10_000_000
assert run(f"{len(s)}\n{s}\n") == s, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`/`z`|`z`| Kích thước tối thiểu và trường hợp không sắp xếp lại | 
|`7`/`aaaaaaa`|`aaaaaaa`| Ký tự lặp lại và đếm tần số | 
|`8`/`zzzzaaaa`|`aaaazzzz`| Ký tự bảng chữ cái nhỏ nhất và lớn nhất, bao gồm các khối lặp lại | 
|`10000000`/ mười triệu`a`nhân vật | Mười triệu giống nhau`a`nhân vật | Kích thước đầu vào tối đa và xử lý tuyến tính | 

Xác nhận kích thước tối đa được xây dựng có chủ ý thay vì được viết dưới dạng một dòng mười triệu ký tự theo nghĩa đen. Trong bài dự thi thực tế, chỉ dữ liệu đầu vào của giám khảo mới cần chứa dữ liệu đó, trong khi bài kiểm tra thể hiện cùng một điều kiện biên theo chương trình. 

## Vỏ cạnh 

Đối với trường hợp một ký tự, hãy xem xét:```
1
z
```Bộ đếm cho`z`trở thành 1 và mọi bộ đếm khác vẫn bằng 0. Giai đoạn phát xạ bỏ qua các chữ cái trống và xuất ra một`z`. Không có chỉ mục của ký tự thứ hai, do đó ranh giới tại (N=1) được xử lý một cách tự nhiên. 

Đối với các ký tự lặp lại, hãy xem xét:```
5
aaaaa
```Mỗi lần lặp lại tăng cùng một bộ đếm, để lại`count['a'] = 5`. Pha đầu ra phát ra`a * 5`, sản xuất`aaaaa`. Không cần logic hoán vị riêng biệt vì biểu diễn tần số đã nắm bắt được toàn bộ trạng thái liên quan đến câu trả lời. 

Đối với các ký tự nhỏ nhất lặp đi lặp lại trộn lẫn với các ký tự lớn hơn, hãy xem xét:```
6
zzabca
```Bộ đếm trở thành`a:2`,`b:1`,`c:1`, Và`z:2`. Thứ tự phát thải là`aa`, sau đó`b`, sau đó`c`, sau đó`zz`, sản xuất:```
aabczz
```Một sự sắp xếp lại bắt đầu chỉ với một`a`, chẳng hạn như`abaczz`, không thể tối ưu vì khác`a`tồn tại và đặt nó ngay sau cái đầu tiên`a`làm cho chuỗi nhỏ hơn. 

Đối với trường hợp ranh giới bảng chữ cái, hãy xem xét:```
4
zaba
```Các tần số là`a:2`,`b:1`, Và`z:1`. Thuật toán phát ra`a`chặn trước, sau đó`b`, sau đó`z`, sản xuất:```
aabz
```Điều này xác nhận rằng thuật toán không phụ thuộc vào thứ tự đầu vào và xử lý chính xác cả hai đầu của bảng chữ cái viết thường. 

Trường hợp kích thước tối đa tuân theo chính xác logic tương tự. Nếu tất cả (10^7) ký tự là`a`, quá trình quét sẽ thực hiện (10^7) số gia của bộ đếm thời gian không đổi và giai đoạn đầu ra sẽ phát ra một khối gồm (10^7) ký tự. Không có bước sắp xếp và không có nỗ lực liệt kê các sắp xếp, do đó thời gian chạy tăng tuyến tính với kích thước đầu vào thực tế.
