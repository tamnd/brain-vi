---
title: "CF 102263F - Ghế Nhạc"
description: "Chúng ta có n người chơi xếp xung quanh một vòng tròn và Essa bắt đầu ở vị trí p. Có n - 1 vòng loại trừ. Đối với vòng i, bài hát kéo dài [i] giây và thứ tự ghế[i] là chiếc ghế sẽ không còn trống khi vòng đó kết thúc."
date: "2026-08-17T20:00:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "F"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 147
verified: true
draft: false
---

[CF 102263F - Ghế âm nhạc](https://codeforces.com/problemset/problem/102263/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`người chơi sắp xếp xung quanh một vòng tròn và Essa bắt đầu vào vị trí`p`. có`n - 1`các vòng loại trừ. Đối với vòng`i`, bài hát kéo dài`a[i]`giây, và cái ghế`order[i]`là chiếc ghế sẽ không còn trống khi vòng đó kết thúc. Trước mỗi bài hát, Essa có thể chọn chuyển động theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ và mỗi người chơi sẽ di chuyển một vị trí mỗi giây theo hướng đã chọn. 

Chi tiết quan trọng là vòng tròn không cố định. Sau khi loại bỏ một chiếc ghế, vị trí tương ứng sẽ biến mất khỏi vòng tròn nên tất cả các vị trí sau nó sẽ dịch chuyển một vị trí. Do đó, vòng tiếp theo sẽ diễn ra theo một vòng tròn có ít vị trí hơn. Đầu vào cung cấp độ dài bài hát và thứ tự chính xác các ghế biến mất. Chúng ta chỉ cần quyết định xem liệu ít nhất một chuỗi lựa chọn theo chiều kim đồng hồ và ngược chiều kim đồng hồ có giúp Essa sống sót hay không.`n - 1`vòng. Vấn đề chính thức có`n <= 1000`, giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Độ dài bài hát có thể lớn như`10^9`, vì vậy việc mô phỏng từng giây một là không thể. Chỉ có modulo độ dài của kích thước vòng tròn hiện tại mới quan trọng vì việc di chuyển theo kích thước vòng tròn sẽ đưa người chơi trở lại vị trí cũ. Từ`n`chỉ là 1000, một`O(n^2)`chương trình động là thực tế, trong khi việc liệt kê theo cấp số nhân của tất cả các chuỗi hướng thì không. 

Có một số trường hợp dễ xảy ra khi việc triển khai trực tiếp có thể âm thầm gặp trục trặc. Đầu tiên là khi độ dài bài hát lớn hơn rất nhiều so với vòng tròn hiện tại. Ví dụ,```
2 1
2
1
```có câu trả lời`No`. Có hai vị trí, Essa bắt đầu ở vị trí 1 và di chuyển hai bước theo một trong hai hướng sẽ khiến anh ta ở vị trí 1. Ghế 1 là chiếc ghế bị loại nên anh ta thua cuộc. Việc quên thao tác modulo hoặc xử lý hai hướng như thể chúng khác nhau sẽ cho kết quả không chính xác. 

Một trường hợp tinh tế khác là khi chiếc ghế được tháo ra nằm ở vị trí bên trong. Ví dụ,```
5 3
4 4 4 4
4 3 2 1
```có câu trả lời`Yes`. Sau khi chiếc ghế biến mất, các vị trí sau chiếc ghế đó sẽ được đánh số lại. Nếu chúng ta giữ chỉ số cũ của Essa mà không nén nó thì các vòng sau sẽ sử dụng sai vị trí. 

Trường hợp cạnh thứ ba xảy ra khi cả hai lựa chọn hướng đều dẫn đến cùng một vị trí. Điều này xảy ra bất cứ khi nào`2 * a[i]`chia hết cho kích thước vòng tròn hiện tại. Ví dụ,```
3 1
3 1
1 2
```có câu trả lời`No`. Ở vòng đầu tiên, vòng tròn có kích thước 3, ba bước ở một trong hai hướng khiến Essa ở vị trí 1, chính là chiếc ghế bị loại. 

## Đầu vào 

Dòng đầu tiên chứa`n`Và`p`, Ở đâu`n`là số người chơi ban đầu và`p`là vị trí ban đầu của Essa khi sử dụng chỉ mục dựa trên một. 

Dòng thứ hai chứa`n - 1`độ dài bài hát. các`i`-giá trị thứ là số giây cho vòng`i`. 

Dòng thứ ba chứa`n - 1`nhãn ghế riêng biệt. các`i`-giá trị thứ xác định chiếc ghế biến mất trong vòng`i`. 

Các nhãn ghế đều khác biệt nên việc duy trì những chiếc ghế còn sót lại một cách rõ ràng là điều dễ dàng đối với`n <= 1000`. 

## Đầu ra 

In`Yes`nếu tồn tại một chuỗi các quyết định theo chiều kim đồng hồ và ngược chiều kim đồng hồ cho phép Essa ở lại cho đến cuối cùng. Ngược lại, in`No`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi chuỗi hướng dẫn có thể. Có hai sự lựa chọn trong mỗi`n - 1`vòng, vì vậy có`2^(n-1)`các trình tự có thể. Đối với mỗi chuỗi, chúng tôi có thể mô phỏng vị trí của Essa qua tất cả các vòng và cập nhật vòng tròn sau mỗi lần loại. Ngay cả khi mỗi vòng được xử lý trong thời gian không đổi sau khi duy trì vòng tròn, điều này mang lại`O(n * 2^n)`làm việc trong trường hợp xấu nhất. Tại`n = 1000`, đây là theo thứ tự của`1000 * 2^1000`chuyển đổi trạng thái, điều này hoàn toàn không thể thực hiện được. 

Lực lượng vũ phu là chính xác vì một chuỗi hướng hoàn chỉnh quyết định hoàn toàn quỹ đạo của Essa. Vấn đề là nhiều chuỗi hướng khác nhau đạt đến cùng một vị trí trong cùng một vòng. Một khi hai lịch sử đã hợp nhất vào cùng một vị trí hiện tại thì những khác biệt trước đó của chúng không còn liên quan nữa. Tương lai chỉ phụ thuộc vào vòng tròn hiện tại và vị trí hiện tại của Essa. 

Điều này mang lại cho chúng ta trạng thái lập trình động tự nhiên. Vào đầu mỗi hiệp đấu, thay vì ghi nhớ toàn bộ chuỗi các quyết định, hãy nhớ mọi vị trí mà Essa hiện có thể đảm nhiệm. Một mảng boolean`dp`là đủ:`dp[q]`đúng chính xác khi có một chuỗi các lựa chọn hợp lệ trước đó khiến Essa ở vị trí`q`. 

Giả sử vòng tròn hiện tại có kích thước`m`, chiếc ghế đã bị loại bỏ nằm ở vị trí mục tiêu`r`, và bài hát kéo dài`a`giây. Từ một vị trí có thể tiếp cận`q`, chuyển động theo chiều kim đồng hồ đạt tới`(q + a) mod m`và chuyển động ngược chiều kim đồng hồ đạt đến`(q - a) mod m`. 

Nếu một trong hai điểm đến bằng`r`, lựa chọn đó sẽ thua vòng và bị loại bỏ. Mọi điểm đến khác đều tồn tại, nhưng sau chiếc ghế ở`r`biến mất, vị trí lớn hơn`r`dịch sang trái một chỗ. Chúng ta có thể áp dụng cách nén này trực tiếp trong khi xây dựng mảng DP tiếp theo. 

Bản thân vòng tròn có thể được lưu trữ dưới dạng danh sách Python thông thường chứa nhãn của những chiếc ghế còn sót lại. Tìm chiếc ghế đã bị loại bỏ với`alive.index(...)`và xóa nó bằng`pop(...)`chi phí`O(n)`mỗi vòng, vốn đã nằm trong`O(n^2)`ngân sách. Không có lý do gì để giới thiệu cây Fenwick hoặc cấu trúc dữ liệu khác cho những ràng buộc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 2^n)`|`O(n)`| Quá chậm | 
| Lập trình động |`O(n^2)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các nhãn ghế còn tồn tại theo thứ tự hình tròn. Ban đầu đây là`[1, 2, ..., n]`. Chuyển đổi vị trí bắt đầu của Essa từ lập chỉ mục dựa trên một sang lập chỉ mục dựa trên 0 và đánh dấu vị trí đó là vị trí duy nhất có thể tiếp cận. 
2. Với mỗi bài hát, hãy`m`là số vị trí còn sống hiện tại. Xác định vị trí chiếc ghế bị loại bỏ trong`alive`liệt kê và gọi chỉ mục dựa trên số 0 của nó`r`. Chỉ số là yếu tố quan trọng đối với chuyển động, vì vòng tròn được thể hiện bằng các vị trí liên tiếp chứ không phải bằng nhãn ghế ban đầu. 
3. Giảm modulo độ dài bài hát`m`. Nếu bài hát kéo dài`a`giây, di chuyển`a`vị trí hoàn toàn giống với việc di chuyển`a % m`vị trí xung quanh một vòng tròn có kích thước`m`. Điều này tránh thực hiện bất kỳ công việc nào tỷ lệ thuận với độ dài bài hát có thể rất lớn. 
4. Đối với mọi vị trí có thể tiếp cận`q`, tính toán cả hai điểm đến có thể. Điểm đến theo chiều kim đồng hồ là`(q + step) % m`, và đích ngược chiều kim đồng hồ là`(q - step) % m`. 
5. Từ chối mọi điểm đến bằng`r`. Vị trí đó không còn ghế ở cuối bài hát nên Essa sẽ là người chơi bị loại ở vòng đó. 
6. Đối với mọi điểm đến còn sót lại`x`, chuyển đổi nó thành chỉ mục của nó sau ghế`r`được gỡ bỏ. Nếu như`x < r`, chỉ số của nó không thay đổi. Nếu như`x > r`, nó trở thành`x - 1`. Lưu trữ vị trí kết quả trong mảng DP tiếp theo. 
7. Tháo ghế tương ứng ra khỏi`alive`và tiếp tục với bài hát tiếp theo. Nếu mảng DP trở nên trống thì mọi chiến lược khả thi đều đã bị mất, vì vậy câu trả lời là ngay lập tức`No`. 
8. Rốt cuộc thì`n - 1`vòng, vẫn còn chính xác một vị trí. Nếu có thể đạt được vị trí cuối cùng đó, Essa có chuỗi lựa chọn hướng chiến thắng, vì vậy hãy in`Yes`. 

Điều bất biến là ngay trước mỗi vòng,`dp[q]`hoàn toàn đúng với các vị trí trong vòng tròn hiện tại nơi Essa có thể đứng sau một số chuỗi lựa chọn tồn tại ở mọi vòng trước. Quá trình chuyển đổi xem xét cả hai lựa chọn hợp pháp từ mọi vị trí như vậy, loại bỏ chính xác các vị trí tương ứng với chiếc ghế bị loại và sau đó chuyển đổi các vị trí còn sống sang chỉ số mới sau khi vòng tròn co lại. Do đó, bất biến được bảo toàn sau mỗi vòng. Cuối cùng, một vị trí có thể tiếp cận được tồn tại chính xác khi có một chuỗi lựa chọn hoàn chỉnh mà Essa không bao giờ bị loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    values = list(map(int, data.split()))
    it = iter(values)

    n = next(it)
    p = next(it)

    songs = [next(it) for _ in range(n - 1)]
    order = [next(it) for _ in range(n - 1)]

    alive = list(range(1, n + 1))

    dp = [False] * n
    dp[p - 1] = True

    for song, removed_chair in zip(songs, order):
        m = len(alive)

        # Index of the chair that disappears in the current circle.
        r = alive.index(removed_chair)

        # Only the remainder modulo the current circle size matters.
        step = song % m

        after_move = [False] * m

        for q in range(m):
            if not dp[q]:
                continue

            clockwise = (q + step) % m
            if clockwise != r:
                after_move[clockwise] = True

            counterclockwise = (q - step) % m
            if counterclockwise != r:
                after_move[counterclockwise] = True

        if not any(after_move):
            return "No"

        # Remove the chair and compress all surviving positions.
        next_dp = [False] * (m - 1)

        for x in range(m):
            if not after_move[x]:
                continue

            new_pos = x if x < r else x - 1
            next_dp[new_pos] = True

        dp = next_dp
        alive.pop(r)

    return "Yes" if dp[0] else "No"

def main():
    data = sys.stdin.buffer.read().decode()
    print(solve(data))

if __name__ == "__main__":
    main()
```các`alive`list đại diện cho vòng tròn hiện tại theo thứ tự vòng tròn. Giá trị của nó là nhãn ghế ban đầu, cho phép chúng tôi tìm thấy chiếc ghế từ đầu vào mà không mất dấu chiếc ghế vật lý nào đang bị loại bỏ. 

Mảng DP được lập chỉ mục theo vị trí hình tròn hiện tại chứ không phải theo nhãn ghế ban đầu. Sự khác biệt này rất cần thiết vì việc loại bỏ một chiếc ghế sẽ thay đổi vị trí của tất cả các ghế sau nó. các`alive.index(removed_chair)`cuộc gọi chuyển đổi nhãn ghế ban đầu thành vị trí hiện tại. 

biểu thức`song % m`là cần thiết vì sự chuyển động có tính tuần hoàn. Số nguyên Python không bị tràn, do đó, ngay cả độ dài bài hát tối đa cũng an toàn, nhưng việc giảm sớm cũng giúp quá trình chuyển đổi trở nên đơn giản. 

Hai điểm đến được tính toán độc lập. Chúng có thể bằng nhau, ví dụ khi kích thước vòng tròn là 2 hoặc khi chuyển động là nửa vòng tròn. Việc chỉ định cả hai đích cho cùng một ô mảng boolean sẽ xử lý trường hợp đó một cách tự nhiên. 

Bước nén sử dụng`x if x < r else x - 1`. Các vị trí trước ghế đã loại bỏ vẫn giữ nguyên chỉ số, trong khi mỗi vị trí sau ghế sẽ dịch chuyển sang trái một. Bản thân vị trí bị xóa đã bị loại bỏ nên nó không bao giờ được truy cập trong mảng tiếp theo. 

Mảng DP cuối cùng có độ dài bằng một. Kiểm tra`dp[0]`là đủ vì trò chơi chỉ còn lại một vị trí sau lần loại cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu, Essa bắt đầu ở vị trí 2 dựa trên số 0. Các nhãn ghế còn sót lại và các vị trí có thể tiếp cận sẽ phát triển như sau. 

| Vòng | Vòng tròn trước khi xóa | Ghế bỏ đi |`m`|`step`| Có thể truy cập trước khi di chuyển | Có thể truy cập sau khi di chuyển | Có thể truy cập sau khi xóa | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 |`[1,2,3,4,5]`| 4 | 5 | 4 |`{2}`|`{1}`|`{1}`| 
| 2 |`[1,2,3,5]`| 3 | 4 | 0 |`{1}`|`{1}`|`{1}`| 
| 3 |`[1,2,5]`| 2 | 3 | 1 |`{1}`|`{0,2}`|`{0,1}`| 
| 4 |`[1,5]`| 1 | 2 | 0 |`{0,1}`|`{0,1}`|`{1}`| 

Ở vòng đầu tiên, di chuyển bốn vị trí theo chiều kim đồng hồ từ vị trí 2 sẽ đến vị trí 1, đồng thời di chuyển bốn vị trí ngược chiều kim đồng hồ sẽ đến vị trí 3. Vị trí 3 là chiếc ghế bị loại bỏ nên chỉ có vị trí 1 còn sót lại. Sau vòng thứ hai, độ dài bài hát được chia cho kích thước vòng tròn nên Essa không thay đổi vị trí. Các vòng sau lại phân nhánh và ở vòng cuối cùng, ít nhất một trạng thái có thể tiếp cận được sẽ tránh được chiếc ghế bị loại bỏ. Câu trả lời là`Yes`. 

### Ví dụ về va chạm cưỡng bức 

Hãy xem xét:```
3 1
3 1
1 2
```Bài hát đầu tiên có độ dài 3 trong khi vòng tròn có kích thước 3 nên chuyển động hiệu quả bằng không. 

| Vòng | Vòng tròn trước khi xóa | Ghế bỏ đi |`m`|`step`| Có thể truy cập trước khi di chuyển | Điểm đến | Có thể truy cập sau khi xóa | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 |`[1,2,3]`| 1 | 3 | 0 |`{0}`|`{0}`|`{}`| 

Cả chuyển động theo chiều kim đồng hồ và ngược chiều kim đồng hồ đều khiến Essa ở vị trí 0 vì ba bước hoàn thành một vòng quay đầy đủ. Vị trí 0 chính xác là chiếc ghế bị loại. Không có bang nào sống sót qua vòng đầu tiên, vì vậy câu trả lời là`No`. 

Ví dụ này chứng minh tại sao DP phải kiểm tra điểm đến dựa vào chiếc ghế đã được loại bỏ sau khi áp dụng chuyển động. Việc kiểm tra xem Essa có bắt đầu ngồi trên ghế an toàn hay không là chưa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| có`n - 1`vòng và mỗi vòng sẽ quét nhiều nhất`n`DP định vị và biểu diễn`O(n)`làm việc trong danh sách ghế còn sống. | 
| Không gian |`O(n)`| Mảng DP hiện tại và tiếp theo, cùng với danh sách ghế còn tồn tại, mỗi mảng chứa tối đa`n`các phần tử. | 

Với`n <= 1000`DP chỉ thực hiện khoảng vài triệu thao tác đơn giản trong trường hợp xấu nhất. Độ dài bài hát không ảnh hưởng đến thời gian chạy vì mỗi bài được giảm theo kích thước vòng tròn hiện tại. Việc sử dụng bộ nhớ là tuyến tính và thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve(data: str) -> str:
    values = list(map(int, data.split()))
    it = iter(values)

    n = next(it)
    p = next(it)

    songs = [next(it) for _ in range(n - 1)]
    order = [next(it) for _ in range(n - 1)]

    alive = list(range(1, n + 1))

    dp = [False] * n
    dp[p - 1] = True

    for song, removed_chair in zip(songs, order):
        m = len(alive)
        r = alive.index(removed_chair)
        step = song % m

        after_move = [False] * m

        for q in range(m):
            if not dp[q]:
                continue

            x = (q + step) % m
            if x != r:
                after_move[x] = True

            x = (q - step) % m
            if x != r:
                after_move[x] = True

        if not any(after_move):
            return "No"

        next_dp = [False] * (m - 1)

        for x in range(m):
            if after_move[x]:
                next_dp[x if x < r else x - 1] = True

        dp = next_dp
        alive.pop(r)

    return "Yes" if dp[0] else "No"

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample
assert run("""\
5 3
4 4 4 4
4 3 2 1
""") == "Yes", "sample 1"

# Minimum size, large even song length.
# Essa starts on the chair that disappears and makes a full number
# of revolutions, so both directions lose.
assert run("""\
2 1
2
1
""") == "No", "minimum size and modulo"

# Minimum size, boundary starting position.
# Essa starts at chair 2, so the same even movement keeps him safe.
assert run("""\
2 2
2
1
""") == "Yes", "minimum size, last position"

# Forced collision caused by the song length being a multiple
# of the current circle size.
assert run("""\
3 1
3 1
1 2
""") == "No", "forced collision"

# Boundary starting position with a changing circle.
assert run("""\
3 3
1 1
3 2
""") == "Yes", "initial position n"

# Maximum-size case. All songs are equal and the elimination order
# is increasing. The DP still finishes with a reachable position.
n = 1000
maximum_case = (
    f"{n} 1\n"
    + " ".join(["1"] * (n - 1))
    + "\n"
    + " ".join(map(str, range(1, n)))
    + "\n"
)
assert run(maximum_case) == "Yes", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 2 / 1`|`No`| Kích thước tối thiểu và chuyển động theo kích thước vòng tròn | 
|`2 2 / 2 / 1`|`Yes`| Vị trí bắt đầu ranh giới tại`p = n`| 
|`3 1 / 3 1 / 1 2`|`No`| Cả hai lựa chọn hướng đều sụp đổ đến cùng một vị trí thua | 
|`3 3 / 1 1 / 3 2`|`Yes`| Vị trí ban đầu tại nhãn ghế cuối cùng và vị trí nén | 
|`n = 1000`, tất cả các bài hát`1`, thứ tự loại bỏ tăng dần |`Yes`| Kích thước đầu vào tối đa và độ dài bài hát bằng nhau | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2 1
2
1
```vòng tròn hiện tại có kích thước 2 và độ dài bài hát giảm xuống`2 % 2 = 0`. Do đó, Essa vẫn ở vị trí 0 bất kể hướng nào. Vì ghế 1 ở vị trí 0 và bị loại bỏ nên cả hai lựa chọn đều bị từ chối. DP trở nên trống rỗng và quay trở lại`No`. 

Đối với cùng một vòng tròn với Essa ở ranh giới khác,```
2 2
2
1
```vị trí dựa trên số 0 ban đầu của anh ta là 1. Chuyển động hiệu quả vẫn bằng 0, nhưng vị trí 1 không phải là chiếc ghế bị loại bỏ ở vị trí 0. Trạng thái tồn tại trong vòng duy nhất, để lại một vị trí có thể tiếp cận được, vì vậy câu trả lời là`Yes`. 

Để loại bỏ nội thất,```
5 3
4 4 4 4
4 3 2 1
```chiếc ghế bị loại bỏ đầu tiên, số 4, có chỉ số hiện tại là 3. Essa bắt đầu ở chỉ số 2. Với chuyển động`4 mod 5 = 4`, hai đích đến là chỉ số 1 và 3. Chỉ số 3 bị loại bỏ, để lại chỉ số 1. Sau khi loại bỏ ghế 4, vòng tròn sống sót sẽ trở thành`[1,2,3,5]`, vì vậy chỉ số 1 vẫn đề cập đến ghế 2. Việc nén giúp giữ cho các vị trí tiếp theo luôn chính xác. 

Đối với va chạm cưỡng bức,```
3 1
3 1
1 2
```chuyển động đầu tiên là`3 mod 3 = 0`. Essa bắt đầu ở chỉ số 0, cả hai hướng đều đạt chỉ số 0 và chỉ số 0 là chiếc ghế bị loại. DP không còn trạng thái sống sót sau vòng đầu tiên nên các bài hát sau không bao giờ cần phải xem xét. 

Đối với trường hợp kích thước tối đa, vòng tròn bắt đầu với 1000 vị trí và thu nhỏ từng vị trí một. Mỗi bài hát có độ dài 1 nên mỗi vòng chỉ xét hai điểm đến liền kề. Mặc dù có thể tồn tại nhiều chuỗi hướng khác nhau, DP sẽ hợp nhất tất cả các chuỗi đạt đến cùng một vị trí. Tối đa 1000 trạng thái được kiểm tra trong bất kỳ vòng nào, đưa ra mức tăng trưởng bậc hai thay vì tăng trưởng theo cấp số nhân.
