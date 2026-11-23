---
title: "CF 436A - Cho ăn bằng kẹo"
description: "Chúng tôi có tới 2000 viên kẹo. Mỗi viên kẹo có một loại, 0 hoặc 1, độ cao nhảy bắt buộc và khối lượng. Om Nom bắt đầu bằng sức nhảy x. Anh ta có thể ăn bất kỳ viên kẹo chưa ăn nào có chiều cao tối đa bằng sức nhảy hiện tại của anh ta. Sau khi ăn một viên kẹo có khối lượng m, sức nhảy của anh ta tăng thêm m."
date: "2026-06-07T03:00:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 436
codeforces_index: "A"
codeforces_contest_name: "Zepto Code Rush 2014"
rating: 1500
weight: 436
solve_time_s: 96
verified: true
draft: false
---

[CF 436A - Cho ăn bằng kẹo](https://codeforces.com/problemset/problem/436/A) 

**Đánh giá:** 1500 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 2000 viên kẹo. Mỗi viên kẹo có một loại, 0 hoặc 1, độ cao nhảy bắt buộc và khối lượng. 

Om Nom bắt đầu bằng sức bật nhảy`x`. Anh ta có thể ăn bất kỳ viên kẹo chưa ăn nào có chiều cao tối đa bằng sức nhảy hiện tại của anh ta. Sau khi ăn kẹo với khối lượng`m`, sức nhảy của anh ta tăng thêm`m`. 

Có một hạn chế bổ sung: kẹo liên tiếp phải có loại khác nhau. Nếu viên kẹo cuối cùng được ăn là loại 0 thì viên kẹo tiếp theo phải là loại 1 và ngược lại. 

Mục tiêu là tối đa hóa tổng số kẹo được ăn. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Với`n ≤ 2000`, MỘT`O(n²)`giải pháp này hoàn toàn an toàn vì nó thực hiện khoảng bốn triệu thao tác. Mặt khác, việc tìm kiếm theo cấp số nhân trên tất cả các đơn hàng ăn uống hợp lệ là không thể vì số lượng đơn hàng tăng theo giai thừa. 

Phần khó khăn là việc ăn kẹo sẽ thay đổi khả năng tiếp cận trong tương lai. Một lựa chọn có vẻ tốt ở địa phương có thể mở khóa được nhiều kẹo hơn sau này. 

Một sai lầm dễ mắc phải là luôn lấy những viên kẹo có thể với tới với chiều cao nhỏ nhất. 

Ví dụ:```
3 3
0 2 10
1 3 1
1 20 1
```Đầu ra đúng:```
2
```Nếu chúng ta ăn kẹo loại 0 trước, sức nhảy sẽ trở thành 13 và chúng ta vẫn có thể ăn kẹo loại 1. Thay vào đó, nếu chúng ta tập trung vào chiều cao hơn là tăng cân, chúng ta có thể không tối đa hóa khả năng tiếp cận trong tương lai. 

Một trường hợp tinh vi khác là trình tự tối ưu có thể bắt đầu bằng một trong hai loại.```
2 1
0 1 1
1 100 100
```Đầu ra đúng:```
1
```Bắt đầu từ loại 0 được một viên kẹo. Bắt đầu từ loại 1 cho số không. Bất kỳ giải pháp nào giả định kiểu bắt đầu cố định sẽ thất bại. 

Cạm bẫy thứ ba là giả định rằng trong số các loại kẹo có sẵn thuộc loại được yêu cầu, bất kỳ lựa chọn nào cũng tốt như nhau.```
3 5
0 5 1
0 5 10
1 15 1
```Đầu ra đúng:```
2
```Khi chọn kẹo loại 0 trước, lấy khối 10 thực sự tốt hơn lấy khối 1 vì nó ngay lập tức mở khóa kẹo loại 1. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Ở mỗi bước, hãy thử mọi loại kẹo có thể tiếp cận thuộc loại được yêu cầu, lặp lại và nhận được kết quả tốt nhất. Điều này khám phá tất cả các đơn đặt hàng ăn uống hợp lệ. 

Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng mọi trình tự pháp lý. Thật không may, số lượng trình tự là rất lớn. Ngay cả với vài chục viên kẹo cũng không thể thực hiện được, và đối với`n = 2000`điều đó là hoàn toàn không thể. 

Để có được thứ gì đó nhanh hơn, chúng ta cần hiểu điều gì thực sự quan trọng. 

Giả sử viên kẹo tiếp theo phải có loại 0. Trong số tất cả những viên kẹo loại 0 chưa sử dụng có thể tiếp cận được, hãy tưởng tượng chọn một viên có khối lượng`a`trong khi một lựa chọn khác có thể tiếp cận được có khối lượng lớn hơn`b`. 

Sau khi ăn viên kẹo có khối lượng lớn hơn, sức nhảy sẽ trở nên lớn hơn sau khi ăn viên kẹo nhỏ hơn. Mỗi viên kẹo có thể tiếp cận sau khi đạt được`a`cũng có thể truy cập sau khi đạt được`b`. Lợi ích lớn hơn không bao giờ loại bỏ các khả năng. 

Quan sát này là tài sản tham lam quan trọng. Bất cứ khi nào chúng ta phải chọn một loại kẹo nhất định, sự lựa chọn tốt nhất chỉ đơn giản là loại kẹo đó có khối lượng tối đa có thể tiếp cận được. 

Sự không chắc chắn duy nhất còn lại là loại bắt đầu. Vì chỉ có hai loại nên chúng ta có thể mô phỏng quá trình tham lam hai lần. 

Trong một mô phỏng, chúng tôi nhấn mạnh rằng viên kẹo đầu tiên là loại 0. 

Trong mô phỏng khác, chúng tôi nhấn mạnh rằng viên kẹo đầu tiên là loại 1. 

Đối với mỗi mô phỏng: 

1. Liên tục tìm loại kẹo có thể tiếp cận chưa sử dụng thuộc loại yêu cầu với khối lượng tối đa. 
2. Ăn nó đi. 
3. Tăng sức nhảy. 
4. Chuyển đổi loại yêu cầu. 

Khi không có loại kẹo nào có thể tiếp cận được, quá trình mô phỏng sẽ kết thúc. 

Câu trả lời là kết quả lớn hơn của hai mô phỏng. Chiến lược tham lam này là giải pháp tiêu chuẩn được chấp nhận cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Mô phỏng tham lam tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các loại kẹo và lưu trữ loại, chiều cao và khối lượng của chúng. 
2. Chạy mô phỏng giả định viên kẹo được ăn đầu tiên phải là loại 0. 
3. Tạo một`used`mảng đánh dấu kẹo nào đã được ăn. 
4. Đặt công suất nhảy hiện tại thành`x`. 
5. Hãy để`need_type`là loại phải ăn tiếp theo. 
6. Quét tất cả các loại kẹo và tìm một loại kẹo chưa sử dụng có loại`need_type`, có chiều cao tối đa là sức bật nhảy hiện tại và có khối lượng lớn nhất trong số tất cả các ứng cử viên như vậy. 
7. Nếu không có ứng viên nào tồn tại, hãy dừng mô phỏng này. 

Tại thời điểm này, không có động thái hợp pháp nào được thực hiện vì viên kẹo tiếp theo phải có loại được yêu cầu. 
8. Ăn kẹo đã chọn. 

Tăng sức nhảy theo khối lượng của nó, đánh dấu nó là đã sử dụng, tăng câu trả lời cho mô phỏng này và lật`need_type`. 
9. Lặp lại từ bước 6. 
10. Thực hiện lại quy trình tương tự, lần này bắt đầu với loại 1. 
11. Xuất kết quả lớn hơn trong hai kết quả. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ bước nào mà viên kẹo tiếp theo phải có một số loại cố định. 

Trong số tất cả các loại kẹo có thể tiếp cận được, hãy để`A`là viên kẹo có khối lượng lớn nhất Giả sử một viên kẹo khác có thể tiếp cận được`B`được chọn thay thế. 

Sau khi ăn`A`, sức nhảy ít nhất cũng lớn bằng sau khi ăn`B`. Mỗi viên kẹo có thể tiếp cận sau khi chọn`B`vẫn có thể truy cập sau khi chọn`A`. Vì yêu cầu luân phiên loại chỉ phụ thuộc vào loại kẹo chứ không phụ thuộc vào khối lượng nên việc chọn`A`không thể giảm tập hợp các nước đi hợp lệ trong tương lai. 

Việc thay thế bất kỳ lựa chọn có khối lượng nhỏ hơn nào bằng khối lượng lớn nhất có thể tiếp cận được không bao giờ gây tổn hại. Áp dụng lập luận này ở mỗi bước chứng tỏ rằng lựa chọn tham lam luôn tối ưu cho kiểu xuất phát cố định. 

Vì mọi chuỗi hợp lệ phải bắt đầu bằng loại 0 hoặc loại 1, nên việc kiểm tra cả hai loại bắt đầu đảm bảo rằng tìm thấy mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(start_type, candies, x):
    n = len(candies)
    used = [False] * n

    cur_jump = x
    need_type = start_type
    eaten = 0

    while True:
        best_idx = -1
        best_mass = -1

        for i, (t, h, m) in enumerate(candies):
            if not used[i] and t == need_type and h <= cur_jump:
                if m > best_mass:
                    best_mass = m
                    best_idx = i

        if best_idx == -1:
            break

        used[best_idx] = True
        cur_jump += candies[best_idx][2]
        eaten += 1
        need_type ^= 1

    return eaten

def solve():
    n, x = map(int, input().split())

    candies = []
    for _ in range(n):
        t, h, m = map(int, input().split())
        candies.append((t, h, m))

    ans = max(
        simulate(0, candies, x),
        simulate(1, candies, x)
    )

    print(ans)

solve()
```Hàm mô phỏng thực hiện trực tiếp quá trình tham lam. các`used`mảng ngăn chặn việc tái sử dụng kẹo. Ở mỗi bước, chúng tôi tìm kiếm tất cả các loại kẹo và chọn loại kẹo có thể tiếp cận được với khối lượng tối đa. 

Sự so sánh sử dụng khối lượng thay vì chiều cao vì bằng chứng dựa vào việc tối đa hóa sức bật trong tương lai. Việc chọn loại kẹo có khả năng tiếp cận cao nhất sẽ không bảo toàn được tài sản tham lam. 

Thứ tự cập nhật quan trọng. Trước tiên, chúng tôi đánh dấu kẹo là đã sử dụng, sau đó thêm khối lượng của nó vào sức nhảy, sau đó chuyển loại theo yêu cầu. Điều này khớp chính xác với chuỗi sự kiện của vấn đề. 

Tất cả các giá trị dễ dàng khớp với số nguyên Python. Ngay cả trong trường hợp cực đoan, tổng khối lượng đạt được nhiều nhất là`2000 × 2000 = 4,000,000`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
0 2 4
1 3 1
0 8 3
0 20 10
1 5 5
```Bắt đầu với loại 0: 

| Bước | Cần Loại | Nhảy sức mạnh trước | Kẹo được chọn | Thánh lễ | Nhảy sức mạnh sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | (0,2,4) | 4 | 7 | 
| 2 | 1 | 7 | (1,5,5) | 5 | 12 | 
| 3 | 0 | 12 | (0,8,3) | 3 | 15 | 
| 4 | 1 | 15 | (1,3,1) | 1 | 16 | 

Không còn kẹo loại 0 nào có thể tiếp cận được nên quá trình dừng lại với 4 viên kẹo. 

Bắt đầu với loại 1 chỉ có 3 viên kẹo nên đáp án là 4. 

Ví dụ này cho thấy việc tối đa hóa khối lượng của số kẹo có thể tiếp cận sẽ nhanh chóng tăng sức mạnh nhảy và mở ra nhiều tùy chọn hơn sau này. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 5
0 5 1
0 5 10
1 15 1
```Bắt đầu với loại 0: 

| Bước | Cần Loại | Nhảy sức mạnh trước | Kẹo được chọn | Thánh lễ | Nhảy sức mạnh sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | (0,5,10) | 10 | 15 | 
| 2 | 1 | 15 | (1,15,1) | 1 | 16 | 

Kết quả: 2 cái kẹo. 

Nếu chúng ta chọn kẹo khối 1 trước, sức nhảy sẽ chỉ là 6 và kẹo loại 1 sẽ không thể tiếp cận được. 

Điều này chứng tỏ chính xác tại sao tiêu chí tham lam lại dựa trên khối lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hai mô phỏng, mỗi mô phỏng thực hiện tối đa n lần lặp và quét tất cả n kẹo | 
| Không gian | O(n) | Kho đựng kẹo và mảng đã qua sử dụng | 

Với`n ≤ 2000`,`O(n²)`có nghĩa là khoảng bốn triệu séc, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n, x = map(int, input().split())

    candies = [tuple(map(int, input().split())) for _ in range(n)]

    def simulate(start_type):
        used = [False] * n
        cur = x
        need = start_type
        ans = 0

        while True:
            best = -1
            best_mass = -1

            for i, (t, h, m) in enumerate(candies):
                if not used[i] and t == need and h <= cur and m > best_mass:
                    best_mass = m
                    best = i

            if best == -1:
                break

            used[best] = True
            cur += candies[best][2]
            ans += 1
            need ^= 1

        return ans

    print(max(simulate(0), simulate(1)))

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# provided sample
assert run(
"""5 3
0 2 4
1 3 1
0 8 3
0 20 10
1 5 5
"""
) == "4", "sample 1"

# minimum size
assert run(
"""1 1
0 1 1
"""
) == "1", "single reachable candy"

# unreachable candy
assert run(
"""1 1
0 2 1
"""
) == "0", "cannot reach"

# must choose larger mass
assert run(
"""3 5
0 5 1
0 5 10
1 15 1
"""
) == "2", "greedy by mass"

# alternating restriction
assert run(
"""4 10
0 1 1
0 1 1
1 1 1
1 1 1
"""
) == "4", "alternation handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Kẹo có thể tiếp cận duy nhất | 1 | Phiên bản hợp lệ tối thiểu | 
| Kẹo duy nhất không thể tiếp cận | 0 | Kiểm tra khả năng tiếp cận | 
| Hai viên kẹo có thể tiếp cận cùng loại với khối lượng khác nhau | 2 | Tham lam phải tối đa hóa khối lượng | 
| Hai loại kẹo mỗi loại | 4 | Logic thay thế hoạt động chính xác | 

## Vỏ cạnh 

### Chỉ có một kiểu khởi đầu hoạt động 

đầu vào:```
2 1
0 1 1
1 100 100
```Bắt đầu từ loại 0, Om Nom ăn một viên kẹo và đạt sức mạnh nhảy cấp 2. 

Bắt đầu với loại 1, không có kẹo nào có thể tiếp cận được. 

Thuật toán mô phỏng rõ ràng cả hai loại bắt đầu và trả về kết quả tốt hơn, là 1. 

### Khối lượng lớn hơn không phải là tùy chọn 

đầu vào:```
3 5
0 5 1
0 5 10
1 15 1
```Ở mức nhảy sức mạnh 5, cả hai loại kẹo loại 0 đều có thể truy cập được. 

Thuật toán chọn khối lượng 10, nâng sức nhảy lên 15 và mở khóa kẹo loại 1. 

Nếu nó chọn khối 1, sức nhảy sẽ chỉ còn 6 và câu trả lời sẽ giảm từ 2 xuống 1. 

### Kẹo có thể tiếp cận nhưng sai loại 

đầu vào:```
3 10
0 1 1
0 2 1
1 20 1
```Sau khi ăn một viên kẹo loại 0, loại kẹo bắt buộc tiếp theo là loại 1. 

Mặc dù có thể sử dụng được loại kẹo loại 0 khác nhưng không thể ăn nó ngay lập tức do quy tắc luân phiên. 

Quá trình mô phỏng dừng chính xác khi không có loại kẹo nào có thể truy cập được thuộc loại được yêu cầu. Điều này khớp chính xác với tuyên bố vấn đề.
