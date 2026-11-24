---
title: "CF 71D - Solitaire"
description: "Chúng ta được phát một tấm bảng hình chữ nhật chứa đầy các quân bài. Kích thước bàn cờ nhiều nhất là $17 nhân 17$, nhưng tổng số quân bài được đặt không bao giờ vượt quá 52 vì bộ bài chỉ có 52 quân bài thông thường cộng với hai lá bài pha trò. Hình vuông 3×3 hợp lệ phải đáp ứng một trong hai thuộc tính: 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 71
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 65 (Div. 2)"
rating: 2200
weight: 71
solve_time_s: 138
verified: true
draft: false
---

[CF 71D - Solitaire](https://codeforces.com/problemset/problem/71/D) 

**Đánh giá:** 2200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một tấm bảng hình chữ nhật chứa đầy các quân bài. Kích thước bảng tối đa là$17 \times 17$, nhưng tổng số quân bài được đặt không bao giờ vượt quá 52 vì bộ bài chỉ có 52 quân bài thông thường cộng với hai lá bài pha trò. 

Hình vuông 3×3 hợp lệ phải đáp ứng một trong hai thuộc tính: 

1. Cả 9 lá bài đều có chất giống nhau. 
2. Tất cả 9 lá bài đều có thứ hạng riêng biệt theo từng cặp. 

Hai hình vuông như vậy phải tồn tại mà không chồng chéo lên nhau. 

Sự thay đổi đến từ những người pha trò. Đầu vào có thể chứa`J1`Và`J2`, đại diện cho hai kẻ pha trò riêng biệt từ bộ bài. Trước khi kiểm tra tình trạng, chúng ta có thể thay thế mỗi lá bài joker bằng bất kỳ lá bài nào chưa sử dụng từ bộ bài còn lại. Các quân bài thay thế phải khác biệt và chưa xuất hiện trên bảng. 

Nhiệm vụ không chỉ là quyết định liệu một giải pháp có tồn tại hay không mà còn phải in ra một phép thay thế joker hợp lệ và tọa độ của hai ô vuông 3 × 3. 

Các ràng buộc đủ nhỏ để có thể thực hiện được việc sử dụng vũ lực đối với các vị trí trên bảng. Số ô vuông 3×3 có thể có nhiều nhất là:$$(17 - 2)^2 = 225$$Vì vậy, ngay cả việc kiểm tra tất cả các cặp hình vuông cũng rẻ:$$225^2 \approx 5 \times 10^4$$Phần đắt tiền là thay thế joker. Một tìm kiếm đơn giản trên tất cả các bài tập thẻ có thể có thể đạt được khoảng:$$54^2 \cdot 225^2 \approx 1.5 \times 10^8$$điều này gây khó chịu trong Python khi logic xác thực được thêm vào. 

Điều quan trọng là mỗi ô vuông 3×3 chỉ chứa 9 ô. Việc một hình vuông có thể trở thành hợp lệ hay không chỉ phụ thuộc vào 9 lá bài đó chứ không phụ thuộc vào phần còn lại của bàn cờ. Điều đó cho phép chúng tôi xử lý trước từng ô vuông một cách độc lập và ghi lại các phép gán joker nào làm cho nó hợp lệ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một người pha trò có thể nằm ngoài các ô đã chọn. Trong trường hợp đó, việc thay thế nó vẫn có vấn đề vì mỗi lá bài trong bộ bài phải là duy nhất. Việc triển khai bất cẩn có thể chỉ định cùng một thẻ thay thế cho cả hai người pha trò chỉ vì không người pha trò nào tham gia vào một hình vuông. 

Ví dụ:```
3 6
2S 3S 4S 5S 6S 7S
8S 9S TS JS QS KS
AS J1 J2 2H 3H 4H
```Hai người pha trò không liên quan đến ô vuông hợp lệ, nhưng họ vẫn phải nhận được các quân bài chưa sử dụng riêng biệt. 

Một trường hợp tinh tế khác là phát hiện chồng chéo. Hai ô vuông 3×3 không hợp lệ nếu chúng có chung một ô. 

Ví dụ: trong bảng 4 × 4:```
(1,1) square covers rows 1..3 and cols 1..3
(2,2) square covers rows 2..4 and cols 2..4
```Những vùng này chồng lên nhau trong vùng 2×2 và không thể sử dụng cùng nhau. 

Một lỗi phổ biến là chỉ kiểm tra xem các góc trên bên trái có khác nhau hay không. 

Điều kiện xếp hạng cũng yêu cầu tất cả 9 cấp bậc phải khác biệt theo từng cặp. Các cấp bậc trùng lặp ngay lập tức làm mất hiệu lực của hình vuông, ngay cả khi chất khác nhau. 

Ví dụ:```
2S 3H 4D
5C 6S 7H
8D 2C 9S
```Điều này không hợp lệ theo quy tắc xếp hạng vì thứ hạng`2`xuất hiện hai lần. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. 

Chúng ta có thể liệt kê từng cặp hình vuông 3×3, liệt kê mọi khả năng thay thế cho mỗi người pha trò, xây dựng bảng cuối cùng và kiểm tra xem cả hai hình vuông có thỏa mãn một trong hai điều kiện hay không. 

Bảng có tối đa 225 ô vuông có thể có. Có tối đa 54 lá bài ứng cử viên cho mỗi lá bài joker. Việc kiểm tra một ô vuông tốn thời gian không đổi vì nó luôn chứa chính xác 9 ô. 

Điều đó mang lại sự phức tạp trong trường hợp xấu nhất xung quanh:$$225^2 \cdot 54^2$$đã có hơn 100 triệu kết hợp trước khi tính đến chi phí xác thực. Python có thể đấu tranh với điều này. 

Quan sát cấu trúc quan trọng là các hình vuông là độc lập. Một hình vuông chỉ quan tâm đến các lá bài bên trong 9 ô của chính nó. Vì vậy, thay vì thử tất cả các bài tập joker toàn cục trước tiên, chúng ta có thể xử lý trước từng ô vuông và hỏi: 

"Việc thay thế joker nào làm cho hình vuông cụ thể này hợp lệ?" 

Vì có nhiều nhất hai joker nên số lượng kết hợp thay thế là rất nhỏ: 

- không có trò đùa: chính xác một trạng thái 
- một người pha trò: tối đa 52 quân bài có thể có 
- hai người pha trò: nhiều nhất$52 \cdot 51$Đối với mỗi ô vuông, chúng tôi kiểm tra mọi phép gán joker hợp pháp một lần và lưu trữ tất cả các phép gán làm cho ô vuông hợp lệ. 

Sau khi tiền xử lý, việc giải quyết vấn đề trở nên dễ dàng: 

1. Liệt kê các cặp hình vuông không chồng lên nhau. 
2. Kiểm tra xem các bộ bài tập hợp lệ của chúng có tương thích hay không. 
3. Khả năng tương thích có nghĩa là cả hai ô vuông đều đồng ý về việc thay thế joker cuối cùng. 

Điều này biến vấn đề từ việc xác thực tốn kém nhiều lần thành vấn đề tra cứu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(S^2 \cdot C^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(S \cdot C^2 + S^2 \cdot A)$|$O(S \cdot A)$| Đã chấp nhận | 

Đây:

-$S \le 225$là số ô vuông 
-$C \le 54$là số lượng bài tập thẻ 
-$A$là số lượng bài tập hợp lệ trên mỗi ô vuông 

Trong thực tế,$A$là nhỏ. 

## Hướng dẫn thuật toán 

1. Phân tích bảng và thu thập tất cả các thẻ thông thường đã sử dụng. 

Joker bị loại trừ vì sau này chúng có thể trở thành bất kỳ quân bài nào không được sử dụng. 
2. Tạo danh sách các lá bài chưa sử dụng từ bộ bài đầy đủ 52 lá. 

Đây là những sự thay thế hợp pháp duy nhất cho những người pha trò. 
3. Liệt kê từng ô vuông 3×3 trên bảng. 

Một hình vuông được xác định bởi góc trên bên trái của nó. 
4. Đối với mỗi ô vuông, hãy kiểm tra mọi phép gán của joker. 

Nếu không có người pha trò thì chỉ có một nhiệm vụ. 

Nếu có một lá joker, hãy thử từng lá bài chưa sử dụng. 

Nếu có hai người pha trò, hãy thử từng cặp thẻ chưa sử dụng riêng biệt đã đặt hàng. 
5. Sau khi thay thế các giá trị joker vào trong hình vuông, hãy kiểm tra xem hình vuông có hợp lệ hay không. 

Một hình vuông hợp lệ nếu: 

- tất cả các bộ quần áo đều giống hệt nhau 
- tất cả các cấp bậc đều khác biệt 
6. Lưu trữ mọi phép gán làm cho hình vuông hợp lệ. 

Khóa được lưu trữ là bài tập joker toàn cầu đầy đủ: 

-`()`nếu không có trò đùa 
-`(card,)`nếu một joker tồn tại 
-`(card1, card2)`nếu cả hai tồn tại 
7. Liệt kê tất cả các cặp hình vuông. 

Bỏ qua các cặp có diện tích trùng nhau. 
8. Đối với một cặp không chồng chéo, hãy kiểm tra xem có tồn tại phép gán joker trong tập hợp phép gán hợp lệ của cả hai ô vuông hay không. 

Vì việc thay thế joker có tính toàn cầu nên cả hai ô vuông phải đồng ý về nhiệm vụ cuối cùng giống nhau. 
9. Ngay khi tìm thấy một cặp tương thích, hãy in kết quả được yêu cầu. 
10. Nếu không có cặp nào hoạt động, hãy in`"No solution."`### Tại sao nó hoạt động 

Mỗi ô vuông được đánh giá theo mỗi lần thay thế joker hợp pháp. Bước tiền xử lý ghi lại chính xác các phép gán mà theo đó bình phương trở thành hợp lệ. 

Sau này, khi kết hợp hai ô vuông, việc yêu cầu gán một nhiệm vụ chung sẽ đảm bảo rằng cùng một sự thay thế joker toàn cục sẽ làm cho cả hai ô vuông có giá trị đồng thời. 

Tính năng không chồng chéo được kiểm tra rõ ràng, do đó cấu hình cuối cùng luôn đáp ứng mọi ràng buộc. 

Bởi vì mọi phép gán pháp lý và mọi cặp ô vuông đều được kiểm tra nên không thể bỏ sót giải pháp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

RANKS = "23456789TJQKA"
SUITS = "CDHS"

ALL_CARDS = [r + s for r in RANKS for s in SUITS]

def overlap(r1, c1, r2, c2):
    return not (
        r1 + 2 < r2 or
        r2 + 2 < r1 or
        c1 + 2 < c2 or
        c2 + 2 < c1
    )

def valid_square(cells):
    suits = [x[1] for x in cells]
    if len(set(suits)) == 1:
        return True

    ranks = [x[0] for x in cells]
    return len(set(ranks)) == 9

def solve():
    n, m = map(int, input().split())
    board = [input().split() for _ in range(n)]

    jokers = []
    used = set()

    for i in range(n):
        for j in range(m):
            card = board[i][j]
            if card in ("J1", "J2"):
                jokers.append(card)
            else:
                used.add(card)

    unused = [c for c in ALL_CARDS if c not in used]

    joker_count = len(jokers)

    assignments = []

    if joker_count == 0:
        assignments.append(())
    elif joker_count == 1:
        for c in unused:
            assignments.append((c,))
    else:
        for c1 in unused:
            for c2 in unused:
                if c1 != c2:
                    assignments.append((c1, c2))

    square_info = []

    for r in range(n - 2):
        for c in range(m - 2):
            valid_assignments = set()

            for assn in assignments:
                cells = []

                ok = True

                for i in range(r, r + 3):
                    for j in range(c, c + 3):
                        card = board[i][j]

                        if card == "J1":
                            if joker_count == 1:
                                card = assn[0]
                            else:
                                card = assn[0]
                        elif card == "J2":
                            card = assn[1]

                        cells.append(card)

                if valid_square(cells):
                    valid_assignments.add(assn)

            square_info.append((r, c, valid_assignments))

    total = len(square_info)

    for i in range(total):
        r1, c1, s1 = square_info[i]

        if not s1:
            continue

        for j in range(i + 1, total):
            r2, c2, s2 = square_info[j]

            if not s2:
                continue

            if overlap(r1, c1, r2, c2):
                continue

            common = s1 & s2

            if common:
                assn = next(iter(common))

                print("Solution exists.")

                if joker_count == 0:
                    print("There are no jokers.")
                elif joker_count == 1:
                    print(f"Replace {jokers[0]} with {assn[0]}.")
                else:
                    print(f"Replace J1 with {assn[0]} and J2 with {assn[1]}.")

                print(f"Put the first square to ({r1 + 1}, {c1 + 1}).")
                print(f"Put the second square to ({r2 + 1}, {c2 + 1}).")
                return

    print("No solution.")

solve()
```Việc thực hiện tuân theo chiến lược tiền xử lý trực tiếp.`valid_square`kiểm tra hai mẫu được phép. Sự bình đẳng được kiểm tra đầu tiên vì nó rẻ hơn một chút. Tính duy nhất của thứ hạng chỉ đơn giản là sử dụng một bộ. 

các`assignments`danh sách đại diện cho mọi sự thay thế joker toàn cầu hợp pháp. Thứ tự có vấn đề khi cả hai người pha trò đều tồn tại bởi vì`J1 -> A`,`J2 -> B`khác với`J1 -> B`,`J2 -> A`. 

Mỗi ô vuông lưu trữ một tập hợp các bài tập hợp lệ. Việc sử dụng một bộ sẽ giúp việc kiểm tra giao lộ sau này trở nên hiệu quả hơn. 

Việc kiểm tra chồng chéo rất dễ xảy ra sai sót. Hai hình vuông 3×3 không chồng lên nhau chỉ khi một hình nằm hoàn toàn ở trên, dưới, bên trái hoặc bên phải của hình kia. Mọi cấu hình khác đều chồng chéo. 

Logic thay thế bên trong cấu trúc hình vuông có chủ ý chỉ thay thế các ô joker. Tất cả các thẻ thông thường không thay đổi. 

Việc tìm kiếm cuối cùng chỉ đơn giản là tìm thấy một cặp hình vuông có giao điểm gán không trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
2S 3S 4S 7S 8S AS
5H 6H 7H 5S TC AC
8H 9H TH 7C 8C 9C
2D 2C 3C 4C 5C 6C
```Các hình vuông có thể có: 

| Quảng trường | Trên cùng bên trái | hợp lệ | 
| --- | --- | --- | 
| A | (1,1) | Không | 
| B | (1,2) | Không | 
| C | (1,3) | Không | 
| D | (1,4) | Không | 
| E | (2,1) | Không | 
| F | (2,2) | Không | 
| G | (2,3) | Không | 
| H | (2,4) | Không | 

Không có hình vuông nào thỏa mãn cả hai quy tắc, vì vậy không có cặp nào có thể hoạt động. 

Đầu ra:```
No solution.
```Ví dụ này chứng minh rằng ngay cả các vùng có cấu trúc trực quan cũng có thể thất bại vì tính duy nhất của thứ hạng yêu cầu tất cả chín thứ hạng phải khác nhau một cách chính xác. 

### Ví dụ 2 

đầu vào:```
3 6
2S 3S 4S 5H 6H 7H
5S 6S 7S 8H 9H TH
8S 9S TS J1 QH KH
```Thẻ chưa sử dụng bao gồm nhiều khả năng. 

Hình vuông 3×3 bên trái chứa: 

| 2S | 3S | 4S | 
| --- | --- | --- | 
| 5S | 6S | 7S | 
| 8S | 9S | TS | 

Tất cả các bộ đồ đều`S`, do đó hình vuông có giá trị ngay lập tức. 

Hình vuông 3×3 bên phải chứa các cấp bậc: 

| H | H | H | 
| --- | --- | --- | 
| H | H | H | 
| J1 | H | H | 

Thay thế`J1`với`AH`làm cho tất cả các cấp bậc khác biệt:$$5,6,7,8,9,T,A,Q,K$$Thuật toán ghi lại phép gán này hợp lệ cho ô vuông thứ hai và tìm ra một cặp tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S \cdot C^2 + S^2 \cdot A)$| xử lý trước tất cả các ô vuông và bài tập, sau đó kiểm tra các cặp ô vuông | 
| Không gian |$O(S \cdot A)$| lưu trữ các bài tập hợp lệ cho mỗi ô vuông | 

Với tối đa 225 ô vuông và nhiều nhất là vài nghìn bài tập joker, lời giải dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    RANKS = "23456789TJQKA"
    SUITS = "CDHS"

    ALL_CARDS = [r + s for r in RANKS for s in SUITS]

    def overlap(r1, c1, r2, c2):
        return not (
            r1 + 2 < r2 or
            r2 + 2 < r1 or
            c1 + 2 < c2 or
            c2 + 2 < c1
        )

    def valid_square(cells):
        suits = [x[1] for x in cells]
        if len(set(suits)) == 1:
            return True

        ranks = [x[0] for x in cells]
        return len(set(ranks)) == 9

    input = sys.stdin.readline

    n, m = map(int, input().split())
    board = [input().split() for _ in range(n)]

    jokers = []
    used = set()

    for i in range(n):
        for j in range(m):
            card = board[i][j]
            if card in ("J1", "J2"):
                jokers.append(card)
            else:
                used.add(card)

    unused = [c for c in ALL_CARDS if c not in used]

    joker_count = len(jokers)

    assignments = []

    if joker_count == 0:
        assignments.append(())
    elif joker_count == 1:
        for c in unused:
            assignments.append((c,))
    else:
        for c1 in unused:
            for c2 in unused:
                if c1 != c2:
                    assignments.append((c1, c2))

    square_info = []

    for r in range(n - 2):
        for c in range(m - 2):
            valid_assignments = set()

            for assn in assignments:
                cells = []

                for i in range(r, r + 3):
                    for j in range(c, c + 3):
                        card = board[i][j]

                        if card == "J1":
                            card = assn[0]
                        elif card == "J2":
                            card = assn[1]

                        cells.append(card)

                if valid_square(cells):
                    valid_assignments.add(assn)

            square_info.append((r, c, valid_assignments))

    total = len(square_info)

    for i in range(total):
        r1, c1, s1 = square_info[i]

        for j in range(i + 1, total):
            r2, c2, s2 = square_info[j]

            if overlap(r1, c1, r2, c2):
                continue

            if s1 & s2:
                print("Solution exists.")
                return out.getvalue()

    print("No solution.")
    return out.getvalue()

# provided sample
assert run(
"""4 6
2S 3S 4S 7S 8S AS
5H 6H 7H 5S TC AC
8H 9H TH 7C 8C 9C
2D 2C 3C 4C 5C 6C
"""
).strip() == "No solution.", "sample 1"

# minimum board
assert "No solution." in run(
"""3 3
2S 3S 4S
5S 6S 7S
8S 9S TS
"""
), "single square cannot form two non-overlapping squares"

# simple positive case
assert "Solution exists." in run(
"""3 6
2S 3S 4S 2H 3H 4H
5S 6S 7S 5H 6H 7H
8S 9S TS 8H 9H TH
"""
), "two disjoint suit squares"

# overlap trap
assert "No solution." in run(
"""4 4
2S 3S 4S 5S
6S 7S 8S 9S
TS JS QS KS
AH 2H 3H 4H
"""
), "overlapping valid squares are forbidden"

# joker replacement
assert "Solution exists." in run(
"""3 6
2S 3S 4S 5H 6H 7H
5S 6S 7S 8H 9H TH
8S 9S TS J1 QH KH
"""
), "joker creates distinct ranks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông hợp lệ duy nhất 3×3 | Không có giải pháp | Cần hai hình vuông không chồng lên nhau | 
| Hai bộ đồ vuông ngang | Giải pháp tồn tại | Trường hợp tích cực cơ bản | 
| Ứng viên chồng chéo 4x4 | Không có giải pháp | Phát hiện chồng chéo chính xác | 
| Sửa chữa một joker | Giải pháp tồn tại | Logic thay thế Joker | 

## Vỏ cạnh 

Hãy xem xét lại vấn đề chồng chéo. 

đầu vào:```
4 4
2S 3S 4S 5S
6S 7S 8S 9S
TS JS QS KS
AH 2H 3H 4H
```Quảng trường ở`(1,1)`là hợp lệ vì tất cả các chất đều là quân bích. 

Quảng trường ở`(2,2)`cũng có vẻ đầy hứa hẹn:```
7S 8S 9S
JS QS KS
2H 3H 4H
```Việc triển khai có lỗi có thể chấp nhận cả hai vì tọa độ trên cùng bên trái của chúng khác nhau. 

Thuật toán từ chối chúng một cách chính xác vì các hàng`2..3`và cột`2..3`chồng chéo. 

Một trường hợp phức tạp khác liên quan đến cấp bậc trùng lặp. 

đầu vào:```
3 3
2S 3H 4D
5C 6S 7H
8D 2C 9S
```Bộ đồ không đồng nhất. 

Các cấp bậc là:```
2 3 4
5 6 7
8 2 9
```Thứ hạng`2`xuất hiện hai lần, vì vậy hình vuông không hợp lệ. 

Thuật toán sử dụng`len(set(ranks)) == 9`, nó nắm bắt được điều này ngay lập tức. 

Cuối cùng, hãy xem xét những người pha trò bên ngoài các ô vuông đang hoạt động. 

đầu vào:```
3 6
2S 3S 4S 5S 6S 7S
8S 9S TS JS QS KS
AS J1 J2 2H 3H 4H
```Hình vuông bên trái đã thỏa mãn điều kiện phù hợp. Những trò đùa không liên quan đến nó. 

Một giải pháp bất cẩn có thể giao cho cả hai người pha trò cùng một lá bài chưa sử dụng vì họ không bao giờ được kiểm tra. 

Thuật toán tạo ra các bài tập chỉ sử dụng các cặp riêng biệt theo thứ tự, đảm bảo tính nhất quán của bộ bài trên toàn cầu.
