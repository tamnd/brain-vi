---
title: "CF 1282A - Tạm thời không có"
description: "Cho một người di chuyển dọc theo đoạn thẳng từ vị trí a đến vị trí b với vận tốc không đổi 1 đơn vị/phút. Độc lập, có một trạm gốc mạng ở vị trí c bao phủ mọi thứ trong khoảng cách r."
date: "2026-06-11T19:26:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1282
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 610 (Div. 2)"
rating: 900
weight: 1282
solve_time_s: 119
verified: true
draft: false
---

[CF 1282A - Tạm thời không khả dụng](https://codeforces.com/problemset/problem/1282/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một người di chuyển dọc theo một đoạn thẳng từ vị trí`a`để định vị`b`với tốc độ không đổi 1 đơn vị mỗi phút. Độc lập có trạm gốc mạng tại vị trí`c`bao gồm mọi thứ trong khoảng cách`r`. Trong khi người đó di chuyển, tại mọi thời điểm họ đều ở trong vùng phủ sóng hoặc ở ngoài vùng phủ sóng. Nhiệm vụ là tính toán tổng thời gian họ ở bên ngoài khu vực được bảo hiểm trong toàn bộ chuyến đi. 

Vì vận tốc bằng 1 nên thời gian bằng quãng đường đi được. Vì vậy, vấn đề giảm xuống còn việc đo lường bao nhiêu phân khúc từ`a`ĐẾN`b`nằm ngoài khoảng phủ sóng. 

Quan sát hình học quan trọng là vùng phủ sóng trên trục số chính là một đoạn có tâm ở`c`, mở rộng`r`đơn vị theo cả hai hướng. Vì vậy vùng được phủ là khoảng`[c - r, c + r]`. Chuyển động cũng là một khoảng nhưng nó có thể sang trái hoặc sang phải tùy theo`a < b`hoặc`a > b`. 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với tọa độ có độ lớn lên tới 10^8. Điều này ngay lập tức gợi ý rằng cần phải có O(1) cho mỗi giải pháp thử nghiệm, vì mọi thứ liên quan đến quét hoặc mô phỏng trên khoảng cách đều không thể thực hiện được. 

Một cách giải thích ngây thơ sẽ mô phỏng chuyển động từng phút hoặc từng đơn vị, nhưng phạm vi tọa độ khiến điều này không khả thi. Ngay cả một trường hợp thử nghiệm cũng có thể kéo dài tới 2⋅10^8 bước. 

Các trường hợp cạnh thường phá vỡ các cách tiếp cận không chính xác bao gồm hướng đảo ngược (khi`a > b`), bảo hiểm đầy đủ (toàn bộ phân khúc bên trong`[c-r, c+r]`) và không có sự trùng lặp nào cả (phân đoạn hoàn toàn tách rời khỏi vùng phủ sóng). Một trường hợp khó phát hiện khác là khi đoạn này chỉ chồng lấp một phần khoảng thời gian phủ sóng, đòi hỏi phải xử lý cẩn thận các điểm cuối giao lộ. 

Ví dụ, nếu`a = 1, b = 10, c = 7, r = 1`, phạm vi bảo hiểm là`[6, 8]`. Đoạn đã đi chỉ chồng chéo trong`[6, 8]`, vì vậy các phần không được che chắn là`[1, 6]`Và`[8, 10]`, cho tổng chiều dài không che chắn là 7. Một sai lầm ở đây là quên kẹp chồng lên nhau đúng cách hoặc cho rằng hướng không quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng chuyển động của Polycarp ở độ phân giải đơn vị, kiểm tra từng điểm nguyên hoặc từng bước nhỏ xem nó có nằm bên trong không.`[c - r, c + r]`. Điều này hoạt động về mặt khái niệm vì mức độ phù hợp rất dễ kiểm tra trên mỗi vị trí. Tuy nhiên, độ dài đường dẫn có thể lên tới 2⋅10^8 cho mỗi lần kiểm tra và với tối đa 1000 lần kiểm tra, điều này hoàn toàn không khả thi. 

Cấu trúc của vấn đề loại bỏ sự cần thiết phải mô phỏng. Cả chuyển động và vùng phủ sóng đều là những khoảng thời gian liên tục trên một đường. Thay vì theo dõi vị trí theo thời gian, chúng tôi chỉ cần tính toán xem phân khúc du lịch giao với phân khúc phủ sóng bao nhiêu. Một khi chúng ta nhận ra điều này, bài toán sẽ trở thành một bài toán giao khoảng thuần túy. 

Chúng tôi tính toán sự chồng chéo giữa khoảng thời gian di chuyển`[min(a,b), max(a,b)]`và khoảng thời gian phủ sóng`[c-r, c+r]`. Độ dài của sự chồng chéo này là thời gian dành cho phạm vi phủ sóng. Trừ đi nó khỏi tổng khoảng cách di chuyển sẽ cho biết thời gian sử dụng bên ngoài vùng phủ sóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(khoảng cách) | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại mọi thứ theo khoảng thời gian. 

1. Chuyển chuyển động thành đoạn chuẩn hóa`[l1, r1]`Ở đâu`l1 = min(a, b)`Và`r1 = max(a, b)`. Điều này loại bỏ hoàn toàn hướng nên chúng ta chỉ xử lý độ dài. 
2. Chuyển đổi phạm vi bảo hiểm thành`[l2, r2]`Ở đâu`l2 = c - r`Và`r2 = c + r`. Điều này đại diện cho tất cả các điểm có tín hiệu. 
3. Tính giao điểm của hai đoạn này bằng cách lấy`left = max(l1, l2)`Và`right = min(r1, r2)`. 
4. Nếu`right > left`, giao lộ có độ dài dương nên thời gian phủ sóng là`right - left`. Nếu như`right <= left`, không có sự chồng chéo và thời gian phủ sóng bằng không. 
5. Tính tổng thời gian đi lại như sau`r1 - l1`, sau đó trừ đi thời gian che phủ để có thời gian che phủ. 

Lý do điều này có tác dụng là vì chuyển động ở tốc độ 1 biến thời gian thành chiều dài hình học. Vấn đề được rút gọn thành việc đo xem khoảng này nằm ngoài khoảng khác bao nhiêu, chính xác bằng tổng chiều dài trừ đi chiều dài giao điểm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, r = map(int, input().split())

    l1, r1 = min(a, b), max(a, b)
    l2, r2 = c - r, c + r

    left = max(l1, l2)
    right = min(r1, r2)

    overlap = max(0, right - left)
    total = r1 - l1

    print(total - overlap)
```Mã trực tiếp thực hiện chế độ xem theo khoảng thời gian của vấn đề. Bước đầu tiên bình thường hóa hướng, giúp tránh bất kỳ vỏ bọc đặc biệt nào cho`a > b`. Bước thứ hai xây dựng khoảng thời gian phủ sóng một cách rõ ràng. 

Logic giao lộ dựa trên công thức tiêu chuẩn cho các đoạn chồng chéo. các`max(0, right - left)`đảm bảo rằng các khoảng không chồng chéo đóng góp bằng 0, tránh độ dài âm. 

Cuối cùng, trừ đi sự chồng chéo từ tổng số sẽ cho ra chính xác thời gian dành cho phạm vi phủ sóng bên ngoài. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = 1, b = 10, c = 7, r = 1`| Bước | l1,r1 | l2,r2 | trái | đúng | chồng chéo | tổng cộng | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | [1,10] | [6,8] | - | - | - | - | - | 
| tính toán | [1,10] | [6,8] | 6 | 8 | 2 | 9 | 7 | 

Sự chồng chéo`[6,8]`đóng góp 2 đơn vị thời gian được bảo hiểm. Toàn bộ hành trình dài 9 đơn vị nên thời gian chưa khám phá là 7, phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:`a = 8, b = 2, c = 10, r = 4`| Bước | l1,r1 | l2,r2 | trái | đúng | chồng chéo | tổng cộng | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | [2,8] | [6,14] | - | - | - | - | - | 
| tính toán | [2,8] | [6,14] | 6 | 8 | 2 | 6 | 4 | 

Đoạn này chồng lên một phần phạm vi phủ sóng từ 6 đến 8. Tổng hành trình là 6, do đó thời gian không che phủ là 4. 

Những ví dụ này xác nhận rằng việc đảo hướng và chồng chéo một phần được xử lý thống nhất bằng cách chuẩn hóa khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm và mỗi trường hợp được xử lý trong thời gian không đổi, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        a, b, c, r = map(int, input().split())
        l1, r1 = min(a, b), max(a, b)
        l2, r2 = c - r, c + r
        overlap = max(0, min(r1, r2) - max(l1, l2))
        print((r1 - l1) - overlap)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""9
1 10 7 1
3 3 3 0
8 2 10 4
8 2 10 100
-10 20 -17 2
-3 2 2 0
-3 1 2 0
2 3 2 3
-1 3 -2 2
""") == """7
0
4
0
30
5
4
0
3"""

# custom cases
assert run("""1
0 10 5 0
""") == "10", "no coverage anywhere"

assert run("""1
0 10 5 100
""") == "0", "full coverage"

assert run("""1
5 5 0 0
""") == "0", "zero-length movement"

assert run("""1
-5 5 0 2
""") == "6", "partial symmetric overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có vùng phủ sóng ở khắp mọi nơi | 10 | khoảng rời rạc | 
| bảo hiểm đầy đủ | 0 | ngăn chặn hoàn toàn | 
| chuyển động bằng không | 0 | đoạn thoái hóa | 
| chồng chéo đối xứng | 6 | đúng giao lộ một phần | 

## Vỏ cạnh 

Khi nào`a == b`, khoảng chuyển động thu gọn về một điểm duy nhất. Trong trường hợp này`r1 - l1`bằng 0, do đó câu trả lời tự động bằng 0 bất kể phạm vi bao phủ. Đối với đầu vào`a = 5, b = 5, c = 0, r = 0`, chế độ xem khoảng thời gian cho`[5,5]`Và`[0,0]`, tạo ra sự chồng chéo bằng 0 và tổng chiều dài bằng 0, khớp với kết quả chính xác. 

Khi không có sự chồng chéo nào cả, chẳng hạn như`a = 0, b = 10, c = 100, r = 1`, tính toán giao lộ mang lại`min(r1,r2) < max(l1,l2)`, do đó sự chồng chéo trở thành số không. Câu trả lời giảm xuống độ dài toàn bộ đoạn là 10. 

Khi phạm vi bao phủ chứa đầy đủ phân đoạn chuyển động, chẳng hạn như`a = 2, b = 5, c = 3, r = 10`, giao điểm bằng toàn bộ đoạn, do đó phần chồng chéo bằng tổng chiều dài và kết quả trở thành 0.
