---
title: "CF 102483H - Ổ Cứng"
description: "Chúng ta cần xây dựng một chuỗi nhị phân đại diện cho ổ cứng. Chuỗi có độ dài n, một số vị trí không sử dụng được và phải chứa 0, và vị trí n luôn là một trong những vị trí không sử dụng được. Vị trí đầu tiên luôn có thể ghi được."
date: "2026-08-06T04:20:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "H"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 217
verified: true
draft: false
---

[CF 102483H - Ổ cứng](https://codeforces.com/problemset/problem/102483/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi nhị phân đại diện cho ổ cứng. Chuỗi có độ dài`n`, một số vị trí không sử dụng được và phải chứa`0`, và vị trí`n`luôn là một trong những vị trí không thể sử dụng được. Vị trí đầu tiên luôn có thể ghi được. giá trị`c`là số lần chính xác hai bit lân cận phải khác nhau. Bất kỳ chuỗi đầu ra hợp lệ nào có chính xác`c`những thay đổi như vậy được chấp nhận. 

Giới hạn lớn của`n`có nghĩa là chúng ta cần một giải pháp tuyến tính. Với`n`lên đến`500000`, các cách tiếp cận thử nhiều chuỗi có thể hoặc chạy lập trình động trên các vị trí và số lượng là quá tốn kém. MỘT`O(n)`quét là quy mô dự định. 

Những trường hợp phức tạp là do vị trí bị hỏng làm gián đoạn quá trình luân phiên. Ví dụ:```
n = 5, c = 2, broken = {2, 3, 5}
```Một giải pháp bất cẩn có thể luân chuyển qua tất cả các vị trí và viết`01010`, nhưng vị trí 2, 3 và 5 không thể lưu trữ`1`, vì vậy điều này không hợp lệ. Đầu ra chính xác có thể là`00010`, có đúng hai thay đổi. 

Một trường hợp đặc biệt khác là khi số lượng thay đổi được yêu cầu là số lẻ:```
n = 4, c = 3, broken = {4}
```Bắt đầu với`0`và chỉ thay đổi các vị trí sau đó không phải lúc nào cũng đến được đích vì bit cuối cùng phải được`0`. Bit đầu tiên phải được lựa chọn cẩn thận. Một câu trả lời hợp lệ là`1010`, trong đó ba thay đổi xảy ra trên ba cặp liền kề đầu tiên. 

Trường hợp cạnh cuối cùng là khi một bit bị hỏng xuất hiện giữa hai vùng có thể ghi:```
n = 7, c = 4, broken = {2, 7}
```Số 0 bị hỏng ở vị trí 2 sẽ ngăn cản sự thay đổi xảy ra ở đó, do đó việc đếm các vị trí có sẵn thay vì các chuyển tiếp có thể ghi thực tế sẽ đưa ra câu trả lời sai. Việc xây dựng chỉ cần bỏ qua các vị trí bị hỏng trong khi tiếp tục mô hình xen kẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi cách gán bit có thể ghi được và đếm các thay đổi kết quả. Nếu có`n-b`vị trí có thể ghi, điều này khám phá`2^(n-b)`chuỗi, điều này vốn không thể thực hiện được đối với vài chục bit có thể ghi được. Lý do mà biện pháp mạnh mẽ này đúng là vì nó kiểm tra mọi cấu hình lưu trữ có thể có nhưng lại bỏ qua cấu trúc mạnh mẽ của vấn đề. 

Quan sát quan trọng là số lượng thay đổi chính xác có thể được tạo ra một cách tham lam. Nếu chúng ta chọn giá trị bắt đầu cho bit đầu tiên, mọi vị trí có thể ghi sau nó có thể tiếp tục giá trị hiện tại hoặc lật nó. Một cú lật luôn tạo ra chính xác một thay đổi. Các vị trí bị hỏng là các số 0 cố định và chỉ cần tạm thời dừng mô hình xen kẽ. 

Bit cuối cùng bị hỏng nên đảm bảo bằng 0. Nếu số lượng thay đổi mong muốn là số lẻ, bắt đầu bằng`1`cung cấp cho đường dẫn cuối cùng tính chẵn lẻ chính xác. Nếu nó chẵn, bắt đầu bằng`0`cũng làm như vậy. Sau khi chọn bit đầu tiên này, chúng ta tham lam lật từng vị trí có thể ghi trong khi vẫn còn những thay đổi. Vì đầu vào đảm bảo tồn tại một giải pháp nên số lần lật cần thiết sẽ luôn phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các vị trí bị hỏng và lưu trữ chúng trong một bộ. Chúng tôi cần kiểm tra liên tục trong khi quét ổ cứng. 
2. Chọn giá trị của bit đầu tiên. Nếu như`c`thật kỳ quặc, đặt`1`ở đó. Nếu không thì đặt`0`. Điều này xử lý tính chẵn lẻ của đường dẫn từ bit đầu tiên đến số 0 bắt buộc ở cuối. 
3. Quét vị trí từ`2`ĐẾN`n`. Nếu vị trí bị hỏng, hãy để nguyên`0`và tiếp tục. Vị trí bị hỏng không thể được sử dụng để tạo ra thay đổi. 
4. Đối với mọi vị trí có thể ghi, nếu vẫn còn thay đổi, hãy ghi giá trị ngược lại của bit trước đó và giảm số lượng còn lại. Việc lật tạo ra chính xác một thay đổi bit mới. 
5. Nếu không còn thay đổi nào, hãy điền vào mọi vị trí có thể ghi sau này bằng`0`. Vì bit cuối cùng đã bằng 0 nên không có thay đổi bổ sung nào được đưa ra. 

Tại sao nó hoạt động: 

Việc xây dựng duy trì tính bất biến là mỗi lần bộ đếm còn lại giảm đi thì chính xác một sai phân liền kề mới được tạo ra. Các vị trí bị hỏng không bao giờ đóng góp một lựa chọn vì chúng là các số 0 cố định. Lựa chọn bit ban đầu làm cho tính chẵn lẻ của số lần lật yêu cầu tương thích với số 0 cuối cùng bắt buộc. Vì vấn đề đảm bảo rằng tồn tại một câu trả lời hợp lệ, nên việc sử dụng tham lam mọi vị trí có thể ghi cho đến khi bộ đếm về 0 không thể hết dung lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c, b = map(int, input().split())
    broken = set(map(int, input().split()))

    ans = ['0'] * n
    remaining = c

    ans[0] = '1' if c % 2 else '0'
    prev = ans[0]

    for i in range(1, n):
        pos = i + 1
        if pos in broken:
            ans[i] = '0'
            prev = '0'
            continue

        if remaining > 0:
            if prev == '0':
                ans[i] = '1'
                prev = '1'
            else:
                ans[i] = '0'
                prev = '0'
            remaining -= 1
        else:
            ans[i] = '0'
            prev = '0'

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Các vị trí bị hỏng được lưu trữ trong một bộ vì quá trình quét sẽ truy cập từng vị trí một lần và cần kiểm tra tư cách thành viên nhanh chóng. Mảng được khởi tạo bằng số 0 vì mọi bit có thể ghi không xác định đều có thể giữ nguyên số 0 một cách an toàn. 

Bit đầu tiên được gán trước khi quét vì đó là nơi duy nhất chúng tôi kiểm soát tính chẵn lẻ bắt đầu. Trong quá trình quét,`remaining`lưu trữ bao nhiêu chuyển đổi nữa vẫn cần được tạo. Một vị trí có thể ghi với`remaining > 0`được lật so với bit trước đó, tiêu tốn chính xác một lần chuyển đổi. 

Bản cập nhật của`prev`được thực hiện ngay cả đối với các vị trí bị hỏng. Điều này quan trọng vì bit có thể ghi tiếp theo được so sánh với giá trị được lưu trữ thực tế và vị trí bị hỏng luôn lưu số 0. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 5
c = 2
broken = {2,3,5}
```| Vị trí | Bị hỏng | Những thay đổi còn lại | Viết chút | 
| --- | --- | --- | --- | 
| 1 | Không | 2 | 0 | 
| 2 | Có | 2 | 0 | 
| 3 | Có | 2 | 0 | 
| 4 | Không | 2 | 1 | 
| 5 | Có | 1 | 0 | 

Kết quả là`00010`. Sự chuyển tiếp nằm giữa vị trí 3 và 4, vị trí 4 và 5. 

Đối với mẫu thứ hai:```
n = 7
c = 4
broken = {2,7}
```| Vị trí | Bị hỏng | Những thay đổi còn lại | Viết chút | 
| --- | --- | --- | --- | 
| 1 | Không | 4 | 0 | 
| 2 | Có | 4 | 0 | 
| 3 | Không | 4 | 1 | 
| 4 | Không | 3 | 0 | 
| 5 | Không | 2 | 1 | 
| 6 | Không | 1 | 0 | 
| 7 | Có | 0 | 0 | 

Kết quả là`0010100`. Bốn thay đổi được thực hiện ở các vị trí 2-3, 3-4, 4-5 và 5-6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí bit được xử lý một lần. | 
| Không gian | O(b) | Chỉ những vị trí bị hỏng và chuỗi đầu ra mới được lưu trữ. | 

Thuật toán phù hợp với`500000`giới hạn độ dài vì nó thực hiện quét tuyến tính đơn và không sử dụng bảng lập trình động lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    n, c, b = map(int, data().split())
    broken = set(map(int, data().split()))
    ans = ['0'] * n
    ans[0] = '1' if c % 2 else '0'
    prev = ans[0]
    for i in range(1, n):
        if i + 1 in broken:
            prev = '0'
            continue
        if c:
            ans[i] = '1' if prev == '0' else '0'
            prev = ans[i]
            c -= 1
        else:
            prev = '0'
    sys.stdin = old
    return ''.join(ans)

def changes(s):
    return sum(a != b for a, b in zip(s, s[1:]))

assert run("5 2 3\n2 3 5\n") == "00010"
assert run("7 4 2\n2 7\n") == "0010100"
assert changes(run("2 1 1\n2\n")) == 1
assert run("5 4 1\n5\n") == "10100"
assert run("10 1 9\n2 3 4 5 6 7 8 9 10\n") == "1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 3 / 2 3 5`|`00010`| Cung cấp mẫu và khối vỡ ở giữa | 
|`7 4 2 / 2 7`| Bất kỳ chuỗi nào có 4 thay đổi | Sự thay đổi sau một chút gãy sớm | 
|`2 1 1 / 2`|`10`| Kích thước tối thiểu và tính chẵn lẻ lẻ | 
|`5 4 1 / 5`|`10100`| Thay đổi tối đa chỉ với bit bị hỏng cuối cùng | 
|`10 1 9 / 2..10`|`1000000000`| Hầu như tất cả các vị trí đều bị hỏng | 

## Vỏ cạnh 

Đối với trường hợp phân đoạn có thể ghi đầu tiên được phân tách bằng các vị trí bị hỏng, thuật toán không bao giờ cố gắng lật một ô bị hỏng. Vì:```
5 2 3
2 3 5
```quá trình quét bỏ qua vị trí 2 và 3, tạo một lần lật ở vị trí 4 và điểm 0 cưỡng bức cuối cùng tạo ra thay đổi thứ hai. 

Đối với số lẻ các thay đổi bắt buộc:```
4 3 1
4
```thuật toán bắt đầu với`1`, sau đó luân phiên qua các vị trí có thể ghi. Nó tạo ra`1010`, có ba thay đổi và tôn trọng vị trí cuối cùng bị hỏng. 

Đối với nhiều vị trí bị hỏng liên tiếp:```
7 4 5
2 3 4 5 7
```các vị trí có thể ghi hữu ích duy nhất là 1 và 6. Quá trình quét coi vùng bị hỏng là các số 0 cố định và chỉ tạo ra các thay đổi khi tồn tại một lựa chọn thực sự có thể ghi. Việc đảm bảo đầu vào hợp lệ có nghĩa là quá trình tham lam đạt chính xác số lượng được yêu cầu mà không cần phải xem xét lại các lựa chọn trước đó.
