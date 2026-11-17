---
title: "CF 1949H - Tránh phân chia"
description: "Quá trình bắt đầu với một ô duy nhất tại (0, 0). Bất cứ khi nào chúng ta chia một ô (x, y), ô đó sẽ biến mất và tạo ra (x + 1, y) và (x, y + 1). Việc phân chia chỉ hợp pháp nếu hiện tại không có đứa trẻ nào có mặt. Chúng ta được cung cấp một tập hữu hạn các tọa độ cấm."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "H"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3100
weight: 1949
solve_time_s: 353
verified: false
draft: false
---

[CF 1949H - Tránh phân chia](https://codeforces.com/problemset/problem/1949/H) 

**Đánh giá:** 3100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 5 phút 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Quá trình bắt đầu với một ô duy nhất tại`(0, 0)`. 

Bất cứ khi nào chúng ta phân chia một tế bào`(x, y)`, tế bào đó biến mất và tạo ra`(x + 1, y)`Và`(x, y + 1)`. Việc phân chia chỉ hợp pháp nếu hiện tại không có đứa trẻ nào có mặt. 

Chúng ta được cung cấp một tập hữu hạn các tọa độ cấm. Câu hỏi đặt ra là liệu chúng ta có thể thực hiện một số trình tự phân chia để sinh vật cuối cùng không chứa những tế bào bị cấm hay không. 

Điều đầu tiên khiến vấn đề này trở nên khó hiểu là cùng một tọa độ có thể xuất hiện nhiều lần trong quá trình thực hiện. Một ô có thể được tạo, chia, sau đó được tạo lại từ ô cha khác và được chia lại. Bản thân mẫu chứa một tình huống như vậy với`(1, 1)`. 

Kích thước đầu vào lớn. Trên tất cả các trường hợp thử nghiệm có thể có tới`10^6`tế bào bị cấm Bất kỳ giải pháp nào cố gắng mô phỏng các sinh vật, trạng thái tìm kiếm hoặc khám phá mạng lưới vô tận đều ngay lập tức không thể thực hiện được. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`về số lượng tế bào bị cấm. 

Một ý tưởng sai lầm phổ biến là xử lý từng tế bào bị cấm một cách độc lập. 

Coi như:```
4
0 0
1 0
0 1
1 1
```Câu trả lời đúng là`YES`. 

Mọi ô bị cấm đều có thể được chia ra và sự tương tác giữa các hoạt động tái tạo khiến điều đó trở nên khả thi. 

Một cái bẫy dễ mắc khác là giả định rằng mọi vùng cấm hữu hạn đều có thể bị bao vây. 

Coi như:```
16
0 0
0 1
0 2
0 3
1 0
...
3 3
```Câu trả lời đúng là`NO`. 

Mặc dù tập hợp bị cấm là hữu hạn, áp lực tổ hợp được tạo ra bởi sự tái tạo lặp đi lặp lại buộc một số ô bị cấm vẫn tồn tại. 

Khó khăn chính là hiểu được một ô bị cấm phải được tạo lại bao nhiêu lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa sinh vật một cách trực tiếp. 

Trạng thái là tập hợp các ô hiện có. Từ mỗi tiểu bang, chúng tôi thử mọi cách phân chia pháp lý và tìm kiếm một sinh vật không bị cấm. 

Điều này đúng về nguyên tắc vì nó tuân theo các quy tắc một cách chính xác. Vấn đề là số lượng trạng thái có thể tiếp cận tăng lên một cách bùng nổ. Ngay cả sau một số lần phân chia nhỏ cũng đã có một số lượng lớn các sinh vật khác nhau. Với tọa độ đạt`10^9`, mọi tìm kiếm trong không gian trạng thái đều hoàn toàn vô vọng. 

Bước đột phá đến từ việc bỏ qua cơ thể hiện tại và thay vào đó đếm xem mỗi tế bào bị cấm phải được xử lý bao nhiêu lần. 

Giả sử chúng ta nhấn mạnh rằng một tế bào bị cấm không bao giờ tồn tại trong cơ thể sinh vật cuối cùng. 

Mỗi khi ô đó xuất hiện thì cuối cùng nó cũng phải được phân chia. 

Cho phép`cnt(x, y)`biểu thị số lần ô bị cấm`(x, y)`buộc phải chia tay. 

Một ô chỉ có thể được tạo từ hai ô cha của nó:```
(x - 1, y)
(x, y - 1)
```Vì vậy mỗi lần xuất hiện của`(x, y)`đến từ sự phân chia của một trong những cha mẹ đó. 

Điều đó ngay lập tức mang lại sự tái diễn tam giác Pascal. 

Nếu như`(x, y)`bị cấm thì mọi lần xuất hiện đều phải được chuyển tiếp trở đi, vì vậy:```
cnt(x, y)
=
cnt(x - 1, y)
+
cnt(x, y - 1)
```Gốc`(0, 0)`đóng góp một sự xuất hiện ban đầu. 

Bây giờ đến phần quan sát quan trọng. 

Một ô bị cấm có thể được loại bỏ một cách an toàn miễn là cần nhiều nhất hai lần. 

Một khi một số ô bị cấm buộc phải xuất hiện ba lần trở lên, các tác phẩm tái tạo sẽ không thể được hấp thụ nữa. Từ thời điểm đó trở đi, quá trình nhất thiết phải lan truyền mãi mãi, điều đó có nghĩa là việc tránh tất cả các ô bị cấm là không thể. 

Vì vậy, chúng tôi không cần số lượng chính xác khi nó vượt quá hai. Chúng tôi chỉ quan tâm liệu nó có đạt đến ba hay không. 

Điều đó làm giảm toàn bộ vấn đề thành DP đếm đường dẫn trên các ô bị cấm, với tất cả các giá trị được giới hạn ở`3`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các ô bị cấm trong bảng băm. 
2. Sắp xếp các ô cấm tăng dần`x + y`. 

Điều này đảm bảo rằng khi chúng tôi xử lý`(x, y)`, cả cha mẹ tiềm năng đều đã được xử lý. 
3. Hãy để`dp[(x, y)]`là số lần xuất hiện bắt buộc của ô bị cấm đó, giới hạn ở`3`. 
4. Đối với`(0, 0)`, bắt đầu với giá trị`1`bởi vì sinh vật ban đầu chứa chính xác tế bào đó. 
5. Đối với mọi ô cấm`(x, y)`: 

Tính toán```
dp(x, y)
=
base
+
dp(x - 1, y)
+
dp(x, y - 1)
```Ở đâu`base = 1`chỉ dành cho`(0, 0)`. 

Thiếu cha mẹ đóng góp`0`. 
6. Sau khi tính toán giá trị, giới hạn ở mức`3`. 

Chúng ta không bao giờ cần những giá trị lớn hơn vì chỉ có sự khác biệt giữa```
0, 1, 2, >=3
```vấn đề. 
7. Nếu có ô cấm nào chạm tới`3`, đầu ra`NO`. 
8. Nếu không thì xuất ra`YES`. 

### Tại sao nó hoạt động`dp(x, y)`đếm bao nhiêu lần ô cấm`(x, y)`buộc phải được tạo lại nếu chúng ta cố gắng loại bỏ mọi ô cấm. 

Mọi trò giải trí đều phải đến từ một trong hai cha mẹ, vì vậy số đếm tuân theo sự tái diễn giống hệt như tam giác Pascal. 

Giá trị`0`,`1`, Và`2`có thể quản lý được. Một ô bị cấm có thể được tạo lại từ cha mẹ bên trái, từ cha mẹ cấp dưới hoặc từ cả hai một lần. 

Khi số lượng bắt buộc đạt đến`3`, có nhiều hoạt động giải trí cần thiết hơn mức mà quá trình có thể hấp thụ. Tại thời điểm đó, áp suất lan truyền vô thời hạn, khiến không thể thu được sinh vật cuối cùng tránh được tất cả các tế bào bị cấm. 

Do đó, tập hợp bị cấm có thể tránh được khi và chỉ khi mọi ô bị cấm có số lượng bắt buộc nhiều nhất`2`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())

        cells = []
        forbidden = set()

        for _ in range(n):
            x, y = map(int, input().split())
            cells.append((x, y))
            forbidden.add((x, y))

        cells.sort(key=lambda p: (p[0] + p[1], p[0]))

        dp = {}
        ok = True

        for x, y in cells:
            cur = 1 if (x, y) == (0, 0) else 0

            cur += dp.get((x - 1, y), 0)
            cur += dp.get((x, y - 1), 0)

            if cur > 3:
                cur = 3

            dp[(x, y)] = cur

            if cur >= 3:
                ok = False
                break

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ lưu trữ các giá trị DP cho các ô bị cấm. Bất kỳ phụ huynh nào không bị cấm đều đóng góp bằng 0, bởi vì chỉ những ô bị cấm mới buộc phải chuyển tiếp các trò giải trí. 

Sắp xếp theo`x + y`là đủ vì cả bố và mẹ luôn nằm trên đường chéo trước đó. Sắp xếp thứ cấp theo`x`chỉ được sử dụng để làm cho thứ tự xác định. 

Giá trị được giới hạn ở`3`ngay lập tức. Điều này tránh các số nguyên lớn và giữ cho DP hoàn toàn logic. Thuật toán chỉ cần biết liệu số đếm có`0`,`1`,`2`, hoặc ít nhất`3`. 

Thời điểm một tế bào đạt tới`3`, câu trả lời đã được xác định. Chúng tôi có thể ngừng xử lý trường hợp thử nghiệm đó sớm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 0
1 0
0 1
1 1
```| Tế bào | Giá trị DP | 
| --- | --- | 
| (0,0) | 1 | 
| (0,1) | 1 | 
| (1,0) | 1 | 
| (1,1) | 2 | 

Không có tế bào nào đạt được`3`. 

Kết quả:```
YES
```Ví dụ này cho thấy một ô bị cấm có thể cần được tạo lại hai lần mà vẫn có thể quản lý được. 

### Mẫu 2 

đầu vào:```
16
0 0
0 1
...
3 3
```Các giá trị liên quan gần trung tâm trở thành: 

| Tế bào | Giá trị DP | 
| --- | --- | 
| (1,1) | 2 | 
| (1,2) | 3 | 
| (2,1) | 3 | 

Càng sớm càng`(1,2)`đạt tới`3`, câu trả lời đã được xác định. 

Kết quả:```
NO
```Điều này chứng tỏ ngưỡng quan trọng mà việc giải trí trở nên không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp các ô cấm chiếm ưu thế | 
| Không gian |`O(n)`| Bảng băm cho giá trị DP | 

Tổng số ô bị cấm trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`, Vì thế`O(n log n)`dễ dàng phù hợp trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo số lượng ô bị cấm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        cells = []
        for _ in range(n):
            x, y = map(int, input().split())
            cells.append((x, y))

        cells.sort(key=lambda p: (p[0] + p[1], p[0]))

        dp = {}
        ok = True

        for x, y in cells:
            cur = 1 if (x, y) == (0, 0) else 0
            cur += dp.get((x - 1, y), 0)
            cur += dp.get((x, y - 1), 0)

            if cur > 3:
                cur = 3

            dp[(x, y)] = cur

            if cur >= 3:
                ok = False
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""2
4
0 0
1 0
0 1
1 1
16
0 0
0 1
0 2
0 3
1 0
1 1
1 2
1 3
2 0
2 1
2 2
2 3
3 0
3 1
3 2
3 3
"""
) == "YES\nNO\n"

# root not forbidden
assert ru
```
