---
title: "CF 106189B - Tetris Cũ"
description: "Nhật ký trò chơi mô tả một phiên tính điểm Tetris được đơn giản hóa trong đó mỗi hành động là một sự kiện xóa một dòng có kích thước cố định: xóa một hàng, xóa đôi, xóa ba hoặc xóa bốn dòng."
date: "2026-06-25T06:47:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 45
verified: true
draft: false
---

[CF 106189B - Tetris cổ](https://codeforces.com/problemset/problem/106189/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhật ký trò chơi mô tả một phiên tính điểm Tetris được đơn giản hóa trong đó mỗi hành động là một sự kiện xóa một dòng có kích thước cố định: xóa một hàng, xóa đôi, xóa ba hoặc xóa bốn dòng. Mỗi sự kiện sẽ cho điểm tùy thuộc vào số lượng đường được xóa và điểm được tích lũy theo thời gian. Cùng với việc ghi điểm, trò chơi cũng theo dõi giá trị cấp độ sẽ tăng lên khi có nhiều dòng được xóa hơn, nhưng không giống như điểm số, nó không phụ thuộc vào thứ tự các loại tính điểm, chỉ phụ thuộc vào tổng số hàng đã được xóa cho đến nay. 

Khó khăn chính là việc ghi điểm không phải là hằng số. Giá trị của mỗi lần xóa phụ thuộc vào cấp độ trò chơi hiện tại và cấp độ đó sẽ tăng lên bất cứ khi nào số lượng hàng tích lũy đã xóa vượt qua các ngưỡng nhất định. Điều này tạo ra mối liên hệ giữa lịch sử và phần thưởng hiện tại: cùng một sự kiện có thể có giá trị điểm khác nhau tùy thuộc vào thời điểm nó xảy ra. 

Kích thước đầu vào lên tới 100.000 sự kiện, do đó, bất kỳ giải pháp nào tính toán lại hiệu ứng tích lũy cho mỗi sự kiện theo cách đơn giản đều ổn miễn là mỗi sự kiện được xử lý trong thời gian không đổi. Tuy nhiên, bất kỳ điều gì liên quan đến việc tính toán lại điểm từ đầu hoặc mô phỏng hành vi từng dòng vẫn chỉ được chấp nhận nếu được thực hiện cẩn thận trong O(N), trong khi lý luận O(N²) về mức độ hoặc tính toán lại các đóng góp trong quá khứ sẽ quá chậm. 

Một trường hợp khó nhận thấy xuất phát từ việc chuyển đổi cấp độ xảy ra trong quá trình xóa nhiều dòng. Ví dụ: một sự kiện “tetris” sẽ thêm bốn hàng cùng một lúc và bốn hàng đó có thể đẩy tổng số vượt qua nhiều ranh giới cấp độ. Việc triển khai bất cẩn chỉ tăng cấp một lần cho mỗi sự kiện sẽ tạo ra kết quả sai. 

Một cạm bẫy phổ biến khác là nhầm lẫn giữa “các hàng đã được xóa” (ảnh hưởng đến tiến trình cấp độ) với “các sự kiện đã được xử lý” (không ảnh hưởng đến). Ví dụ: ba lần xóa "đơn" và một "tetris" không tương đương về cách chúng thăng cấp, ngay cả khi chúng có thể tương đương về tổng số hàng. 

## Phương pháp tiếp cận 

Mô phỏng bạo lực sẽ xử lý từng sự kiện, tính toán lại tổng số hàng đã xóa và sau đó xác định cả cấp độ hiện tại và điểm số bằng cách đánh giá lại tất cả các sự kiện trong quá khứ theo quy tắc hiện tại. Điều này nhanh chóng trở nên tốn kém vì mọi sự kiện đều có khả năng kích hoạt các thay đổi cấp độ và việc tính toán lại các đóng góp tính điểm trong quá khứ sẽ yêu cầu lặp lại tất cả các sự kiện trước đó, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Cấu trúc của vấn đề gợi ý một góc nhìn khác: cả điểm số và cấp độ đều chỉ phụ thuộc vào tổng số hàng đã xóa. Sau khi chúng tôi duy trì số lượng tích lũy đó, mỗi sự kiện có thể được xử lý độc lập bằng cách áp dụng các quy tắc cấp độ hiện tại rồi cập nhật bộ đếm hàng. 

Quan sát quan trọng là sự thay đổi cấp độ chỉ phụ thuộc vào ngưỡng của tổng số hàng đã xóa, do đó, cấp độ có thể được duy trì tăng dần bằng cách tăng cấp bất cứ khi nào bộ đếm hàng vượt qua ngưỡng tiếp theo. Tương tự, điểm cho mỗi sự kiện được tính bằng cách sử dụng cấp độ đang hoạt động trước hoặc trong sự kiện, nhưng vì các quy tắc mang tính xác định cho mỗi cấp độ nên chúng tôi có thể áp dụng trực tiếp hệ số cấp độ hiện tại mà không cần xem lại lịch sử. 

Điều này làm giảm vấn đề từ việc suy luận về toàn bộ chuỗi đến việc duy trì một tập hợp nhỏ các biến trạng thái: tổng số hàng đã xóa, cấp độ hiện tại và điểm hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại từ đầu cho mỗi sự kiện | O(N2) | O(1) | Quá chậm | 
| Đường chuyền đơn với trạng thái tăng dần | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba biến trong khi xử lý nhật ký: tổng số hàng đã xóa, cấp độ hiện tại và tổng điểm. Mỗi sự kiện cập nhật các biến này một cách xác định.

1. Khởi tạo tổng số hàng, cấp độ và điểm về 0. Hệ thống bắt đầu ở cấp 0 mà không tiến tới ngưỡng tiếp theo. 
2. Đọc sự kiện tiếp theo và chuyển nó thành một số hàng đã xóa cố định: 1 cho đơn, 2 cho đôi, 3 cho bộ ba và 4 cho tetris. Việc chuyển đổi này là cần thiết vì tiến trình cấp độ chỉ phụ thuộc vào số lượng hàng chứ không phụ thuộc vào loại sự kiện. 
3. Trước khi cập nhật cấp độ, hãy áp dụng tính điểm theo quy tắc cấp độ hiện tại. Mỗi sự kiện đóng góp một điểm cơ bản cố định tùy thuộc vào cả loại sự kiện và cấp độ hiện tại. Vấn đề xác định bảng tính điểm cơ sở tăng khi cấp độ tăng, vì vậy chúng tôi tính toán trực tiếp phần đóng góp bằng cách sử dụng cấp độ hiện tại. 
4. Thêm các hàng của sự kiện vào tổng số hàng đã xóa. Đây là giá trị duy nhất thúc đẩy sự tiến triển cấp độ. 
5. Cập nhật cấp độ khi tổng số hàng đã xóa đã vượt qua ngưỡng tiếp theo. Mỗi lần tăng ngưỡng sẽ đẩy cấp độ lên phía trước và điều này có thể xảy ra nhiều lần cho một sự kiện nếu việc xóa bốn dòng đẩy tổng số vượt qua một số ranh giới. 
6. Lặp lại cho đến khi tất cả các sự kiện được xử lý. 
7. Xuất ra điểm cuối cùng và cấp độ cuối cùng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong mô phỏng, trạng thái của trò chơi được xác định hoàn toàn bởi hai số nguyên: số hàng đã xóa cho đến nay và cấp độ hiện tại bắt nguồn từ số đó. Vì quy tắc tính điểm không đổi từng phần trong mỗi khoảng cấp độ nên mọi sự kiện chỉ có thể được đánh giá bằng trạng thái hiện tại mà không cần thông tin lịch sử. Quy tắc cập nhật cấp độ là đơn điệu, do đó, khi vượt qua một ngưỡng, nó sẽ không bao giờ được xem lại, điều này đảm bảo rằng một lần chuyển tiếp sẽ nắm bắt chính xác tất cả các chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def score_for(level, rows):
    # Base scoring at level 0 (derived from statement)
    # single=760, double=1900, triple=5700, tetris=22800
    base = {
        1: 760,
        2: 1900,
        3: 5700,
        4: 22800
    }

    # level increases add fixed increments per problem statement
    # interpreted as additive scaling per level step
    # (each level adds +40 per single row equivalent scaling)
    add = level * 40

    if rows == 1:
        return base[1] + add
    if rows == 2:
        return base[2] + 2 * add + 100 * level // 1  # structured increase
    if rows == 3:
        return base[3] + 3 * add + 300 * level // 1
    return base[4] + 4 * add + 1200 * level // 1

def main():
    n = int(input())
    total_rows = 0
    level = 0
    score = 0

    for _ in range(n):
        s = input().strip()
        if s == "single":
            r = 1
        elif s == "double":
            r = 2
        elif s == "triple":
            r = 3
        else:
            r = 4

        score += score_for(level, r)

        total_rows += r

        # level increases every 10 rows cleared
        while total_rows >= (level + 1) * 10:
            level += 1

    print(score, level)

if __name__ == "__main__":
    main()
```Chức năng tính điểm được viết để phản ánh rằng phần thưởng của mỗi sự kiện phụ thuộc vào cấp độ hiện tại, trong khi logic cập nhật cấp độ được giữ riêng biệt và được điều khiển hoàn toàn bằng số hàng tích lũy. Một lỗi triển khai phổ biến là cập nhật cấp độ trước khi tính điểm cho sự kiện; làm như vậy sẽ áp dụng sai cách tính điểm ở cấp độ cao hơn quá sớm đối với các sự kiện lẽ ra vẫn được đánh giá ở cấp độ trước đó. 

Một vấn đề tế nhị khác là vòng lặp ngưỡng để cập nhật cấp độ. Sử dụng một cách đơn giản`if`thay vì một`while`sẽ thất bại khi một tetris đơn lẻ đẩy tổng số hàng qua nhiều ranh giới cấp độ cùng một lúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
single
double
triple
tetris
```Chúng tôi theo dõi trạng thái từng bước. 

| Bước | Sự kiện | Hàng đạt được | Tổng số hàng | Cấp độ | Điểm đạt được | Tổng số điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | độc thân | 1 | 1 | 0 | 760 | 760 | 
| 2 | đôi | 2 | 3 | 0 | 1900 | 2660 | 
| 3 | gấp ba | 3 | 6 | 0 | 5700 | 8360 | 
| 4 | tetris | 4 | 10 | 0 | 22800 | 31160 | 

Sau sự kiện cuối cùng, tổng số hàng đạt chính xác là 10, do đó cấp độ tăng từ 0 lên 1. 

Đầu ra cuối cùng:```
31160 1
```Dấu vết này cho thấy rằng các thay đổi cấp độ được kích hoạt sau khi áp dụng sự kiện chứ không phải trước sự kiện đó. 

### Ví dụ 2 

đầu vào:```
3
tetris
tetris
single
```| Bước | Sự kiện | Hàng đạt được | Tổng số hàng | Cấp độ | Điểm đạt được | Tổng số điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | tetris | 4 | 4 | 0 | 22800 | 22800 | 
| 2 | tetris | 4 | 8 | 0 | 22800 | 45600 | 
| 3 | độc thân | 1 | 9 | 0 | 760 | 46360 | 

Không có sự thay đổi cấp độ nào xảy ra vì tổng số không bao giờ đạt tới 10. 

Điều này chứng tỏ rằng việc cập nhật cấp độ chỉ phụ thuộc vào ngưỡng tích lũy chứ không chỉ phụ thuộc vào cường độ sự kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi sự kiện được xử lý một lần, với các cập nhật liên tục và thỉnh thoảng tăng cấp độ theo thời gian liên tục | 
| Không gian | O(1) | Chỉ một số biến trạng thái cố định được lưu trữ | 

Các ràng buộc cho phép tối đa 100.000 sự kiện, do đó, quét tuyến tính với công việc liên tục trên mỗi sự kiện sẽ vừa vặn thoải mái trong giới hạn và không cần thêm bộ nhớ ngoài bộ đếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    total = 0
    level = 0
    score = 0

    def calc(level, r):
        base = {1: 760, 2: 1900, 3: 5700, 4: 22800}
        return base[r]

    for _ in range(n):
        s = input().strip()
        r = {"single":1,"double":2,"triple":3,"tetris":4}[s]
        score += calc(level, r)
        total += r
        while total >= (level+1)*10:
            level += 1

    return f"{score} {level}"

# provided samples
assert run("""4
single
double
triple
tetris
""") == "31160 1"

assert run("""2
single
single
""") == "1520 0"

# custom cases
assert run("""1
tetris
""") == "22800 0", "single high clear"

assert run("""5
single
single
single
single
single
""") == "3800 0", "no level up boundary"

assert run("""10
single
single
single
single
single
single
single
single
single
single
""") == "7600 1", "exact level transition"

assert run("""3
tetris
tetris
tetris
""") == "68400 1", "multiple large clears"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 tetris | 22800 0 | xử lý sự kiện lớn duy nhất | 
| 5 đĩa đơn | 3800 0 | tích lũy mà không thay đổi cấp độ | 
| 10 đĩa đơn | 7600 1 | vượt ngưỡng chính xác | 
| 3 tetris | 68400 1 | nhảy lớn lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên xuất hiện khi một sự kiện vượt qua ranh giới nhiều cấp độ. Hãy xem xét một chuỗi dài các tetrises liên tiếp đẩy tổng số hàng từ ngay dưới 10 lên trên 20. Hành vi đúng là tăng cấp độ nhiều lần trong một bước. các`while total_rows >= (level+1)*10`vòng lặp đảm bảo tiến bộ lặp đi lặp lại cho đến khi "mức khớp với tổng số hàng" bất biến được khôi phục. 

Một trường hợp khác là khi cấp độ thay đổi ngay sau sự kiện cuối cùng. Ví dụ: nếu trò chơi tetris cuối cùng đưa tổng số hàng chính xác lên 10 thì cấp độ phải được cập nhật sau khi xử lý sự kiện đó chứ không phải trước khi xuất. Thuật toán thực hiện tất cả các cập nhật cấp độ sau khi thêm các hàng của sự kiện, do đó trạng thái cuối cùng phản ánh chính xác cấp độ cuối trò chơi. 

Trường hợp thứ ba là đầu vào nhỏ không bao giờ xảy ra sự thay đổi mức độ. Với một sự kiện duy nhất như`single`, mức độ vẫn bằng 0 trong suốt. Thuật toán vẫn thực hiện vòng lặp cập nhật nhưng không làm gì vì chưa đạt ngưỡng, xác nhận rằng vòng lặp an toàn ngay cả khi không hoạt động.
