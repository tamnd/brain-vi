---
title: "CF 96A - Bóng đá"
description: "Chúng ta được cung cấp một chuỗi chỉ gồm '0' và '1'. Mỗi nhân vật đại diện cho đội của một cầu thủ bóng đá đứng thành hàng. Một tình huống nguy hiểm xảy ra nếu có ít nhất bảy người chơi liên tiếp thuộc cùng một đội."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 96
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 77 (Div. 2 Only)"
rating: 900
weight: 96
solve_time_s: 84
verified: true
draft: false
---

[CF 96A - Bóng đá](https://codeforces.com/problemset/problem/96/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một sợi dây chỉ được làm từ`'0'`Và`'1'`. Mỗi nhân vật đại diện cho đội của một cầu thủ bóng đá đứng thành hàng. Một tình huống nguy hiểm xảy ra nếu có ít nhất bảy người chơi liên tiếp thuộc cùng một đội. 

Nhiệm vụ đơn giản là kiểm tra xem chuỗi có chứa bảy chuỗi liên tiếp hay không.`'0'`ký tự hoặc bảy ký tự liên tiếp`'1'`nhân vật. Nếu một phân đoạn như vậy tồn tại, chúng tôi in`"YES"`. Ngược lại, chúng tôi in`"NO"`. 

Kích thước đầu vào rất nhỏ, tối đa 100 ký tự. Ngay cả một giải pháp không hiệu quả cũng dễ dàng nằm trong giới hạn. Một thuật toán bậc hai sẽ thực hiện khoảng 10.000 phép tính trong trường hợp xấu nhất, điều này không đáng kể đối với giới hạn 2 giây. Tuy nhiên, vấn đề đương nhiên thừa nhận quét tuyến tính, đơn giản và sạch sẽ hơn. 

Phần khó khăn không phải là hiệu suất, mà là xử lý chính xác các lần chạy liên tiếp. 

Một lỗi dễ mắc phải là đặt lại bộ đếm không chính xác khi ký tự thay đổi. 

Coi như:```
1111110
```Câu trả lời đúng là`"NO"`bởi vì chuỗi ký tự bằng nhau dài nhất có độ dài 6. Việc triển khai bất cẩn chỉ kiểm tra tổng số ký tự`'1'`ký tự sẽ in không chính xác`"YES"`. 

Một lỗi phổ biến khác là chỉ kiểm tra chính xác bảy ký tự thay vì ít nhất bảy ký tự. 

Ví dụ:```
11111111
```Câu trả lời đúng là`"YES"`bởi vì chiều dài 8 vẫn thỏa mãn điều kiện. Nếu mã chỉ kiểm tra các lần chạy bằng 7 thì mã sẽ thất bại ở đây. 

Trường hợp cạnh thứ ba xuất hiện khi chuỗi nguy hiểm bắt đầu ở đầu hoặc kết thúc ở cuối chuỗi. 

Ví dụ:```
0000001
```Câu trả lời đúng là`"NO"`bởi vì thời gian dài nhất chỉ là 6. 

Ví dụ:```
10000000
```Câu trả lời đúng là`"YES"`bởi vì bảy ký tự cuối cùng đều là`'0'`. 

Việc triển khai chỉ so sánh các vị trí ở giữa có thể vô tình bỏ lỡ các lần chạy ranh giới. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là kiểm tra mọi chuỗi con có độ dài 7 và kiểm tra xem tất cả các ký tự bên trong nó có giống nhau hay không. 

Đối với mỗi vị trí bắt đầu, chúng ta có thể kiểm tra bảy ký tự tiếp theo và xác minh xem tất cả chúng có`'0'`hoặc tất cả`'1'`. Vì độ dài chuỗi tối đa là 100 nên cách tiếp cận này hoàn toàn đủ nhanh. Trong trường hợp xấu nhất, chúng tôi kiểm tra khoảng 100 cửa sổ, mỗi cửa sổ có kích thước 7, do đó công việc so sánh khoảng 700 ký tự. 

Lực lượng vũ phu hoạt động vì tình trạng nguy hiểm chỉ phụ thuộc vào các phân đoạn liên tiếp cục bộ. Nếu tồn tại bất kỳ lần chạy hợp lệ nào thì ít nhất một cửa sổ có độ dài 7 phải chứa các ký tự giống hệt nhau. 

Một quan sát rõ ràng hơn sẽ đơn giản hóa việc thực hiện hơn nữa. Thay vì liên tục kiểm tra các cửa sổ, chúng ta có thể quét chuỗi một lần trong khi theo dõi độ dài vệt hiện tại. 

Nếu ký tự hiện tại khớp với ký tự trước đó, chúng tôi sẽ kéo dài chuỗi. Nếu không, chúng tôi đặt lại chuỗi thành 1 vì một lần chạy mới đã bắt đầu. Thời điểm chuỗi đạt đến con số 7, chúng ta đã biết câu trả lời là`"YES"`. 

Điều này biến bài toán thành một phép truyền tuyến tính đơn giản với bộ nhớ không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × 7) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Khởi tạo bộ đếm`count = 1`bởi vì ký tự đầu tiên đã tạo thành một chuỗi dài 1. 
3. Duyệt chuỗi bắt đầu từ chỉ số 1. 
4. Đối với mỗi vị trí, so sánh ký tự hiện tại với ký tự trước đó. 
5. Nếu bằng nhau thì tăng`count`bởi vì việc chạy liên tiếp vẫn tiếp tục. 
6. Nếu không, hãy đặt lại`count = 1`bởi vì một chuỗi mới bắt đầu từ ký tự hiện tại. 
7. Sau khi cập nhật bộ đếm, hãy kiểm tra xem`count >= 7`. 
8. Nếu có, hãy in`"YES"`và dừng lại ngay vì phát hiện tình huống nguy hiểm. 
9. Nếu vòng lặp kết thúc mà không tìm thấy vệt nào như vậy, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`count`luôn bằng độ dài của lần chạy liên tiếp hiện tại kết thúc ở vị trí hiện tại. 

Khi hai ký tự liền kề khớp nhau, lượt chạy sẽ kéo dài một cách tự nhiên. Khi chúng khác nhau, lần chạy trước kết thúc và lần chạy mới có độ dài 1 bắt đầu. 

Vì mỗi đoạn liên tiếp trong chuỗi được xử lý chính xác một lần nên bất kỳ chuỗi nào có độ dài ít nhất 7 cuối cùng sẽ gây ra`count`đạt tới 7. Nếu thuật toán không bao giờ đạt đến giá trị đó thì không có phân đoạn nguy hiểm nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

count = 1

for i in range(1, len(s)):
    if s[i] == s[i - 1]:
        count += 1
    else:
        count = 1

    if count >= 7:
        print("YES")
        break
else:
    print("NO")
```Chương trình bắt đầu bằng cách đọc chuỗi và xóa dòng mới ở cuối bằng`strip()`. 

Biến`count`lưu trữ độ dài của chuỗi liên tiếp hiện tại. Nó bắt đầu từ 1 vì một ký tự đơn đã tạo thành một chuỗi có độ dài hợp lệ. 

Vòng lặp bắt đầu từ chỉ mục 1 vì mọi so sánh đều sử dụng`s[i - 1]`. Bắt đầu từ chỉ số 0 sẽ gây ra truy cập không hợp lệ. 

Khi các ký tự liền kề bằng nhau, vệt sẽ tăng thêm một. Nếu không, chuỗi sẽ đặt lại vì tính liên tục đã bị phá vỡ. 

các`if count >= 7`kiểm tra được đặt bên trong vòng lặp ngay sau khi cập nhật độ dài vệt. Việc đánh bắt này sẽ diễn ra ngay khi chúng trở nên nguy hiểm. 

các`for ... else`cấu trúc rất hữu ích ở đây. các`else`khối chỉ thực thi nếu vòng lặp không bao giờ gặp một`break`. Điều đó có nghĩa là chúng tôi in`"NO"`chỉ khi không tìm thấy đoạn nguy hiểm nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
001001
```| Chỉ mục | Nhân vật | Trước | Đếm | 
| --- | --- | --- | --- | 
| 0 | 0 | - | 1 | 
| 1 | 0 | 0 | 2 | 
| 2 | 1 | 0 | 1 | 
| 3 | 0 | 1 | 1 | 
| 4 | 0 | 0 | 2 | 
| 5 | 1 | 0 | 1 | 

Độ dài vệt không bao giờ đạt tới 7, vì vậy câu trả lời là`"NO"`. Ví dụ này xác nhận rằng thuật toán đặt lại chính xác bất cứ khi nào ký tự thay đổi. 

### Ví dụ 2 

đầu vào:```
00100110111111101
```| Chỉ mục | Nhân vật | Trước | Đếm | 
| --- | --- | --- | --- | 
| 0 | 0 | - | 1 | 
| 1 | 0 | 0 | 2 | 
| 2 | 1 | 0 | 1 | 
| 3 | 0 | 1 | 1 | 
| 4 | 0 | 0 | 2 | 
| 5 | 1 | 0 | 1 | 
| 6 | 1 | 1 | 2 | 
| 7 | 0 | 1 | 1 | 
| 8 | 1 | 0 | 1 | 
| 9 | 1 | 1 | 2 | 
| 10 | 1 | 1 | 3 | 
| 11 | 1 | 1 | 4 | 
| 12 | 1 | 1 | 5 | 
| 13 | 1 | 1 | 6 | 
| 14 | 1 | 1 | 7 | 

Ở chỉ số 14, chuỗi 7 trận liên tiếp`'1'`nhân vật. Thuật toán in ngay lập tức`"YES"`và kết thúc sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với độ dài chuỗi tối đa chỉ 100, giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    count = 1

    for i in range(1, len(s)):
        if s[i] == s[i - 1]:
            count += 1
        else:
            count = 1

        if count >= 7:
            print("YES")
            break
    else:
        print("NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("001001\n") == "NO", "sample 1"

# custom cases
assert run("1111111\n") == "YES", "exactly seven consecutive ones"
assert run("0000001\n") == "NO", "six consecutive zeros only"
assert run("10000000\n") == "YES", "seven zeros at the end"
assert run("1010101010\n") == "NO", "alternating characters"
assert run("11111111\n") == "YES", "more than seven consecutive ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1111111`|`YES`| Ranh giới chính xác của 7 | 
|`0000001`|`NO`| Sáu ký tự liên tiếp là không đủ | 
|`10000000`|`YES`| Chuỗi nguy hiểm ở cuối | 
|`1010101010`|`NO`| Thường xuyên đặt lại bộ đếm | 
|`11111111`|`YES`| Chạy dài hơn 7 cũng phải có tác dụng | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1111110
```Thuật toán xử lý sáu liên tiếp`'1'`nhân vật, vì vậy`count`đạt 6. Ở ký tự cuối cùng`'0'`, chuỗi đặt lại về 1. Vì`count`không bao giờ trở thành 7, đầu ra là`"NO"`. 

Bây giờ hãy xem xét:```
11111111
```Vệt này phát triển như sau:```
1 → 2 → 3 → 4 → 5 → 6 → 7
```Ngay sau lần thứ bảy liên tiếp`'1'`đạt được, thuật toán in`"YES"`. Ký tự thứ tám không bao giờ gây ra vấn đề vì điều kiện kiểm tra`>= 7`, không hẳn là 7. 

Cuối cùng, hãy xem xét:```
10000000
```Ký tự đầu tiên tạo ra một chuỗi có độ dài 1. Bắt đầu từ ký tự thứ hai, thuật toán tiếp tục kéo dài chuỗi`'0'`ký tự cho đến khi bộ đếm đạt 7 ở vị trí cuối cùng. Đầu ra trở thành`"YES"`. 

Điều này xác nhận rằng các chuỗi ở ranh giới của chuỗi được xử lý chính xác.
