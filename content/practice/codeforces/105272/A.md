---
title: "CF 105272A - Giám sát hồ quang"
description: "Chúng tôi được cung cấp vị trí của các phòng giam được bố trí xung quanh một nhà tù hình tròn có nhãn từ 1 đến 360. Vì cấu trúc là hình tròn nên phòng 360 nằm liền kề với phòng giam 1, do đó, bất kỳ khoảng giám sát liền kề nào cũng có thể bao quanh ranh giới này."
date: "2026-06-23T14:10:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "A"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 49
verified: true
draft: false
---

[CF 105272A - Giám sát hồ quang](https://codeforces.com/problemset/problem/105272/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp vị trí của các phòng giam được bố trí xung quanh một nhà tù hình tròn có nhãn từ 1 đến 360. Vì cấu trúc là hình tròn nên phòng 360 nằm liền kề với phòng giam 1, do đó, bất kỳ khoảng giám sát liền kề nào cũng có thể bao quanh ranh giới này. 

Nhiệm vụ của chúng tôi là đặt các camera bao phủ chính xác một đoạn ô hình tròn liền kề. Phân đoạn này phải chứa tất cả các ô bị chiếm dụng. Trong số tất cả các phân đoạn hợp lệ như vậy, chúng tôi muốn phân đoạn có độ dài tối thiểu, trong đó độ dài được tính là số ô trong khoảng bao gồm cả hai đầu. 

Đầu vào đưa ra danh sách các chỉ số ô bị chiếm dụng. Đầu ra là kích thước nhỏ nhất có thể có của một cung tròn liền kề bao phủ mọi vị trí bị chiếm dụng. 

Ràng buộc n 360 đủ nhỏ để ngay cả các cách tiếp cận bậc ba hoặc bậc hai trên toàn bộ vòng tròn cũng có thể được chấp nhận. Tuy nhiên, cấu trúc vòng tròn là khó khăn chính: các khoảng bao trùm từ 360 trở lại 1 phải được xử lý một cách nhất quán. Cách tiếp cận khoảng tuyến tính đơn giản sẽ thất bại trừ khi nó giải thích rõ ràng về sự bao bọc. 

Một trường hợp lỗi nhỏ xuất hiện khi các ô bị chiếm tập trung ở gần cả hai đầu của phạm vi đánh số. Ví dụ: nếu các ô bị chiếm là {350, 355, 5}, thì khoảng tuyến tính từ 5 đến 350 không có ý nghĩa trực tiếp, nhưng giải pháp tối ưu là khoảng bao gồm 350 → 5, tức là ngắn. Bất kỳ giải pháp nào chỉ xem xét các vị trí tối thiểu và tối đa tuyến tính sẽ trả về không chính xác một giá trị gần bằng 345 thay vì cung nhỏ chính xác. 

Một trường hợp cạnh khác là khi các ô bị chiếm đã liên tiếp trong một đoạn nhỏ, chẳng hạn như {10, 11, 12, 13}. Khi đó, câu trả lời chỉ đơn giản là 4 và không cần bọc lại. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi ô bắt đầu có thể có trên vòng tròn và mở rộng về phía trước cho đến khi bao gồm tất cả các ô đã chiếm. Đối với mỗi lần bắt đầu, chúng tôi sẽ di chuyển theo chiều kim đồng hồ cho đến khi bao phủ tất cả các vị trí cần thiết, kiểm tra xem phân đoạn có bao gồm tất cả các ô bị chiếm dụng hay không và tính toán độ dài của nó. Vì có thể có 360 điểm bắt đầu và có thể có tới 360 điểm cuối và mỗi lần kiểm tra có thể quét tối đa n điểm, điều này dẫn đến khoảng O(360 × 360 × n), đủ nhỏ cho ràng buộc này nhưng lại lãng phí về mặt khái niệm. 

Sự kém hiệu quả xuất phát từ việc liên tục kiểm tra lại xem một phân đoạn có bao gồm tất cả các ô bị chiếm dụng hay không. Thay vì kiểm tra tất cả các khoảng một cách rõ ràng, chúng ta có thể trình bày lại vấn đề: chúng ta đang chọn một đường cắt hình tròn trong đó các điểm bị chiếm "dải rộng nhất" trên đường tròn. Nếu chúng ta sắp xếp các vị trí bị chiếm giữ thì đoạn tối ưu là phần bù của khoảng cách lớn nhất giữa các điểm bị chiếm liên tiếp trên vòng tròn. Loại bỏ khoảng cách lớn nhất đó sẽ mang lại cung nhỏ nhất vẫn chứa tất cả các điểm. 

Thông tin chi tiết quan trọng là bất kỳ khoảng bao phủ hình tròn nào cũng có thể được coi là một đoạn đường sau khi chọn điểm cắt trên đường tròn. Nếu chúng ta chọn đường cắt nằm bên trong khoảng trống lớn nhất thì tất cả các điểm bị chiếm sẽ rơi vào một khoảng tuyến tính duy nhất và khoảng đó là tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo từng khoảng thời gian | O(360² · n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Sắp xếp + khoảng cách tối đa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các vị trí bị chiếm đóng và sắp xếp chúng theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta suy luận về các ô bị chiếm liên tiếp trên vòng tròn. 
2. Tính toán khoảng cách giữa các ô bị chiếm liên tiếp theo thứ tự được sắp xếp này. Với mỗi i từ 0 đến n − 2, hãy tính ci+1 − ci. Chúng đại diện cho những đoạn trống khi di chuyển theo chiều kim đồng hồ. 
3. Đồng thời tính khoảng cách hình tròn giữa phần tử cuối cùng và phần tử đầu tiên, bằng (360 − cuối cùng + đầu tiên). Điều này ghi lại phân đoạn trống bao quanh. 
4. Tìm mức tối đa trong số tất cả những khoảng trống này. Khoảng trống này đại diện cho vùng lớn nhất của các ô không được sử dụng trên vòng tròn. 
5. Đáp án là 360 − (khoảng cách tối đa). Điều này tương ứng với việc chọn vết cắt bên trong vùng trống lớn nhất sao cho tất cả các ô bị chiếm nằm trong một vòng cung liền kề. 

### Tại sao nó hoạt động 

Bất kỳ khoảng tròn nào bao gồm tất cả các ô bị chiếm phải loại trừ chính xác một cung trống liền kề của vòng tròn. Nếu chúng ta chọn loại trừ cung trống lớn nhất, chúng ta sẽ giảm thiểu cung bị che phủ còn lại. Bất kỳ lựa chọn nào khác đều loại trừ một khoảng trống nhỏ hơn, điều này buộc khoảng thời gian được bao phủ phải mở rộng hơn nữa xung quanh vòng tròn. Vì vậy, lời giải tối ưu được xác định hoàn toàn bằng khoảng cách lớn nhất giữa các điểm chiếm liên tiếp theo thứ tự vòng tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    a.sort()

    if n == 1:
        print(1)
        return

    max_gap = 0

    for i in range(n - 1):
        max_gap = max(max_gap, a[i + 1] - a[i])

    # circular gap
    max_gap = max(max_gap, 360 - a[-1] + a[0])

    print(360 - max_gap)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách sắp xếp các ô bị chiếm sao cho các khác biệt liên tiếp tương ứng với các khoảng trống thực tế theo chiều kim đồng hồ. Vòng lặp trên các cặp liền kề sẽ tính toán tất cả các khoảng trống bên trong. Khoảng cách hình tròn được xử lý riêng biệt bằng cách kết nối phần tử cuối cùng và phần tử đầu tiên thông qua ranh giới bao quanh ở 360. Trừ khoảng cách lớn nhất từ ​​​​360 sẽ mang lại vòng cung che phủ tối thiểu. 

Trường hợp đặc biệt n = 1 được xử lý tường minh vì không có khoảng trống; cung tối thiểu bao phủ một điểm là một ô. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
occupied = [10, 20, 30]
```Thứ tự sắp xếp đã giống nhau rồi. Khoảng trống là: 

| Bước | Cặp | Khoảng cách | 
| --- | --- | --- | 
| 1 | 10 → 20 | 10 | 
| 2 | 20 → 30 | 10 | 
| 3 | 30 → 10 (bọc) | 340 | 

Khoảng cách tối đa là 340. 

Đáp án là 360 − 340 = 20. 

Điều này tương ứng với việc cắt đường tròn qua vùng trống lớn từ 30 đến 10, để lại một vòng cung chặt chẽ từ 10 đến 30. 

### Ví dụ 2 

đầu vào:```
n = 4
occupied = [350, 355, 2, 10]
```Đã sắp xếp: 

350, 355, 2, 10 

Bây giờ tính theo vòng tròn: 

| Bước | Cặp | Khoảng cách | 
| --- | --- | --- | 
| 1 | 350 → 355 | 5 | 
| 2 | 355 → 2 | 7 (quấn 360) | 
| 3 | 2 → 10 | 8 | 
| 4 | 10 → 350 (bọc) | 340 | 

Khoảng cách tối đa là 340. 

Đáp án là 360 − 340 = 20. 

Điều này cho thấy lý do tại sao cung tối ưu bao quanh ranh giới và bỏ qua vùng trống lớn trong khoảng từ 10 đến 350. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét khoảng cách là tuyến tính | 
| Không gian | O(n) | Lưu giữ các vị trí đã chiếm đóng | 

Giới hạn giới hạn n ở mức 360, do đó việc sắp xếp và quét tuyến tính đơn lẻ rất nhanh. Giải pháp phù hợp cả về thời gian và giới hạn bộ nhớ ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input().strip())
    a = list(map(int, input().split()))
    a.sort()

    if n == 1:
        return 1

    max_gap = 0
    for i in range(n - 1):
        max_gap = max(max_gap, a[i + 1] - a[i])
    max_gap = max(max_gap, 360 - a[-1] + a[0])

    return 360 - max_gap

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve())

# minimum case
assert run("1\n100\n") == "1"

# simple linear segment
assert run("3\n10 20 30\n") == "20"

# wraparound case
assert run("3\n350 355 2\n") == "12"

# all consecutive
assert run("4\n1 2 3 4\n") == "4"

# symmetric split
assert run("2\n1 180\n") == "179"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý trường hợp cơ bản | 
| 10 20 30 | 20 | nén tuyến tính cơ bản | 
| 350 355 2 | 12 | bọc tròn đúng cách | 
| 1 2 3 4 | 4 | không quấn, bó chặt | 
| 1 180 | 179 | khoảng cách đối xứng lớn | 

## Vỏ cạnh 

Khi chỉ có một ô bị chiếm dụng, thuật toán trực tiếp trả về 1 vì không có tính toán khoảng trống nào có ý nghĩa. Lý luận vòng tròn vẫn đúng nếu chúng ta tưởng tượng một vòng tròn 360 ô đầy đủ: loại bỏ toàn bộ vòng tròn ngoại trừ một điểm duy nhất sẽ tạo ra một cung có độ dài 1. 

Đối với các đầu vào như [350, 355, 2], việc sắp xếp tạo ra sự gián đoạn ở ranh giới bao bọc. Khoảng trống được tính toán chính xác bao gồm khoảng cách lớn 340 ô giữa 355 và 2 thông qua logic bao quanh. Khoảng cách đó trở thành khoảng cách tối đa, do đó đáp án cuối cùng trở thành 360 − 340 = 20, khớp với cung nhỏ trải dài qua ranh giới 360 → 1. 

Đối với các đầu vào có khoảng cách đều nhau như [1, 121, 241], tất cả các khoảng trống đều bằng nhau và không có một hướng nào chiếm ưu thế. Thuật toán vẫn hoạt động vì bất kỳ khoảng cách tối đa nào được chọn đều dẫn đến một cung tối thiểu hợp lệ và việc trừ đi nó sẽ mang lại kích thước phân đoạn bao phủ tối thiểu chính xác.
