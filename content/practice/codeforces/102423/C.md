---
title: "CF 102423C - Hiệu suất của Yêu tinh"
description: "Bài toán mô tả một chuỗi các đống đá, mỗi đống đá tượng trưng cho một con vật. Một con vật bắt đầu với một số viên đá. Sau đó, ở mỗi vòng, một số k được công bố."
date: "2026-08-12T01:09:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "C"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 123
verified: true
draft: false
---

[CF 102423C - Hiệu suất của Yêu tinh](https://codeforces.com/problemset/problem/102423/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một chuỗi các đống đá, mỗi đống đá tượng trưng cho một con vật. Một con vật bắt đầu với một số viên đá. Sau đó, trong mỗi vòng, một số`k`được công bố. Mỗi cọc có kích thước hiện tại chia hết cho`k`sẽ ghi được một điểm nên Emma ngay lập tức thêm một viên đá vào mỗi đống như vậy. Viên đá được thêm vào vẫn ở đó, có nghĩa là con vật đó có thể được thay đổi lại ở vòng sau. 

Nhiệm vụ là tính tổng số viên đá Emma phải ném. Vì mỗi cọc có thể chia cho dòng điện`k`phải tăng lên thì không có sự lựa chọn nào liên quan đến chiến lược tối ưu. Chúng ta chỉ cần mô phỏng những thay đổi bắt buộc, nhưng chúng ta cần làm như vậy mà không cần kiểm tra tất cả`n`động vật trong mỗi vòng. 

Đầu vào chứa tối đa`10^5`động vật và`10^5`vòng. Kích thước cọc ban đầu tối đa là`3 * 10^5`, và mọi ước số được công bố cũng nhiều nhất là`3 * 10^5`. Một trực tiếp`n * m`mô phỏng có thể yêu cầu`10^10`kiểm tra tính chia hết, vượt xa giới hạn thời gian năm giây có thể hỗ trợ. Kích thước cọc lớn nhất có thể là tối đa`3 * 10^5 + 10^5 = 4 * 10^5`, bởi vì một cọc riêng lẻ có thể tăng tối đa một lần mỗi vòng. Phạm vi giá trị tương đối nhỏ đó là thuộc tính cấu trúc mà chúng tôi khai thác. 

Một trường hợp tế nhị là khi một số con vật có cùng kích thước đống. Ví dụ,```
3 3
2
2
2
2
3
4
```Câu trả lời đúng là`9`. Cả ba cọc đều trở thành`3`ở vòng đầu tiên, sau đó tất cả trở thành`4`trong lần thứ hai, sau đó tất cả trở thành`5`ở phần thứ ba. Một mô phỏng lưu trữ từng con vật riêng biệt thực hiện chín cập nhật riêng lẻ ở đây, trong khi ba con vật có thể được biểu thị bằng một tần số:`3`. 

Một trường hợp quan trọng khác là khi một cọc chỉ chia hết cho ước số sau vì nó đã được tăng lên trước đó. Ví dụ,```
1 2
2
2
3
```đầu tiên`2`thay đổi cọc từ`2`ĐẾN`3`, và sau này`3`thay đổi nó từ`3`ĐẾN`4`. Câu trả lời là`2`. Giải pháp xác định tất cả các động vật bị ảnh hưởng chỉ từ giá trị ban đầu của chúng sẽ bỏ lỡ thao tác thứ hai. 

Trường hợp ranh giới thứ ba là một đống không bao giờ chia hết cho bất kỳ giá trị được công bố nào. Ví dụ,```
1 2
1
2
3
```Câu trả lời là`0`. giá trị`1`không bị thay đổi chỉ vì nó có mặt, vì ước số được công bố là`2`Và`1`không chia hết cho`2`. Việc nhầm lẫn "không phải bội số" với "nhỏ hơn số chia" sẽ đưa ra bản cập nhật không chính xác. 

## Phương pháp tiếp cận 

Giải pháp đơn giản lưu trữ tất cả`n`kích thước cọc và xử lý mỗi vòng bằng cách kiểm tra từng con vật. Nếu số chia hiện tại là`k`, chúng tôi kiểm tra xem`a[i] % k == 0`, thêm một khi có và tích lũy số lượng thay đổi. Điều này đúng vì nó tuân thủ chính xác luật chơi, bao gồm cả những thay đổi được thực hiện ở các vòng trước. 

Vấn đề là chi phí. có thể có`10^5`động vật và`10^5`vòng, bỏ cuộc`10^10`hoạt động modulo trong trường hợp xấu nhất. Ngay cả với số học số nguyên rất rẻ, con số đó vẫn quá lớn. 

Quan sát hữu ích là các động vật có cùng kích thước đống hiện tại hoàn toàn có thể thay thế cho nhau. Giả sử có`cnt[x]`động vật hiện đang giữ chính xác`x`đá. Nếu như`x`được chia cho hiện tại`k`, tất cả`cnt[x]`trong số chúng được thay đổi cùng nhau. Chúng ta có thể thêm`cnt[x]`đến câu trả lời và di chuyển toàn bộ tần số từ`x`ĐẾN`x + 1`. 

Vẫn còn một vấn đề: làm thế nào để chúng ta tìm thấy tất cả các giá trị hiện đang bị chiếm giữ chia hết cho`k`không cần quét tất cả`x`từ`1`ĐẾN`4 * 10^5`? 

Với mọi giá trị`x`, chúng ta biết tất cả các ước của nó. Chúng tôi duy trì một bộ cho mọi ước số được công bố có thể`k`. Bộ dành cho`k`chứa chính xác các kích thước cọc hiện tại được chiếm và chia cho`k`. Sau đó, một truy vấn có thể thu được ngay các kích thước cọc riêng biệt có liên quan từ tập hợp này. 

Khi một giá trị`x`di chuyển đến`x + 1`, thành viên của nó chỉ thay đổi đối với các ước của`x`Và`x + 1`. Chúng tôi loại bỏ`x`từ tập hợp số chia tương ứng với giá trị cũ của nó và chèn`x + 1`vào các bộ ước số tương ứng với giá trị mới của nó. Vì phạm vi giá trị chỉ khoảng`4 * 10^5`, tất cả các mối quan hệ số chia có thể được tính toán trước bằng sàng. 

Lực lượng vũ phu hoạt động vì nó kiểm tra rõ ràng mọi con vật và áp dụng quy tắc. Nó không thành công khi cùng một thao tác được lặp lại trên nhiều con vật và vòng chơi. Quan sát cho thấy các kích thước cọc bằng nhau có thể được xử lý thành một nhóm cho phép chúng tôi thay thế mô phỏng cấp độ động vật bằng mô phỏng cấp giá trị, trong khi các bộ chia làm cho mỗi vòng chỉ truy cập các giá trị thực sự có thể thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(n)`| Quá chậm | 
| Tần số giá trị + bộ chia |`O(V log V + U · D · log V)`|`O(V log V)`| Đã chấp nhận | 

Đây`V = 4 * 10^5`,`D`là số lượng ước số liên quan tối đa của một giá trị và`U`là số lần chuyển đổi giá trị riêng biệt được xử lý thực sự. Phần sàng chia được giới hạn bởi miền giá trị nhỏ, trong khi phần động xử lý các kích thước cọc riêng biệt thay vì từng con vật riêng lẻ. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kích thước cọc ban đầu và tất cả các ước số đã công bố. Chuỗi đầy đủ các ước số được công bố đã được biết trước khi mô phỏng, vì vậy chúng ta chỉ cần duy trì các tập hợp cho các ước số thực sự có thể được truy vấn. 
2. Đếm xem hiện tại có bao nhiêu con vật có mỗi cỡ cọc bằng cách sử dụng`cnt[x]`. Các kích thước cọc bằng nhau phải được nhóm lại vì mọi con vật có cùng giá trị sẽ hành xử giống hệt nhau trong mọi vòng chơi trong tương lai. 
3. Tính toán trước mọi ước số`d >= 2`cho mọi kích thước cọc có thể. Một cái sàng hoạt động tự nhiên ở đây: cho mỗi`d`, thăm nom`d, 2d, 3d, ...`và ghi lại`d`như một số chia. 
4. Với mỗi giá trị ban đầu bị chiếm dụng`x`, chèn`x`vào tập thuộc mọi ước của`x`xảy ra trong chuỗi truy vấn. Điều này thiết lập bất biến rằng một tập hợp chứa chính xác các giá trị chiếm chia hết cho ước số liên quan của nó. 
5. Xử lý các ước đã công bố theo thứ tự. Đối với một truy vấn`k`, lấy tất cả các giá trị hiện được lưu trữ trong tập hợp cho`k`. Mọi giá trị như vậy đều chia hết cho`k`, nên mỗi con vật ở giá trị đó phải nhận một viên đá. 
6. Đối với mỗi giá trị bị ảnh hưởng`x`, thêm vào`cnt[x]`để trả lời. Sau đó chuyển toàn bộ tần số từ`x`ĐẾN`x + 1`, bởi vì tất cả động vật ở`x`được thay đổi đúng một viên đá trong vòng này. 
7. Xóa`x`từ tập hợp số chia được liên kết với giá trị cũ của nó, sau đó chèn`x + 1`vào các tập chia số tương ứng với giá trị mới của nó. Thành viên thay đổi vì đống đã di chuyển vật lý từ số nguyên này sang số nguyên tiếp theo. 
8. Xóa cài đặt hiện tại`k`sau vòng đấu. Mọi giá trị chia hết cho`k`vừa được tăng lên và do đó không còn chia hết cho`k`, vì vậy việc giữ lại các mục nhập cũ sẽ khiến các truy vấn sau này xử lý các trạng thái không tồn tại. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`cnt[x]`là số lượng động vật có kích thước đống hiện tại chính xác bằng`x`và với mọi ước số được truy vấn`k`, tập hợp của nó chứa chính xác các giá trị bị chiếm dụng`x`thỏa mãn`x % k == 0`. 

Lúc đầu, bất biến giữ nguyên vì mọi giá trị chiếm giữ ban đầu được chèn vào các tập hợp ước số của nó. Trong một vòng đấu, tập hợp cho`k`chứa chính xác các giá trị mà động vật của chúng phải được thay đổi. Di chuyển`cnt[x]`từ`x`ĐẾN`x + 1`khớp chính xác với thao tác cần thiết cho tất cả các động vật đó. Đang xóa`x`từ bộ ước số cũ của nó và chèn`x + 1`vào các bộ ước số mới của nó sẽ khôi phục lại bất biến cho vòng tiếp theo. Do đó, mỗi lần bổ sung đá bắt buộc đều được tính chính xác một lần và không có lần bổ sung không cần thiết nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 400000

def solve():
    n, m = map(int, input().split())

    initial = [int(input()) for _ in range(n)]
    queries = [int(input()) for _ in range(m)]

    queried = [False] * (MAXV + 1)
    for k in queries:
        queried[k] = True

    # divisors[x] contains all divisors >= 2 of x.
    divisors = [[] for _ in range(MAXV + 1)]

    for d in range(2, MAXV + 1):
        for x in range(d, MAXV + 1, d):
            divisors[x].append(d)

    # cnt[x] = number of animals currently having x stones.
    cnt = [0] * (MAXV + 2)

    # Only queried divisors need sets.
    buckets = {}
    for k in set(queries):
        buckets[k] = set()

    for x in initial:
        cnt[x] += 1

    # Build the initial membership structure.
    for x in set(initial):
        if cnt[x] == 0:
            continue
        for d in divisors[x]:
            if queried[d]:
                buckets[d].add(x)

    answer = 0

    for k in queries:
        current = buckets[k]

        # The current k cannot receive new elements while it is being
        # processed, because x % k == 0 implies (x + 1) % k != 0.
        for x in list(current):
            c = cnt[x]
            if c == 0:
                continue

            answer += c

            cnt[x] = 0
            cnt[x + 1] += c

            # x is no longer occupied.
            for d in divisors[x]:
                if d != k and queried[d]:
                    buckets[d].discard(x)

            # x + 1 is now occupied.
            for d in divisors[x + 1]:
                if queried[d]:
                    buckets[d].add(x + 1)

        current.clear()

    print(answer)

if __name__ == "__main__":
    solve()
```Mảng tần số là sự triển khai trực tiếp ý tưởng nhóm giá trị từ thuật toán. Khi`cnt[x]`khác không, tất cả các con vật đó đều có hành vi giống nhau trong vòng hiện tại, vì vậy một chuyển đổi sẽ đại diện cho tất cả chúng. 

các`divisors`sàng xây dựng các mối quan hệ cần thiết để duy trì các nhóm. Ví dụ, nếu`x = 12`, các bộ liên quan bao gồm các bộ dành cho`2`,`3`,`4`,`6`, Và`12`. Một ước số của`x`chính xác là một giá trị truy vấn có thể chọn đống trong khi nó ở`x`. 

Từ điển`buckets`được sử dụng thay vì cấp phát một tập hợp cho mọi số nguyên tối đa`400000`. Chỉ những giá trị thực sự xuất hiện dưới dạng truy vấn mới có thể tìm kiếm được. Điều này giúp tiết kiệm một lượng đáng kể chi phí đối tượng Python. 

các`list(current)`chuyển đổi là có chủ ý. Việc xử lý một tập hợp đồng thời thay đổi thành viên trong các tập hợp khác là điều khó giải thích. Cụ thể hơn, nhóm hiện tại không thể đạt được giá trị mới trong vòng này vì nếu`x`chia hết cho`k`, sau đó`x + 1`không chia hết cho`k`. Chụp ảnh nhanh cũng làm cho việc lặp lại không bị ảnh hưởng bởi các đột biến do xử lý một giá trị khác. 

Câu trả lời được lưu trữ dưới dạng số nguyên của Python, do đó không có vấn đề tràn. Câu trả lời lý thuyết có thể đạt được`n * m`, đó là`10^10`, đã vượt quá số nguyên 32 bit có dấu. 

Giới hạn trên`MAXV = 400000`xuất phát từ giá trị ban đầu lớn nhất`300000`cộng tối đa một mức tăng trong mỗi`100000`vòng. Mảng có thêm hai vị trí vì quá trình chuyển đổi sử dụng`x + 1`. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
3 5
10
11
12
2
11
4
13
2
```Ban đầu tần số là`cnt[10] = 1`,`cnt[11] = 1`, Và`cnt[12] = 1`. 

| Vòng |`k`| Giá trị bị ảnh hưởng | Tần số di chuyển | Giá trị chiếm dụng mới | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 |`10, 12`|`1 + 1`|`11, 11, 13`| 2 | 
| 2 | 11 |`11`|`2`|`12, 13`| 4 | 
| 3 | 4 |`12`|`2`|`13`| 6 | 
| 4 | 13 |`13`|`3`|`14`| 9 | 
| 5 | 2 |`14`|`3`|`15`| 12 | 

Điểm mấu chốt là vòng thứ hai. Hai cọc ban đầu`10`Và`12`cả hai đều trở thành`11`sau vòng đầu tiên. Tần số của chúng hợp nhất thành`cnt[11] = 2`, do đó vòng thứ hai xử lý cả hai con vật với một chuyển đổi giá trị. Câu trả lời cuối cùng là`12`, phù hợp với mẫu chính thức. 

Một ví dụ thứ hai là:```
2 3
1
2
2
2
3
```Các giá trị chiếm dụng ban đầu là`1`Và`2`. 

| Vòng |`k`| Giá trị bị ảnh hưởng | Tần số di chuyển | Giá trị chiếm dụng mới | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 |`2`|`1`|`1, 3`| 1 | 
| 2 | 2 | không |`0`|`1, 3`| 1 | 
| 3 | 3 |`3`|`1`|`1, 4`| 2 | 

Ví dụ này chứng minh tại sao cấu trúc phải được cập nhật sau mỗi vòng. giá trị`3`ban đầu không có mặt nhưng nó sẽ bị chiếm dụng sau thao tác đầu tiên và được chọn bởi truy vấn vòng thứ ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(V log V + U D log V)`| Sàng chia xử lý tất cả các bội số, trong khi mọi giá trị phân biệt được xử lý đều được cập nhật thông qua các ước số của nó | 
| Không gian |`O(V log V)`| Danh sách số chia và nhóm số chia động lưu trữ các mối quan hệ giá trị-số chia | 

Đây`V = 400000`, Và`D`là số ước tối đa cho một giá trị trong phạm vi này. Miền giá trị được cố định bởi các ràng buộc, vì vậy việc sàng là thực tế. Cấu trúc động xử lý các kích thước cọc hiện tại khác nhau thay vì tất cả`n`động vật, đó là sự rút gọn quan trọng giúp cho việc mô phỏng trở nên khả thi. 

Phiên bản Codeforces chính thức có giới hạn năm giây và giới hạn bộ nhớ 512 MB. Phiên bản Python được hưởng lợi từ phạm vi giá trị tương đối nhỏ và từ việc nhóm các nhóm bằng nhau, mặc dù cuộc thi ban đầu được thiết kế chủ yếu xung quanh các ngôn ngữ cấp thấp hơn. 

## Trường hợp thử nghiệm```python
import sys
import io

MAXV = 400000

def solve_io(data: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(data)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample
sample1 = """\
3 5
10
11
12
2
11
4
13
2
"""
assert solve_io(sample1) == "12\n", "sample 1"

# Custom: minimum-size input, no operation is needed.
case2 = """\
1 1
1
2
"""
assert solve_io(case2) == "0\n", "minimum case"

# Custom: all animals have the same value and all move every round.
case3 = """\
3 3
2
2
2
2
3
4
"""
assert solve_io(case3) == "9\n", "all equal values"

# Custom: a value created by one round is used by a later round.
case4 = """\
2 3
1
2
2
2
3
"""
assert solve_io(case4) == "2\n", "newly created value"

# Custom: maximum-sized parameters.
# All 100000 animals start at 2.
# Query 2,3,4,...,100001, so every animal moves in every round.
n = 100000
m = 100000
queries = range(2, m + 2)

case5 = (
    f"{n} {m}\n"
    + "2\n" * n
    + "\n".join(map(str, queries))
    + "\n"
)

assert solve_io(case5) == "10000000000\n", "maximum-sized case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 2`|`0`| Đầu vào tối thiểu và một đống không bao giờ được chọn | 
|`3 3 / 2 2 2 / 2 3 4`|`9`| Các giá trị bằng nhau phải được nhóm và cập nhật cùng nhau | 
|`2 3 / 1 2 / 2 2 3`|`2`| Giá trị được tạo bởi bản cập nhật trước đó phải nhập các bộ truy vấn trong tương lai | 
|`100000`đống của`2`, theo sau là`2..100001`|`10000000000`| Tối đa`n`, tối đa`m`, cập nhật hàng loạt lặp đi lặp lại và câu trả lời có kích thước 64 bit | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét một cọc không bao giờ được chọn:```
1 1
1
2
```Tần số ban đầu là`cnt[1] = 1`. Cái xô dành cho`2`không chứa`1`, bởi vì`1`không chia hết cho`2`. Thùng rỗng nên câu trả lời vẫn còn`0`. Thuật toán không thực hiện bất kỳ sự gia tăng không cần thiết nào. 

Đối với trường hợp các giá trị bằng nhau xảy ra nhiều lần, hãy xem xét:```
3 3
2
2
2
2
3
4
```Ban đầu`cnt[2] = 3`, và xô cho`2`chỉ chứa giá trị`2`. Truy vấn đầu tiên thêm`cnt[2] = 3`đến câu trả lời và chuyển toàn bộ tần số tới`cnt[3]`. Truy vấn tiếp theo là`3`, vậy cái xô dành cho`3`chứa`3`, và ba viên đá khác được thêm vào. Truy vấn cuối cùng`4`tương tự di chuyển cả ba con vật. Câu trả lời là`3 + 3 + 3 = 9`. Thuật toán xử lý cả ba con vật chỉ với ba lần chuyển đổi giá trị. 

Đối với trường hợp giá trị được tạo động:```
2 3
1
2
2
2
3
```Truy vấn đầu tiên thấy giá trị`2`, di chuyển nó đến`3`và chèn`3`vào thùng cho số chia`3`. Truy vấn thứ hai là một truy vấn khác`2`, nhưng giá trị hiện tại là`1`Và`3`, nên không có gì thay đổi. Truy vấn thứ ba tìm thấy`3`và di chuyển nó đến`4`. Câu trả lời cuối cùng là`2`. 

Đối với ranh giới trên, giá trị ban đầu có thể là`300000`và nó có thể tăng lên nhiều nhất`100000`lần, vì vậy`400000`là giá trị lớn nhất có thể xuất hiện. Việc thực hiện phân bổ thông qua`400001`và thực hiện mọi chuyển đổi bằng cách sử dụng`x + 1`, do đó giá trị cuối cùng có thể được biểu diễn một cách an toàn. Câu trả lời có thể lớn như`10^10`, mà Python xử lý trực tiếp mà không bị tràn.
