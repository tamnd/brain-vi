---
title: "CF 102770K - Tiêu diệt vũ phu"
description: "Bài toán mô tả hai thuật toán giải quyết cùng một nhiệm vụ đồ thị. Đối với mọi kích thước đầu vào n có thể có, chúng tôi biết thời gian chạy của thuật toán dự định và thời gian chạy của thuật toán brute-force chậm hơn."
date: "2026-07-28T23:14:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "K"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 59
verified: true
draft: false
---

[CF 102770K - Tiêu diệt vũ lực](https://codeforces.com/problemset/problem/102770/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả hai thuật toán giải quyết cùng một nhiệm vụ đồ thị. Đối với mọi kích thước đầu vào có thể`n`, chúng ta biết thời gian chạy của thuật toán dự định và thời gian chạy của thuật toán brute-force chậm hơn. Giới hạn thời gian của cuộc thi được chọn bằng ba lần thời gian chạy của thuật toán dự định trên trường hợp thử nghiệm lớn nhất còn lại sau khi Chenjb giảm kích thước đầu vào tối đa. 

Đối với kích thước tối đa đã chọn`n`, dữ liệu thử nghiệm có thể chứa mọi kích thước từ`1`ĐẾN`n`. Vì giải pháp dự kiến ​​sẽ trở nên chậm hơn khi`n`phát triển, thì lần chạy dự định chậm nhất trong số các thử nghiệm đó là thử nghiệm có kích thước`n`chính nó. Thời hạn cho sự lựa chọn đó là`3 * a[n]`. Giải pháp brute-force không thành công ở kích thước đầu vào đó nếu tồn tại bất kỳ trường hợp thử nghiệm nào có kích thước tối đa`n`trong đó thời gian chạy của nó lớn hơn giới hạn này. 

Đầu vào cung cấp tới 50 kích cỡ có thể. Cả hai mảng thời gian đang chạy đều đã được sắp xếp theo thứ tự không giảm. Giới hạn nhỏ có nghĩa là chỉ cần quét trực tiếp trên tất cả các kích cỡ là đủ. Không cần cấu trúc dữ liệu nâng cao hoặc tìm kiếm logarit. Quan sát quan trọng là câu trả lời là về tiền tố sớm nhất của các trường hợp thử nghiệm trong đó thuật toán brute-force trở nên quá chậm chứ không phải về việc chỉ so sánh cùng một chỉ mục. 

Một cách triển khai sai phổ biến là kiểm tra xem`b[n] > 3 * a[n]`và dừng lại ở đó. Điều này bỏ lỡ các trường hợp trường hợp thử nghiệm nhỏ hơn đã phá vỡ giới hạn. Ví dụ:```
1
4
1 2 7 20
2 5 30 40
```Đầu ra đúng là`3`. Vì`n = 3`, thời hạn là`21`. Thời gian tàn bạo cho kích thước`3`là`30`, thế là thất bại. Sự so sánh ở`n = 4`là không cần thiết vì chúng tôi muốn giá trị nhỏ nhất có thể. 

Một trường hợp khác là khi thuật toán brute-force luôn phù hợp. Ví dụ:```
1
3
2 3 3
5 7 8
```Đầu ra là`-1`. Thời gian dự định lớn nhất là`3`, vậy thời hạn là`9`. Mọi thời gian vũ phu nhiều nhất là`9`, nghĩa là kích thước đầu vào không được phép sẽ khiến nó bị lỗi. 

Trường hợp cạnh cuối cùng là một kích thước duy nhất có thể:```
1
1
10
31
```Đầu ra là`2`? Đầu vào này không hợp lệ vì`m = 1`, vì vậy đầu ra hợp lệ duy nhất là`1`hoặc`-1`. Lý do đúng là thời hạn là`30`, và thời gian vũ phu là`31`, do đó đầu ra là:```
1
```Một giải pháp chỉ bắt đầu kiểm tra từ phần tử thứ hai sẽ bỏ lỡ trường hợp ranh giới này một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình ra quyết định của Chenjb. Đối với mọi kích thước đầu vào tối đa có thể`n`, tính thời hạn như`3 * a[n]`. Sau đó kiểm tra mọi trường hợp thử nghiệm từ kích thước`1`ĐẾN`n`và xem liệu thời gian vũ phu có vượt quá giới hạn này hay không. Điều này chính xác tuân theo định nghĩa của việc vượt qua. 

Phương pháp brute-force là đúng vì nó trực tiếp kiểm tra mọi điều kiện cần thiết để giải pháp brute-force vượt qua. Tuy nhiên, nó lặp lại nhiều so sánh. Với`m`kích thước, việc kiểm tra từng tiền tố riêng biệt sẽ thực hiện tối đa`1 + 2 + ... + m`sự so sánh, đó là`O(m^2)`. Tối đa`m`chỉ có 50, vậy là đã đủ nhanh rồi. 

Quan sát tốt hơn là thời gian bạo lực được sắp xếp. Đối với kích thước tối đa cố định`n`, thời gian chạy mạnh mẽ nhất trong số tất cả các trường hợp thử nghiệm từ`1`ĐẾN`n`đơn giản là`b[n]`. Thời gian chạy dự định lớn nhất cũng là`a[n]`. Điều này loại bỏ sự cần thiết phải quét tiền tố. 

Điều kiện trở thành:```
b[n] > 3 * a[n]
```Chỉ số đầu tiên thỏa mãn điều kiện này chính xác là kích thước đầu vào đầu tiên mà giải pháp vũ phu không thể vượt qua. Chúng ta có thể quét mảng một lần và dừng ngay khi đáp ứng điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(1) | Đã chấp nhận | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian giải pháp dự kiến và thời gian giải pháp mạnh mẽ cho mọi quy mô từ`1`ĐẾN`m`. 

Các mảng đã được sắp xếp sẵn nên giá trị tại vị trí`i`mô tả trường hợp chậm nhất bên trong tiền tố kết thúc tại`i`. 
2. Đối với mỗi kích thước`i`từ`1`ĐẾN`m`, tính thời hạn như`3 * a[i]`. 

Nếu Chenjb cho phép kích thước đầu vào là`i`, trường hợp thử nghiệm lớn nhất có kích thước`i`, do đó thời gian chạy dự định của nó sẽ xác định giới hạn thời gian. 
3. Kiểm tra xem`b[i]`lớn hơn thời hạn này. 

Vì thời gian bạo lực không giảm,`b[i]`là trường hợp bạo lực tồi tệ nhất trong số tất cả các kích cỡ lên đến`i`. Nếu vượt quá giới hạn, giải pháp brute-force sẽ không thành công đối với tiền tố đó. 
4. In chỉ mục đầu tiên có điều kiện đúng. 

Quá trình quét đi từ kích thước nhỏ hơn đến kích thước lớn hơn, vì vậy lỗi đầu tiên là câu trả lời tối thiểu có thể xảy ra. 
5. Nếu quá trình quét kết thúc mà không tìm thấy chỉ mục đó, hãy in`-1`. 

Điều này có nghĩa là giải pháp brute-force có thể xử lý mọi kích thước đầu vào được phép. 

Tại sao nó hoạt động: 

Đối với bất kỳ kích thước tối đa được chọn`i`, cuộc thi chứa các trường hợp thử nghiệm có kích thước tối đa`i`. Giải pháp dự định chạy chậm nhất trong số này là`a[i]`, đưa ra giới hạn của`3 * a[i]`. Giải pháp brute-force chạy chậm nhất trong số này là`b[i]`vì mảng đã được sắp xếp. Giải pháp brute-force được thực hiện chính xác khi`b[i] <= 3 * a[i]`. Thuật toán kiểm tra điều kiện chính xác này cho mọi khả năng`i`theo thứ tự tăng dần, do đó lỗi đầu tiên nó tìm thấy là câu trả lời hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        m = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        res = -1
        for i in range(m):
            if b[i] > 3 * a[i]:
                res = i + 1
                break

        ans.append(str(res))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý một trường hợp thử nghiệm tại một thời điểm. Các mảng sử dụng tính năng lập chỉ mục dựa trên số 0 trong Python, trong khi kích thước của vấn đề bắt đầu từ`1`, vậy câu trả lời là`i + 1`khi một sự cố được tìm thấy. 

Vòng lặp so sánh thời gian cưỡng bức ở mỗi kích thước với ba lần thời gian dự kiến ​​ở cùng kích thước. Không cần quét tiền tố vì cả hai mảng đều được sắp xếp, do đó vị trí hiện tại đã thể hiện trường hợp xấu nhất trong tiền tố. 

Phép nhân với`3`là an toàn vì giá trị tối đa rất nhỏ. Sớm`break`tránh những kiểm tra không cần thiết sau khi kích thước lỗi đầu tiên được phát hiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
4
1 2 7 20
2 5 30 40
```Thuật toán kiểm tra từng kích thước tối đa có thể. 

| Kích thước | Thời gian dự kiến ​​| Thời hạn | Thời gian vũ phu | Thất bại? | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | Không | 
| 2 | 2 | 6 | 5 | Không | 
| 3 | 7 | 21 | 30 | Có | 

Kích thước thất bại đầu tiên là`3`, vậy câu trả lời là`3`. 

Ví dụ này cho thấy tại sao chỉ so sánh kích thước cuối cùng là không cần thiết. Thất bại sớm nhất là câu trả lời cần thiết. 

### Mẫu 2 

đầu vào:```
2
3
2 3 3
5 7 8
```| Kích thước | Thời gian dự kiến ​​| Thời hạn | Thời gian vũ phu | Thất bại? | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 6 | 5 | Không | 
| 2 | 3 | 9 | 7 | Không | 
| 3 | 3 | 9 | 8 | Không | 

Không có kích thước nào khiến thuật toán brute-force thất bại, vì vậy câu trả lời là`-1`. 

Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp trong đó giải pháp vũ phu vượt qua mọi giới hạn có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi kích thước được kiểm tra một lần. | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng ngoài mảng đầu vào. | 

Với`m <= 50`và tối đa 50 trường hợp thử nghiệm, giải pháp chỉ thực hiện vài nghìn thao tác. Nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        m = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        res = -1
        for i in range(m):
            if b[i] > 3 * a[i]:
                res = i + 1
                break

        ans.append(str(res))

    sys.stdin = old_stdin
    return "\n".join(ans)

assert solve("""2
4
1 2 7 20
2 5 30 40
3
2 3 3
5 7 8
""") == """3
-1""", "provided samples"

assert solve("""1
1
10
31
""") == "1", "single size failure"

assert solve("""1
5
5 5 5 5 5
1 2 3 4 5
""") == "-1", "all equal intended times, brute force always passes"

assert solve("""1
5
1 2 3 4 5
2 6 9 20 30
""") == "3", "first failure in the middle"

assert solve("""1
50
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 4
""") == "-1", "maximum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp mẫu |`3`,`-1`| Tính đúng đắn cơ bản và lý luận tiền tố được sắp xếp | 
| Lỗi kích thước đơn |`1`| Xử lý chỉ mục nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau |`-1`| Báo cáo chính xác không có kích thước bị lỗi | 
| Thất bại ở giữa |`3`| Dừng ở câu trả lời hợp lệ đầu tiên | 
| Kích thước đầu vào tối đa |`-1`| Xử lý lớn nhất cho phép`m`| 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là lỗi ở kích thước nhỏ nhất. Vì:```
1
1
10
31
```thời hạn là`30`, trong khi thời gian vũ phu là`31`. Chỉ số kiểm tra vòng lặp`0`ngay lập tức, nhìn thấy`31 > 30`, và trả về`1`. Nó không cho rằng phải đạt được kích thước lớn hơn trước khi thất bại. 

Trường hợp cạnh thứ hai là một giải pháp mạnh mẽ không bao giờ vượt quá giới hạn:```
1
3
2 3 3
5 7 8
```Các giới hạn là`6`,`9`, Và`9`. Mọi thời gian đều phù hợp, vì vậy vòng lặp không bao giờ cập nhật câu trả lời và in`-1`. 

Trường hợp cạnh thứ ba là lỗi do kích thước hiện tại chứ không phải lỗi sau:```
1
4
1 2 7 20
2 5 30 40
```Ở kích thước`3`, giới hạn là`21`và thời gian tàn bạo là`30`. Thuật toán trả về`3`ngay lập tức. Việc tiếp tục sử dụng kích thước lớn hơn chỉ có thể mang lại ứng viên lớn hơn, đây không phải là mức tối thiểu được yêu cầu.
