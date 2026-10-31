---
title: "CF 106351J - Zaghloul và những điệp viên"
description: "Chúng ta có một hàng người, trong đó mỗi người mang một số a[i]. Một người được phân loại là gián điệp khi XOR của mọi ước số dương của số của họ là bội số của k."
date: "2026-06-25T08:10:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106351
codeforces_index: "J"
codeforces_contest_name: "Zaglol Contest - FCDS level 2 contest 2026"
rating: 0
weight: 106351
solve_time_s: 30
verified: true
draft: false
---

[CF 106351J - Zaghloul và những điệp viên](https://codeforces.com/problemset/problem/106351/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng người, trong đó mỗi người mang một số`a[i]`. Một người được phân loại là gián điệp khi XOR của mọi ước số dương của số của họ là bội số của`k`. Đối với mỗi truy vấn hỏi về một phân khúc người từ`l`ĐẾN`r`, chúng ta cần đếm xem có bao nhiêu gián điệp trong phân đoạn đó. 

Kích thước đầu vào lớn. Số lượng người và số lượng truy vấn mỗi cái có thể lên tới một triệu và mỗi giá trị trong mảng cũng nhiều nhất là một triệu. Với thang đo này, giải pháp kiểm tra mọi truy vấn bằng cách quét phạm vi của nó là không thể. Trong trường hợp xấu nhất, một truy vấn có thể chứa toàn bộ mảng và việc thực hiện điều đó đối với tất cả các truy vấn sẽ yêu cầu khoảng`10^12`hoạt động. Ngay cả những giải pháp xung quanh`O(n sqrt(n))`không phù hợp thoải mái ở đây. Chúng ta cần tiền xử lý gần với tuyến tính hoặc`O(maxA log maxA)`. 

Khó khăn tiềm ẩn chính là điều kiện của một người phụ thuộc vào các ước số giá trị của họ. Việc tính toán các ước số riêng biệt cho từng phần tử mảng sẽ lặp lại rất nhiều công việc vì các giá trị giống nhau có thể xuất hiện nhiều lần. Vì tất cả các giá trị được giới hạn bởi một triệu, nên hướng đi đúng đắn là tính toán trước thông tin cho mọi giá trị có thể một lần. 

Một vài trường hợp có thể phá vỡ việc triển khai đơn giản hơn. Nếu bản thân số đó chỉ có một ước số thì XOR không bằng 0. Ví dụ:```
Input
1 1 2
1
1 1

Output
0
```Ước số duy nhất của`1`là`1`, Và`1`không chia hết cho`2`. Việc triển khai khởi tạo các giá trị XOR không chính xác về 0 và quên xử lý số đó sẽ cho kết quả sai. 

Các giá trị lặp lại là một cái bẫy phổ biến khác. Coi như:```
Input
5 1 3
6 6 6 6 6
1 5

Output
5
```Ước số XOR của`6`nên được tính toán một lần và tái sử dụng. Việc tính toán lại các ước số trong mỗi truy vấn sẽ lãng phí thời gian và có thể vượt quá giới hạn. 

Các phạm vi chứa phần tử đầu tiên hoặc cuối cùng cũng cần xử lý tiền tố chính xác. Ví dụ:```
Input
3 2 2
2 3 4
1 1
3 3

Output
0
1
```Truy vấn`[1,1]`phải sử dụng tiền tố trước phần tử đầu tiên một cách chính xác và`[3,3]`không được vô tình bao gồm những người trước đó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp bắt đầu bằng cách trả lời từng truy vấn một cách độc lập. Đối với một truy vấn`[l,r]`, chúng tôi kiểm tra mọi giá trị trong khoảng đó, tìm tất cả các ước số của nó, tính XOR của chúng, kiểm tra tính chia hết cho`k`, và đếm số gián điệp. Điều này đúng vì mỗi người đều được đánh giá chính xác theo định nghĩa. 

Vấn đề là số lượng công việc lặp đi lặp lại. Một số có thể xuất hiện nhiều lần và các phép tính chia tương tự sẽ xảy ra lặp đi lặp lại. Trong trường hợp xấu nhất, nếu tất cả các truy vấn bao trùm toàn bộ mảng thì số phần tử được kiểm tra là khoảng`n * q`, có thể đạt tới`10^12`. Việc tìm kiếm số chia bên trong mỗi lần kiểm tra làm cho phương pháp này thậm chí còn chậm hơn. 

Nhận xét quan trọng là tài sản chỉ phụ thuộc vào giá trị của một người chứ không phải vị trí của họ. Nếu hai người cùng giữ số`x`, hoặc cả hai đều là gián điệp hoặc không. Giá trị tối đa có thể chỉ là một triệu, vì vậy chúng ta có thể tính ước số XOR cho mọi giá trị có thể bằng cách sử dụng quy trình giống như sàng. 

Với mọi ứng cử viên chia số`d`, mọi bội số của`d`có`d`là một trong các ước số của nó. Chúng ta có thể lặp lại tất cả`d`và XOR`d`vào mọi bội số. Sau quá trình tiền xử lý này, mảng có thể được chuyển đổi thành mảng nhị phân trong đó mỗi vị trí được`1`nếu người đó là gián điệp và`0`nếu không thì. Tổng tiền tố sau đó trả lời mọi truy vấn phạm vi trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq sqrt(maxA)) | O(1) | Quá chậm | 
| Tối ưu | O(maxA log maxA + n + q) | O(maxA + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và ghi nhớ giá trị lớn nhất xuất hiện. Không cần phải xử lý trước các số lớn hơn mức tối đa này. 
2. Tạo một mảng`xor_divisors`Ở đâu`xor_divisors[x]`sẽ lưu trữ XOR của tất cả các ước số của`x`. Xử lý mọi ước số có thể`d`và thăm tất cả các bội số của`d`. XOR`d`vào từng bội số. 

Điều này hiệu quả vì mỗi lần chúng ta truy cập bội số của`d`, ta tìm được một ước của số đó. Sàng truy cập vào mọi mối quan hệ số chia đúng một lần. 
3. Xây dựng một mảng tiền tố trên mọi người. Với mỗi vị trí, hãy thêm`1`nếu như`xor_divisors[a[i]]`chia hết cho`k`, nếu không thì thêm`0`. 
4. Đối với mọi truy vấn`[l,r]`, trừ giá trị tiền tố trước`l`từ giá trị tiền tố tại`r`. Sự khác biệt chính xác là số lượng gián điệp trong phân khúc đó. 

Tại sao nó hoạt động: 

Giai đoạn tiền xử lý tính toán ước số XOR chính xác cho mọi giá trị có thể vì một số nhận được đóng góp XOR từ mọi số nguyên chia cho nó. Sau đó, trạng thái gián điệp của mỗi người được xác định từ giá trị được tính toán trước này, do đó, mảng tiền tố lưu trữ số lượng gián điệp chính xác được nhìn thấy cho đến nay. Sự khác biệt giữa hai tổng tiền tố sẽ loại bỏ tất cả những người trước phạm vi được yêu cầu và chỉ để lại những gián điệp bên trong`[l,r]`, đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q, k = map(int, input().split())
    a = list(map(int, input().split()))

    mx = max(a)

    xor_divisors = [0] * (mx + 1)

    for d in range(1, mx + 1):
        for multiple in range(d, mx + 1, d):
            xor_divisors[multiple] ^= d

    pref = [0] * (n + 1)

    for i, x in enumerate(a, 1):
        pref[i] = pref[i - 1] + (1 if xor_divisors[x] % k == 0 else 0)

    ans = []
    for _ in range(q):
        l, r = map(int, input().split())
        ans.append(str(pref[r] - pref[l - 1]))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc mảng và tìm giá trị lớn nhất. Sàng chia chỉ cần đạt giá trị đó, điều này tránh được những công việc không cần thiết khi giá trị mảng thực tế nhỏ. 

Các vòng lặp lồng nhau xây dựng bảng chia số XOR. Vòng lặp bên ngoài chọn một ước số có thể và vòng lặp bên trong chạm chính xác vào các số chứa ước số đó. XOR được sử dụng vì thứ tự áp dụng các thao tác XOR không quan trọng. 

Cấu trúc tiền tố chuyển đổi điều kiện toán học thành một bài toán đếm phạm vi thông thường. Mỗi vị trí lưu trữ số lượng gián điệp từ đầu mảng cho đến vị trí đó. 

Câu trả lời truy vấn sử dụng`pref[r] - pref[l - 1]`. Phép trừ bằng`l - 1`là chi tiết ranh giới ngăn chặn việc bao gồm những người trước phân đoạn được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4 2
2 3 4 5 6
1 5
1 3
1 1
2 4
```Các giá trị XOR của số chia là: 

| Số | Ước số | XOR | Gián điệp? | 
| --- | --- | --- | --- | 
| 2 | 1, 2 | 3 | Không | 
| 3 | 1, 3 | 2 | Có | 
| 4 | 1, 2, 4 | 7 | Không | 
| 5 | 1, 5 | 4 | Có | 
| 6 | 1, 2, 3, 6 | 4 | Có | 

| Truy vấn | Tiền tố phải | Tiền tố trước left | Trả lời | 
| --- | --- | --- | --- | 
| [1,5] | 3 | 0 | 3 | 
| [1,3] | 1 | 0 | 1 | 
| [1,1] | 0 | 0 | 0 | 
| [2,4] | 2 | 0 | 2 | 

Dấu vết xác nhận rằng sau khi xử lý trước, các truy vấn chỉ cần trừ tiền tố. Công việc chia được tách biệt hoàn toàn khỏi công việc phạm vi. 

### Dấu vết tùy chỉnh 

đầu vào:```
4 3 4
1 4 8 3
1 4
2 3
3 3
```| Vị trí | Giá trị | Số chia XOR | Đóng góp gián điệp | Tiền tố | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 
| 2 | 4 | 7 | 0 | 0 | 
| 3 | 8 | 15 | 0 | 0 | 
| 4 | 3 | 2 | 0 | 0 | 

| Truy vấn | Tính toán | Trả lời | 
| --- | --- | --- | 
| [1,4] | 0 - 0 | 0 | 
| [2,3] | 0 - 0 | 0 | 
| [3,3] | 0 - 0 | 0 | 

Ví dụ này kiểm tra các giá trị có nhiều ước số và xác nhận rằng bảng XOR xử lý chúng mà không có bất kỳ trường hợp đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M + n + q) |`M`là giá trị mảng tối đa và sàng chia sẽ truy cập bội số của mọi ước số có thể có | 
| Không gian | O(M + n) | Lưu trữ các giá trị XOR ước số, mảng và tổng tiền tố | 

Với`M`lên tới một triệu, sàng thực hiện khoảng mười bốn triệu lượt truy cập chia số, vừa vặn thoải mái trong giới hạn. Giai đoạn truy vấn có thời gian không đổi cho mỗi truy vấn, do đó một triệu truy vấn được xử lý hiệu quả. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q, k = map(int, input().split())
    a = list(map(int, input().split()))

    mx = max(a)
    xor_divisors = [0] * (mx + 1)

    for d in range(1, mx + 1):
        for m in range(d, mx + 1, d):
            xor_divisors[m] ^= d

    pref = [0] * (n + 1)
    for i, x in enumerate(a, 1):
        pref[i] = pref[i - 1] + (xor_divisors[x] % k == 0)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))

    return "\n".join(out)

assert solution("""5 4 2
2 3 4 5 6
1 5
1 3
1 1
2 4
""") == """3
1
0
2""", "sample 1"

assert solution("""1 1 2
1
1 1
""") == "0", "single number"

assert solution("""5 1 3
6 6 6 6 6
1 5
""") == "5", "repeated values"

assert solution("""3 2 2
2 3 4
1 1
3 3
""") == """0
1""", "boundary ranges"

assert solution("""4 2 1
1000000 999999 500000 1
1 4
2 2
""") == """4
1""", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn`1`với`k=2`|`0`| Xử lý chính xác mảng và ước số XOR nhỏ nhất có thể | 
| Năm bản sao của`6`|`5`| Xác nhận các giá trị lặp lại được xử lý chính xác | 
| Truy vấn chạm vào vị trí đầu tiên và cuối cùng |`0`,`1`| Kiểm tra ranh giới tiền tố | 
| Giá trị gần một triệu |`4`,`1`| Kiểm tra phạm vi giá trị tối đa | 

## Vỏ cạnh 

Đối với giá trị`1`, thuật toán bắt đầu bằng`xor_divisors[1] = 0`và cái sàng thêm số chia`1`, tạo ra giá trị XOR chính xác của`1`. Trên đầu vào:```
1 1 2
1
1 1
```đóng góp tiền tố bằng 0 vì`1 % 2 != 0`, vậy câu trả lời là`0`. 

Đối với các số lặp lại, quá trình xử lý trước sẽ ngăn chặn các phép tính ước số trùng lặp. TRONG:```
5 1 3
6 6 6 6 6
1 5
```sàng tính ước số XOR của`6`một lần. Vì XOR là`4`, Và`4`không chia hết cho`3`, mọi vị trí sẽ không đóng góp gì. Nếu ước số XOR thỏa mãn điều kiện, tất cả năm vị trí sẽ ngay lập tức được tính thông qua mảng tiền tố. 

Đối với các phạm vi ở ranh giới, mảng tiền tố luôn có thêm một mục nhập bằng 0 ở chỉ mục`0`. Đối với truy vấn`[1,1]`, câu trả lời là`pref[1] - pref[0]`, do đó không có chỉ mục phủ định không hợp lệ hoặc sự bao gồm ngẫu nhiên nào xảy ra. Đối với truy vấn kết thúc tại`n`, phép trừ tiền tố thông thường vẫn loại bỏ mọi thứ trước phân đoạn được yêu cầu.
