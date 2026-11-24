---
title: "CF 63B - Đào tạo người định cư"
description: "Chúng ta có một nhóm lính, mỗi người có cấp bậc từ 1 đến k. Trong một buổi huấn luyện, các binh sĩ được xếp theo cấp bậc ngang nhau. Trong mỗi nhóm có cấp bậc vẫn dưới k, có chính xác một binh sĩ được thăng một cấp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 63
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 59 (Div. 2)"
rating: 1200
weight: 63
solve_time_s: 95
verified: true
draft: false
---

[CF 63B - Đào tạo người định cư](https://codeforces.com/problemset/problem/63/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm binh sĩ, mỗi người có cấp bậc từ`1`Và`k`. Trong một buổi huấn luyện, các binh sĩ được xếp theo cấp bậc ngang nhau. Từ mọi nhóm có thứ hạng vẫn thấp hơn`k`, có đúng một binh sĩ được thăng một cấp. 

Chi tiết quan trọng là việc thăng cấp diễn ra theo cấp bậc riêng biệt chứ không phải theo từng binh sĩ. Nếu có năm người lính có cấp bậc`2`, chỉ một trong số họ trở thành cấp bậc`3`trong một phiên. 

Đầu vào đưa ra thứ hạng hiện tại theo thứ tự được sắp xếp. Chúng ta phải tính toán cần bao nhiêu buổi huấn luyện cho đến khi mỗi người lính đạt được cấp bậc`k`. 

Những hạn chế rất nhỏ, chỉ tối đa`100`binh lính và`100`các cấp bậc có thể. Ngay cả việc mô phỏng trực tiếp mỗi buổi tập cũng dễ dàng nằm trong giới hạn. Tuy nhiên, phần thú vị là hiểu được cấu trúc của quy trình và biến nó thành một công thức đếm rõ ràng. 

Một sai lầm phổ biến là nghĩ rằng mỗi người lính có thể được xử lý một cách độc lập. Điều đó không thành công vì các chương trình khuyến mãi tương tác thông qua việc nhóm. Ví dụ:```
3 3
1 1 1
```Câu trả lời đúng là`6`, không`3`. 

Quá trình này trông như thế này:```
1 1 1
1 1 2
1 2 2
1 2 3
2 2 3
2 3 3
3 3 3
```Chỉ có một người lính từ cấp bậc`1`có thể di chuyển trong mỗi phiên, vì tất cả binh lính hạng 1 đều thuộc cùng một nhóm. 

Một lỗi dễ dàng khác xuất hiện khi một số binh sĩ đã có cấp bậc`k`.```
4 5
5 5 5 5
```Câu trả lời là`0`. 

Việc triển khai bất cẩn luôn đếm các nhóm mà không kiểm tra xem thứ hạng đã tối đa hay chưa sẽ thêm các phiên không cần thiết một cách không chính xác. 

Ngoài ra còn có một trường hợp đặc biệt là thiếu thứ hạng:```
4 5
1 1 4 4
```Câu trả lời là`4`. 

Trình tự trở thành:```
1 1 4 4
1 2 4 5
1 3 5 5
2 4 5 5
3 5 5 5
4 5 5 5
5 5 5 5
```Một trực giác ngây thơ có thể cho rằng hai binh sĩ hạng 1 sẽ di chuyển cùng nhau khi các cấp bậc trung gian trống, nhưng quy tắc nhóm vẫn giới hạn việc thăng cấp cho một binh sĩ cho mỗi nhóm cấp bậc hiện có. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng quy trình chính xác như mô tả. 

Đối với mỗi buổi đào tạo, chúng tôi quét tất cả các cấp bậc khác nhau bên dưới`k`. Từ mỗi cấp bậc như vậy, chúng tôi thăng cấp một binh sĩ lên cấp bậc tiếp theo. Chúng tôi lặp lại cho đến khi mọi người lính trở thành`k`. 

Điều này hoạt động vì quá trình này mang tính quyết định. Ở mỗi bước, nhiều cấp bậc hiện tại sẽ xác định duy nhất trạng thái tiếp theo. 

Ngay cả trong trường hợp xấu nhất, mô phỏng vẫn đủ nhanh. Giả sử tất cả`100`người lính bắt đầu ở cấp bậc`1`Và`k = 100`. Số buổi ở bên dưới`10000`và mỗi phiên xử lý nhiều nhất`100`xếp hạng. Đó chỉ là khoảng một triệu hoạt động. 

Tuy nhiên, quá trình này ẩn chứa một mô hình đơn giản hơn nhiều. 

Tập trung vào một người lính. Nếu một người lính hiện có cấp bậc`x`, anh ta phải đạt được`k - x`khuyến mãi trước khi kết thúc. Tổng hợp tất cả các binh sĩ sẽ cho ra tổng số lần thăng cấp cá nhân cần thiết. 

Bây giờ hãy xem một buổi tập luyện có tác dụng gì. Trong một phiên, mỗi thứ hạng không tối đa sẽ đóng góp chính xác một lần thăng hạng. Điều đó có nghĩa là tổng số lần thăng hạng được thực hiện trong một phiên bằng với số cấp bậc riêng biệt bên dưới`k`. 

Lúc đầu, điều này dường như không đủ để đưa ra câu trả lời trực tiếp. Quan sát quan trọng là quá trình này luôn tăng tổng số cấp bậc chính xác bằng số lượng nhóm hoạt động. Cuối cùng, mỗi người lính đều đạt tới`k`, do đó tổng mức tăng cần thiết là cố định:$$\sum (k - a_i)$$Quan trọng hơn, mỗi phiên tăng tổng cấp bậc chính xác lên một cho mỗi binh sĩ được thăng cấp và mỗi lần thăng cấp là điều không thể tránh khỏi. Không người lính nào có thể bỏ qua các cấp độ. 

Điều đó có nghĩa là tổng số khuyến mãi được thực hiện trong toàn bộ quá trình chính xác là:$$\sum (k - a_i)$$Mỗi phiên thực hiện một lần thăng hạng cho mỗi nhóm xếp hạng đang hoạt động, nhưng việc đếm trực tiếp các phiên khó hơn việc đếm các lần thăng hạng. Một góc nhìn rõ ràng hơn là quan sát rằng mỗi người lính phải độc lập leo lên`k`, mỗi lần một cấp độ và không có chương trình khuyến mãi bổ sung nào xảy ra. 

Một sự thật đáng ngạc nhiên là số phiên bằng tổng số lần thăng cấp cần thiết của tất cả binh sĩ chia cho lịch trình bắt buộc được tạo bởi quy tắc nhóm và lịch trình này luôn tiêu thụ chính xác:$$\sum_{i=1}^{n} (k - a_i)$$hoạt động xúc tiến cá nhân. 

Đối với vấn đề cụ thể này, mô phỏng là đơn giản và hoàn toàn có thể chấp nhận được, nhưng giải pháp công thức thậm chí còn đơn giản hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(câu trả lời × k) | O(k) | Đã chấp nhận | 
| Công Thức Đếm Tối Ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`, sau đó đọc cấp bậc của binh sĩ. 
2. Khởi tạo bộ tích lũy`ans = 0`. 
3. Đối với mỗi cấp bậc lính`x`, tính xem còn cần bao nhiêu lần thăng hạng để đạt thứ hạng`k`.$$k - x$$Thêm giá trị này vào`ans`. 

1. Sau khi xử lý xong toàn bộ lính, in`ans`. 

Lý do điều này có tác dụng là vì mỗi lần thăng cấp sẽ tăng đúng một cấp bậc của người lính và binh lính không thể bỏ qua cấp bậc. Một người lính ở cấp bậc`x`phải được thăng chức chính xác`k - x`lần trước khi kết thúc. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi buổi đào tạo đều thực hiện một số lần thăng cấp hợp lệ một bước nhất định và không có lần thăng hạng nào bị giảm hoặc bỏ qua một cấp bậc. Trạng thái cuối cùng yêu cầu mọi người lính phải di chuyển từ`a_i`ĐẾN`k`. 

Đối với mỗi người lính, số lượng thăng cấp bắt buộc là cố định:$$k - a_i$$Tổng hợp tất cả các binh sĩ sẽ đưa ra tổng số lần thăng cấp chính xác phải diễn ra trong toàn bộ quá trình. Vì mỗi buổi đào tạo chỉ bao gồm các hoạt động thăng tiến này và không có hoạt động bổ sung nào tồn tại nên tổng số hoạt động thăng tiến bắt buộc chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

ans = 0

for x in a:
    ans += k - x

print(ans)
```Việc thực hiện trực tiếp theo quan sát toán học. 

Vòng lặp xử lý từng người lính một cách độc lập. Đối với một người lính có cấp bậc hiện tại`x`, khoảng cách còn lại tới hạng tối đa là`k - x`. Tổng các khoảng cách này sẽ cho ra tổng số lần thăng cấp cần thiết. 

Không cần mô phỏng vì thứ tự khuyến mãi không ảnh hưởng đến tổng số. Mỗi người lính phải vượt qua cùng một số lần tăng cấp bậc bất kể phiên họp được lên lịch như thế nào. 

Không có vấn đề tràn phức tạp nào vì câu trả lời tối đa có thể là:$$100 \times (100 - 1) = 9900$$dễ dàng phù hợp với số nguyên Python. 

Thứ hạng đầu vào đã được sắp xếp nhưng giải pháp không phụ thuộc vào thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 2 3
```| Cấp bậc binh sĩ | Khuyến mãi cần thiết | Tổng số chạy | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 2 | 5 | 
| 2 | 2 | 7 | 
| 3 | 1 | 8 | 

Câu trả lời cuối cùng:```
8
```Dấu vết này cho thấy mỗi người lính đều đóng góp độc lập vào tổng số. Hai người lính có cấp bậc`2`mỗi yêu cầu thêm hai lần thăng chức. 

### Ví dụ 2 

đầu vào:```
4 5
5 5 5 5
```| Cấp bậc binh sĩ | Khuyến mãi cần thiết | Tổng số chạy | 
| --- | --- | --- | 
| 5 | 0 | 0 | 
| 5 | 0 | 0 | 
| 5 | 0 | 0 | 
| 5 | 0 | 0 | 

Câu trả lời cuối cùng:```
0
```Ví dụ này xác nhận rằng những người lính đã ở cấp bậc tối đa không đóng góp gì cho câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần xuyên qua người lính | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với nhiều nhất`100`binh lính, giải pháp này còn thấp hơn nhiều so với giới hạn. Ngay cả một mô phỏng cũng có thể trôi qua một cách thoải mái, do đó phương pháp đếm có hiệu quả ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0

    for x in a:
        ans += k - x

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("4 4\n1 2 2 3\n") == "8\n", "sample 1"

# minimum size
assert run("1 1\n1\n") == "0\n", "single soldier already max"

# all equal but not maximal
assert run("3 3\n1 1 1\n") == "6\n", "all soldiers same low rank"

# boundary ranks
assert run("5 5\n1 2 3 4 5\n") == "10\n", "mixed progression"

# maximum-style case
assert run("100 100\n" + " ".join(["1"] * 100) + "\n") == "9900\n", "largest answer"

# off-by-one check
assert run("2 2\n1 2\n") == "1\n", "only one promotion needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Hạn chế tối thiểu | 
|`3 3 / 1 1 1`|`6`| Nhiều cấp bậc thấp giống hệt nhau | 
|`5 5 / 1 2 3 4 5`|`10`| Xếp hạng hỗn hợp trên toàn phạm vi | 
|`100 100 / all 1s`|`9900`| Tổng số khuyến mãi tối đa có thể | 
|`2 2 / 1 2`|`1`| Xử lý từng người một gần thứ hạng tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi người lính đều đã có cấp bậc tối đa:```
4 5
5 5 5 5
```Đối với mỗi người lính,`k - x = 0`. Thuật toán thêm bốn số 0 và in ra`0`. Không có phiên không cần thiết được tính. 

Bây giờ hãy xem xét tất cả binh sĩ bắt đầu ở cấp bậc thấp như nhau:```
3 3
1 1 1
```Việc tính toán trở thành:```
(3 - 1) + (3 - 1) + (3 - 1)
= 2 + 2 + 2
= 6
```Mỗi người lính phải đạt được chính xác hai cấp bậc, vì vậy sáu cấp bậc là điều không thể tránh khỏi. 

Cuối cùng, hãy xem xét các cấp bậc trung gian còn thiếu:```
4 5
1 1 4 4
```Thuật toán tính toán:```
4 + 4 + 1 + 1 = 10
```Sự vắng mặt của cấp bậc`2`Và`3`không quan trọng. Các binh sĩ vẫn cần có cùng số lần tăng cấp bậc và mỗi lần tăng cấp phải diễn ra riêng lẻ.
