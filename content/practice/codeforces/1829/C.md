---
title: "CF 1829C - Mr. Hoàn Mỹ"
description: "Victor có hai kỹ năng độc lập mà anh ấy muốn có được và mỗi cuốn sách anh ấy đọc được sẽ dạy anh ấy kỹ năng 1, kỹ năng 2, cả hai hoặc không có kỹ năng nào. Mỗi cuốn sách cũng có chi phí thời gian."
date: "2026-06-09T07:16:48+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1829
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 871 (Div. 4)"
rating: 800
weight: 1829
solve_time_s: 68
verified: true
draft: false
---

[CF 1829C - Mr. Perfectly Fine](https://codeforces.com/problemset/problem/1829/C) 

**Đánh giá:** 800 
**Tags:** bitmasks, tham lam, triển khai 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Victor có hai kỹ năng độc lập mà anh ấy muốn có được và mỗi cuốn sách anh ấy đọc được sẽ dạy anh ấy kỹ năng 1, kỹ năng 2, cả hai hoặc không có kỹ năng nào. Mỗi cuốn sách cũng có chi phí thời gian. Nhiệm vụ là chọn một bộ sách có kỹ năng tổng hợp bao gồm cả kỹ năng cần thiết đồng thời giảm thiểu tổng thời gian đọc. 

Mỗi cuốn sách về cơ bản là một “gói” nhỏ: nó đóng góp một mặt nạ bit trên hai bit và có tính phí. Chúng ta được phép chọn bất kỳ tập hợp con sách nào, nhưng chúng ta chỉ quan tâm đến sự kết hợp của các kỹ năng đã học được chứ không phải thứ tự hay sự lặp lại. Mục tiêu là đạt được mặt nạ đầy đủ`11`với tổng chi phí tối thiểu. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, với tổng số sách lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc thậm chí kết hợp theo cặp cho mọi trường hợp thử nghiệm một cách độc lập. Cách tiếp cận bậc hai hoặc hàm mũ sẽ không tồn tại. 

Một điểm tế nhị là việc đọc nhiều cuốn sách dạy các kỹ năng chồng chéo là được phép, nhưng không bao giờ có lợi nếu một cuốn sách lấn át cuốn khác cả về kỹ năng và chi phí. Một trường hợp khác là khi không có sự kết hợp nào có thể bao gồm cả hai kỹ năng mặc dù mỗi kỹ năng riêng lẻ tồn tại riêng biệt. 

Ví dụ: nếu chúng ta có sách:```
3 10
4 01
```câu trả lời là`7`, nhưng nếu cả hai cuốn sách chỉ đưa ra`00`, câu trả lời là`-1`mặc dù sách tồn tại. 

Một cạm bẫy khác là cho rằng chỉ một cuốn sách thôi cũng có thể đủ; giải pháp đúng phải xử lý rõ ràng trường hợp một cuốn sách đã có`11`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các tập hợp con của sách và tính toán sự kết hợp các kỹ năng cho từng tập hợp con. Mỗi lần kiểm tra tập hợp con là O(n) và có 2^n tập hợp con, điều này là không thể ngay cả với n = 30, chứ đừng nói đến 2e5. 

Chúng ta có thể giảm cấu trúc bằng cách nhận thấy rằng chỉ có bốn trạng thái kỹ năng có thể có:`00`,`01`,`10`, Và`11`. Điều này thu gọn vấn đề thành việc chọn tối đa hai cuốn sách kết hợp để tạo thành`11`, hoặc có thể là một cuốn sách đã tương đương`11`. 

Quan sát quan trọng là chúng ta chỉ cần: 

- cuốn sách rẻ nhất cung cấp`10`- cuốn sách rẻ nhất cung cấp`01`- cuốn sách rẻ nhất cung cấp`11`Bất kỳ giải pháp tối ưu nào cũng phải thuộc một trong hai loại. Hoặc chúng ta lấy một cái`11`cuốn sách, hoặc chúng tôi lấy một cuốn`10`cuốn sách và một`01`sách. Không có sự kết hợp nào khác cải thiện được điều này vì việc bổ sung thêm sách chỉ có thể làm tăng chi phí mà không cải thiện phạm vi bao phủ ngoài`11`. 

Do đó, vấn đề giảm xuống còn việc duy trì ba giá trị tối thiểu trong khi quét đầu vào một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n) | O(n) | Quá chậm | 
| Theo dõi ứng viên xuất sắc nhất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và giảm tất cả các cuốn sách thành một số ứng cử viên đại diện. 

1. Khởi tạo ba biến:`best_01`,`best_10`, Và`best_11`như vô cùng. Chúng theo dõi chi phí tối thiểu giữa các cuốn sách cung cấp từng mẫu kỹ năng. Chúng tôi chỉ quan tâm đến đại diện rẻ nhất của từng loại. 
2. Lặp lại tất cả các cuốn sách. Đối với mỗi cuốn sách, hãy kiểm tra chuỗi nhị phân của nó và phân loại nó thành một trong bốn loại. 
3. Nếu cuốn sách đưa ra`11`, cập nhật`best_11`với chi phí tối thiểu của nó. Điều này rất quan trọng vì chỉ một cuốn sách thôi cũng có thể giải quyết được vấn đề. 
4. Nếu cuốn sách đưa ra`01`, cập nhật`best_01`. 
5. Nếu cuốn sách đưa ra`10`, cập nhật`best_10`. 
6. Bỏ qua`00`sách vì chúng không bao giờ đóng góp vào bộ kỹ năng cuối cùng. 
7. Sau khi xử lý tất cả các cuốn sách, hãy tính câu trả lời tốt nhất có thể với mức tối thiểu: 

chi phí của`best_11`, và tổng`best_01 + best_10`. 
8. Nếu cả hai`best_11`là vô hạn và một trong hai`best_01`hoặc`best_10`là vô hạn thì không thể hình thành được`11`, do đó đầu ra`-1`. 

### Tại sao nó hoạt động 

Không gian trạng thái được chiếm giữ hoàn toàn bởi hai bit. Bất kỳ giải pháp nào sử dụng nhiều hơn hai cuốn sách đều có thể được nén: nếu nó sử dụng nhiều`10`sách, chỉ những thứ rẻ tiền nhất; tương tự cho`01`. Nếu trộn nhiều loại thì sự kết hợp cuối cùng vẫn giảm xuống còn một loại duy nhất`11`hoặc một`10`cộng một`01`. Do đó, chỉ theo dõi chi phí tối thiểu cho mỗi loại sẽ bảo toàn được tất cả các công trình tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

t = int(input())
for _ in range(t):
    n = int(input())

    best_01 = INF
    best_10 = INF
    best_11 = INF

    for _ in range(n):
        parts = input().split()
        cost = int(parts[0])
        s = parts[1]

        if s == "11":
            best_11 = min(best_11, cost)
        elif s == "10":
            best_10 = min(best_10, cost)
        elif s == "01":
            best_01 = min(best_01, cost)

    ans = min(best_11, best_01 + best_10)

    if ans >= INF:
        print(-1)
    else:
        print(ans)
```Mã này là sự thực hiện trực tiếp việc giảm ba ứng cử viên. Mỗi cuốn sách được phân loại theo thời gian không đổi. Quyết định cuối cùng chỉ so sánh hai khả năng về cấu trúc. 

Một lỗi triển khai phổ biến là quên khởi tạo các ứng viên ở một giá trị lớn, điều này sẽ cho phép các danh mục bị thiếu đóng góp bằng 0 một cách không chính xác. Một người khác đang điều trị`"00"`nếu có liên quan, điều này là không cần thiết và có thể đưa ra mức tối thiểu không chính xác nếu xử lý sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
2 00
3 10
4 01
4 00
```| Sách | Chi phí | Loại | tốt nhất_10 | tốt nhất_01 | tốt nhất_11 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 00 | INF | INF | INF | 
| 2 | 3 | 10 | 3 | INF | INF | 
| 3 | 4 | 01 | 3 | 4 | INF | 
| 4 | 4 | 00 | 3 | 4 | INF | 

Tính toán cuối cùng:`min(INF, 3 + 4) = 7`Điều này xác nhận thuật toán kết hợp chính xác một`10`và một`01`. 

### Ví dụ 2 

đầu vào:```
n = 3
5 11
8 01
7 10
```| Sách | Chi phí | Loại | tốt nhất_10 | tốt nhất_01 | tốt nhất_11 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 11 | INF | INF | 5 | 
| 2 | 8 | 01 | INF | 8 | 5 | 
| 3 | 7 | 10 | 7 | 8 | 5 | 

Tính toán cuối cùng:`min(5, 7 + 8) = 5`Điều này cho thấy tại sao trực tiếp`11`luôn là một ứng cử viên độc lập với các tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cuốn sách được xử lý một lần với phân loại và cập nhật O(1) | 
| Không gian | O(1) | Chỉ có ba biến vô hướng được duy trì | 

Tổng số sách trong các trường hợp thử nghiệm được giới hạn bởi 2e5, do đó quá trình quét tuyến tính nằm trong giới hạn thoải mái. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    INF = 10**18
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        best_01 = INF
        best_10 = INF
        best_11 = INF

        for _ in range(n):
            cost, s = input().split()
            cost = int(cost)

            if s == "11":
                best_11 = min(best_11, cost)
            elif s == "10":
                best_10 = min(best_10, cost)
            elif s == "01":
                best_01 = min(best_01, cost)

        ans = min(best_11, best_01 + best_10)
        out.append(str(ans if ans < INF else -1))

    return "\n".join(out)

# provided sample
assert run("""6
4
2 00
3 10
4 01
4 00
5
3 01
3 01
5 01
2 10
9 10
1
5 11
3
9 11
8 01
7 10
6
4 01
6 01
7 01
8 00
9 01
1 00
4
8 00
9 10
9 11
8 11
""") == """7
5
5
9
-1
8"""

# single book direct solve
assert run("""1
1
5 11
""") == "5"

# impossible case
assert run("""1
2
3 10
4 10
""") == "-1"

# must combine
assert run("""1
2
3 10
4 01
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| duy nhất 11 cuốn | 5 | giải pháp trực tiếp | 
| chỉ có một loại kỹ năng | -1 | phát hiện không thể | 
| Combo 10 + 01 | 7 | logic ghép nối đúng | 

## Vỏ cạnh 

Khi tất cả các cuốn sách đều`00`, thuật toán giữ tất cả các ứng cử viên ở vô cùng và đưa ra kết quả chính xác`-1`bởi vì không có sự kết hợp nào cải thiện trạng thái. 

Khi chỉ có một trong`01`hoặc`10`tồn tại, tổng trở thành không thể vì một số hạng vẫn là vô cùng, ngăn chặn câu trả lời hợp lệ sai. 

Khi tồn tại nhiều ứng cử viên tối ưu, thuật toán luôn giữ được chi phí tối thiểu do liên tục`min`cập nhật, đảm bảo tính chính xác ngay cả khi bị trùng lặp hoặc sắp xếp lại nhiều.
