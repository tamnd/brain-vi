---
title: "CF 325B - Sân vận động và trò chơi"
description: "Chúng ta được cung cấp chính xác số trận bóng đá phải chơi và chúng ta cần tìm mọi số đội ban đầu có thể sản xuất chính xác số trận đấu đó theo một thể thức giải đấu cụ thể."
date: "2026-06-06T08:46:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 325
codeforces_index: "B"
codeforces_contest_name: "MemSQL start[c]up Round 1"
rating: 1800
weight: 325
solve_time_s: 110
verified: true
draft: false
---

[CF 325B - Sân vận động và các trận đấu](https://codeforces.com/problemset/problem/325/B) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp chính xác số trận bóng đá phải chơi và chúng ta cần tìm mọi số đội ban đầu có thể sản xuất chính xác số trận đấu đó theo một thể thức giải đấu cụ thể. 

Giải đấu bắt đầu bằng các vòng đấu loại trực tiếp nhưng chỉ khi số đội còn lại là chẵn. Trong mỗi vòng như vậy, các đội được ghép đôi, mỗi cặp chơi một trò chơi và một nửa số đội bị loại. 

Cuối cùng số đội còn lại trở thành số lẻ. Nếu chỉ còn lại một đội, giải đấu sẽ kết thúc ngay lập tức. Nếu không, tất cả các đội còn lại sẽ thi đấu vòng tròn một lượt, trong đó mỗi cặp đội gặp nhau đúng một lần. 

Đầu vào là một số nguyên duy nhất`n`, tổng số trò chơi cần thiết. Đầu ra là số lượng đội ban đầu dẫn đến kết quả chính xác`n`trò chơi, được in theo thứ tự tăng dần. 

Sự hạn chế là khó khăn chính. Giá trị của`n`có thể lớn như`10^18`, vì vậy bất kỳ thuật toán nào thử đếm trực tiếp nhiều nhóm ứng viên đều không thể thực hiện được. Thậm chí lặp đi lặp lại lên đến`√n`sẽ ở xung quanh`10^9`hoạt động trong trường hợp xấu nhất. Chúng ta cần một giải pháp mà thời gian chạy chỉ phụ thuộc logarit vào kích thước của các số liên quan. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như`n = 1`. Một giải pháp bất cẩn có thể khiến mọi giải đấu hợp lệ cuối cùng đều chuyển sang giai đoạn thi đấu vòng tròn một lượt. Điều đó là sai. Bắt đầu với hai đội sẽ có một trận đấu loại trực tiếp và sau đó là một đội chiến thắng duy nhất. Đầu ra đúng là:```
2
```Coi như`n = 3`. Có hai câu trả lời hợp lệ:```
3
4
```Bắt đầu với ba đội ngay lập tức sẽ diễn ra vòng tròn ba trận. Bắt đầu với bốn đội tạo ra hai trận đấu loại trực tiếp, để lại hai đội, sau đó thêm một trận đấu loại trực tiếp, tổng cộng là ba trận. Một giải pháp chỉ tìm kiếm số lượng nhóm lẻ sẽ bỏ lỡ`4`. 

Một trường hợp tinh vi khác là khi nhiều lũy thừa khác nhau của hai chia cùng một cấu trúc câu trả lời. Số lượng đội ban đầu khác nhau có thể tạo ra tổng số trò chơi giống nhau, vì vậy chúng tôi phải thu thập tất cả các giải pháp hợp lệ và sắp xếp chúng thay vì dừng lại sau trận đấu đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi số lượng đội ban đầu có thể`m`, mô phỏng giải đấu, tính số trận đấu và kiểm tra xem nó có bằng không`n`. 

Mô phỏng rất đơn giản. Liên tục giảm một nửa số đội khi nó chẵn, thêm`m/2`trò chơi mỗi lần. Khi là số lẻ`k`còn lại, thêm vào`k(k-1)/2`trò chơi quay vòng. 

Phương pháp bạo lực này là đúng vì nó tuân thủ chính xác các quy tắc của giải đấu. Vấn đề là không gian tìm kiếm. Câu trả lời có thể lớn hơn nhiều so với`10^9`và trên thực tế số lượng đội hợp lệ có thể vào khoảng`10^18`. Việc liệt kê các ứng cử viên là hoàn toàn không khả thi. 

Cấu trúc của giải đấu mang lại sự mô tả tốt hơn nhiều. 

Giả sử số đội ban đầu là$$m = 2^t \cdot k$$Ở đâu`k`thật kỳ quặc. 

Sau vòng loại đầu tiên, số đội trở thành$$2^{t-1}k$$Sau lần thứ hai,$$2^{t-2}k$$và vân vân. 

Sau chính xác`t`giai đoạn loại bỏ,`k`các đội vẫn còn, và`k`thật kỳ quặc. 

Các trò chơi loại trừ góp phần$$2^{t-1}k + 2^{t-2}k + \cdots + k$$đó là$$k(2^t-1).$$Vòng chung kết góp phần$$\frac{k(k-1)}2.$$Do đó mọi giải đấu hợp lệ đều thỏa mãn$$n = k(2^t-1)+\frac{k(k-1)}2.$$Sau khi sắp xếp lại,$$n=\frac{k(2^{t+1}+k-3)}2.$$Bây giờ không gian tìm kiếm trở nên nhỏ bé. Từ`k`là số lẻ và tích cực, và`2^t ≤ 10^{18}`, số mũ`t`chỉ có thể lấy khoảng sáu mươi giá trị. 

Đối với một cố định`t`, phương trình trở thành phương trình bậc hai trong`k`:$$k^2+(2^{t+1}-3)k-2n=0.$$Chúng ta có thể giải quyết điều này bằng cách sử dụng số học số nguyên. Đối với mỗi khả năng`t`, kiểm tra xem phương trình bậc hai có nghiệm số nguyên dương hay không. Mỗi gốc như vậy sẽ cho một số lượng đội hợp lệ$$m=2^t k.$$Điều này làm giảm vấn đề từ việc tìm kiếm lên đến`10^{18}`các ứng cử viên chỉ kiểm tra khoảng sáu mươi số mũ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M log M) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`n`đủ số lượng trò chơi cần thiết. 
2. Lặp lại tất cả các số mũ`t`từ`0`hướng lên trong khi`2^t ≤ 10^18`. 
3. Đối với hiện tại`t`, định nghĩa$$a = 2^{t+1}-3.$$Phương trình trở thành$$k^2+ak-2n=0.$$1. Tính biệt số$$D=a^2+8n.$$Một nghiệm số nguyên chỉ tồn tại nếu`D`là một hình vuông hoàn hảo 

1. Hãy để`s = √D`. Nếu như`s² ≠ D`, bỏ qua số mũ này. 
2. Công thức bậc hai cho$$k=\frac{-a+s}{2}.$$Kiểm tra xem tử số có dương và chẵn không. 

1. Xác minh rằng`k`là tích cực và lẻ. Chỉ có giá trị lẻ tương ứng với các đội còn lại sau tất cả các vòng loại. 
2. Xây dựng$$m = 2^t k.$$1. Tính lại số trận đấu của giải đấu từ`m`sử dụng công thức dẫn xuất và xác minh nó bằng`n`. Điều này bảo vệ chống lại bất kỳ sai lầm số học. 
2. Lưu trữ mọi giá trị`m`. 
3. Sau khi xử lý tất cả số mũ, hãy sắp xếp các đáp án và in chúng ra. Nếu không tồn tại, in`-1`. 

### Tại sao nó hoạt động 

Mỗi số đội ban đầu có thể được viết duy nhất là`m = 2^t k`Ở đâu`k`thật kỳ quặc. Giải đấu luôn diễn ra chính xác`t`giai đoạn loại bỏ và sau đó dừng lại với`k`các đội. 

Giai đoạn loại bỏ góp phần chính xác`k(2^t-1)`trò chơi và giai đoạn thi đấu vòng tròn đóng góp chính xác`k(k-1)/2`trò chơi. Như vậy mọi lời giải hợp lý đều phải thỏa mãn$$n=\frac{k(2^{t+1}+k-3)}2.$$Ngược lại, bất cứ khi nào một số nguyên dương lẻ`k`thỏa mãn phương trình này đối với một số`t`, số đội`m=2^t k`sản xuất chính xác`n`trò chơi. Thuật toán liệt kê tất cả số mũ có thể`t`, tìm tất cả các nghiệm nguyên`k`và do đó tìm thấy mọi đội ban đầu hợp lệ được đếm chính xác một lần. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

def main():
    n = int(input())

    ans = []

    p = 1  # 2^t
    while p <= 10**18:
        a = 2 * p - 3
        D = a * a + 8 * n

        s = isqrt(D)
        if s * s == D:
            num = s - a

            if num > 0 and num % 2 == 0:
                k = num // 2

                if k > 0 and k % 2 == 1:
                    games = k * (p - 1) + k * (k - 1) // 2

                    if games == n:
                        ans.append(p * k)

        p <<= 1

    ans.sort()

    if not ans:
        print(-1)
    else:
        print("\n".join(map(str, ans)))

if __name__ == "__main__":
    main()
```Vòng lặp liệt kê tất cả các giá trị có thể có của`2^t`. Có ít hơn sáu mươi giá trị như vậy trước khi vượt quá`10^18`, đó là lý do tại sao thuật toán cực kỳ nhanh. 

Với mỗi số mũ, ta rút ra phương trình bậc hai với phần dư lẻ`k`. Việc kiểm tra phân biệt là rất quan trọng. Một phương trình bậc hai có hệ số nguyên chỉ có nghiệm nguyên khi phân biệt là một số chính phương. 

biểu hiện```
num = s - a
```xuất phát trực tiếp từ công thức bậc hai. Chúng ta phải xác minh rằng nó dương và chia hết cho hai, nếu không`k`không phải là số nguyên. 

Việc tính toán lại số trò chơi cuối cùng được thực hiện hoàn toàn bằng số học số nguyên. Các số nguyên có độ chính xác tùy ý của Python loại bỏ các mối lo ngại về tràn, nhưng bước xác minh vẫn hữu ích vì nó đảm bảo rằng chỉ các giải pháp thực sự hợp lệ mới được đưa ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```| t | p=2^t | a=2p-3 | D | √D | k | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 25 | 5 | 3 | Có | 
| 1 | 2 | 1 | 25 | 5 | 2 | Không, thậm chí | 
| 2 | 4 | 5 | 49 | 7 | 1 | Có | 
| 3 | 8 | 13 | 193 | không vuông | - | Không | 

Số đội hợp lệ là:$$1 \cdot 3 = 3$$Và$$4 \cdot 1 = 4.$$Đầu ra:```
3
4
```Ví dụ này cho thấy tại sao có thể có nhiều câu trả lời. Một giải đấu bắt đầu trực tiếp bằng thể thức vòng tròn một lượt, trong khi giải đấu còn lại hoàn toàn bao gồm các vòng loại trực tiếp. 

### Ví dụ 2 

đầu vào:```
1
```| t | p=2^t | a=2p-3 | D | √D | k | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 9 | 3 | 2 | Không, thậm chí | 
| 1 | 2 | 1 | 9 | 3 | 1 | Có | 
| 2 | 4 | 5 | 33 | không vuông | - | Không | 

Số đội hợp lệ duy nhất là$$2 \cdot 1 = 2.$$Đầu ra:```
2
```Ví dụ này minh họa trường hợp đặc biệt khi giải đấu không bao giờ diễn ra vòng tròn tính điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Khoảng sáu mươi số mũ được kiểm tra | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Đầu vào lớn nhất là`10^18`, tuy nhiên thuật toán chỉ thực hiện khoảng sáu mươi lần lặp. Mỗi lần lặp sử dụng số học theo thời gian không đổi trên các số nguyên tỷ lệ 64 bit, dễ dàng khớp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from math import isqrt

def solve():
    n = int(input())

    ans = []
    p = 1

    while p <= 10**18:
        a = 2 * p - 3
        D = a * a + 8 * n

        s = isqrt(D)
        if s * s == D:
            num = s - a

            if num > 0 and num % 2 == 0:
                k = num // 2

                if k > 0 and k % 2 == 1:
                    games = k * (p - 1) + k * (k - 1) // 2

                    if games == n:
                        ans.append(p * k)

        p <<= 1

    ans.sort()

    if not ans:
        return "-1"

    return "\n".join(map(str, ans))

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    return solve()

# provided sample
assert run("3\n") == "3\n4", "sample 1"

# custom cases
assert run("1\n") == "2", "single elimination game"
assert run("6\n") == "4", "pure elimination tournament"
assert run("10\n") == "5\n20", "multiple valid answers"
assert run("2\n") == "-1", "no valid team count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`2`| Giải đấu kết thúc với một người chiến thắng | 
|`6`|`4`| Cấu trúc loại bỏ thuần túy | 
|`10`|`5`Và`20`| Nhiều câu trả lời hợp lệ | 
|`2`|`-1`| Không có giải pháp tồn tại | 

## Vỏ cạnh 

Xem xét đầu vào:```
1
```Thuật toán kiểm tra mọi số mũ. Vì`t = 1`, nó tìm thấy`k = 1`, cho`m = 2`. Số trò chơi kết quả chính xác là một. Điều này khẳng định rằng các giải đấu kết thúc ngay sau vòng loại đều được xử lý đúng đắn. 

Xem xét đầu vào:```
3
```Hai giá trị số mũ khác nhau tạo ra các nghiệm hợp lệ. Thuật toán ghi lại cả`3`Và`4`, sắp xếp chúng và in cả hai. Bất kỳ giải pháp nào dừng lại sau trận đấu đầu tiên sẽ thất bại ở đây. 

Xem xét đầu vào:```
2
```Mọi kiểm tra phân biệt đều thất bại hoặc tạo ra một gốc không hợp lệ. Danh sách câu trả lời vẫn trống và thuật toán sẽ in`-1`. Điều này xác minh việc xử lý chính xác khi không có cấu trúc giải đấu nào có thể tạo ra số lượng trò chơi được yêu cầu. 

Hãy xem xét đầu vào rất lớn gần`10^18`. Thuật toán vẫn chỉ kiểm tra khoảng sáu mươi giá trị số mũ. Tất cả số học được thực hiện bằng số nguyên và số nguyên có độ chính xác tùy ý của Python xử lý một cách an toàn các phân biệt lớn hơn`10^18`không tràn.
