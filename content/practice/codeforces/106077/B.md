---
title: "CF 106077B - Thủy ngân"
description: "Chúng ta có một chuỗi văn bản mã hóa được tạo thành từ các chữ cái viết thường. Mọi ký tự trong chuỗi đã được dịch chuyển bằng cùng một độ lệch mật mã Caesar, nhưng không xác định được độ lệch. Chúng tôi cũng được cung cấp một số cụm từ mục tiêu."
date: "2026-06-25T12:10:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106077
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 2 (Beginner)"
rating: 0
weight: 106077
solve_time_s: 41
verified: true
draft: false
---

[CF 106077B - Thủy ngân](https://codeforces.com/problemset/problem/106077/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi văn bản mã hóa được tạo thành từ các chữ cái viết thường. Mọi ký tự trong chuỗi đã được dịch chuyển bằng cùng một độ lệch mật mã Caesar, nhưng không xác định được độ lệch. Chúng tôi cũng được cung cấp một số cụm từ mục tiêu. Sau khi áp dụng phép dịch ngược chính xác cho văn bản mã hóa, mỗi cụm từ trong số này phải xuất hiện ở đâu đó dưới dạng chuỗi con liền kề. 

Nhiệm vụ là tìm bất kỳ giá trị dịch chuyển nào có thể chuyển đổi bản mã thành một chuỗi chứa tất cả các cụm từ đã cho. Đầu ra là chuỗi được giải mã, vì vậy chúng ta cần áp dụng dịch chuyển được phát hiện và in kết quả. 

Độ dài bản mã tối đa là 1000 và có thể có tối đa 1000 cụm từ. Mỗi cụm từ đều ngắn, có độ dài tối đa là 30. Những giới hạn này đủ nhỏ để chúng ta có thể thử tất cả các phép dịch chuyển Caesar có thể có. Mật mã Caesar chỉ có 26 độ lệch có thể có, vì vậy công việc chính là kiểm tra 26 chuỗi được giải mã ứng viên. Với độ dài chuỗi khoảng 1000 và 1000 cụm từ, ngay cả kiểm tra kiểu bậc hai vẫn còn thấp hơn nhiều so với giới hạn. Các thuật toán dựa trên không gian tìm kiếm lớn hơn nhiều, chẳng hạn như thử thay thế tùy ý, sẽ không phù hợp với cấu trúc của bài toán. 

Các trường hợp chính xuất phát từ việc xử lý hướng dịch chuyển không chính xác hoặc cho rằng chỉ kiểm tra một cụm từ là đủ. Ví dụ, hãy xem xét:```
abc
1
b
```Chuỗi được giải mã đúng là:```
bcd
```bởi vì dịch chuyển từng chữ cái của`abc`chuyển tiếp bởi một người tạo ra cụm từ`b`xuất hiện. Một giải pháp luôn đi lùi sẽ thất bại. 

Một trường hợp khác là khi độ dịch chuyển câu trả lời bằng 0:```
hello
2
hell
llo
```Đầu ra phải là:```
hello
```Việc triển khai bất cẩn giả định rằng cần có một số chuyển động khác 0 sẽ bỏ lỡ câu trả lời hợp lệ này. 

Trường hợp tinh tế cuối cùng là sự xuất hiện lặp đi lặp lại và các cụm từ chồng chéo:```
aaaa
2
aa
aaa
```Đầu ra đúng là:```
aaaa
```Việc kiểm tra chuỗi con phải hoạt động với sự chồng chéo. Tìm kiếm bằng cách đánh dấu thủ công các ký tự đã sử dụng sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi sự dịch chuyển Caesar có thể xảy ra. Vì chỉ có 26 phép biến đổi khả dĩ nên chúng ta có thể giải mã toàn bộ chuỗi cho mỗi phép biến đổi. Đối với mỗi chuỗi được giải mã ứng viên, chúng tôi quét qua tất cả các cụm từ và kiểm tra xem mỗi cụm từ có phải là chuỗi con hay không. Nếu mọi cụm từ xuất hiện, chúng tôi đã tìm thấy câu trả lời hợp lệ. 

Lực lượng mạnh mẽ này là chính xác bởi vì mật mã Caesar có một tập hợp các khả năng hữu hạn. Thông báo ban đầu phải tương ứng với chính xác một trong 26 ca đó, vì vậy việc kiểm tra tất cả các ca đó đảm bảo rằng chúng ta sẽ đạt được thông báo đó. 

Sự kém hiệu quả duy nhất có thể xảy ra là do việc kiểm tra chuỗi con lặp đi lặp lại. Trong trường hợp xấu nhất, chúng tôi thử 26 ca, mỗi ca có 1000 cụm từ và mỗi lần tìm kiếm chuỗi con có thể chiếm khoảng độ dài của văn bản mã hóa. Điều này mang lại khoảng 26 * 1000 * 1000 thao tác, chỉ có khoảng 26 triệu so sánh ký tự cơ bản trong ước tính cấp cao. Điều này hoàn toàn có thể chấp nhận được với giới hạn đã cho. 

Quan sát quan trọng là không gian mật mã rất nhỏ. Chúng tôi không cần khôi phục sự dịch chuyển về mặt toán học hoặc xây dựng các cấu trúc khớp chuỗi nâng cao. Giá trị chưa biết là một số từ 0 đến 25, vì vậy phép liệt kê là giải pháp tự nhiên. Vấn đề trông giống như một vấn đề về chuỗi nhưng không gian tìm kiếm nhỏ lại chiếm ưu thế về độ phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 * n * m * l) | O(n) | Đã chấp nhận | 
| Tối ưu | O(26 * n * m * l) | O(n) | Đã chấp nhận | 

Đây,`n`là độ dài bản mã,`m`là số lượng cụm từ và`l`là độ dài cụm từ tối đa. 

## Hướng dẫn thuật toán 

1. Đọc bản mã và tất cả các cụm từ được yêu cầu. Lưu trữ các cụm từ vì cùng một danh sách phải được kiểm tra dựa trên mọi chuỗi được giải mã có thể. 
2. Hãy thử mọi phép dịch Caesar có thể từ 0 đến 25. Với mỗi lần dịch, hãy tạo chuỗi được giải mã dự kiến ​​bằng cách di chuyển từng ký tự bản mã theo số lượng đó. 
3. Kiểm tra từng cụm từ với chuỗi ứng cử viên. Nếu thiếu một cụm từ, hãy loại bỏ sự thay đổi này và tiếp tục. 
4. Khi một ca làm cho tất cả các cụm từ xuất hiện, hãy in ngay chuỗi đã giải mã. Vì câu trả lời được đảm bảo tồn tại nên một số thay đổi sẽ thành công. 

Lý do điều này có tác dụng là vì chỉ có 26 phiên bản được giải mã của văn bản mật mã. Người đúng được đảm bảo nằm trong số đó, vì vậy ứng cử viên hợp lệ đầu tiên mà chúng tôi tìm thấy là đủ. 

### Tại sao nó hoạt động 

Tính bất biến của tìm kiếm là mọi chuỗi ứng cử viên mà chúng tôi kiểm tra đều là một phép biến đổi Caesar hợp lệ của văn bản mã hóa. Chúng tôi không bao giờ bỏ qua một chuỗi có thể được giải mã vì mọi giá trị dịch chuyển từ 0 đến 25 đều được kiểm tra. 

Để có sự thay đổi chính xác, mọi cụm từ bắt buộc phải xuất hiện cùng với bảo đảm sự cố. Giai đoạn kiểm tra chấp nhận chính xác các chuỗi có tất cả các cụm từ, do đó, sự thay đổi chính xác sẽ vượt qua bài kiểm tra và được trả về. Một sự thay đổi sai cũng có thể vượt qua trong những trường hợp bất thường, nhưng bất kỳ chuỗi giải mã hợp lệ nào cũng được chấp nhận, vì vậy việc trả lại ứng cử viên vượt qua đầu tiên là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = int(input())
    phrases = [input().strip() for _ in range(n)]

    length = len(s)

    for shift in range(26):
        decoded = []
        for ch in s:
            decoded.append(chr((ord(ch) - ord('a') + shift) % 26 + ord('a')))
        decoded = ''.join(decoded)

        ok = True
        for p in phrases:
            if p not in decoded:
                ok = False
                break

        if ok:
            print(decoded)
            return

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên lưu trữ các cụm từ vì chúng cần được sử dụng lại cho tất cả 26 ứng viên. Việc tính toán lại hoặc đọc lại chúng sẽ làm tăng thêm công việc không cần thiết. 

Vòng giải mã chuyển đổi từng ký tự thành một số từ 0 đến 25, áp dụng phép dịch với số học modulo và chuyển đổi ngược lại. Hoạt động modulo xử lý các trường hợp bao quanh như dịch chuyển`z`chuyển tiếp tới`a`. 

Việc xác minh cụm từ sử dụng tìm kiếm chuỗi con của Python. Điều này là an toàn vì tổng kích thước đầu vào nhỏ. Việc nghỉ sớm khi thiếu một cụm từ sẽ tránh được việc kiểm tra không cần thiết sau khi một ứng viên bị trượt. 

Mã kiểm tra mọi ca kể cả số 0. Điều này quan trọng vì văn bản mã hóa có thể đã là tin nhắn được giải mã. 

## Ví dụ đã hoạt động 

Vì tuyên bố ban đầu không cung cấp mẫu nên hãy xem xét hai ví dụ sau. 

Đối với đầu vào:```
abcxyz
2
bcd
yza
```Dấu vết là: 

| Thay đổi | Chuỗi được giải mã | Kiểm tra cụm từ | Kết quả | 
| --- | --- | --- | --- | 
| 0 | abcxyz | thiếu bcd | Từ chối | 
| 1 | bcdyza | cả hai cụm từ được tìm thấy | Chấp nhận | 

Thuật toán dừng lại sau khi tìm thấy ca 1. Điều này thể hiện trường hợp bình thường trong đó chuyển động Caesar chính xác được tìm thấy bằng cách liệt kê. 

Đối với đầu vào:```
hello
2
hell
llo
```Dấu vết là: 

| Thay đổi | Chuỗi được giải mã | Kiểm tra cụm từ | Kết quả | 
| --- | --- | --- | --- | 
| 0 | xin chào | cả hai cụm từ được tìm thấy | Chấp nhận | 

Điều này xác nhận rằng trường hợp dịch chuyển số 0 được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 * | s | 
| Không gian | O( | s | 

Các giá trị lớn nhất vẫn làm cho phương pháp này đủ nhỏ. Ngay cả giới hạn trên của 26 triệu so sánh ký tự cũng có thể chấp nhận được trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    s = input().strip()
    n = int(input())
    phrases = [input().strip() for _ in range(n)]

    for shift in range(26):
        decoded = ''.join(
            chr((ord(c) - ord('a') + shift) % 26 + ord('a'))
            for c in s
        )

        if all(p in decoded for p in phrases):
            return decoded + "\n"

    return ""

assert solve_case("""abcxyz
2
bcd
yza
""") == "bcdyza\n", "sample style case"

assert solve_case("""hello
2
hell
llo
""") == "hello\n", "zero shift"

assert solve_case("""z
1
a
""") == "a\n", "wrap around"

assert solve_case("""aaaa
2
aa
aaa
""") == "aaaa\n", "overlapping substrings"

assert solve_case("""mnop
2
qrst
tuvw
""") == "qrst\n", "larger shift"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abcxyz`với các cụm từ`bcd`,`yza`|`bcdyza`| Tìm một sự dịch chuyển khác không | 
|`hello`với các cụm từ đã có sẵn |`hello`| Xử lý dịch chuyển số 0 | 
|`z`ĐẾN`a`|`a`| Xử lý bảng chữ cái bao quanh | 
| lặp đi lặp lại`a`nhân vật | Cùng một chuỗi | Cho phép trùng khớp | 
|`mnop`sang cụm từ đã chuyển | Chuỗi đã dịch chuyển | Kiểm tra chuyển đổi chung | 

## Vỏ cạnh 

Đối với trường hợp dịch chuyển số 0:```
hello
2
hell
llo
```Thuật toán bắt đầu với shift 0, xây dựng`hello`và kiểm tra cả hai cụm từ. Vì cả hai đều có mặt nên nó quay trở lại ngay lập tức. Một giải pháp bắt đầu tìm kiếm từ ca 1 cuối cùng vẫn có hiệu quả, nhưng việc kiểm tra số 0 giúp lý luận đơn giản và tránh các giả định không cần thiết. 

Đối với trường hợp bao quanh:```
z
1
a
```Ở ca 1, việc tính toán ký tự trở thành`(25 + 1) % 26 = 0`, ánh xạ trở lại`a`. Đây là lý do tại sao hoạt động modulo là cần thiết. 

Đối với các kết quả trùng lặp:```
aaaa
2
aa
aaa
```Chuỗi ứng cử viên không thay đổi ở shift 0. Việc kiểm tra chuỗi con tìm`aa`Và`aaa`bên trong cùng một khu vực của chuỗi. Thuật toán chỉ quan tâm đến sự tồn tại chứ không quan tâm đến việc tiêu thụ các ký tự, do đó sự trùng lặp được xử lý một cách tự nhiên.
