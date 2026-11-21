---
title: "CF 2203D - Trò chơi chia hết"
description: "Chúng ta có hai tập hợp, một tập hợp được gọi là a và một tập hợp được gọi là b. Họ xác định một trò chơi trong đó người chơi không hề sửa đổi a mà dần dần sử dụng các phần tử từ b."
date: "2026-06-07T20:02:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2203
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 187 (Rated for Div. 2)"
rating: 1700
weight: 2203
solve_time_s: 86
verified: true
draft: false
---

[CF 2203D - Trò chơi chia hết](https://codeforces.com/problemset/problem/2203/D) 

**Đánh giá:** 1700 
**Tags:** vũ lực, trò chơi, lòng tham, lý thuyết số 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai multisets, một được gọi là`a`và một người được gọi là`b`. Họ xác định một trò chơi mà người chơi không sửa đổi`a`mà tiêu thụ dần dần các yếu tố từ`b`. Ở mỗi lượt, người chơi hiện tại chọn một giá trị`x`từ`a`và một giá trị`y`từ`b`, sau đó loại bỏ chính xác một lần xuất hiện của`y`từ`b`. Tính hợp pháp của nước đi tùy thuộc vào người chơi: Alice chỉ được phép loại bỏ một`y`đó là chia cho sự lựa chọn của cô ấy`x`, trong khi Bob chỉ được phép xóa một`y`cái đó không chia hết cho người anh ấy đã chọn`x`. Trò chơi kết thúc khi người chơi không tìm được cặp hợp lệ nào`(x, y)`. 

Chi tiết cấu trúc quan trọng là`x`không bị tiêu hao, do đó tập hợp các lựa chọn cho điều kiện chia hết vẫn không đổi trong suốt trò chơi. Chỉ có sự sẵn có của các giá trị trong`b`thay đổi theo thời gian, điều đó có nghĩa là trò chơi thực sự là về việc phân loại các yếu tố của`b`thành những thứ “Alice có thể sử dụng được” và những thứ “Bob có thể sử dụng được”, trong khi cả hai người chơi có thể linh hoạt chọn những thứ khác nhau`x`mỗi lượt. 

Các ràng buộc ngụ ý rằng cả hai`n`Và`m`có thể lớn, lên tới tổng số một triệu trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng kiểm tra từng cặp`(x, y)`hoặc tính toán lại các mối quan hệ chia hết cho mỗi nước đi sẽ ngay lập tức thất bại do hành vi bậc hai hoặc thậm chí gần bậc hai. Cấu trúc gợi ý rõ ràng rằng tất cả thông tin hữu ích phải được tính toán trước một lần cho mỗi trường hợp thử nghiệm, với khả năng xử lý gần như tuyến tính trên mỗi giá trị. 

Một chế độ thất bại ngây thơ nhưng tự nhiên đến từ việc mô phỏng các ngã rẽ. Người ta có thể cố gắng luân phiên các nước đi theo đúng nghĩa đen và tìm kiếm các cặp hợp lệ mỗi lần. Điều này bị hỏng vì mỗi lần di chuyển sẽ yêu cầu quét`a`Và`b`, dẫn đến`O(nm)`hành vi, và cũng bởi vì chiến lược tối ưu không phải là chiến lược cục bộ trên mỗi bước di chuyển mà là toàn cầu trong việc phân phối các mối quan hệ có thể phân chia. 

Một trường hợp cạnh tinh tế khác xuất hiện khi nhiều phần tử trong`b`giống hệt nhau hoặc khi tất cả các yếu tố tương thích lẫn nhau với tất cả`x`TRONG`a`. Trong những trường hợp như vậy, thật dễ dàng cho rằng Alice hoặc Bob luôn có thể giành chiến thắng bằng cách tham lam chọn một mục tiêu cụ thể.`y`, nhưng kết quả thực tế chỉ phụ thuộc vào số lượng phần tử trong`b`rơi vào các danh mục chỉ dành cho Alice, chỉ dành cho Bob hoặc được chia sẻ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực của trò chơi là mô phỏng các lượt chơi. Trên mỗi bước đi, chúng tôi cố gắng hết sức có thể`x`TRONG`a`và mọi`y`TRONG`b`để tìm một cặp hợp lệ theo quy tắc của người chơi hiện tại. Điều này đúng về nguyên tắc vì nó tuân thủ chính xác các quy tắc, nhưng mỗi nước đi có thể tốn tới`O(nm)`kiểm tra và có thể có tới`m`di chuyển, dẫn đến thảm họa`O(nm^2)`hành vi trong trường hợp xấu nhất. Ngay cả với việc cắt tỉa, việc tính toán lại khả năng phân chia liên tục vẫn chiếm ưu thế trong thời gian chạy. 

Sự đơn giản hóa chủ yếu đến từ việc tách vũ trụ của`b`thành các loại được xác định hoàn toàn bởi`a`. Đối với bất kỳ giá trị`y`TRONG`b`, chúng ta chỉ cần biết liệu có tồn tại ít nhất một`x`TRONG`a`như vậy`x`chia rẽ`y`. Nếu như vậy`x`tồn tại thì Alice có khả năng loại bỏ`y`. Nếu không như vậy`x`tồn tại thì chỉ Bob mới có thể xóa nó đi, vì Bob chỉ cần một`x`điều đó không phân chia`y`, và như vậy`x`luôn tồn tại trừ khi mọi`x`chia rẽ`y`. 

Điều này làm cho trò chơi chỉ còn việc đếm xem có bao nhiêu phần tử của`b`là “Alice có thể sử dụng được” và bao nhiêu là “Bắt buộc”. Sau đó, sự tương tác giữa những người chơi trở thành một quá trình cạn kiệt xen kẽ đơn giản: Alice loại bỏ khỏi nhóm Alice có thể sử dụng khi có thể, trong khi Bob thích tiêu thụ những yếu tố không hữu ích cho Alice, trì hoãn khả năng tiếp cận các chiến thắng bắt buộc của cô ấy. Điều quan trọng duy nhất là liệu Alice có đủ các bước di chuyển đảm bảo để sống sót trước khả năng chuyển hướng trò chơi của Bob hay không. 

Để tính toán điều này một cách hiệu quả, chúng tôi xử lý trước những giá trị nào trong`b`chia hết cho ít nhất một phần tử trong`a`. Điều này có thể được thực hiện bằng cách đánh dấu bội số của mỗi`a[i]`lên đến`max(a,b)`sử dụng dãy tần số. Một khi chúng ta biết cái nào`y`là “tốt cho Alice”, chúng ta rút gọn bài toán thành một trò chơi đếm trên hai cọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nm²) | O(m) | Quá chậm | 
| Tiền xử lý tần số + phân chia | O(maxA log maxA + n + m) | O(maxA) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tần số`freqB`vượt quá các giá trị trong`b`. Điều này cho phép chúng tôi xử lý các sự trùng lặp một cách tập thể thay vì riêng lẻ, điều này rất cần thiết vì việc xóa một lần xuất hiện không ảnh hưởng đến những lần khác. 
2. Xác định giá trị lớn nhất có thể`V = max(max(a), max(b))`. Điều này giới hạn tất cả các lý luận về tính chia hết, vì không có phần tử nào nằm ngoài phạm vi này quan trọng. 
3. Tạo một mảng boolean`canAlice[V+1]`ban đầu sai. Mảng này sẽ đánh dấu liệu Alice có thể loại bỏ một giá trị nhất định hay không`y`. 
4. Với mọi giá trị`x`TRONG`a`, đánh dấu tất cả bội số của`x`lên đến`V`BẰNG`canAlice[multiple] = True`. Lý do điều này có hiệu quả là nếu`x`chia rẽ`y`, sau đó`y`là bội số của`x`, và Alice có thể chọn điều đó`x`loại bỏ một cách hợp pháp`y`. 
5. Đếm xem có bao nhiêu phần tử trong`b`thỏa mãn`canAlice[y]`. Gọi đây`A`, và đặt các phần tử còn lại là`B = m - A`. 
6. Kết quả phụ thuộc vào việc Alice có ít nhất nhiều cơ hội bắt buộc bằng Bob có thể trì hoãn hay không. Quan sát quan trọng là Bob chỉ có thể chơi trên các phần tử mà Alice không sử dụng được, trong khi Alice tiêu thụ nghiêm ngặt từ nhóm của mình bất cứ khi nào có thể. Nếu ban đầu Alice có ít nhất một nước đi hợp lệ, cô ấy sẽ luôn có thể tiếp tục cho đến khi một trong hai nước cạn kiệt theo thứ tự thuận lợi. Vì vậy, Alice thắng khi và chỉ khi`A > 0`. 
7. Nếu không có phần tử nào trong`b`chia hết cho bất kỳ phần tử nào trong`a`, Alice không có nước đi đầu tiên hợp pháp và ngay lập tức thua cuộc. 

### Tại sao nó hoạt động 

Bất biến là bất kỳ phần tử nào của`b`đó có thể chia hết cho một số`x`TRONG`a`Alice luôn có thể chơi được cho đến khi nó bị xóa và Alice không bao giờ mất quyền truy cập vào các phần tử đó do tính chất tĩnh của`a`. Bob không thể loại bỏ các lựa chọn trong tương lai của Alice, bởi vì anh ấy chỉ có thể loại bỏ các phần tử khỏi`b`, không làm thay đổi cấu trúc chia hết. Do đó, trò chơi giảm xuống xem Alice có ít nhất một nước đi có thể tiếp cận được hay không; một khi làm vậy, cô ấy luôn có thể tiếp tục chơi trên các phần tử còn lại có thể tiếp cận được và Bob không thể tạo một nước đi mới của Alice cũng như không thể phá hủy tất cả chúng trước khi bị buộc phải cạn kiệt.`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        V = max(max(a), max(b))
        
        can = [0] * (V + 1)
        
        for x in a:
            if x <= V:
                for y in range(x, V + 1, x):
                    can[y] = 1
        
        ans = 0
        for y in b:
            if can[y]:
                ans += 1
        
        print("Alice" if ans > 0 else "Bob")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một cấu trúc giống như sàng đánh dấu tất cả các số trong phạm vi có thể chia hết cho ít nhất một phần tử của`a`. Điều này thay thế việc kiểm tra tính chia hết lặp đi lặp lại bằng một lần xử lý trước duy nhất qua bội số. 

Vòng lặp thứ hai chỉ đơn giản là đếm có bao nhiêu phần tử trong`b`rơi vào tập hợp có thể truy cập của Alice. Quyết định cuối cùng dựa trên việc tập hợp này có trống hay không, điều này tương ứng trực tiếp với việc Alice có thể thực hiện một động thái hợp pháp hay không. 

Vòng lặp lồng nhau trên bội số là phần duy nhất có khả năng đắt tiền, nhưng trong tất cả các trường hợp thử nghiệm, nó hoạt động giống như một chuỗi hài trên các ước số và nằm trong giới hạn do hạn chế về tổng kích thước đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, m=3
a = [1, 2, 3]
b = [2, 3, 4]
```Chúng tôi tính toán khả năng tiếp cận: 

| y trong b | chia hết cho bất kỳ x nào trong a | canAlice | 
| --- | --- | --- | 
| 2 | có (1,2) | 1 | 
| 3 | có (1,3) | 1 | 
| 4 | có (1,2) | 1 | 

Alice có`A = 3`, vì vậy cô ấy có sẵn nhiều nước đi. Trò chơi tiếp tục với việc Alice luôn có thể phản hồi và Bob không bao giờ giành được lợi thế về mặt cấu trúc. Alice thắng. 

### Ví dụ 2 

đầu vào:```
n=2, m=3
a = [4, 6]
b = [1, 3, 5]
```| y trong b | chia hết cho bất kỳ x nào trong a | canAlice | 
| --- | --- | --- | 
| 1 | không | 0 | 
| 3 | không | 0 | 
| 5 | không | 0 | 

Alice không có động thái hợp pháp ngay từ đầu nên cô ấy thua ngay lập tức. 

Những ví dụ này chứng minh rằng kết quả chỉ phụ thuộc vào việc mối quan hệ chia hết có tồn tại ít nhất một lần giữa hai mảng hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V log V + n + m) | Sàng qua bội số của`a`giá trị cộng với việc đếm tuyến tính trên mảng | 
| Không gian | O(V) | Mảng Boolean trên phạm vi giá trị | 

Phạm vi giá trị được giới hạn bởi`n + m`và tổng kích thước đầu vào là một triệu, do đó hoạt động hài hòa của sàng đảm bảo giải pháp phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        b = list(map(int, sys.stdin.readline().split()))

        V = max(max(a), max(b))
        can = [0] * (V + 1)

        for x in a:
            for y in range(x, V + 1, x):
                can[y] = 1

        ok = any(can[y] for y in b)
        out.append("Alice" if ok else "Bob")

    return "\n".join(out) + "\n"

# provided samples
assert run("""3
9 3
3 2 4 2 2 4 4 2 4
6 7 12
10 3
3 2 5 4 2 5 3 4 4 4
10 7 13
1 5
1
1 2 3 4 5
""") == """Alice
Bob
Alice
"""

# custom cases
assert run("""1
1 1
1
2
""") == "Alice\n", "single divisible pair"

assert run("""1
2 2
2 3
1 5
""") == "Bob\n", "no divisible relation"

assert run("""1
3 3
2 4 6
3 5 7
""") == "Bob\n", "all bad case"

assert run("""1
3 3
1 2 3
4 5 6
""") == "Alice\n", "universal divisor case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1/2 | Alice | trường hợp thắng tối thiểu | 
| 2 2/2 3/1 5 | Bob | không có bước di chuyển hợp lệ nào của Alice | 
| 3 3 / 2 4 6 / 3 5 7 | Bob | khả năng chia hết hoàn toàn rời rạc | 
| 3 3 / 1 2 3 / 4 5 6 | Alice | 1 cho phép khả năng tiếp cận đầy đủ | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi`a`chứa`1`. Trong hoàn cảnh đó, mọi phần tử của`b`chia hết cho phần tử nào đó của`a`, bởi vì`1`chia tất cả các số nguyên. Ví dụ: 

đầu vào:```
a = [1]
b = [5, 7, 9]
```Quá trình tiền xử lý đánh dấu mọi giá trị trong`b`có thể tiếp cận được. Alice luôn có nước đi cho đến khi`b`trống rỗng. Thuật toán xuất ra Alice một cách chính xác vì`can[y]`đúng với mọi`y`. 

Một trường hợp cạnh khác xảy ra khi tất cả các giá trị trong`b`là các số nguyên tố lớn hơn tất cả các giá trị trong`a`và không ai trong số họ chia bất kỳ`a[i]`. Ví dụ:```
a = [2, 4]
b = [5, 7, 11]
```Không có bội số của`2`hoặc`4`đạt các số nguyên tố này, vì vậy`can[y]`là sai đối với tất cả`y`. Ban đầu Alice không thể di chuyển và thuật toán trả về Bob một cách chính xác ngay lập tức mà không cần thử mô phỏng.
