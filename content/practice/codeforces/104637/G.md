---
title: "CF 104637G - Nhảy ếch"
description: "Một con ếch bắt đầu ở vị trí 0 trên trục số và liên tục nhảy sang trái và sang phải theo một mẫu cố định. Lần nhảy đầu tiên di chuyển nó sang phải một khoảng a, lần nhảy thứ hai di chuyển nó sang trái một khoảng b, và sự luân phiên này tiếp tục với tổng số k lần nhảy."
date: "2026-06-29T17:01:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "G"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 59
verified: true
draft: false
---

[CF 104637G - Nhảy ếch](https://codeforces.com/problemset/problem/104637/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một con ếch bắt đầu ở vị trí 0 trên trục số và liên tục nhảy sang trái và sang phải theo một mẫu cố định. Bước nhảy đầu tiên sẽ di chuyển nó sang bên phải`a`, lần nhảy thứ hai sẽ di chuyển nó sang trái`b`, và sự luân phiên này tiếp tục cho`k`tổng số lần nhảy. Mỗi truy vấn đưa ra các giá trị khác nhau của`a`,`b`, Và`k`, và chúng ta phải xác định vị trí cuối cùng của con ếch sau khi thực hiện chính xác những bước nhảy đó. 

Điều quan trọng ở đây là chuyển động hoàn toàn xác định và tuần hoàn với chu kỳ gồm hai lần nhảy. Một chu kỳ bao gồm một bước di chuyển đúng kích thước`a`tiếp theo là di chuyển sang trái có kích thước`b`. Vị trí của ếch tiến hóa dưới dạng tổng xen kẽ đơn giản, nhưng số lần nhảy có thể lên tới 10^9, vì vậy việc mô phỏng rõ ràng từng bước là không thể. 

Các ràng buộc ngụ ý rằng bất kỳ mô phỏng mỗi lần nhảy nào đều không thể thực hiện được ngay lập tức. Với tối đa 1000 truy vấn và tối đa 10^9 bước cho mỗi truy vấn, cách tiếp cận O(k) sẽ yêu cầu tới 10^12 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta phải nén quy trình thành thời gian không đổi cho mỗi truy vấn. 

Một trường hợp cạnh tinh tế phát sinh khi`k`là số lẻ hoặc số chẵn vì từng phần của chu kỳ cuối cùng hoạt động khác đi. Ví dụ, nếu`a = 5`,`b = 2`, Và`k = 3`, trình tự là`+5, -2, +5`, kết quả là 8. Nếu`k = 4`, bước cuối cùng sẽ là`-2`, thay đổi số dư. Bất kỳ giải pháp nào giả định các cặp đầy đủ mà không xử lý phần còn sót lại sẽ thất bại. 

Một cạm bẫy phổ biến khác là giả sử tính đối xứng hoặc triệt tiêu ngoài các cặp đầy đủ. Khi`a`bằng`b`, các chu kỳ đầy đủ sẽ bị hủy, nhưng các chu kỳ không đầy đủ vẫn tạo ra các giá trị khác 0. Ví dụ, với`a = b = 1`Và`k = 999999999`, kết quả là 1 chứ không phải 0. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì vị trí hiện tại và thay thế việc thêm`a`và trừ`b`cho mỗi lần nhảy. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa một cách chính xác. Tuy nhiên, nó thực hiện một phép tính số học cho mỗi lần nhảy, vì vậy với số lượng lớn`k`nó trở nên quá chậm. 

Quan sát quan trọng là các bước nhảy diễn ra theo cặp lặp lại. Cứ hai lần nhảy sẽ đóng góp một lượng dịch chuyển ròng là`a - b`. Nếu chúng ta nhóm quy trình thành các cặp đầy đủ, chúng ta chỉ cần tính xem có bao nhiêu chu kỳ đầy đủ của hai lần nhảy và liệu có thêm một lần nhảy nào ở cuối hay không. 

Nếu như`k`chẵn, con ếch hoàn thành chính xác`k/2`chu kỳ đầy đủ, mỗi chu kỳ đóng góp`a - b`. Nếu như`k`thật kỳ lạ, có`(k-1)/2`chu kỳ đầy đủ cộng với một lần nhảy phải cuối cùng của`+a`. 

Điều này làm giảm vấn đề từ mô phỏng tuyến tính sang số học đơn giản dựa trên tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) | O(1) | Quá chậm | 
| Nén cặp số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi truy vấn: 

1. Đọc`a`,`b`, Và`k`. Chúng xác định kiểu chuyển động xen kẽ cố định và số bước chúng ta thực hiện. 
2. Tính xem có bao nhiêu cặp bước nhảy đầy đủ bằng cách sử dụng`k // 2`. Mỗi cặp bao gồm một`+a`di chuyển theo sau là một`-b`di chuyển. 
3. Nhân số cặp đầy đủ với`(a - b)`để có được tổng đóng góp của tất cả các chu kỳ hoàn chỉnh. Điều này có hiệu quả vì mỗi chu kỳ đầy đủ trả về một độ dịch chuyển ròng bằng với chênh lệch giữa bước nhảy tiến và lùi. 
4. Nếu`k`kỳ quặc, thêm một cái nữa`+a`cho bước chưa hoàn thành cuối cùng. Điều này là cần thiết vì trình tự luôn bắt đầu bằng bước nhảy về phía trước và các chuỗi có độ dài lẻ kết thúc ở bước di chuyển về phía trước. 
5. Xuất ra vị trí kết quả. 

### Tại sao nó hoạt động 

Trình tự này có tính tuần hoàn nghiêm ngặt với giai đoạn 2 và mỗi giai đoạn đóng góp một sự thay đổi ròng cố định của`a - b`. Sự sai lệch duy nhất so với sự ghép đôi hoàn hảo xảy ra khi`k`thật kỳ lạ, bước cuối cùng luôn là bước nhảy về phía trước. Vì quá trình này là tuyến tính và cộng tính nên việc chia thành các cặp rời rạc cộng với phần dư có thể có sẽ bảo toàn tổng chuyển vị chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    a, b, k = map(int, input().split())
    
    pairs = k // 2
    res = pairs * (a - b)
    
    if k % 2 == 1:
        res += a
    
    out.append(str(res))

print("\n".join(out))
```Giải pháp xử lý từng truy vấn một cách độc lập. Tính toán chính là phép chia số nguyên`k // 2`, tính chu kỳ (a, -b) đầy đủ. nhân với`(a - b)`thu gọn tất cả các chu trình đó thành một biểu thức số học. 

điều kiện`if k % 2 == 1`xử lý bước nhảy về phía trước còn lại khi`k`thật kỳ quặc. Vì bước nhảy đầu tiên luôn là về phía trước nên bước còn lại không bao giờ là một phép trừ. 

Tất cả số học được thực hiện bằng số nguyên Python, hỗ trợ các giá trị lớn lên tới 10^9 * 10^9 mà không gặp vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = 5, b = 2, k = 3`| Bước | Kiểu nhảy | Vị trí | 
| --- | --- | --- | 
| 0 | bắt đầu | 0 | 
| 1 | +5 | 5 | 
| 2 | -2 | 3 | 
| 3 | +5 | 8 | 

Thuật toán tính toán`pairs = 1`, cho`1 * (5 - 2) = 3`, sau đó thêm phần bổ sung`+5`bởi vì`k`là số lẻ, dẫn đến kết quả là 8. Điều này xác nhận rằng việc nén cặp phù hợp với mô phỏng rõ ràng. 

### Ví dụ 2 

đầu vào:`a = 100, b = 1, k = 4`| Bước | Kiểu nhảy | Vị trí | 
| --- | --- | --- | 
| 0 | bắt đầu | 0 | 
| 1 | +100 | 100 | 
| 2 | -1 | 99 | 
| 3 | +100 | 199 | 
| 4 | -1 | 198 | 

Thuật toán tính toán`pairs = 2`, cho`2 * (100 - 1) = 198`, không còn dư vì`k`là chẵn. Kết quả khớp chính xác với mô phỏng từng bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý trong thời gian không đổi chỉ bằng các phép toán số học | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả 1000 truy vấn cũng được xử lý bằng một số thao tác cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    t = int(input())
    out = []
    
    for _ in range(t):
        a, b, k = map(int, input().split())
        pairs = k // 2
        res = pairs * (a - b)
        if k % 2 == 1:
            res += a
        out.append(str(res))
    
    return "\n".join(out)

# provided samples
assert run("""6
5 2 3
100 1 4
1 10 5
1000000000 1 6
1 1 1000000000
1 1 999999999
""") == """8
198
-17
2999999997
0
1"""

# custom cases
assert run("""1
1 1 1
""") == "1", "single jump"

assert run("""1
10 20 2
""") == "-10", "one full cycle negative net"

assert run("""1
7 3 1
""") == "7", "only first jump"

assert run("""1
100 100 5
""") == "100", "full cancellation with leftover"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1,1,1`|`1`| trường hợp tối thiểu, lẻ k | 
|`10,20,2`|`-10`| toàn bộ chu kỳ âm ròng | 
|`7,3,1`|`7`| chỉ nhảy một lần | 
|`100,100,5`|`100`| hủy với phần còn lại | 

## Vỏ cạnh 

Khi nào`k = 1`, bộ thuật toán`pairs = 0`và thêm vào`+a`. Đối với đầu vào`a = 7, b = 3, k = 1`, kết quả trở thành 7, khớp với định nghĩa vì chỉ xảy ra bước nhảy về phía trước đầu tiên. 

Khi`a = b`, mỗi cặp đầy đủ đóng góp bằng không. Vì`a = b = 10, k = 5`, chúng tôi nhận được`pairs = 2`, do đó đóng góp là 0 và bước lẻ còn lại sẽ cộng thêm`+10`, tạo ra 10. Giả định hủy đơn giản sẽ trả về 0 không chính xác. 

Khi nào`k`chẵn, không có phần dư và kết quả phụ thuộc hoàn toàn vào`(a - b)`. Vì`a = 5, b = 2, k = 4`, chúng tôi nhận được`2 * 3 = 6`, khớp với trình tự rõ ràng`5 - 2 + 5 - 2`.
