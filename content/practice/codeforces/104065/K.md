---
title: "CF 104065K - Khớp mẫu trong A Minor `` Không gian thấp"
description: "Chúng ta có hai chuỗi: một chuỗi mẫu s và một chuỗi văn bản t. Nhiệm vụ là đếm xem có bao nhiêu vị trí bắt đầu trong t tạo ra sự xuất hiện của s dưới dạng chuỗi con liền kề. Sự chồng chéo được cho phép, vì vậy mọi kết quả trùng khớp hợp lệ đều đóng góp vào câu trả lời một cách độc lập."
date: "2026-07-02T03:20:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104065
codeforces_index: "K"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Mianyang Onsite"
rating: 0
weight: 104065
solve_time_s: 49
verified: true
draft: false
---

[CF 104065K - Khớp mẫu trong âm thứ ``Dấu cách thấp''](https://codeforces.com/problemset/problem/104065/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi: một chuỗi mẫu`s`và một chuỗi văn bản`t`. Nhiệm vụ là đếm xem có bao nhiêu vị trí bắt đầu trong`t`tạo ra sự xuất hiện của`s`như một chuỗi con liền kề. Sự chồng chéo được cho phép, vì vậy mọi kết quả trùng khớp hợp lệ đều đóng góp vào câu trả lời một cách độc lập. 

Một cách trực tiếp để xem điều này là chúng ta trượt`s`trên mọi sự liên kết có thể có trong`t`và kiểm tra xem tất cả các ký tự có khớp không. Đầu ra là tổng số cách sắp xếp khớp hoàn toàn. 

Các ràng buộc rất nghiêm trọng: cả hai chuỗi có thể dài bằng$10^7$. Điều đó loại trừ mọi giải pháp so sánh các ký tự lặp đi lặp lại theo cách lồng nhau. Ngay cả việc quét tuyến tính trên mỗi căn chỉnh cũng có thể có tới$10^{14}$so sánh ký tự trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện. 

Trí nhớ cũng bị thắt chặt về tinh thần, mặc dù giới hạn danh nghĩa rất lớn. Lưu ý về truy cập tuần tự và khung "không gian thấp" gợi ý rằng chúng ta nên tránh lưu trữ các cấu trúc phụ trợ lớn như bảng chức năng tiền tố đầy đủ cho các phép nối rất lớn hoặc tiền xử lý nhiều lượt yêu cầu các mẫu truy cập ngẫu nhiên. 

Vỏ ngoài tinh tế đến từ các chuỗi có tính lặp lại cao. Nếu như`s = "aaaaa"`Và`t = "aaaaaaaaaa"`, mọi căn chỉnh đều hợp lệ theo kiểu chồng chéo. Chiến lược thoát sớm không khớp ngây thơ vẫn có thể chuyển sang hành vi bậc hai vì sự không khớp rất hiếm và các so sánh liên tục khởi động lại. 

Một trường hợp cạnh khác là khi`s`có độ dài 1. Khi đó mọi ký tự trong`t`là một kiểm tra đối sánh độc lập và câu trả lời chỉ đơn giản là tần số của ký tự đó. Điều này thường bộc lộ từng lỗi một trong quá trình triển khai trượt. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi vị trí`i`TRONG`t`từ`0`ĐẾN`m - n`, chúng tôi so sánh`s`với chuỗi con`t[i:i+n]`từng nhân vật. Nếu tất cả các ký tự khớp nhau, chúng ta sẽ tăng câu trả lời. 

Điều này đúng vì nó trực tiếp thực thi định nghĩa về đẳng thức chuỗi con. Chế độ thất bại là hiệu suất. Mỗi căn chỉnh có thể yêu cầu tới`n`so sánh và có`m - n + 1`sắp xếp, tạo ra độ phức tạp trong trường hợp xấu nhất$O(nm)$. Với cả hai lên đến$10^7$, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải so sánh lại các ký tự từ đầu ở mỗi ca. Cấu trúc của thông tin tiền tố lặp lại trong mẫu cho phép chúng ta sử dụng lại các kết quả khớp một phần. Khi xảy ra sự không khớp, chúng tôi muốn biết chúng tôi có thể “lùi lại” mô hình đó đến mức nào mà không cần bắt đầu lại từ con số 0. 

Đây chính xác là những gì hàm tiền tố từ thuật toán Knuth-Morris-Pratt nắm bắt được. Nó mã hóa tiền tố thích hợp dài nhất của`s`đó cũng là hậu tố cho mọi tiền tố của`s`. Với thông tin này, chúng tôi có thể quét`t`một lần và duy trì bao nhiêu ký tự của`s`hiện tại chúng tôi đã khớp. Mỗi nhân vật trong`t`gây ra nhiều nhất một số lần điều chỉnh con trỏ không đổi trong`s`. 

Mối quan tâm về bộ nhớ được giải quyết vì mảng tiền tố có kích thước`n`, điều này có thể chấp nhận được và quá trình quét hoàn toàn là tuần tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| KMP |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng KMP để đếm số lần xuất hiện mẫu trong một lần truyền qua văn bản. 

### Các bước 

1. Tính hàm tiền tố`pi`cho chuỗi`s`. 

Đối với mỗi vị trí`i`,`pi[i]`lưu trữ tiền tố thích hợp dài nhất của`s[:i+1]`đó cũng là hậu tố của chuỗi con này. Cấu trúc này cho chúng ta biết chúng ta có thể tiếp tục khớp bao xa sau khi không khớp mà không cần khởi động lại từ số 0. 
2. Khởi tạo con trỏ`j = 0`, đại diện cho bao nhiêu ký tự của`s`hiện đang phù hợp với`t`. 
3. Quét văn bản`t`từ trái sang phải sử dụng chỉ mục`i`. Đối với mỗi nhân vật`t[i]`, cố gắng kéo dài trận đấu hiện tại. 
4. Nếu`t[i] != s[j]`, liên tục sử dụng lại`j = pi[j-1]`cho đến khi`j == 0`hoặc một kết quả phù hợp được tìm thấy. 

Điều này đảm bảo chúng tôi luôn giữ phần khớp hợp lệ lâu nhất thay vì khởi động lại từ đầu. 
5. Nếu`t[i] == s[j]`, tăng`j`thêm 1, mở rộng tiền tố phù hợp. 
6. Nếu`j == n`, một trận đấu đầy đủ của`s`kết thúc ở vị trí`i`. Tăng câu trả lời và đặt lại`j = pi[j-1]`để cho phép các trận đấu chồng chéo. 

### Tại sao nó hoạt động 

Tại mọi vị trí trong`t`, biến`j`đại diện cho độ dài của tiền tố dài nhất của`s`khớp với hậu tố của tiền tố được xử lý của`t`. Hàm tiền tố đảm bảo rằng sau khi không khớp, bất kỳ tiền tố ngắn hơn nào vẫn có thể khớp đều đã được mã hóa trong`pi`, vì vậy chúng tôi không bao giờ bỏ lỡ căn chỉnh hợp lệ. Mỗi trận đấu đầy đủ được phát hiện chính xác một lần vào lúc này`j`đạt tới`n`, đảm bảo tính đúng đắn, không trùng lặp, thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_pi(s):
    n = len(s)
    pi = [0] * n
    j = 0
    for i in range(1, n):
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def solve():
    n, m = map(int, input().split())
    s = input().strip()
    t = input().strip()

    pi = build_pi(s)

    j = 0
    ans = 0

    for ch in t:
        while j > 0 and ch != s[j]:
            j = pi[j - 1]
        if ch == s[j]:
            j += 1
        if j == n:
            ans += 1
            j = pi[j - 1]

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai được chia tự nhiên thành giai đoạn xây dựng hàm tiền tố và giai đoạn khớp trực tuyến. Mảng tiền tố được xây dựng lại một lần`s`và sau đó được sử dụng lại trong quá trình quét`t`. 

Quá trình quét chính chỉ duy trì hai biến, chỉ mục hiện tại trong`s`và bộ đếm câu trả lời. Vòng lặp dự phòng rất cần thiết: nó đảm bảo rằng các thông tin không khớp không khiến chúng ta phải khởi động lại một cách không cần thiết. 

Một cạm bẫy phổ biến là quên thiết lập lại`j`sau khi tìm thấy một trận đấu. Không có`j = pi[j - 1]`, các kết quả trùng lặp sẽ bị mất. Một vấn đề khác là lập chỉ mục:`s[j]`chỉ có hiệu lực khi`j < n`, do đó việc kiểm tra tính bằng nhau phải xảy ra trước khi truy cập vượt quá giới hạn, điều này được đảm bảo bằng cách kiểm tra`j == n`ngay sau khi tăng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = "aba"
t = "abababc"
```| tôi | t[i] | j trước | hành động | j sau | trận đấu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | 0 | trận đấu | 1 | không | 
| 1 | b | 1 | trận đấu | 2 | không | 
| 2 | một | 2 | khớp → đầy đủ | 3 → 1 | vâng | 
| 3 | b | 1 | trận đấu | 2 | không | 
| 4 | một | 2 | khớp → đầy đủ | 3 → 1 | vâng | 
| 5 | b | 1 | trận đấu | 2 | không | 
| 6 | c | 2 | dự phòng rồi dừng lại | 0 | không | 

Trả lời = 2. 

Dấu vết này cho thấy các kết quả trùng lặp được xử lý chính xác thông qua thiết lập lại dự phòng thành`pi[j-1]`. 

### Ví dụ 2 

đầu vào:```
s = "a"
t = "abracadabra"
```| tôi | t[i] | j trước | hành động | j sau | trận đấu | 
| --- | --- | --- | --- | --- | --- | 
| tất cả các vị trí | bất kỳ ký tự nào | 0 | so sánh | 1 hoặc 0 | đếm khi 'a' | 

Mọi`'a'`tăng câu trả lời ngay lập tức vì`n = 1`. 

Đáp án = 5. 

Điều này xác nhận rằng thuật toán sẽ giảm một cách tự nhiên việc đếm ký tự trong trường hợp mẫu ký tự đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| việc xây dựng hàm tiền tố là tuyến tính trong`n`, và mỗi ký tự trong`t`được xử lý với các bước dự phòng được khấu hao không đổi | 
| Không gian |$O(n)$| mảng tiền tố lưu trữ một số nguyên cho mỗi vị trí trong`s`| 

Các ràng buộc cho phép lên đến$10^7$tổng kích thước đầu vào, do đó, thuật toán truyền phát theo thời gian tuyến tính với một lần chuyển qua`t`vừa vặn thoải mái, miễn là việc triển khai tránh được các hoạt động nặng nề trên cao. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# We redefine solve-safe runner
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = backup_stdin
        sys.stdout = backup_stdout

# provided samples
assert run("3 7\naba\nabababc\n") == "2"
assert run("1 11\na\nabracadabra\n") == "5"

# custom cases
assert run("1 5\na\naaaaa\n") == "5"
assert run("2 4\naa\naaaa\n") == "3"
assert run("3 3\nabc\nabc\n") == "1"
assert run("3 3\nabc\ndef\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char lặp lại đơn | 5 | xử lý chồng chéo tối đa | 
| mẫu tiền tố lặp lại | 3 | chồng chéo trận đấu chính xác | 
| khớp chính xác | 1 | độ đúng cơ sở | 
| không khớp | 0 | con đường thất bại | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mẫu có tính tự chồng chéo cao. Ví dụ,`s = "aaaa"`Và`t = "aaaaaaaa"`. Trong quá trình quét,`j`liên tục đạt 4 và đặt lại thành`pi[3] = 3`, cho phép tiếp tục ngay lập tức. Điều này đảm bảo mọi căn chỉnh hợp lệ đều được tính, bao gồm cả những căn chỉnh gần như trùng lặp hoàn toàn. 

Một trường hợp khác là văn bản hoàn toàn không khớp, chẳng hạn như`s = "abc"`Và`t = "zzz..."`. Đây`j`liên tục giữ nguyên ở mức 0 và thuật toán chỉ thực hiện các phép so sánh đơn giản mà không cần nhập các vòng lặp dự phòng, thể hiện hành vi không đổi được khấu hao của KMP. 

Trường hợp thứ ba là mẫu ký tự đơn. Thuật toán suy biến rõ ràng thành việc đếm số lần xuất hiện mà không có bất kỳ cấu trúc tiền tố nào, vì`j`luôn là 0 hoặc 1 và`pi`là tầm thường.
