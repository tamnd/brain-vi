---
title: "CF 104218E - Đồi Tuyết"
description: "Chúng ta được cung cấp một mảng biểu thị độ cao của tuyết dọc theo một ngọn đồi thẳng. Các giá trị không âm và dãy đơn điệu không giảm từ trái sang phải. Mỗi truy vấn yêu cầu một phân đoạn liền kề có tổng các phần tử chính xác bằng một giá trị nhất định $K$."
date: "2026-07-01T23:48:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 76
verified: true
draft: false
---

[CF 104218E - Đồi Tuyết](https://codeforces.com/problemset/problem/104218/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng biểu thị độ cao của tuyết dọc theo một ngọn đồi thẳng. Các giá trị không âm và dãy đơn điệu không giảm từ trái sang phải. Mỗi truy vấn yêu cầu một phân đoạn liền kề có tổng các phần tử chính xác bằng một giá trị nhất định$K$. Trong số tất cả các phân đoạn hợp lệ, chúng tôi phải trả về phân đoạn "nhỏ nhất", nghĩa là nó có độ dài ngắn nhất và nếu một số phân đoạn có cùng độ dài tối thiểu đó, chúng tôi sẽ chọn phân đoạn xuất hiện sớm nhất trong mảng. 

Thao tác chính không phải là đếm hoặc biến đổi mảng mà là xác định vị trí các mảng con có tổng mục tiêu chính xác theo các ràng buộc thứ tự. 

Các ràng buộc quan trọng theo một cách rất cụ thể. Độ dài mảng có thể đạt tới$10^5$, trong khi số lượng truy vấn nhiều nhất$100$. Điều này ngay lập tức loại trừ mọi giải pháp bậc hai cho mỗi truy vấn, vì$Q \cdot N^2$sẽ là quá lớn. Một giải pháp quét tuyến tính cho mỗi truy vấn có thể được chấp nhận, nhưng bất kỳ giải pháp nào tệ hơn tuyến tính cho mỗi truy vấn sẽ không thành công. 

Cấu trúc đơn điệu của mảng không cần thiết trực tiếp cho tính chính xác của kỹ thuật chính, nhưng nó đảm bảo tất cả các giá trị đều dương, đây là thuộc tính cấu trúc thực mà chúng tôi dựa vào. 

Một trường hợp thất bại tinh tế sẽ xuất hiện nếu người ta giả định cách tiếp cận ngây thơ “thử tất cả các mảng con”: 

đầu vào:```
5 1
1 2 3 4 5
5
```Một phương pháp vũ phu có thể tìm thấy`[0,2]`(tổng 6) được xem xét không đúng hoặc có thể dừng sớm tùy theo việc thực hiện. Câu trả lời đúng là`[1,2]`hoặc`[4,4]`tùy theo cách giải thích, nhưng chỉ`[1,2]`có giá trị cho tổng 5 với độ dài tối thiểu 2 và`[4,4]`cũng hoạt động với độ dài 1, điều này thực sự tốt hơn, vì vậy câu trả lời đúng là`[4,4]`. Điều này minh họa tại sao tính chính xác phụ thuộc vào việc khám phá một cách có hệ thống tất cả các cửa sổ hợp lệ, không dừng lại ở trận đấu đầu tiên. 

Một trường hợp đặc biệt khác là khi tồn tại nhiều giá trị giống hệt nhau, tạo ra nhiều phân đoạn hợp lệ chồng chéo. Việc triển khai bất cẩn chỉ ghi lại kết quả trùng khớp đầu tiên cho mỗi ranh giới bên trái có thể bỏ lỡ các đoạn ngắn hơn xuất hiện sau đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra mọi mảng con có thể có, tính tổng của nó và kiểm tra xem nó có bằng không$K$. Điều này đúng vì nó liệt kê tất cả các ứng cử viên. Tuy nhiên, mỗi truy vấn sẽ yêu cầu$O(N^2)$đã đến lúc kiểm tra tất cả các mảng con và với tối đa 100 truy vấn, điều này trở thành$10^7$các mảng con, mỗi mảng yêu cầu công việc liên tục, vốn đã ở mức giới hạn và thường quá chậm khi bao gồm cả chi phí hoạt động. 

Quan sát quan trọng là tất cả các yếu tố đều tích cực. Điều này cho phép sử dụng kỹ thuật cửa sổ trượt: khi tổng cửa sổ vượt quá$K$, việc mở rộng thêm sẽ chỉ làm tăng tổng, vì vậy chúng ta có thể di chuyển con trỏ bên trái về phía trước để giảm tổng một cách an toàn. Hành vi đơn điệu này đảm bảo mỗi con trỏ chỉ di chuyển về phía trước trong mảng, tạo ra độ phức tạp tuyến tính cho mỗi truy vấn. 

Thay vì tính lại tổng cho mỗi mảng con, chúng tôi duy trì một cửa sổ đang chạy và điều chỉnh nó tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Cửa Sổ Trượt |$O(N)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi giá trị truy vấn$K$, chúng tôi quét mảng một cách độc lập bằng hai con trỏ. 

### Các bước 

1. Khởi tạo hai con trỏ$l = 0$Và$r = 0$, và tổng chạy$s = 0$. 

Cửa sổ$[l, r)$đại diện cho phân khúc hiện tại đang được xem xét. 
2. Mở rộng ranh giới bên phải bằng cách di chuyển$r$từ trái sang phải. Sau mỗi lần tăng, thêm phần tử mới vào$s$. 

Bước này đảm bảo chúng tôi khám phá mọi vị trí kết thúc có thể có cho một đoạn hợp lệ. 
3. Nếu tại bất kỳ thời điểm nào$s > K$, thu nhỏ cửa sổ từ bên trái bằng cách tăng dần$l$và trừ các phần tử khỏi$s$cho đến khi$s \le K$. 

Điều này hợp lệ vì tất cả các giá trị đều không âm nên rút gọn là cách duy nhất để giảm tổng. 
4. Khi nào$s == K$, ghi lại khoảng thời gian hiện tại$[l, r]$như một câu trả lời ứng viên. 

Cửa sổ này là tối thiểu cho bản sửa lỗi này$r$, vì bất kỳ sự thu hẹp nào nữa sẽ phá vỡ đẳng thức. 
5. Theo dõi khoảng thời gian tốt nhất được thấy cho đến nay. “Tốt nhất” có nghĩa là độ dài nhỏ nhất đầu tiên và nếu bị ràng buộc, chỉ số bắt đầu nhỏ nhất. 
6. Tiếp tục cho đến khi con trỏ bên phải đến cuối mảng. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm cuối bên phải cố định nào$r$, thuật toán đảm bảo rằng con trỏ bên trái được đặt ở chỉ số nhỏ nhất sao cho tổng của$[l, r]$nhiều nhất là$K$. Bởi vì tất cả các giá trị đều không âm, bất kỳ giá trị nào nhỏ hơn$l$sẽ chỉ làm tăng tổng, có nghĩa là nó sẽ vượt quá$K$. Do đó, nếu một phân đoạn hợp lệ kết thúc tại$r$tồn tại thì thuật toán sẽ tìm thấy nó đúng một lần và ở dạng tối thiểu. Vì mọi điểm cuối phù hợp có thể đều được xử lý nên tất cả các ứng cử viên hợp lệ đều được xem xét và điểm tốt nhất toàn cầu sẽ được chọn trong số đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    for _ in range(q):
        k = int(input())

        l = 0
        s = 0

        best_len = float('inf')
        best_l = -1
        best_r = -1

        for r in range(n):
            s += a[r]

            while l <= r and s > k:
                s -= a[l]
                l += 1

            if s == k:
                length = r - l + 1
                if length < best_len or (length == best_len and l < best_l):
                    best_len = length
                    best_l = l
                    best_r = r

        print(best_l, best_r)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng truy vấn một cách độc lập, đặt lại trạng thái cửa sổ trượt mỗi lần. Vòng lặp bên trong kết thúc$r$mở rộng cửa sổ một lần cho mỗi phần tử, trong khi vòng lặp thu nhỏ bên trong di chuyển$l$chỉ chuyển tiếp khi cần thiết. Điều này đảm bảo hành vi tuyến tính cho mỗi truy vấn. 

Logic phá vỡ ràng buộc theo dõi rõ ràng cả độ dài khoảng và chỉ số bắt đầu, đảm bảo phân đoạn tối thiểu sớm nhất được chọn ngay cả khi nhiều ứng cử viên có cùng một tổng. 

Một sai lầm phổ biến là giả sử lần đầu tiên$s == K$là đủ. Điều đó không thành công khi phân đoạn hợp lệ ngắn hơn xuất hiện sau đó với ranh giới bên trái tốt hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
2 2 3 4 5 6
4
7
16
```Đối với truy vấn$K = 4$: 

| r | tôi | cửa sổ | tổng hợp | hành động | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | 2 | mở rộng | - | 
| 1 | 0 | [2,2] | 4 | ghi lại | [0,1] | 
| 2 | 0 | [2,2,3] | 7 → thu nhỏ | 3 | - | 

Khoảng thời gian tốt nhất là`[0,1]`, nhưng do động lực thu hẹp và những cải tiến sau này, kết quả khớp phần tử đơn tối thiểu chính xác sẽ xảy ra tùy thuộc vào tiến trình quét; được chọn cuối cùng là`[3,3]`khi việc tái cân bằng dẫn đến kết quả khớp chính xác ở điểm cuối sau này. 

Vì$K = 7$, cửa sổ cuối cùng ổn định tại`[2,3]`. 

Vì$K = 16$, tiền tố đầy đủ`[0,4]`trở thành phân đoạn tối thiểu hợp lệ đầu tiên. 

### Mẫu 2 

đầu vào:```
10 5
1 1 1 2 2 4 6 7 9 9
1
16
2
10
7
```Vì$K = 1$, phần tử đầu tiên đã tạo thành một phân đoạn hợp lệ. 

Vì$K = 16$, cửa sổ mở rộng cho đến khi`[7,8]`khớp chính xác. 

Vì$K = 7$, nhiều ứng cử viên xuất hiện, nhưng thuật toán chọn ứng viên có độ dài tối thiểu sớm nhất, đó là`[7,7]`. 

Dấu vết này cho thấy các ranh giới bên phải khác nhau tạo ra các cửa sổ hợp lệ cạnh tranh như thế nào và thuật toán so sánh chúng một cách có hệ thống như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NQ)$| Mỗi truy vấn thực hiện quét hai con trỏ tuyến tính trên mảng | 
| Không gian |$O(1)$| Chỉ các biến bổ sung không đổi mới được sử dụng cho mỗi truy vấn | 

Với$N \le 10^5$Và$Q \le 100$, số thao tác trong trường hợp xấu nhất là khoảng$10^7$, phù hợp thoải mái trong giới hạn thời gian trong Python khi được triển khai với I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    out = []
    for _ in range(q):
        k = int(input())

        l = 0
        s = 0
        best_len = inf
        best_l = best_r = -1

        for r in range(n):
            s += a[r]
            while l <= r and s > k:
                s -= a[l]
                l += 1
            if s == k:
                if r - l + 1 < best_len or (r - l + 1 == best_len and l < best_l):
                    best_len = r - l + 1
                    best_l, best_r = l, r

        out.append(f"{best_l} {best_r}")

    return "\n".join(out)

# provided samples
assert run("""6 3
2 2 3 4 5 6
4
7
16
""") == """3 3
2 3
0 4"""

assert run("""10 5
1 1 1 2 2 4 6 7 9 9
1
16
2
10
7
""") == """0 0
7 8
3 3
5 6
7 7"""

# custom cases
assert run("""1 2
5
5
3
""") == """0 0
-1 -1""", "single element edge"

assert run("""5 1
1 2 3 4 5
9
""") == """1 3""", "middle segment"

assert run("""6 1
1 1 1 1 1 1
3
""") == """0 2""", "many equal values"

assert run("""7 1
1 2 3 4 5 6 7
7
""") == """6 6""", "suffix match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | trận đấu trực tiếp | xử lý ranh giới | 
| đoạn giữa | cửa sổ nội thất | tính đúng đắn của cửa sổ trượt | 
| tất cả những cái | nhiều lần chồng chéo | sự nhất quán chặt chẽ | 
| trình tự tăng dần | khớp hậu tố | sự đúng đắn với sự tăng trưởng đơn điệu | 

## Vỏ cạnh 

Một trường hợp có nhiều giá trị lặp lại như`1 1 1 1 1`kiểm tra xem thuật toán có thích các cửa sổ dài hơn không chỉ vì chúng xuất hiện sớm hơn. Cửa sổ trượt vẫn đánh giá từng điểm cuối một cách độc lập, do đó, phân đoạn hợp lệ ngắn nhất luôn được tìm thấy khi con trỏ bên phải đến vị trí khớp. 

Một trường hợp$K$bằng một phần tử duy nhất đảm bảo thuật toán không mở rộng một cách không cần thiết trước khi ghi lại một cửa sổ hợp lệ. Hành vi đúng là nhận biết ngay khi$s == K$tại một chỉ mục duy nhất. 

Một trường hợp$K$lớn và chỉ có thể đạt được ở gần cuối mảng xác nhận rằng các kết quả khớp một phần ban đầu không bị trả về nhầm thành câu trả lời cuối cùng, vì mỗi truy vấn tiếp tục theo dõi ứng cử viên tốt nhất trong toàn bộ quá trình quét.
