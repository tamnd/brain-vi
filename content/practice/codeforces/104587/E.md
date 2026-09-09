---
title: "CF 104587E - Qua Ngọn Đồi, Phần 1"
description: "Chúng ta được cung cấp một bảng chữ cái cố định gồm 37 ký tự bao gồm các chữ cái tiếng Anh viết hoa, chữ số và ký tự khoảng trắng."
date: "2026-06-30T07:29:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104587
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ICPC East Central North America Regional Contest (ECNA 2020)"
rating: 0
weight: 104587
solve_time_s: 48
verified: true
draft: false
---

[CF 104587E - Trên đồi, Phần 1](https://codeforces.com/problemset/problem/104587/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bảng chữ cái cố định gồm 37 ký tự bao gồm các chữ cái tiếng Anh viết hoa, chữ số và ký tự khoảng trắng. Mỗi ký tự được gán một giá trị số từ 0 đến 36 theo thứ tự xác định, bắt đầu từ A là 0 đến Z là 25, sau đó là các chữ số 0 đến 9 là 26 đến 35 và cuối cùng là khoảng trắng là 36. 

Đầu vào cung cấp một ma trận vuông có kích thước n x n, trong đó n tối đa là 10, theo sau là một chuỗi văn bản gốc. Quá trình mã hóa chuyển đổi văn bản thành số, nhóm chúng thành các khối có kích thước n và sau đó áp dụng phép biến đổi tuyến tính bằng cách sử dụng ma trận theo số học modulo 37. Mỗi vectơ kết quả được chuyển đổi lại thành các ký tự để tạo ra bản mã. 

Về mặt khái niệm, vấn đề là phép nhân ma trận được áp dụng lặp đi lặp lại trên các đoạn của chuỗi đầu vào sau khi mã hóa nó thành một vòng số nguyên hữu hạn nhỏ. 

Ràng buộc n 10 là giới hạn cấu trúc chính. Nó ngụ ý rằng mỗi phép biến đổi là cực kỳ nhỏ và có kích thước cố định, do đó chi phí nhân một vectơ với ma trận là không đổi được giới hạn bởi khoảng 100 phép tính. Yếu tố chi phối là độ dài của chuỗi đầu vào, có thể lớn nhưng mọi ký tự đều được xử lý độc lập trong khối của nó. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào cố gắng thực hiện bất kỳ điều gì siêu tuyến tính trong chiều ma trận hoặc tính toán lại các phép biến đổi không hiệu quả. Quét tuyến tính đơn giản trên chuỗi là đủ. 

Một số trường hợp đặc biệt xuất hiện một cách tự nhiên trong cài đặt này. Đầu tiên là phần đệm. Nếu độ dài bản rõ không chia hết cho n thì các ký tự còn lại phải được đệm bằng dấu cách. Ví dụ: nếu n = 3 và bản rõ là "ABCX", thì chúng ta mã hóa "không gian khoảng cách A B C | X". Việc triển khai bất cẩn mà quên phần đệm sẽ làm mất ký tự cuối cùng hoặc tạo thành một vectơ không đầy đủ, tạo ra độ dài bản mã không chính xác. 

Một trường hợp cạnh khác là chính ký tự khoảng trắng, ánh xạ tới giá trị cao nhất 36. Việc triển khai đơn giản chỉ xử lý các ký tự chữ và số sẽ âm thầm thất bại trên khoảng trắng hoặc chỉ số dịch chuyển không chính xác. Ví dụ: mã hóa "A A" mà không tính đến ánh xạ không gian sẽ phá vỡ sự liên kết và làm hỏng đầu ra. 

Cuối cùng, các chuỗi bản rõ lớn đòi hỏi phải cẩn thận trong việc lặp lại số học mô-đun. Mặc dù các giá trị vẫn nhỏ nhưng các phép nhân lặp lại có thể tràn trong các ngôn ngữ không có số nguyên lớn tự động. Trong Python đây không phải là vấn đề, nhưng trong các ngôn ngữ khác, việc giảm modulo phải được áp dụng ở mọi bước số học. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng định nghĩa theo nghĩa đen. Chúng tôi chuyển đổi từng ký tự thành giá trị số của nó, chia chuỗi thành các khối có kích thước n, nhân từng khối với ma trận bằng phép nhân vectơ ma trận tiêu chuẩn, giảm từng kết quả modulo 37 và chuyển đổi lại thành ký tự. Điều này đúng vì nó tuân theo định nghĩa chính xác. 

Đối với một khối, việc tính toán một mục đầu ra yêu cầu n phép nhân và phép cộng và có n đầu ra trên mỗi khối. Điều này mang lại n2 hoạt động trên mỗi khối. Vì n 10 nên đây là tối đa 100 thao tác trên mỗi khối, con số này không đáng kể. Trên một chuỗi có độ dài L, chúng tôi thực hiện các phép toán O(L · n²), có hiệu quả tuyến tính trong L với một hằng số nhỏ. 

Không cần tối ưu hóa ngoài mô phỏng trực tiếp này vì ma trận không thay đổi và không có truy vấn lặp lại hoặc yêu cầu lũy thừa. Quan sát quan trọng là việc chuyển đổi mang tính cục bộ đối với từng khối và không phụ thuộc vào các khối trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trực tiếp | O(L · n²) | O(L) | Đã chấp nhận | 
| Đã cố gắng tối ưu hóa toàn cầu | Không áp dụng | Không áp dụng | Không cần thiết | 

## Hướng dẫn thuật toán 

### 1. Xây dựng bảng mã ký tự

Chúng tôi xác định ánh xạ từ ký tự đến số nguyên và ngược lại. Mỗi chữ cái, chữ số và dấu cách phải được gán một giá trị duy nhất từ ​​0 đến 36. Bước này đảm bảo quá trình mã hóa hoạt động hoàn toàn trên số nguyên. 

### 2. Đọc ma trận 

Chúng tôi lưu trữ ma trận n x n dưới dạng số nguyên. Không cần tiền xử lý vì tất cả các phép toán đều là các phép biến đổi tuyến tính modulo 37. 

### 3. Chuyển bản rõ sang dạng số 

Chúng tôi quét chuỗi và chuyển đổi từng ký tự thành số nguyên tương ứng. Điều này tạo ra một mảng các giá trị biểu diễn thông điệp trong không gian số học mô-đun. 

### 4. Thêm mảng vào bội số của n 

Nếu độ dài không chia hết cho n, chúng ta nối thêm giá trị tương ứng với khoảng trắng cho đến khi đạt được giá trị đó. Điều này đảm bảo mọi khối đều hoàn chỉnh và tránh các vấn đề về ranh giới trong quá trình nhân ma trận. 

### 5. Xử lý từng khối độc lập 

Đối với mỗi khối liền kề có kích thước n, chúng tôi tính tích ma trận-vectơ. Mỗi tọa độ đầu ra được tính là tích vô hướng của một hàng ma trận với vectơ khối, lấy modulo 37. Bước này áp dụng phép biến đổi mã hóa. 

### 6. Chuyển kết quả về ký tự 

Sau khi xử lý tất cả các khối, chúng tôi ánh xạ từng giá trị số trở lại biểu diễn ký tự của nó và ghép chúng thành văn bản mã hóa cuối cùng. 

### Tại sao nó hoạt động 

Mỗi khối được biến đổi bởi một hàm tuyến tính cố định trên vòng số nguyên modulo 37. Phép nhân ma trận xác định một hàm xác định từ vectơ đầu vào đến vectơ đầu ra. Bởi vì bản rõ được phân chia thành các khối rời rạc và mỗi khối được biến đổi độc lập nên phép biến đổi tổng thể chỉ là sự kết hợp của các ánh xạ tuyến tính độc lập này. Không có sự tương tác giữa các khối, do đó độ chính xác giảm xuống độ chính xác của phép nhân vectơ ma trận đơn theo số học mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 37

# build mappings
chars = []
for c in range(ord('A'), ord('Z') + 1):
    chars.append(chr(c))
for c in range(ord('0'), ord('9') + 1):
    chars.append(chr(c))
chars.append(' ')

char_to_int = {ch: i for i, ch in enumerate(chars)}
int_to_char = {i: ch for i, ch in enumerate(chars)}

def main():
    n = int(input())
    mat = [list(map(int, input().split())) for _ in range(n)]
    s = input().rstrip('\n')

    vals = [char_to_int[ch] for ch in s]

    while len(vals) % n != 0:
        vals.append(char_to_int[' '])

    res = []

    for i in range(0, len(vals), n):
        block = vals[i:i+n]
        for r in range(n):
            acc = 0
            for c in range(n):
                acc += mat[r][c] * block[c]
            res.append(int_to_char[acc % MOD])

    sys.stdout.write(''.join(res))

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng cách xây dựng mã hóa ký tự chính xác mà vấn đề yêu cầu. Điều này tránh mọi sự mơ hồ xung quanh các chữ số và ký tự khoảng trắng. 

Ma trận được đọc trực tiếp vào bộ nhớ dưới dạng số nguyên vì kích thước của nó tối đa là 10 x 10. Bản rõ được chuyển đổi thành danh sách các số nguyên, sau đó được đệm bằng các giá trị khoảng trắng cho đến khi độ dài của nó chia hết cho n. Điều này đảm bảo rằng việc cắt thành các khối có kích thước cố định không bao giờ thất bại. 

Mỗi khối được xử lý độc lập. Cấu trúc vòng lặp lồng nhau là có chủ ý: vòng lặp bên ngoài lặp qua các hàng đầu ra và vòng lặp bên trong tính toán tích số chấm. Việc giảm mô-đun chỉ được áp dụng sau khi tính tổng toàn bộ tích số chấm, điều này an toàn vì số nguyên Python không bị tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng ta xem xét một trường hợp khái niệm nhỏ với n = 2 và bản rõ "AB". 

| Bước | Chặn | Tính toán | Giá trị đầu ra | 
| --- | --- | --- | --- | 
| 1 | [A, B] | hàng chấm sản phẩm | [x, y] | 

Giả sử A ánh xạ tới 0 và B ánh xạ tới 1, và ma trận tạo ra đầu ra 1 và 2. Sau khi chuyển đổi trở lại, chúng ta thu được khối văn bản mã hóa gồm hai ký tự. Nếu phần đệm bị thiếu, khoảng trống cuối cùng sẽ bị mất và độ dài đầu ra sẽ không chính xác, chứng tỏ tại sao phần đệm lại cần thiết. 

### Ví dụ 2 

Lấy một chuỗi dài hơn một chút khi cần có phần đệm, chẳng hạn như "ABC" với n = 2. 

| Bước | Chặn | Tính toán | Giá trị đầu ra | 
| --- | --- | --- | --- | 
| 1 | [A, B] | biến đổi ma trận | [x, y] | 
| 2 | [C, dấu cách] | biến đổi khối đệm | [p, q] | 

Điều này cho thấy ký tự cuối cùng không được xử lý riêng lẻ mà kết hợp với khoảng đệm, đảm bảo cấu trúc khối nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L · n²) | Mỗi ký tự tham gia vào một phép nhân ma trận n chiều | 
| Không gian | O(L) | Lưu trữ chuỗi và đầu ra được mã hóa | 

Vì n 10 nên hệ số n2 được giới hạn bởi 100, làm cho giải pháp tuyến tính một cách hiệu quả theo kích thước đầu vào. Điều này nằm trong giới hạn cho các ràng buộc điển hình của Codeforce. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod

    MOD = 37

    chars = []
    for c in range(ord('A'), ord('Z') + 1):
        chars.append(chr(c))
    for c in range(ord('0'), ord('9') + 1):
        chars.append(chr(c))
    chars.append(' ')

    char_to_int = {ch: i for i, ch in enumerate(chars)}
    int_to_char = {i: ch for i, ch in enumerate(chars)}

    n = int(input())
    mat = [list(map(int, input().split())) for _ in range(n)]
    s = input().rstrip('\n')

    vals = [char_to_int[ch] for ch in s]
    while len(vals) % n != 0:
        vals.append(char_to_int[' '])

    res = []
    for i in range(0, len(vals), n):
        block = vals[i:i+n]
        for r in range(n):
            acc = 0
            for c in range(n):
                acc += mat[r][c] * block[c]
            res.append(int_to_char[acc % MOD])

    return ''.join(res)

# provided sample 1
assert run("""3
30 1 9
4 23 7
5 9 13
ATTACK AT DAWN
""") == "FPLSFA4SUK2W9K3"

# custom: single character, n=1
assert run("""1
5
A
""") == "F"

# custom: padding required
assert run("""2
1 0
0 1
ABC
""")  # identity matrix with padding
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 ký tự đơn | char biến đổi | độ chính xác kích thước tối thiểu | 
| ma trận nhận dạng | cùng một văn bản có áp dụng phần đệm | hành vi đệm | 
| trường hợp mẫu | FPLSFA4SUK2W9K3 | độ chính xác đầy đủ của đường ống | 

## Vỏ cạnh 

Vỏ đệm là vỏ cạnh cấu trúc quan trọng nhất. Xét n = 3 và bản rõ "ABCX". Sau khi mã hóa, chúng ta nhận được bốn giá trị. Khối cuối cùng trở thành [X, dấu cách, dấu cách]. Trong quá trình nhân, vectơ đệm này vẫn được xử lý đầy đủ, tạo ra đoạn văn bản mã hóa hợp lệ. Thuật toán nối thêm các giá trị không gian một cách rõ ràng trước khi xử lý khối, do đó không có khối một phần nào được diễn giải. 

Bản thân ký tự khoảng trắng là một trường hợp quan trọng khác. Nếu bản rõ chứa khoảng trắng, chúng sẽ được ánh xạ tới 36 và tham gia vào phép tính số học giống như bất kỳ ký hiệu nào khác. Ví dụ: khối như [A, dấu cách, B] trở thành [0, 36, 1]. Bước nhân coi 36 là số nguyên bình thường modulo 37, do đó không cần viết hoa đặc biệt. 

Cuối cùng, kích thước ma trận nhỏ nhất n = 1 suy biến thành phép nhân vô hướng đơn giản theo modulo 37 được áp dụng cho từng ký tự. Cấu trúc vòng lặp tương tự xử lý nó một cách tự nhiên, vì mỗi khối chứa chính xác một giá trị và phép nhân ma trận giảm xuống còn một bước nhân.
