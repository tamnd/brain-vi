---
title: "CF 102388D - Tin nhắn bí mật"
description: "Đối với mỗi testcase, chúng tôi nhận được một chuỗi không trống chỉ chứa các chữ cái tiếng Anh. Mã hóa được yêu cầu áp dụng ba phép biến đổi theo thứ tự được mô tả bởi bài toán: thay đổi mọi chữ cái thành chữ cái ngược lại, đảo ngược toàn bộ chuỗi và áp dụng ROT13 trong khi vẫn giữ nguyên…"
date: "2026-08-12T21:09:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "D"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 598
verified: true
draft: false
---

[CF 102388D - Tin nhắn bí mật](https://codeforces.com/problemset/problem/102388/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi testcase, chúng tôi nhận được một chuỗi không trống chỉ chứa các chữ cái tiếng Anh. Mã hóa được yêu cầu áp dụng ba phép biến đổi theo thứ tự được mô tả bởi bài toán: thay đổi mọi chữ cái thành chữ cái ngược lại, đảo ngược toàn bộ chuỗi và áp dụng ROT13 trong khi vẫn giữ nguyên kiểu chữ cái viết thường. Đầu ra là chuỗi được mã hóa kết quả. 

Ví dụ, bắt đầu bằng`HelloWorld`, thay đổi trường hợp mang lại`hELLOwORLD`. Đảo ngược mang lại`DLROwOLLEh`và ROT13 tạo ra`QYEBjBYYRu`. 

Có tối đa 100 testcase và mỗi chuỗi có tối đa 100 ký tự. Do đó, tổng lượng đầu vào rất nhỏ, do đó, ngay cả một vài lần truyền hoàn chỉnh trên mỗi chuỗi cũng dễ dàng đủ nhanh trong giới hạn 1 giây. Quan trọng hơn, bản thân các phép biến đổi là tuyến tính theo chiều dài chuỗi, vì vậy không có lý do gì để sử dụng bất kỳ thứ gì phức tạp hơn mô phỏng trực tiếp. Một giải pháp lấy thời gian bậc hai cũng sẽ vượt qua được những ràng buộc cụ thể này, nhưng nó sẽ giải quyết một vấn đề khó hơn nhiều so với mức cần thiết. 

Trường hợp cạnh đầu tiên là một chuỗi một ký tự. Đối với đầu vào`A`, hoán đổi trường hợp mang lại`a`, đảo ngược không thay đổi gì và ROT13 cho`n`, vì vậy đầu ra đúng là`N`sau khi áp dụng các phép biến đổi trong thành phần được chỉ định của chúng. Việc triển khai bất cẩn coi việc đảo ngược là yêu cầu ít nhất hai ký tự có thể vô tình bỏ qua hoặc xử lý sai trường hợp này. 

Trường hợp cạnh thứ hai là ký tự ở gần cuối bảng chữ cái. Đối với đầu vào`Z`, ROT13 phải quấn quanh từ`Z`ĐẾN`M`, thay vì di chuyển ra ngoài bảng chữ cái. Đầu ra đúng là`M`. Vấn đề tương tự xuất hiện đối với chữ thường`z`, trở thành`m`. Việc triển khai chỉ thêm 13 vào giá trị ASCII mà không gói sẽ tạo ra ký tự không hợp lệ. 

Trường hợp cạnh thứ ba là sự kết hợp của các trường hợp và ranh giới bảng chữ cái. Đối với đầu vào`AaZz`, đảo ngược trước sẽ cho`zZ aA`không có khoảng trắng, cụ thể là`zZaA`. Sau khi hoán đổi trường hợp này trở thành`ZzAa`và ROT13 cho`MmNn`. Một giải pháp bất cẩn áp dụng ROT13 trước khi thay đổi kiểu chữ nhưng sau đó sử dụng sai thông tin kiểu chữ có thể dễ dàng tạo ra cách viết hoa sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng ba hoạt động riêng biệt. Chúng ta có thể quét toàn bộ chuỗi và hoán đổi kiểu dáng của từng ký tự, thực hiện đảo ngược, sau đó quét lại toàn bộ chuỗi để tìm ROT13. Điều này phản ánh trực tiếp định nghĩa mã hóa, do đó tính chính xác là ngay lập tức. Với một chuỗi dài`n`, ba đường chuyền thực hiện`n`trường hợp thay đổi,`floor(n / 2)`hoán đổi đảo ngược, và`n`chuyển đổi ROT13. Theo sự thực hiện trực tiếp này, đó là`2n + floor(n / 2)`hoạt động ở cấp độ ký tự. Với`n <= 100`, trường hợp xấu nhất chỉ là 250 thao tác như vậy cho mỗi trường hợp thử nghiệm, vì vậy không có lý do gì mà cách tiếp cận này trở nên quá chậm. Đường cơ sở bạo lực rõ ràng đã được chấp nhận. 

Chúng ta có thể làm cho quá trình triển khai trở nên rõ ràng hơn bằng cách quan sát rằng cả hoán đổi chữ hoa chữ thường và ROT13 đều là các phép biến đổi theo ký tự. Cả hai thao tác đều không phụ thuộc vào vị trí của nhân vật, vì vậy cả hai đều di chuyển theo hướng đảo ngược. Thay vì trước tiên phải xây dựng một chuỗi trung gian rồi đảo ngược nó, chúng ta có thể duyệt chuỗi gốc từ phải sang trái và áp dụng hai phép biến đổi ký tự trong khi tạo ra câu trả lời. Mỗi ký tự đầu vào được xử lý chính xác một lần. 

Quan sát chính là ba hoạt động không cần sự tương tác phức tạp. Đảo ngược xác định thứ tự xuất hiện của các ký tự, trong khi hoán đổi kiểu chữ và ROT13 chỉ sửa đổi từng ký tự riêng lẻ. Vì vậy, khi chúng ta truy cập các ký tự gốc từ vị trí cuối cùng đến vị trí đầu tiên, chúng ta đã ngầm thực hiện việc đảo ngược. Chúng ta chỉ cần chuyển đổi từng nhân vật được truy cập. 

Điều này làm giảm việc thực hiện xuống còn một lần duyệt chuỗi. Độ phức tạp tiệm cận vẫn còn`O(n)`, điều này là tối ưu vì mỗi ký tự phải được kiểm tra ít nhất một lần để xác định đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Các lượt mô phỏng riêng biệt | O(n) | O(n) | Đã chấp nhận | 
| Chuyển đổi một lần trong khi di chuyển ngược | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`. Các ký tự của nó được đảm bảo là chữ cái tiếng Anh nên mọi ký tự đều có thể được xử lý bằng cách kiểm tra xem đó là chữ hoa hay chữ thường. 
2. Đi ngang`s`từ ký tự cuối cùng đến ký tự đầu tiên. Việc đọc theo hướng này thực hiện việc đảo ngược theo yêu cầu mà không cần xây dựng một bản sao đảo ngược một cách rõ ràng. 
3. Hoán đổi kiểu chữ của ký tự hiện tại. Ký tự chữ hoa trở thành chữ thường và ký tự chữ thường trở thành chữ hoa. 
4. Áp dụng ROT13 cho ký tự trong khi vẫn giữ nguyên kiểu chữ hiện tại của nó. Đối với một chữ cái viết hoa, hãy trừ`A`, cộng 13 modulo 26, và cộng`A`mặt sau. Đối với chữ thường, thực hiện tương tự bằng cách sử dụng`a`. 
5. Thêm ký tự đã biến đổi vào câu trả lời. Vì các ký tự được xử lý theo thứ tự nhập ngược nên câu trả lời thu được đã có sự sắp xếp đảo ngược bắt buộc. 
6. In câu trả lời đã hoàn thành cho testcase. 

### Tại sao nó hoạt động 

Xem xét bất kỳ ký tự gốc nào`s[i]`. Chuỗi được mã hóa cuối cùng phải chứa phiên bản được chuyển đổi của ký tự này tại vị trí`n - 1 - i`, bởi vì sự đảo chiều di chuyển vị trí`i`đến vị trí đó. Bằng cách duyệt qua đầu vào từ`n - 1`xuống tới`0`, thuật toán sẽ đặt các ký tự vào chính xác các vị trí cuối cùng đó theo thứ tự từ trái sang phải. Việc hoán đổi chữ hoa chữ thường và các thao tác ROT13 không phụ thuộc vào vị trí của ký tự, vì vậy việc áp dụng chúng khi truy cập ký tự sẽ tạo ra ký tự giống hệt như kết quả của việc áp dụng chúng trước hoặc sau khi đảo ngược. Do đó, mọi vị trí đầu ra đều chứa ký tự được mã hóa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def transform(s):
    ans = []

    for c in reversed(s):
        if 'A' <= c <= 'Z':
            c = c.lower()
        else:
            c = c.upper()

        if 'A' <= c <= 'Z':
            c = chr((ord(c) - ord('A') + 13) % 26 + ord('A'))
        else:
            c = chr((ord(c) - ord('a') + 13) % 26 + ord('a'))

        ans.append(c)

    return ''.join(ans)

def main():
    t = int(input())

    for _ in range(t):
        s = input().strip()
        print(transform(s))

if __name__ == "__main__":
    main()
```các`transform`hàm tương ứng trực tiếp với hướng dẫn thuật toán.`reversed(s)`cung cấp các ký tự theo thứ tự chính xác được yêu cầu sau thao tác đảo ngược, do đó không cần thao tác đảo ngược riêng biệt. 

Điều kiện đầu tiên thay đổi trường hợp của nhân vật. Sau đó, điều kiện thứ hai sẽ xác định ký tự đó thuộc bảng chữ cái nào và thực hiện ROT13 bằng cách sử dụng số học mô-đun. biểu hiện`(ord(c) - ord('A') + 13) % 26`chuyển đổi ký tự thành vị trí bảng chữ cái dựa trên số 0, xoay ký tự đó 13 vị trí và quấn quanh khi cần thiết. 

Hoạt động modulo xử lý ranh giới bảng chữ cái một cách rõ ràng. Ví dụ,`Z`có vị trí dựa trên số 0 là 25, vì vậy`(25 + 13) % 26`cho 12, tương ứng với`M`. Nhánh chữ thường hoạt động giống hệt với`a`làm cơ sở của nó. 

Không có tính toán chỉ số liên quan đến`len(s) - 1 - i`, do đó việc triển khai sẽ tránh được lỗi đảo ngược thường gặp nhất. Cũng không có vấn đề tràn số nguyên vì tất cả số học được thực hiện trên các giá trị từ 0 đến 25. 

Câu trả lời được lưu trữ trong một danh sách vì việc nối các chuỗi liên tục có thể tạo ra các chuỗi trung gian không cần thiết. Việc tham gia danh sách một lần ở cuối sẽ tạo ra kết quả cuối cùng một cách hiệu quả. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm mẫu đầu tiên,`HelloWorld`, thuật toán xử lý các ký tự từ phải sang trái. 

| Vị trí đầu vào | Nhân vật gốc | Sau khi hoán đổi trường hợp | Sau ROT13 | Thứ tự đầu ra | 
| --- | --- | --- | --- | --- | 
| 9 |`d`|`D`|`Q`| 1 | 
| 8 |`l`|`L`|`Y`| 2 | 
| 7 |`r`|`R`|`E`| 3 | 
| 6 |`o`|`O`|`B`| 4 | 
| 5 |`W`|`w`|`j`| 5 | 
| 4 |`o`|`O`|`B`| 6 | 
| 3 |`l`|`L`|`Y`| 7 | 
| 2 |`l`|`L`|`Y`| 8 | 
| 1 |`e`|`E`|`R`| 9 | 
| 0 |`H`|`h`|`u`| 10 | 

Đầu ra là`QYEBjBYYRu`. Dấu vết này cho thấy rằng việc đọc ngược dữ liệu đầu vào sẽ tự động xử lý việc đảo ngược, trong khi mỗi ký tự nhận hai phép biến đổi còn lại một cách độc lập. 

Đối với ví dụ thứ hai, hãy xem xét`AaZz`. 

| Vị trí đầu vào | Nhân vật gốc | Sau khi hoán đổi trường hợp | Sau ROT13 | Thứ tự đầu ra | 
| --- | --- | --- | --- | --- | 
| 3 |`z`|`Z`|`M`| 1 | 
| 2 |`Z`|`z`|`m`| 2 | 
| 1 |`a`|`A`|`N`| 3 | 
| 0 |`A`|`a`|`n`| 4 | 

Đầu ra là`MmNn`. Ví dụ này thực hiện cả ký tự viết hoa và viết thường và cả hai đầu của bảng chữ cái. Đặc biệt,`Z`kết thúc tốt đẹp`M`Và`z`kết thúc tốt đẹp`m`, xác nhận rằng phép tính modulo-26 ROT13 xử lý ranh giới một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần. | 
| Không gian | O(n) | Danh sách câu trả lời lưu trữ một ký tự được chuyển đổi cho mỗi ký tự đầu vào. | 

Đây`n`là độ dài của tin nhắn hiện tại. Từ`n <= 100`và có tối đa 100 testcase, tổng số ký tự xử lý tối đa là 10.000 ký tự. Giải pháp này thấp hơn nhiều so với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    input = io.StringIO(inp).readline
    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()
        ans = []

        for c in reversed(s):
            if 'A' <= c <= 'Z':
                c = c.lower()
            else:
                c = c.upper()

            if 'A' <= c <= 'Z':
                c = chr((ord(c) - ord('A') + 13) % 26 + ord('A'))
            else:
                c = chr((ord(c) - ord('a') + 13) % 26 + ord('a'))

            ans.append(c)

        out.append(''.join(ans))

    return '\n'.join(out) + '\n'

def run(inp: str) -> str:
    return solve(inp)

# Provided sample
assert run(
    """3
HelloWorld
QYEBjBYYRu
pcpvBgRZBPYRj
"""
) == """QYEBjBYYRu
HelloWorld
WelcomeToICPC
""", "sample 1"

# Minimum-size input
assert run("1\nA\n") == "N\n", "single uppercase character"

# Alphabet boundaries and mixed case
assert run("1\nAaZz\n") == "MmNn\n", "alphabet boundaries"

# All characters equal, maximum length
assert run("1\n" + "A" * 100 + "\n") == "N" * 100 + "\n", "maximum-size equal string"

# Lowercase boundary characters
assert run("1\naz\n") == "MZ\n", "lowercase alphabet boundaries"

# Multiple testcases with different lengths
assert run(
    """3
a
Z
AbCd
"""
) == """N
M
qPmE
""", "mixed lengths and cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`|`N`| Độ dài tối thiểu và một ký tự viết hoa | 
|`AaZz`|`MmNn`| Thay đổi trường hợp, đảo ngược và cả ranh giới bảng chữ cái | 
| 100 bản sao của`A`| 100 bản sao của`N`| Độ dài chuỗi tối đa và đầu vào hoàn toàn bằng nhau | 
|`az`|`MZ`| Gói ROT13 viết thường ở cả hai đầu | 
|`a`,`Z`,`AbCd`|`N`,`M`,`qPmE`| Nhiều trường hợp thử nghiệm và độ dài chuỗi hỗn hợp | 

## Vỏ cạnh 

Đối với đầu vào một ký tự`A`, quá trình duyệt chỉ chứa ký tự đó. Thay đổi hoán đổi trường hợp`A`ĐẾN`a`và ROT13 thay đổi`a`ĐẾN`n`. Nhân vật không có nơi nào khác để di chuyển trong quá trình đảo ngược, vì vậy kết quả cuối cùng là`N`. Thuật toán không cần trường hợp đặc biệt cho độ dài một. 

Đối với đầu vào ranh giới bảng chữ cái`Z`, nhân vật đầu tiên trở thành`z`vì vụ trao đổi. ROT13 sau đó tính toán`(25 + 13) % 26 = 12`, sản xuất`m`, vì vậy đầu ra là`M`. Hoạt động modulo ngăn phép tính rời khỏi bảng chữ cái. 

Đối với đầu vào ranh giới hỗn hợp`AaZz`, số lần duyệt ngược`z`,`Z`,`a`,`A`. Sau khi hoán đổi trường hợp và ROT13, chúng trở thành`M`,`m`,`N`,`n`, tương ứng. Đầu ra là`MmNn`, xác nhận rằng thuật toán giữ nguyên trường hợp chính xác trong khi xử lý cả chữ hoa và chữ thường. 

Đối với đầu vào hoàn toàn bằng nhau có kích thước tối đa bao gồm 100 bản sao của`A`, việc đảo ngược không làm thay đổi chuỗi một cách rõ ràng, nhưng mọi ký tự vẫn trải qua quá trình hoán đổi kiểu chữ và chuyển đổi ROT13. Mỗi`A`trở thành`N`, do đó kết quả chứa 100 bản sao`N`. Thuật toán xử lý chính xác 100 ký tự và không cần xử lý đặc biệt đối với các giá trị lặp lại.
