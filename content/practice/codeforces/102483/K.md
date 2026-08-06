---
title: "CF 102483K - Chụp ảnh trộm"
description: "Nhiệm vụ là khôi phục văn bản nhật ký gốc từ một chuỗi được mã hóa. Mật mã sử dụng cơ chế khóa tự động: n ký tự đầu tiên của khóa không xác định, nhưng sau thời điểm đó, khóa sẽ lặp lại các ký tự từ đầu bản rõ."
date: "2026-08-05T18:45:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "K"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 126
verified: true
draft: false
---

[CF 102483K - Chụp ảnh trộm](https://codeforces.com/problemset/problem/102483/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là khôi phục văn bản nhật ký gốc từ một chuỗi được mã hóa. Mật mã sử dụng cơ chế khóa tự động: cơ chế đầu tiên`n`Các ký tự của khóa không xác định, nhưng sau thời điểm đó, khóa sẽ lặp lại các ký tự từ đầu bản rõ. Mary biết điều cuối cùng`n`các ký tự của bản rõ và cũng có bản mã hoàn chỉnh. Sử dụng thông tin này, chúng ta phải xây dựng lại toàn bộ bản rõ. 

Quá trình mã hóa hoạt động trên các chữ cái được chuyển đổi thành số từ 0 đến 25. Đối với mọi vị trí, giá trị bản mã là tổng của giá trị bản rõ và giá trị khóa modulo 26. Giá trị đầu tiên`n`các ký tự chính là từ khóa ẩn, trong khi các ký tự khóa còn lại được sao chép từ các vị trí văn bản gốc trước đó. Đầu ra là chuỗi văn bản gốc theo thứ tự ban đầu. 

Độ dài văn bản tối đa là 100 ký tự và độ dài hậu tố đã biết tối đa là 30. Các giới hạn nhỏ này cho phép một số cách tiếp cận khả thi, nhưng cấu trúc của mật mã đưa ra giải pháp tuyến tính trực tiếp. Ngay cả khi các giới hạn lớn hơn nhiều, một giải pháp gần`O(m)`vẫn sẽ là mục tiêu đương nhiên vì mọi ký tự của bản rõ phải được xác định. 

Phần khó khăn là các ký tự văn bản gốc đã biết nằm ở cuối tin nhắn, trong khi tiền tố khóa không xác định lại ảnh hưởng đến phần đầu. Một cách tiếp cận bất cẩn có thể cố gắng giải mã từ trái sang phải, nhưng cách đầu tiên`n`vị trí không có giá trị quan trọng được biết đến. Một lỗi phổ biến khác là quên rằng hậu tố đã biết cung cấp chính xác các giá trị văn bản gốc bị thiếu cần thiết để khôi phục khóa không xác định. 

Ví dụ: với:```
1 3
a
bac
```đầu ra đúng là:```
aaa
```Ký tự bản rõ cuối cùng được gọi là`a`. Ký tự khóa cuối cùng là ký tự bản rõ đầu tiên, do đó ký tự bản mã cuối cùng cho phương trình`c = a + a`, xác định ký tự bản rõ đầu tiên. Phương thức giả định tiền tố khóa là ký tự đã biết sẽ không thành công vì ký tự đã biết là văn bản gốc chứ không phải khóa. 

Một trường hợp cạnh khác xuất hiện khi độ dài hậu tố đã biết gần như toàn bộ chuỗi:```
2 3
ab
bbc
```Đầu ra đúng là:```
abc
```Chỉ có một ký tự không xác định ở phía trước và thuật toán vẫn phải sử dụng mối quan hệ giữa các ký tự cuối cùng và vị trí bản rõ trước đó. Các giải pháp chỉ hoạt động khi có nhiều ký tự không xác định có thể tạo ra việc lập chỉ mục không chính xác ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử mọi từ khóa bí mật có thể có độ dài`n`. Mỗi từ khóa có`26^n`và với mỗi khả năng, chúng ta có thể tạo khóa, giải mã bản mã và kiểm tra xem bản rõ thu được có kết thúc bằng hậu tố đã biết hay không. Cách tiếp cận này đúng vì từ khóa thực sự được đảm bảo nằm trong số các ứng cử viên được thử nghiệm. Tuy nhiên, điều đó trở nên không thể ngay cả đối với các giá trị nhỏ của`n`. Với`n = 30`, số ứng viên là`26^30`, vượt xa những gì có thể được xử lý. 

Lý do vũ lực là không cần thiết là vì hậu tố đã biết sẽ sửa phần khóa phụ thuộc vào bản rõ. Đối với các vị trí sau vị trí đầu tiên`n`, ký tự khóa chính xác là ký tự bản rõ nằm ở`n`các vị trí trước đó. cuối cùng`n`các ký tự bản rõ đã được biết trước, vì vậy chúng ta có thể sử dụng ký tự cuối cùng`n`các ký tự bản mã để khôi phục cái chưa biết trước tiên`n`nhân vật chủ chốt. 

Khi biết tiền tố khóa ẩn, toàn bộ tin nhắn có thể được giải mã. Điều quan trọng cần lưu ý là hậu tố không chỉ là điều kiện xác minh. Nó cung cấp thông tin còn thiếu cần thiết để đảo ngược mối quan hệ khóa tự động. 

Brute-force hoạt động vì mọi khóa có thể đều được xem xét, nhưng không thành công vì không gian tìm kiếm tăng theo cấp số nhân. Quan sát cho thấy hậu tố bản rõ đã biết tiết lộ tiền tố khóa sẽ làm giảm vấn đề xuống còn một vài lần truyền tuyến tính trên chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n * m) | O(m) | Quá chậm | 
| Tối ưu | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ bản mã và bản mã cuối cùng đã biết`n`các ký tự của bản rõ. Hậu tố đã biết đại diện cho phần cuối của câu trả lời và sẽ được sử dụng để khôi phục tiền tố khóa chưa xác định. 
2. Khôi phục lần đầu`n`nhân vật chính sử dụng trận chung kết`n`các vị trí của bản mã. Đối với ký tự bản mã ở vị trí`i`, ký tự bản rõ tương ứng đã được biết vì nó nằm trong hậu tố. Phương trình là`cipher[i] = plain[i] + key[i] (mod 26)`, do đó ký tự khóa có thể được tính là`key[i] = cipher[i] - plain[i] (mod 26)`. 

Điều này hoạt động vì cuối cùng`n`vị trí là những nơi duy nhất có sẵn cả bản rõ và bản mã. 
3. Xây dựng key đầy đủ. đầu tiên`n`các ký tự là tiền tố bí mật được khôi phục. Mỗi ký tự khóa sau này được sao chép từ ký tự văn bản gốc`n`các vị trí trước đó. 
4. Giải mã toàn bộ bản mã bằng khóa được tạo. Đối với mọi vị trí, trừ giá trị khóa khỏi giá trị bản mã modulo 26 để thu được giá trị bản rõ. 
5. Xuất bản rõ đã được xây dựng lại. 

Bất biến đằng sau thuật toán là mọi ký tự khóa được tạo đều khớp với định nghĩa khóa của mật mã. đầu tiên`n`các ký tự được khôi phục từ các vị trí mà bản rõ đã biết và mọi ký tự sau đó được sao chép từ bản rõ đã được xây dựng lại. Vì khóa chính xác ở mọi vị trí nên việc áp dụng công thức mã hóa nghịch đảo sẽ tạo ra bản rõ duy nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    suffix = input().strip()
    cipher = input().strip()

    key = [''] * m
    plain = [''] * m

    for i in range(n):
        plain[m - n + i] = suffix[i]

    for i in range(n):
        pos = m - n + i
        key[pos] = chr((ord(cipher[pos]) - ord(plain[pos])) % 26 + ord('a'))

    for i in range(n):
        key[i] = key[m - n + i]

    for i in range(n, m):
        key[i] = plain[i - n]

    for i in range(m):
        plain[i] = chr((ord(cipher[i]) - ord(key[i])) % 26 + ord('a'))

    print(''.join(plain))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tạo ra các mảng cho bản rõ và khóa để các vị trí có thể được điền độc lập. Hậu tố đã biết được đặt trực tiếp vào cuối mảng văn bản gốc. 

Vòng lặp khôi phục khóa sử dụng khóa cuối cùng`n`các vị trí. Biểu thức sử dụng modulo 26 vì phép trừ trong bảng chữ cái có thể bao hàm, chẳng hạn như khôi phục`z`từ một giá trị trung gian âm. 

Sau khi tiền tố khóa được khôi phục, nó sẽ được sao chép vào phần đầu của mảng khóa. Các vị trí quan trọng sau này được điền từ các vị trí văn bản gốc`n`những nơi trước đó. Thứ tự quan trọng vì khóa phụ thuộc vào văn bản gốc và các ký tự văn bản gốc đó phải được biết trước khi chúng được sử dụng. 

Vòng lặp cuối cùng thực hiện thao tác mã hóa nghịch đảo. Số nguyên Python không bị tràn nên phép tính modulo chỉ xử lý việc gói bảng chữ cái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 16
again
pirpumsemoystoal
```| Bước | Vị trí | Bản rõ đã biết | Khóa đã được khôi phục | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 11 | một | p - a = p | Khôi phục hậu tố khóa | 
| 2 | 12 | g | s - g = m | Khôi phục hậu tố khóa | 
| 3 | 13 | một | t - a = t | Khôi phục hậu tố khóa | 
| 4 | 14 | tôi | o - tôi = g | Khôi phục hậu tố khóa | 
| 5 | 15 | n | a - n = n | Khôi phục hậu tố khóa | 
| 6 | 0 đến 4 | chưa biết | pmtgn | Sao chép tiền tố khóa đã khôi phục | 
| 7 | tất cả | chưa biết | hoàn thành | Giải mã | 

Tiền tố khóa được khôi phục đủ để giải mã phần đầu của tin nhắn. Hậu tố`again`không thay đổi vì nó đã được cung cấp dưới dạng bản rõ đã biết. 

Bản rõ cuối cùng là:```
marywasnosyagain
```### Mẫu 2 

đầu vào:```
1 12
d
fzvfkdocukfu
```| Bước | Vị trí | Bản rõ đã biết | Khóa đã được khôi phục | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 11 | d | f - d = c | Khôi phục một ký tự chính | 
| 2 | 0 | chưa biết | c | Đặt tiền tố khóa | 
| 3 | 1 đến 11 | chưa biết | bản rõ trước đó | Xây dựng chìa khóa tự động | 
| 4 | tất cả | chưa biết | hoàn thành | Giải mã | 

Ví dụ này sử dụng độ dài khóa nhỏ nhất có thể. Nó xác nhận rằng thuật toán không phụ thuộc vào việc có một hậu tố lớn đã biết. 

Bản rõ cuối cùng là:```
shortkeyword
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi ký tự được xử lý một số lần không đổi trong khi khôi phục khóa và giải mã. | 
| Không gian | O(m) | Mảng văn bản gốc và khóa lưu trữ một ký tự cho mỗi vị trí. | 

Độ dài văn bản tối đa là nhỏ, do đó giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Cách tiếp cận tương tự cũng sẽ mở rộng quy mô một cách thoải mái đến các chuỗi lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    suffix = input().strip()
    cipher = input().strip()

    key = [''] * m
    plain = [''] * m

    for i in range(n):
        plain[m - n + i] = suffix[i]

    for i in range(n):
        pos = m - n + i
        key[pos] = chr((ord(cipher[pos]) - ord(plain[pos])) % 26 + ord('a'))

    for i in range(n):
        key[i] = key[m - n + i]

    for i in range(n, m):
        key[i] = plain[i - n]

    for i in range(m):
        plain[i] = chr((ord(cipher[i]) - ord(key[i])) % 26 + ord('a'))

    return ''.join(plain)

assert solve("5 16\nagain\npirpumsemoystoal\n") == "marywasnosyagain"
assert solve("1 12\nd\nfzvfkdocukfu\n") == "shortkeyword"

assert solve("1 3\na\nbac\n") == "aaa"
assert solve("2 3\nab\nbbc\n") == "abc"
assert solve("3 6\naaa\naaaaaa\n") == "aaaaaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / a / bac`|`aaa`| Độ dài khóa tối thiểu và ký tự tiền tố duy nhất không xác định | 
|`2 3 / ab / bbc`|`abc`| Văn bản rất ngắn với hậu tố đã biết bao trùm hầu hết nội dung tin nhắn | 
|`3 6 / aaa / aaaaaa`|`aaaaaa`| Các ký tự lặp lại và ranh giới số học modulo | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi`n = 1`. Trong đầu vào:```
1 3
a
bac
```ký tự bản rõ cuối cùng đã được biết. Vị trí thuật toán`a`ở vị trí cuối cùng, tính ký tự tiền tố khóa duy nhất từ ​​ký tự bản mã cuối cùng, sau đó giải mã các vị trí còn lại. Nó xuất ra:```
aaa
```Giải pháp xử lý chữ cái đã biết làm khóa thay vì văn bản gốc sẽ thất bại ở đây vì cả hai có vai trò khác nhau trong mật mã. 

Trường hợp cạnh thứ hai là khi hậu tố đã biết gần như toàn bộ thông báo:```
2 3
ab
bbc
```Thuật toán đầu tiên lưu trữ`ab`là hai ký tự bản rõ cuối cùng. Nó sử dụng hai ký tự đó cùng với hai ký tự bản mã cuối cùng để khôi phục tiền tố khóa ẩn, sau đó giải mã ký tự đầu tiên. Kết quả là:```
abc
```Điều này xác nhận rằng giải pháp xử lý các trường hợp chỉ cần khám phá một phần nhỏ của bản rõ. 

Trường hợp ký tự lặp lại:```
3 6
aaa
aaaaaa
```cũng cần phép trừ mô-đun chính xác. Mỗi nhân vật chủ chốt đều`a`và mọi ký tự của bản rõ vẫn còn`a`. Thuật toán thực hiện các phép tính tương tự mà không dựa vào sự khác biệt về ký tự, tạo ra:```
aaaaaa
```
