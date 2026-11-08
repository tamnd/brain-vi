---
title: "CF 1392D - Cuộc chiến giữa Omkar và giường ngủ"
description: "Mỗi người chơi ngồi trên một vòng tròn và tấn công chính xác một trong hai người hàng xóm của họ. Chúng ta được cấp một chuỗi hình tròn gồm L và R. Nếu người chơi i tấn công bên trái, chúng ta viết L. Nếu họ tấn công bên phải, chúng ta viết R."
date: "2026-06-11T10:10:54+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1392
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 10"
rating: 1700
weight: 1392
solve_time_s: 337
verified: false
draft: false
---

[CF 1392D - Cuộc chiến giữa Omkar và giường ngủ](https://codeforces.com/problemset/problem/1392/D) 

**Đánh giá:** 1700 
**Tags:** dp, tham lam 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi ngồi trên một vòng tròn và tấn công chính xác một trong hai người hàng xóm của họ. Chúng ta được cho một chuỗi hình tròn gồm`L`Và`R`. 

Nếu người chơi`i`tấn công trái, chúng tôi viết`L`. Nếu họ tấn công đúng, chúng tôi viết`R`. 

Một người chơi được coi là hợp lý theo quy tắc của trò chơi: 

- Nếu đúng một người hàng xóm tấn công họ, họ phải tấn công lại người hàng xóm đó. 
- Nếu không có hoặc có hai người hàng xóm tấn công họ thì hướng tấn công nào cũng được chấp nhận. 

Chúng tôi có thể lật lại lựa chọn của bất kỳ người chơi nào từ`L`ĐẾN`R`hoặc từ`R`ĐẾN`L`. Mỗi người chơi bị lật tốn một thao tác. Chúng ta phải tìm số lần lật tối thiểu cần thiết để mọi người chơi trở nên logic. 

Thử thách đầu tiên là hiểu cấu hình logic cuối cùng trông như thế nào. 

Giả sử chúng ta nhìn vào một người chơi có hướng tấn công khác với một trong những người hàng xóm. Ví dụ, một ranh giới`RL`xuất hiện. Người chơi bên trái tấn công bên phải và người chơi bên phải tấn công bên trái nên họ tấn công lẫn nhau. Những quan điểm như vậy đã hoàn toàn nhất quán. 

Phần thú vị là những gì xảy ra bên trong một khối tối đa các ký tự bằng nhau:```
RRRRR
```hoặc```
LLLL
```Bên trong một khối như vậy, những người chơi ở giữa có vấn đề. Chúng nhận đúng một đòn tấn công sắp tới nhưng không tấn công đáp trả, vì vậy cần phải có một số thay đổi. 

Những hạn chế là lớn. Tổng chiều dài của tất cả các chuỗi tối đa là`2·10^5`, và có thể có tới`10^4`trường hợp thử nghiệm. Bất kỳ giải pháp nào kiểm tra tất cả các kết hợp lật có thể xảy ra đều không thể thực hiện được. Thậm chí một`O(n²)`thuật toán sẽ thực hiện xung quanh`4·10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một giải pháp tuyến tính. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
n = 5
RRRRR
```Vì dây có hình tròn nên đây là một dây có độ dài 5 chứ không phải năm vị trí riêng biệt. Thuật toán xử lý chạy tuyến tính quên vòng tròn sẽ tính ra câu trả lời sai. 

Một ví dụ khác là:```
RLRLRL
```Mọi người chơi đều đã tấn công lại người chơi duy nhất đang tấn công họ. Câu trả lời là`0`. 

Trường hợp tế nhị thứ ba là:```
RRRLLL
```Lần chạy đầu tiên và cuối cùng chỉ chạm qua ranh giới hình tròn nếu chúng có cùng ký tự. Ở đây thì không, vì vậy thực tế có hai lần chạy riêng biệt có độ dài 3. Việc coi chuỗi là một lần chạy hợp nhất sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi nhóm người chơi lật và kiểm tra xem cấu hình kết quả có hợp lý hay không. có`2^n`các kiểu lật có thể xảy ra, điều này trở nên hoàn toàn không khả thi ngay cả đối với`n = 40`, huống hồ là`n = 200000`. 

Để có được điều gì đó hữu ích, chúng ta cần hiểu cấu trúc của các cấu hình hợp lệ. 

Nhìn vào một chuỗi tối đa các ký tự giống hệt nhau:```
RRRRRR
```Hãy xem xét ba người chơi liên tiếp trong đường chạy. Người chơi ở giữa chỉ bị tấn công từ một phía. Vì họ tấn công cùng hướng với những người khác nên họ không tấn công lại. Những người chơi như vậy vi phạm quy tắc. 

Việc thay đổi hướng của một người chơi có thể sửa chữa tối đa ba vị trí liên tiếp trong một lần chạy. Điều này dẫn đến một mô hình quen thuộc: mỗi khối có ba hướng liên tiếp bằng nhau cần một sửa đổi. 

Đối với một đoạn tuyến tính có chiều dài`k`, số lần thay đổi tối thiểu là:```
k // 3
```Đây chính xác là đối số đếm tương tự được sử dụng trong nhiều bài toán "ngắt quãng". 

Sự phức tạp duy nhất là cấu trúc hình tròn. 

Nếu toàn bộ vòng tròn bao gồm một ký tự duy nhất, chẳng hạn như:```
RRRRRRR
```chỉ có một chiều dài duy nhất`n`, và câu trả lời là:```
(n + 2) // 3
```Ngược lại, tồn tại ít nhất một ranh giới trong đó các ký tự liền kề khác nhau. Một ranh giới như vậy sẽ phá vỡ vòng tròn một cách tự nhiên. Khi chúng ta cắt vòng tròn ở bất kỳ vị trí nào có các ký tự khác nhau, bài toán sẽ trở thành tuyến tính. Mỗi lần chạy tối đa sau đó có thể được xử lý độc lập, góp phần`length // 3`. 

Điều này mang lại một giải pháp thời gian tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem tất cả các ký tự trong chuỗi tròn có giống nhau không. 

Nếu đúng như vậy thì toàn bộ vòng tròn tạo thành một đoạn dài`n`. Câu trả lời là`(n + 2) // 3`. 
2. Ngược lại, hãy tìm bất kỳ vị trí nào có các ký tự liền kề khác nhau. 

Vị trí như vậy luôn tồn tại vì không phải tất cả các ký tự đều bình đẳng. 
3. Xoay vòng tròn một cách khái niệm để vị trí này trở thành điểm bắt đầu. 

Sau lần cắt này, các ký tự đầu tiên và cuối cùng của chuỗi kết quả sẽ khác nhau, điều đó có nghĩa là không có đường chạy nào bao quanh ranh giới nữa. 
4. Duyệt chuỗi đã xoay và tính độ dài của mỗi lần chạy tối đa các ký tự bằng nhau. 
5. Đối với mỗi lần chạy dài`len`, thêm vào`len // 3`để trả lời. 

Cứ ba hướng bằng nhau liên tiếp trong một lần chạy cần một lần lật và các yêu cầu này là độc lập giữa các lần chạy. 
6. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Khi một lần chạy có độ dài`len`, mọi người chơi ngoại trừ những người ở gần ranh giới đều hành xử giống hệt nhau. Các vị trí có vấn đề xảy ra theo nhóm ba hướng bằng nhau liên tiếp. Một lần lật được lựa chọn cẩn thận có thể sửa được nhiều nhất một nhóm như vậy, vì vậy ít nhất`len // 3`lật là cần thiết. 

Ngược lại, việc lật mỗi vị trí thứ ba trong đường chạy sẽ đạt được chính xác`len // 3`thay đổi và làm cho tất cả người chơi hợp lý. Vì các lần chạy khác nhau được phân tách bằng sự thay đổi hướng nên sự điều chỉnh của chúng không tương tác với nhau. Tổng hợp`len // 3`trên tất cả các lần chạy mang lại sự tối ưu toàn cục. 

Đối với trường hợp đặc biệt khi toàn bộ vòng tròn là một đường chạy thì không có ranh giới tự nhiên. Đường tròn có chiều dài`n`yêu cầu`ceil(n/3)`thay đổi, tương đương với`(n + 2) // 3`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        if s.count(s[0]) == n:
            print((n + 2) // 3)
            continue

        pos = 0
        while s[pos] == s[(pos + 1) % n]:
            pos += 1

        s = s[pos + 1:] + s[:pos + 1]

        ans = 0
        cnt = 1

        for i in range(1, n):
            if s[i] == s[i - 1]:
                cnt += 1
            else:
                ans += cnt // 3
                cnt = 1

        ans += cnt // 3
        print(ans)

solve()
```Nhánh đầu tiên xử lý tình huống đặc biệt trong đó toàn bộ vòng tròn là một lần chạy. Trong trường hợp đó, quá trình phân tách chạy tiêu chuẩn không hoạt động vì không có ranh giới khác nhau để cắt giảm. 

Đối với tất cả các trường hợp khác, chúng tôi định vị bất kỳ cặp liền kề nào có các ký tự khác nhau. Việc cắt ngay sau cặp như vậy đảm bảo rằng ký tự đầu tiên và ký tự cuối cùng của chuỗi được xoay là khác nhau. Điều này chuyển đổi cấu trúc chạy vòng tròn thành cấu trúc chạy tuyến tính thông thường. 

Quá trình truyền tải tính toán độ dài chạy tối đa với bộ đếm tiêu chuẩn. Bất cứ khi nào quá trình chạy kết thúc, chúng tôi sẽ thêm`run_length // 3`để trả lời. Sau vòng lặp, lần chạy cuối cùng cũng phải được xử lý. 

Một lỗi phổ biến là quên trường hợp hoàn toàn bằng nhau. Nếu không có cách xử lý đặc biệt, bước xoay không thể tìm được đường cắt hợp lệ và công thức sẽ trở nên không chính xác. 

Một lỗi dễ xảy ra khác là cắt ở vị trí tùy ý. Nếu vết cắt nằm bên trong một đường chạy, đường chạy vòng tròn có thể được chia thành hai đường chạy nhỏ hơn, gây ra việc đếm thiếu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
LRRRRL
```Dây không đồng đều. 

Chọn ranh giới nơi các ký tự khác nhau:```
L | RRRR | L
```Sau khi quay, quãng đường chạy là: 

| Chạy | Chiều dài | Đóng góp | 
| --- | --- | --- | 
| L | 1 | 0 | 
| RRR | 4 | 1 | 
| L | 1 | 0 | 

Tổng số câu trả lời: 

| Độ dài chạy hiện tại | Đã thêm | 
| --- | --- | 
| 1 | 0 | 
| 4 | 1 | 
| 1 | 0 | 

Trả lời =`1`. 

Cuộc chạy đáng kể duy nhất là khối bốn`R`S. Một đoạn dài bốn yêu cầu chính xác một sửa đổi. 

### Ví dụ 2 

đầu vào:```
5
RRRRR
```Tất cả các nhân vật đều giống hệt nhau. 

| n | Công thức | Trả lời | 
| --- | --- | --- | 
| 5 | (5 + 2) // 3 | 2 | 

Trả lời =`2`. 

Ví dụ này chứng minh tại sao trường hợp đặc biệt là cần thiết. Không có ranh giới khác nhau để cắt bỏ, vì vậy phương pháp xử lý chạy chung không thể được áp dụng trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một số lần không đổi | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, do đó việc quét tuyến tính từng chuỗi dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ chính chuỗi đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            s = input().strip()

            if s.count(s[0]) == n:
                out.append(str((n + 2) // 3))
                continue

            pos = 0
            while s[pos] == s[(pos + 1) % n]:
                pos += 1

            s = s[pos + 1:] + s[:pos + 1]

            ans = 0
            cnt = 1

            for i in range(1, n):
                if s[i] == s[i - 1]:
                    cnt += 1
                else:
                    ans += cnt // 3
                    cnt = 1

            ans += cnt // 3
            out.append(str(ans))

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""5
4
RLRL
6
LRRRRL
8
RLLRRRLL
12
LLLLRRLRRRLL
5
RRRRR
"""
) == """0
1
1
3
2"""

# minimum size
assert run(
"""1
3
RLR
"""
) == "0"

# all equal
assert run(
"""1
6
LLLLLL
"""
) == "2"

# single long run after cut
assert run(
"""1
8
RRRRLLRL
"""
) == "1"

# boundary wraparound case
assert run(
"""1
6
RRRLLL
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`RLR`|`0`| Kích thước hợp lệ nhỏ nhất, đã hợp lý | 
|`LLLLLL`|`2`| Chạy vòng tròn hoàn toàn bằng nhau | 
|`RRRRLLRL`|`1`| Một thời gian dài góp phần`4 // 3`| 
|`RRRLLL`|`2`| Nhiều lần chạy, không hợp nhất toàn diện | 

## Vỏ cạnh 

Hãy xem xét:```
n = 5
RRRRR
```Thuật toán phát hiện rằng mọi ký tự đều giống hệt nhau. Nó ngay lập tức trả về:```
(5 + 2) // 3 = 2
```Không có vòng quay nào được thử. Điều này tránh được sự thất bại xảy ra nếu chúng ta mãi mãi tìm kiếm một ranh giới khác. 

Bây giờ hãy xem xét:```
n = 6
RRRLLL
```Dây không đồng đều. Có một ranh giới khác nhau tồn tại giữa phần thứ ba`R`và đầu tiên`L`. 

Việc phân rã chạy trở thành:```
RRR
LLL
```Những đóng góp là:```
3 // 3 + 3 // 3 = 2
```đưa ra câu trả lời đúng`2`. 

Cuối cùng, hãy xem xét:```
n = 8
RLRLRLRL
```Mỗi lần chạy đều có độ dài`1`. 

Thuật toán tính toán:```
1 // 3 + 1 // 3 + ...
```tổng cộng là`0`. 

Điều này xác nhận rằng các cấu hình đã hợp lệ được nhận dạng mà không có bất kỳ sửa đổi không cần thiết nào.
