---
title: "CF 102870H - Mã Hamming và gấu trúc Orz"
description: "Thông điệp được truyền đi là một khối được mã hóa Hamming. Một khối có 2^k bit và các bit được lập chỉ mục từ 0 đến 2^k - 1. Khối nhận được có thể đã được sửa đổi bằng cách thay đổi tối đa hai bit."
date: "2026-07-25T13:17:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "H"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 49
verified: true
draft: false
---

[CF 102870H - Mã Hamming và gấu trúc Orz](https://codeforces.com/problemset/problem/102870/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thông điệp được truyền đi là một khối được mã hóa Hamming. Một khối có`2^k`bit và các bit được lập chỉ mục từ`0`ĐẾN`2^k - 1`. Khối nhận được có thể đã được sửa đổi bằng cách thay đổi nhiều nhất là hai bit. Nhiệm vụ là xác định xem khối nhận được có còn hợp lệ hay không, liệu chính xác một bit có bị hỏng và xác định chỉ mục của nó hay không, hoặc liệu hai bit có bị thay đổi và lỗi không thể sửa được hay không. 

Đầu vào chứa một số khối độc lập cho đến hết tệp. Đối với mỗi khối, chúng tôi nhận được`k`và sau đó là chuỗi nhị phân hoàn chỉnh biểu thị các bit nhận được. Đầu ra mô tả trạng thái của khối đó: một từ mã hợp lệ, một vị trí bị lỗi hoặc một lỗi hai bit không thể sửa được. 

Các ràng buộc cho chúng ta biết loại giải pháp nào là cần thiết. Từ`k`có thể lớn như`16`, một khối có thể chứa`2^16 = 65536`bit. Tổng số bit trong tất cả các trường hợp thử nghiệm được giới hạn ở khoảng một triệu, do đó, việc truyền tuyến tính qua từng khối là phương pháp dự kiến. Bất kỳ phương pháp nào thử các vị trí lỗi có thể có hoặc so sánh với tất cả các từ mã hợp lệ đều không thể thực hiện được vì không gian tìm kiếm tăng theo cấp số nhân. 

Có một số trường hợp dễ bỏ sót vì bit ở vị trí chỉ mục`0`cư xử khác với các vị trí khác. Đó là bit chẵn lẻ bổ sung, do đó, một lỗi duy nhất ở đó có hội chứng vị trí bằng 0 và có thể bị nhầm lẫn với khối hợp lệ nếu chúng ta chỉ kiểm tra các bit chẵn lẻ được lập chỉ mục. 

Ví dụ: khối nhận được này biểu thị trường hợp chỉ có bit đầu tiên sai:```
3
10000000
```Câu trả lời đúng là:```
d(0) is changed
```Một giải pháp chỉ tính toán xor của các chỉ số của các bit được đặt sẽ tính bằng 0 và in không chính xác`good`. 

Một trường hợp tinh tế khác là hai lỗi trong đó một lỗi là bit chẵn lẻ:```
3
11000000
```Đầu ra đúng là:```
broken
```Xor vị trí trỏ đến vị trí`1`, nhưng kiểm tra tính chẵn lẻ tổng thể cho biết có hai bit bị đảo ngược. Coi hội chứng như một lỗi duy nhất được đảm bảo sẽ báo cáo không chính xác`d(1) is changed`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng sửa chữa mọi thứ có thể. Chúng ta có thể lật từng cái`2^k`vị trí, kiểm tra xem khối kết quả có thỏa mãn tất cả các điều kiện chẵn lẻ Hamming hay không và quyết định xem vị trí đó có bị hỏng hay không. Điều này đúng vì từ mã gốc phải cách khối nhận được một bit khi có đúng một lỗi. Tuy nhiên, nó đòi hỏi phải kiểm tra tới`2^k`ứng viên, và mọi chi phí kiểm tra`O(k)`hoặc`O(2^k)`tùy theo việc thực hiện. Vì`k = 16`, ngay cả phiên bản đơn giản hơn cũng thực hiện hàng tỷ thao tác trên các đầu vào lớn. 

Cấu trúc của mã Hamming cho chúng ta một cách nhỏ hơn nhiều để xác định lỗi. Mọi chỉ số khác 0 đều có một biểu diễn nhị phân và mỗi điều kiện chẵn lẻ tương ứng với một bit của biểu diễn đó. Nếu chúng ta xor chỉ số của tất cả các vị trí chứa`1`, kết quả là xor của tất cả các vị trí bị hỏng. Giá trị này được gọi là hội chứng. 

Bit chẵn lẻ tổng thể tại chỉ mục`0`cho chúng ta biết có bao nhiêu bit đã được thay đổi theo modulo hai. Nếu tổng số bit được thiết lập trong khối nhận được có tính chẵn lẻ chính xác thì không có số lượng thay đổi lẻ. Kết hợp hội chứng với tính chẵn lẻ tổng thể sẽ đưa ra tất cả các trường hợp có thể xảy ra: 

Nếu hội chứng bằng 0 và tính chẵn lẻ tổng thể là chính xác thì khối đó hợp lệ. 

Nếu hội chứng khác 0 và tính chẵn lẻ tổng thể không chính xác thì chính xác một bit đã thay đổi. Bản thân hội chứng là chỉ số bị hỏng. 

Nếu hội chứng bằng 0 và tính chẵn lẻ tổng thể không chính xác, chỉ bit`0`đã thay đổi. 

Nếu hội chứng này khác 0 và tính chẵn lẻ tổng thể là chính xác thì có đúng hai bit bị thay đổi, do đó khối bị hỏng. 

Giải pháp brute-force hoạt động vì nó tìm kiếm một từ mã hợp lệ gần đó nhưng không thành công khi khối lớn. Quan sát cho thấy việc kiểm tra tính chẵn lẻ của Hamming đã mã hóa vị trí lỗi cho phép chúng tôi giảm toàn bộ quá trình xuống còn một lần quét đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * k) | O(1) | Quá chậm | 
| Tối ưu | O(2^k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`k`và chuỗi bit nhận được. Độ dài của chuỗi là kích thước khối,`2^k`. 
2. Tính hội chứng bằng cách xoring các chỉ số của tất cả các vị thế từ`1`ĐẾN`2^k - 1`có chứa một`1`. Chức vụ`0`bị bỏ qua vì nó là bit chẵn lẻ tổng thể và không có thông tin chỉ mục. 
3. Đếm số lượng`1`bit trong toàn bộ khối nhận được. Tính chẵn lẻ của số này cho biết liệu điều kiện chẵn lẻ tổng thể có được thỏa mãn hay không. 
4. Sử dụng kết quả hội chứng và chẵn lẻ để phân loại lỗi. 

Nếu hội chứng bằng 0 và tính chẵn lẻ hợp lệ, xuất ra`good`. 

Nếu hội chứng khác 0 và tính chẵn lẻ không hợp lệ, xuất ra hội chứng dưới dạng vị trí đã thay đổi. 

Nếu hội chứng bằng 0 và tính chẵn lẻ không hợp lệ, hãy xuất vị trí đó`0`đã thay đổi. 

Ngược lại, xuất ra`broken`. 

Lý do điều này có tác dụng là vì hội chứng này chính xác là xor của tất cả các vị trí đã thay đổi. Một vị trí thay đổi duy nhất để lại chỉ số riêng của nó là hội chứng. Hai vị trí thay đổi tạo ra xor của hai chỉ số khác nhau và bit chẵn lẻ cho phép chúng ta phân biệt điều này với trường hợp lỗi đơn. 

Tại sao nó hoạt động: các phương trình chẵn lẻ Hamming không thay đổi đối với một khối hợp lệ, do đó xor của tất cả các vị trí chứa`1`phải bằng không. Một bit bị lật sẽ đóng góp chỉ mục của nó cho xor, có nghĩa là hội chứng chứa chính xác thông tin vị trí tổng hợp của tất cả các lỗi. Bit chẵn lẻ bổ sung tách số lỗi lẻ khỏi số lỗi chẵn, cho phép chúng ta phân biệt lỗi một bit với lỗi hai bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(k, s):
    n = 1 << k

    syndrome = 0
    ones = 0

    for i, ch in enumerate(s):
        if ch == '1':
            ones += 1
            if i != 0:
                syndrome ^= i

    if ones % 2 == 0:
        parity_ok = True
    else:
        parity_ok = False

    if syndrome == 0 and parity_ok:
        return "good"

    if syndrome != 0 and not parity_ok:
        return f"d({syndrome}) is changed"

    if syndrome == 0 and not parity_ok:
        return "d(0) is changed"

    return "broken"

def main():
    ans = []
    while True:
        line = input()
        if not line:
            break
        if not line.strip():
            continue
        k = int(line)
        s = input().strip()
        ans.append(solve_case(k, s))

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Việc triển khai tuân theo bốn trường hợp từ hướng dẫn trực tiếp. Biến`syndrome`lưu trữ xor của tất cả các vị trí không chẵn lẻ có chứa`1`. Điều quan trọng là vòng lặp bỏ qua chỉ mục`0`, bởi vì việc bao gồm nó sẽ phá hủy sự khác biệt giữa lỗi bit chẵn lẻ và lỗi vị trí thông thường. 

Biến`ones`đếm tất cả các bit được đặt, bao gồm cả vị trí`0`. Tổng số chẵn lẻ xác định số bit bị thay đổi là số lẻ hay số chẵn. Vì vấn đề đảm bảo nhiều nhất là hai thay đổi, nên tính chẵn lẻ tổng thể không hợp lệ có nghĩa là chính xác một bit bị thay đổi. 

Không cần mảng hoặc bộ nhớ bổ sung. Chuỗi được xử lý một lần và tất cả các phép tính đều khớp thoải mái bên trong các số nguyên Python vì chỉ số lớn nhất chỉ là`65535`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
1011101110111011
```Thuật toán tính toán trạng thái sau: 

| Bước | Vị trí hiện tại | Chút | Hội chứng | Số cái | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 0 | 0 | 
| Đọc tất cả`1`vị trí ngoại trừ`0`| hoàn thành | hỗn hợp | 0 | 12 | 

Hội chứng bằng 0 và tổng số đơn vị là số chẵn, vì vậy tất cả các kiểm tra chẵn lẻ đều đạt. 

Đầu ra:```
good
```Điều này xác nhận rằng khối Hamming hợp lệ không tạo ra thông tin lỗi. 

Đối với mẫu thứ hai:```
4
1011101110111010
```Bit cuối cùng đã thay đổi so với khối hợp lệ. 

| Bước | Vị trí hiện tại | Chút | Hội chứng | Số cái | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 0 | 0 | 
| Đọc vị trí bị hỏng | 15 | 1 | 15 | lẻ | 
| Kết thúc quá trình quét | - | - | 15 | lẻ | 

Hội chứng chỉ vào vị trí`15`và tính chẵn lẻ lẻ cho thấy chỉ có một bit bị thay đổi. 

Đầu ra:```
d(15) is changed
```Điều này thể hiện mục đích chính của hội chứng: nó xác định trực tiếp một bit bị hỏng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^k) | Mỗi bit trong khối nhận được được xử lý một lần. | 
| Không gian | O(1) | Chỉ có bộ đếm hội chứng và chẵn lẻ được lưu trữ. | 

Khối lớn nhất có`65536`bit và tổng kích thước đầu vào được giới hạn ở khoảng một triệu bit. Quét tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(k, s):
    syndrome = 0
    ones = 0
    for i, ch in enumerate(s):
        if ch == '1':
            ones += 1
            if i != 0:
                syndrome ^= i

    if syndrome == 0 and ones % 2 == 0:
        return "good"
    if syndrome != 0 and ones % 2 == 1:
        return f"d({syndrome}) is changed"
    if syndrome == 0 and ones % 2 == 1:
        return "d(0) is changed"
    return "broken"

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = []
    while True:
        line = sys.stdin.readline()
        if not line:
            break
        if not line.strip():
            continue
        k = int(line)
        s = sys.stdin.readline().strip()
        out.append(solve_case(k, s))
    sys.stdin = old
    return "\n".join(out)

assert run("""4
1011101110111011
4
1011101110111010
4
1011101110111110
""") == """good
d(15) is changed
broken""", "samples"

assert run("""3
00000000
""") == "good", "minimum valid case"

assert run("""3
10000000
""") == "d(0) is changed", "parity bit error"

assert run("""3
11000000
""") == "broken", "two bit error"

assert run("""4
1011101110111111
""") == "d(10) is changed", "single positional error"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc |`good`,`d(15) is changed`,`broken`| Trường hợp tiêu chuẩn | 
|`3 / 00000000`|`good`| Khối hợp lệ có kích thước tối thiểu | 
|`3 / 10000000`|`d(0) is changed`| Xử lý đặc biệt bit chẵn lẻ | 
|`3 / 11000000`|`broken`| Hai lỗi liên quan đến vị trí`0`| 
|`4 / 1011101110111111`|`d(10) is changed`| Xử lý hội chứng khác không | 

## Vỏ cạnh 

Trường hợp đặc biệt đầu tiên là lỗi ở chính bit chẵn lẻ. Coi như:```
3
10000000
```Quá trình quét không tìm thấy vị trí nào khác 0 có giá trị`1`, do đó hội chứng vẫn còn`0`. Tuy nhiên, tổng số cái là số lẻ, có nghĩa là việc kiểm tra tính chẵn lẻ tổng thể không thành công. Vì không có thông tin vị trí nên lỗi duy nhất có thể xảy ra là ở chỉ mục`0`và thuật toán in ra:```
d(0) is changed
```Trường hợp đặc biệt thứ hai là hai bit bị thay đổi trong đó một trong số chúng là bit chẵn lẻ:```
3
11000000
```Hội chứng là`1`bởi vì vị trí`1`đóng góp cho xor. Tổng số bit là số chẵn, nghĩa là số bit bị thay đổi là số chẵn. Vì hội chứng không bằng 0 nên tình huống duy nhất có thể xảy ra trong các ràng buộc bài toán là hai bit bị hỏng, do đó thuật toán đưa ra:```
broken
```Đây chính xác là lý do tại sao việc kiểm tra tính chẵn lẻ phải được kết hợp với hội chứng thay vì chỉ sử dụng hội chứng.
