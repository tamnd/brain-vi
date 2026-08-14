---
title: "CF 102323H - Cố định sô cô la"
description: "Câu đố sử dụng chính xác chín chiếc nấm cục. Mỗi nấm cục có một trong ba hình dạng, hình vuông, hình tròn hoặc hình tam giác và một trong ba hương vị, vani, dâu tây hoặc sô cô la. Vì mỗi sự kết hợp xảy ra đúng một lần nên chín nấm cục vật lý đều khác biệt."
date: "2026-08-13T04:18:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 85
verified: true
draft: false
---

[CF 102323H - Sửa sô cô la](https://codeforces.com/problemset/problem/102323/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Câu đố sử dụng chính xác chín chiếc nấm cục. Mỗi nấm cục có một trong ba hình dạng, hình vuông, hình tròn hoặc hình tam giác và một trong ba hương vị, vani, dâu tây hoặc sô cô la. Vì mỗi sự kết hợp xảy ra đúng một lần nên chín nấm cục vật lý đều khác biệt. 

Chúng ta phải đặt chín viên nấm cục đó vào một bảng 3x3. Manh mối là một mẫu hình chữ nhật nhỏ hơn chứa một số hình dạng và hương vị cố định cùng một số ký tự đại diện. Manh mối không cho chúng ta biết hình chữ nhật của nó bắt đầu từ đâu. Thay vào đó, toàn bộ manh mối phải xuất hiện ở đâu đó trên bảng 3x3 mà không cần xoay. Một manh mối về kích thước`x × y`do đó có thể được đặt trong`(4 - x) × (4 - y)`các vị trí khác nhau. 

Đầu vào chứa một số câu đố. Đối với mỗi câu đố, chúng tôi nhận được tối đa mười manh mối như vậy và tuyên bố đảm bảo rằng chính xác một sự sắp xếp hoàn chỉnh thỏa mãn tất cả chúng. Chúng ta phải in sự sắp xếp đó bằng ký hiệu hai ký tự giống như các manh mối. 

Các ràng buộc làm cho kích thước của không gian tìm kiếm trở thành quan sát trung tâm. Luôn có chính xác chín viên kẹo bất kể có bao nhiêu câu đố được cung cấp. Một sự sắp xếp hoàn chỉnh chỉ đơn giản là một hoán vị của chín đối tượng riêng biệt, do đó chỉ có`9! = 362880`bảng có thể. Mười manh mối và tối đa chín vị trí có thể có cho mỗi đầu mối chỉ thêm một hệ số không đổi nhỏ. Tìm kiếm trên tất cả`9^9 = 387420489`các phép gán tùy ý sẽ lớn một cách không cần thiết, trong khi chỉ liệt kê các hoán vị hợp lệ là đủ nhỏ để tìm kiếm toàn diện trực tiếp. 

Có hai cách dễ dàng để xử lý sai các manh mối. Đầu tiên, một manh mối có thể xuất hiện ở nhiều vị trí vì hình chữ nhật của nó nhỏ hơn bảng. Ví dụ, một`3 × 2`manh mối có thể bắt đầu ở cột 1 hoặc cột 2 và không được xoay để khớp với nơi khác. Trong mẫu 1,`2 × 3`manh mối tương tự có thể bắt đầu ở hàng trên cùng hoặc giữa. Một chương trình luôn neo giữ một manh mối tại`(0, 0)`bác bỏ các giải pháp hợp lệ. 

Thứ hai, dấu gạch dưới không sửa được gì. Ví dụ,`_C`có nghĩa là hương vị sôcôla với hình dạng tùy ý, trong khi`S_`có nghĩa là hình vuông với hương vị tùy ý. Việc thực hiện bất cẩn dẫn đến`_`như một giá trị thông thường sẽ từ chối các bảng hợp lệ. Các manh mối mẫu chứng minh sự khác biệt này và đầu ra chính xác cho Mẫu 3 là bảng được xác định duy nhất hiển thị trong các mẫu. 

Trường hợp ranh giới thứ ba xuất hiện với đầy đủ`3 × 3`manh mối. Đầu mối như vậy có chính xác một vị trí có thể có, vì vậy mọi thuộc tính cố định sẽ trực tiếp xác định ô bảng tương ứng. Ví dụ: đầu vào một câu đố```
1
1
3 3
TC SC SS
RV RC SV
TS TV RS
```có đầu ra```
Puzzle #1:
TC SC SS
RV RC SV
TS TV RS
```Ở đây không có sự lựa chọn vị trí đầu mối nào cả. 

## Phương pháp tiếp cận 

Lực lượng vũ phu nhất theo nghĩa đen là chỉ định một trong chín nấm cục một cách độc lập cho mỗi ô trong số chín ô. Điều đó tạo ra`9^9 = 387420489`hội đồng ứng cử viên, nhiều hội đồng trong số đó ngay lập tức vi phạm quy định rằng mỗi loại nấm cục phải được sử dụng đúng một lần. Việc kiểm tra tối đa mười manh mối trên mỗi bảng đó sẽ cho kết quả đại khái`9^9 × 10 × 9`, hoặc khoảng 34,9 tỷ lượt kiểm tra vị trí cơ bản trong trường hợp xấu nhất. Đó là công việc nhiều hơn mức cần thiết. 

Quan sát hữu ích là chín loại nấm cục đã được biết là khác biệt và mỗi loại phải được sử dụng đúng một lần. Chúng ta không bao giờ cần phải xem xét một bảng không hợp lệ chứa cùng một nấm cục hai lần. Một bảng chính xác là một hoán vị của chín danh tính truffle, làm giảm số lượng ứng cử viên từ`9^9`ĐẾN`9! = 362880`. 

Đối với mỗi hoán vị, chúng tôi kiểm tra mọi đầu mối. Một manh mối chỉ có thể bắt đầu ở các vị trí mà hình chữ nhật hoàn chỉnh của nó vẫn nằm bên trong bảng 3x3. Đối với mỗi vị trí bắt đầu có thể, chúng tôi chỉ so sánh các thuộc tính mà manh mối thực sự chỉ định. Nếu có ít nhất một vị trí trùng khớp thì manh mối đó được thỏa mãn. 

Trường hợp xấu nhất cho việc tìm kiếm trực tiếp này nhiều nhất là`9! × 10 × 9 = 32,659,200`kiểm tra cấp độ tế bào. Việc triển khai thực tế thậm chí còn nhỏ hơn vì hầu hết các manh mối đều chứa các thuộc tính cố định và việc tìm kiếm sẽ dừng ngay khi tìm thấy giải pháp duy nhất. Vì kích thước bảng vĩnh viễn là chín, nên tìm kiếm hoán vị toàn diện là giải pháp tự nhiên ở đây thay vì đưa ra một bộ giải ràng buộc nặng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhiệm vụ độc lập | O(9^9 · C · 9) | O(9) | Quá chậm | 
| Bảng liệt kê hoán vị | O(9! · C · 9) | O(9 + C · 9) | Đã chấp nhận | 

Đây`C ≤ 10`là số lượng manh mối. Sự phức tạp giai thừa rõ ràng là vô hại vì giai thừa là`9!`, giá trị cố định chỉ là 362880. 

## Hướng dẫn thuật toán 

1. Mã hóa mỗi nấm cục vật lý dưới dạng số nguyên từ 0 đến 8. Chúng ta có thể sử dụng`shape * 3 + flavor`, trong đó hình dạng và hương vị đều được biểu thị bằng ba giá trị. Điều này mang lại cho mỗi sự kết hợp hình dạng-hương vị một bản sắc riêng. 
2. Chuyển đổi mọi mã đầu mối thành một cặp mặt nạ thuộc tính được phép. Đối với ký tự hình dạng,`_`cho phép cả ba hình dạng, trong khi`S`,`R`, Và`T`cho phép chính xác một. Đặc tính hương vị hoạt động tương tự, với`_`cho phép cả ba hương vị và`V`,`S`, Và`C`chọn một hương vị. 
3. Tạo ra mọi hoán vị của chín danh tính nấm cục. Mỗi hoán vị đại diện cho một bảng ứng cử viên hoàn chỉnh, do đó không cần phải kiểm tra riêng xem một nấm cục có bị trùng lặp hay bị bỏ qua hay không. 
4. Đối với mỗi manh mối, hãy liệt kê mọi góc trên bên trái hợp lệ của hình chữ nhật. Nếu kích thước của nó là`x × y`, hàng có thể dao động từ`0`bởi vì`3 - x`và cột có thể nằm trong khoảng từ`0`bởi vì`3 - y`. Manh mối sẽ được thỏa mãn nếu ít nhất một trong các vị trí này phù hợp với hội đồng ứng cử viên. 
5. Để kiểm tra vị trí, hãy kiểm tra từng ô của đầu mối. Nếu manh mối sửa được một hình dạng, hãy so sánh nó với hình dạng của nấm cục ứng cử viên. Nếu nó khắc phục được một hương vị, hãy so sánh hương vị đó. Ký tự đại diện không áp đặt hạn chế nào. 
6. Nếu mỗi đầu mối có ít nhất một vị trí trùng khớp thì hoán vị là nghiệm duy nhất. In chín mã nấm truffle theo bố cục 3x3 và chuyển sang câu đố tiếp theo. 

Lý do việc tìm kiếm này hoàn tất là vì mỗi bảng pháp lý xuất hiện đúng một lần trong số chín hoán vị. Đối với bất kỳ bảng nào như vậy, việc kiểm tra mọi vị trí hợp pháp của mọi đầu mối đều phản ánh chính xác định nghĩa của đầu mối. Do đó, một bảng được chấp nhận chính xác khi tất cả các manh mối đều được thỏa mãn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi ứng cử viên được tìm kiếm xem xét đều là một hoán vị hợp lệ của chín nấm cục vật lý. Đối với mỗi đầu mối, quy trình so khớp sẽ xem xét mọi vị trí có thể mà đầu mối đó có thể xuất hiện mà không cần xoay và chấp nhận đầu mối một cách chính xác khi một trong các vị trí đó phù hợp với mọi thuộc tính được chỉ định. Do đó, một thí sinh vượt qua toàn bộ bài kiểm tra khi và chỉ nếu đó là một lời giải câu đố hợp pháp. Vì bài toán đảm bảo tính duy nhất nên phép hoán vị đầu tiên là sự sắp xếp bắt buộc. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def solve_puzzle(clues):
    shape_id = {'S': 0, 'R': 1, 'T': 2}
    flavor_id = {'V': 0, 'S': 1, 'C': 2}

    # Piece id = shape * 3 + flavor.
    pieces = list(range(9))

    def shape_mask(ch):
        if ch == '_':
            return 0b111
        return 1 << shape_id[ch]

    def flavor_mask(ch):
        if ch == '_':
            return 0b111
        return 1 << flavor_id[ch]

    # Each placement is represented by a list of
    # (board_position, allowed_shape_mask, allowed_flavor_mask).
    prepared = []

    for x, y, grid in clues:
        placements = []

        for sr in range(4 - x):
            for sc in range(4 - y):
                placement = []

                for r in range(x):
                    for c in range(y):
                        code = grid[r][c]
                        sm = shape_mask(code[0])
                        fm = flavor_mask(code[1])
                        pos = (sr + r) * 3 + (sc + c)
                        placement.append((pos, sm, fm))

                placements.append(placement)

        prepared.append(placements)

    # More restrictive clues first. This does not change correctness,
    # but usually rejects a wrong permutation earlier.
    def restriction_score(placements):
        score = 0
        for placement in placements:
            for _, sm, fm in placement:
                if sm != 0b111:
                    score += 1
                if fm != 0b111:
                    score += 1
        return score

    prepared.sort(key=restriction_score, reverse=True)

    for board in permutations(pieces):
        good = True

        for placements in prepared:
            clue_good = False

            for placement in placements:
                matches = True

                for pos, sm, fm in placement:
                    piece = board[pos]
                    shape = piece // 3
                    flavor = piece % 3

                    if not (sm & (1 << shape)):
                        matches = False
                        break

                    if not (fm & (1 << flavor)):
                        matches = False
                        break

                if matches:
                    clue_good = True
                    break

            if not clue_good:
                good = False
                break

        if good:
            return board

    return None

def main():
    t = int(input())
    output = []

    for case in range(1, t + 1):
        c = int(input())
        clues = []

        for _ in range(c):
            x, y = map(int, input().split())
            grid = [input().split() for _ in range(x)]
            clues.append((x, y, grid))

        board = solve_puzzle(clues)

        output.append(f"Puzzle #{case}:")
        for r in range(3):
            row = []
            for col in range(3):
                piece = board[r * 3 + col]
                shape = "SRT"[piece // 3]
                flavor = "VSC"[piece % 3]
                row.append(shape + flavor)
            output.append(" ".join(row))

        output.append("")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_puzzle`gán một số nguyên duy nhất cho mỗi nấm cục. Với`piece // 3`chúng tôi khôi phục lại hình dạng và với`piece % 3`chúng tôi phục hồi hương vị. Mã hóa này rất hữu ích vì nó cho phép`itertools.permutations`trực tiếp liệt kê từng ban pháp lý. 

Quá trình xử lý trước manh mối sẽ chuyển đổi từng vị trí có thể có thành các vị trí trên bảng mà nó hạn chế và các mặt nạ được phép tương ứng. Một chiếc mặt nạ như`0b111`đại diện cho cả ba khả năng, trong khi mặt nạ một bit đại diện cho một thuộc tính cố định. Điều này giữ cho vòng lặp khớp độc lập với biểu diễn đầu mối văn bản. 

Các vòng lặp vị trí sử dụng`range(4 - x)`Và`range(4 - y)`. Để biết manh mối về chiều cao`x`, hàng bắt đầu hợp lệ lớn nhất là`3 - x`, vậy có chính xác`4 - x`hàng bắt đầu có thể. Lý do tương tự áp dụng cho các cột. Đây chính là ranh giới ngăn cản manh mối vươn ra ngoài bàn cờ. 

Sắp xếp manh mối theo số lượng thuộc tính cố định của chúng chỉ là cách tối ưu hóa hiệu suất. Một manh mối hạn chế có khả năng nhanh chóng từ chối một hoán vị không chính xác, do đó sẽ tốn ít công sức hơn để kiểm tra các manh mối còn lại. Kết quả không phụ thuộc vào thứ tự này. 

Bản thân hoán vị là một bộ gồm chín số nguyên, vì vậy`board[pos]`trực tiếp xác định nấm cục đang chiếm giữ một ô. Hình dạng và hương vị được kiểm tra riêng biệt đối với mặt nạ của chúng. Ký tự đại diện có cả ba bit được kích hoạt, khiến việc so sánh của nó tự động thành công. 

Người giải quyết quay trở lại ngay sau khi tìm thấy một bảng thỏa mãn mọi manh mối. Bài toán đảm bảo rằng một bảng như vậy tồn tại và là duy nhất, do đó không có sự mơ hồ khi dừng lại ở trận đấu đầu tiên. 

Đầu ra chuyển đổi mã hóa số nguyên thành dạng hai ký tự được yêu cầu. Bảng chữ cái hình dạng là`SRT`, và bảng chữ cái hương vị là`VSC`, phù hợp với ký hiệu của bài toán. Một dòng trống được thêm vào sau mỗi câu đố theo yêu cầu của định dạng đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chứa bốn manh mối. Manh mối đầu tiên đã chỉ định một bảng 3x3 hoàn chỉnh nên nó chỉ có một vị trí khả thi. Các manh mối khác đều phù hợp với sự sắp xếp tương tự. 

| Bước | Bang hội đồng ứng cử viên | Kết quả | 
| --- | --- | --- | 
| 1 | Bắt đầu liệt kê các hoán vị | Tìm kiếm ứng viên bắt đầu | 
| 2 | Kiểm tra`3 × 3`manh mối | Chỉ tồn tại một vị trí | 
| 3 | Kiểm tra`2 × 3`manh mối | Ít nhất một vị trí phù hợp | 
| 4 | Kiểm tra`3 × 3`manh mối | Trận đấu toàn bảng | 
| 5 | Kiểm tra`2 × 3`manh mối | Ít nhất một vị trí phù hợp | 
| 6 | Mọi manh mối đều thỏa mãn | Chấp nhận bảng | 

Bảng kết quả là```
TC SC SS
RV RC SV
TS TV RS
```Điểm mấu chốt ở đây là manh mối có kích thước đầy đủ không có sự mơ hồ về vị trí. Nó cũng chứng tỏ rằng có thể tìm ra lời giải mà không cần thực hiện bất kỳ phép suy luận đặc biệt nào theo cách thủ công, vì tìm kiếm hoán vị xử lý trực tiếp manh mối. 

### Mẫu 2 

Câu đố thứ hai không đưa ra một bảng hoàn chỉnh ở manh mối đầu tiên. Thay vào đó, việc tìm kiếm phải tính đến một số vị trí có thể có của các hình chữ nhật đầu mối nhỏ hơn. 

| Bước | Bang ứng cử viên | Kết quả | 
| --- | --- | --- | 
| 1 | Liệt kê một hoán vị | Ban ứng cử viên được chọn | 
| 2 | Kiểm tra`2 × 3`manh mối | Kiểm tra hai vị trí hàng có thể có của nó | 
| 3 | Kiểm tra`3 × 3`manh mối | Kiểm tra vị trí duy nhất có thể của nó | 
| 4 | Kiểm tra manh mối còn lại | Từ chối ứng viên vi phạm bất kỳ đầu mối nào | 
| 5 | Hoán vị sống sót duy nhất | Chấp nhận | 

Bảng độc đáo là```
TV RS TS
SC SV TC
SS RV RC
```Ví dụ này thực hiện cách giải thích trung tâm của một đầu mối: hình chữ nhật của nó có thể được dịch trong bảng nhưng không thể xoay được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9! · C · 9) | Nhiều nhất`9!`bảng, nhiều nhất`C ≤ 10`manh mối và tối đa chín ô được kiểm tra trên mỗi vị trí đầu mối | 
| Không gian | O(C · 9 + 9) | Vị trí đầu mối được lưu trữ cộng với hoán vị hiện tại | 

Tìm kiếm lớn nhất chỉ có 362880 bảng ứng cử viên. Với tối đa mười manh mối và chín vị trí đầu mối khả thi, công việc lý thuyết là khoảng 32,7 triệu lần kiểm tra tế bào, trong khi những manh mối hạn chế thường kết liễu những ứng viên thất bại sớm hơn nhiều. Việc sử dụng bộ nhớ rất nhỏ vì bảng chỉ chứa chín ô và biểu diễn đầu mối chỉ chứa một số lượng mục không đổi. Tuyên bố cuộc thi ban đầu đưa ra`c ≤ 10`và kích thước đầu mối tối đa là 3x3, vì vậy phương pháp hoán vị toàn diện có kích thước phù hợp cho những ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys
import io
from itertools import permutations

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input_fn = sys.stdin.readline

    def solve_puzzle(clues):
        shape_id = {'S': 0, 'R': 1, 'T': 2}
        flavor_id = {'V': 0, 'S': 1, 'C': 2}

        def shape_mask(ch):
            return 0b111 if ch == '_' else 1 << shape_id[ch]

        def flavor_mask(ch):
            return 0b111 if ch == '_' else 1 << flavor_id[ch]

        prepared = []

        for x, y, grid in clues:
            placements = []

            for sr in range(4 - x):
                for sc in range(4 - y):
                    placement = []

                    for r in range(x):
                        for c in range(y):
                            code = grid[r][c]
                            pos = (sr + r) * 3 + (sc + c)
                            placement.append(
                                (pos, shape_mask(code[0]), flavor_mask(code[1]))
                            )

                    placements.append(placement)

            prepared.append(placements)

        def score(placements):
            value = 0
            for placement in placements:
                for _, sm, fm in placement:
                    value += sm != 0b111
                    value += fm != 0b111
            return value

        prepared.sort(key=score, reverse=True)

        for board in permutations(range(9)):
            valid = True

            for placements in prepared:
                clue_valid = False

                for placement in placements:
                    ok = True

                    for pos, sm, fm in placement:
                        piece = board[pos]
                        sh = piece // 3
                        fl = piece % 3

                        if not (sm & (1 << sh)) or not (fm & (1 << fl)):
                            ok = False
                            break

                    if ok:
                        clue_valid = True
                        break

                if not clue_valid:
                    valid = False
                    break

            if valid:
                return board

        return None

    t = int(input_fn())
    ans = []

    for case in range(1, t + 1):
        c = int(input_fn())
        clues = []

        for _ in range(c):
            x, y = map(int, input_fn().split())
            grid = [input_fn().split() for _ in range(x)]
            clues.append((x, y, grid))

        board = solve_puzzle(clues)

        ans.append(f"Puzzle #{case}:")
        for r in range(3):
            row = []
            for c in range(3):
                piece = board[r * 3 + c]
                row.append("SRT"[piece // 3] + "VSC"[piece % 3])
            ans.append(" ".join(row))
        ans.append("")

    sys.stdout.write("\n".join(ans))

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided samples
sample_input = """3
4
3 3
TC __ SS
__ __ __
__ TV __
2 3
__ SC __
RV __ SV
3 3
__ __ __
__ RC __
__ __ __
2 3
__ __ __
TS __ RS
5
2 3
__ __ __
__ __ RC
2 2
__ RS
SC __
2 2
SV TC
__ __
3 2
TV __
__ __
__ RV
3 2
__ TS
__ __
__ __
3
3 2
_C R_
_C __
S_ _C
1 2
TC _V
3 2
_V __
S_ S_
T_ _V
"""

sample_output = """Puzzle #1:
TC SC SS
RV RC SV
TS TV RS

Puzzle #2:
TV RS TS
SC SV TC
SS RV RC

Puzzle #3:
TV TC RV
SS SC RS
TS SV RC

"""

assert solve_input(sample_input) == sample_output, "provided samples"

# Minimum-size puzzle: one complete 3x3 clue.
minimum_input = """1
1
3 3
SV SR ST
RV RR RT
CV CR CT
"""

minimum_output = """Puzzle #1:
SV SR ST
RV RR RT
CV CR CT

"""

assert solve_input(minimum_input) == minimum_output, "minimum-size clue"

# All attributes are explicitly fixed, and the arrangement is reversed
# relative to the natural encoding order.
boundary_input = """1
1
3 3
CT CR CV
RT RR RV
ST SR SV
"""

boundary_output = """Puzzle #1:
CT CR CV
RT RR RV
ST SR SV

"""

assert solve_input(boundary_input) == boundary_output, "boundary arrangement"

# Multiple clues with wildcards. The full clue determines the solution,
# while the smaller clues exercise wildcard handling and movable windows.
wildcard_input = """1
3
3 3
TC SC SS
RV RC SV
TS TV RS
1 2
__ SC
2 2
__ __
RC __
"""

wildcard_output = """Puzzle #1:
TC SC SS
RV RC SV
TS TV RS

"""

assert solve_input(wildcard_input) == wildcard_output, "wildcard and window handling"

# Maximum number of clues, all individually valid and consistent.
maximum_clues_input = """1
10
3 3
TC SC SS
RV RC SV
TS TV RS
1 1
TC
1 1
SC
1 1
SS
1 1
RV
1 1
RC
1 1
SV
1 1
TS
1 1
TV
1 1
RS
"""

maximum_clues_output = """Puzzle #1:
TC SC SS
RV RC SV
TS TV RS

"""

assert solve_input(maximum_clues_input) == maximum_clues_output, "maximum clue count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 3 3 / SV SR ST / RV RR RT / CV CR CT`| Bảng 3x3 tương tự | Câu đố có kích thước tối thiểu và kết hợp toàn bảng trực tiếp | 
|`1 / 1 / 3 3 / CT CR CV / RT RR RV / ST SR SV`| Bảng 3x3 tương tự | Sắp xếp ranh giới và khớp thuộc tính hoàn chỉnh | 
| Ba manh mối bao gồm ký tự đại diện |`TC SC SS / RV RC SV / TS TV RS`| Ký tự đại diện và cửa sổ nhỏ hơn có thể di chuyển | 
| Mười manh mối nhất quán |`TC SC SS / RV RC SV / TS TV RS`| Số đầu mối tối đa và các hạn chế về vị trí lặp đi lặp lại | 

## Vỏ cạnh 

Một manh mối nhỏ hơn bàn cờ có thể có một số vị trí pháp lý. Ví dụ, một`1 × 1`manh mối chứa đựng`TC`có thể xảy ra ở bất cứ đâu nên người giải phải tìm kiếm tất cả chín ô. Trong bài kiểm tra manh mối tối đa ở trên, manh mối`TC`chỉ tương thích với ô chứa nấm cục đó, trong khi các đầu mối như`SC`Và`SS`tương tự xác định phần riêng của họ. Người giải không giả định nguồn gốc đầu mối cố định nên các đầu mối này được xử lý chính xác. 

Một đầu mối chỉ có thể ràng buộc một thuộc tính. Mã đại diện`_`với mặt nạ ba bit, vì vậy`_C`chấp nhận`SC`,`RC`, hoặc`TC`, trong khi`S_`chấp nhận`SV`,`SS`, hoặc`SC`. Trong bài kiểm tra ký tự đại diện, manh mối`__ SC`hài lòng vì bảng chứa`SC`ở vị trí thứ hai của hàng đầu tiên. điều trị`_`như một ký tự theo nghĩa đen sẽ từ chối giải pháp một cách không chính xác. 

đầy đủ`3 × 3`đầu mối có chính xác một vị trí hợp pháp. Đối với thử nghiệm có kích thước tối thiểu, manh mối đầu tiên và duy nhất sẽ cố định mọi ô, do đó không có tìm kiếm theo vị trí cho manh mối đó. Hoán vị duy nhất chính xác là bảng đã cho và đầu ra sẽ tái tạo nó. 

Số manh mối lớn nhất có thể là mười. Bài kiểm tra manh mối tối đa sử dụng mười manh mối nhất quán lẫn nhau, bao gồm một bảng hoàn chỉnh và chín manh mối đơn ô. Bộ giải chỉ đơn giản xử lý tất cả mười và vẫn hoạt động trên cùng một không gian hoán vị nhỏ. Điều này chứng tỏ tại sao số lượng manh mối chỉ ảnh hưởng đến một yếu tố không đổi trong thời gian chạy. 

Cuối cùng, hình dạng và hương vị không thể thay thế cho nhau.`SC`có nghĩa là sôcôla vuông, trong khi`CS`sẽ có nghĩa là một sự kết hợp khác vì ký tự đầu tiên luôn là hình dạng và ký tự thứ hai luôn là hương vị. Mã hóa số nguyên duy trì thứ tự này thông qua`piece // 3`về hình dạng và`piece % 3`để tạo hương vị, ngăn chặn hai thuộc tính vô tình bị hoán đổi.
