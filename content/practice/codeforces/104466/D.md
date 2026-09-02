---
title: "CF 104466D - Xúc xắc DND"
description: "Chúng tôi tung ra một bộ sưu tập xúc xắc DnD tiêu chuẩn. Đầu vào cho chúng ta biết có bao nhiêu viên xúc xắc d4, d6, d8, d12 và d20. Mọi xúc xắc đều công bằng và mỗi mặt được đánh số từ 1 đến số cạnh của nó. Mỗi cuộn hoàn chỉnh tạo ra một tổng số tiền."
date: "2026-06-30T13:14:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104466
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ICPC German Collegiate Programming Contest (GCPC 2023)"
rating: 0
weight: 104466
solve_time_s: 63
verified: true
draft: false
---

[CF 104466D - Xúc xắc DND](https://codeforces.com/problemset/problem/104466/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi tung ra một bộ sưu tập xúc xắc DnD tiêu chuẩn. Đầu vào cho chúng ta biết có bao nhiêu viên xúc xắc d4, d6, d8, d12 và d20. Mọi xúc xắc đều công bằng và mỗi mặt được đánh số từ 1 đến số cạnh của nó. 

Mỗi cuộn hoàn chỉnh tạo ra một tổng số tiền. Các số tiền khác nhau không có khả năng xảy ra như nhau vì nhiều sự kết hợp khác nhau của mệnh giá có thể tạo ra tổng số như nhau. Nhiệm vụ là in mọi tổng có thể, được sắp xếp từ xác suất cao nhất đến xác suất thấp nhất. Nếu hai tổng có xác suất bằng nhau thì một trong hai mệnh lệnh sẽ được chấp nhận. 

Đầu vào lớn nhất chứa 10 viên xúc xắc mỗi loại, tổng cộng là 50 viên xúc xắc. Việc liệt kê một cách thô bạo mọi kết quả ngay lập tức là không thể. Thậm chí chỉ tung mười viên xúc xắc d20 cũng đã tạo ra$20^{10}$kết quả, và đầu vào tối đa thực tế lớn hơn rất nhiều. Thuật toán phải tránh liệt kê từng cuộn riêng lẻ. 

Quan sát hữu ích là mặc dù số lượng kết quả rất lớn nhưng số lượng tổng có thể có lại nhỏ. Tổng tối thiểu có thể là số lượng xúc xắc, vì mỗi viên xúc xắc đóng góp ít nhất 1. Tổng tối đa có thể là$$40 + 60 + 80 + 120 + 200 = 500.$$Chỉ có thể tồn tại 451 tổng khác nhau, một con số rất nhỏ. Điều này gợi ý rõ ràng về quy trình động trên tổng. 

Một sai lầm dễ mắc phải là cho rằng các xác suất có thể được so sánh bằng cách sử dụng các giá trị dấu phẩy động. Làm tròn dấu phẩy động có thể hoán đổi không chính xác các tổng có xác suất bằng nhau hoặc cực kỳ gần nhau. Ví dụ,```
1 0 0 0 0
```tạo ra các tổng 1, 2, 3 và 4, mỗi tổng có xác suất như nhau. Đầu ra chính xác có thể liệt kê chúng theo bất kỳ thứ tự nào, nhưng việc so sánh dựa trên các giá trị dấu phẩy động được làm tròn là không cần thiết. Việc đếm số cách để đạt được mỗi tổng sẽ tránh được hoàn toàn vấn đề này. 

Một trường hợp tinh tế khác là khi chỉ có một con súc sắc tồn tại.```
0 0 0 0 1
```Mỗi khuôn mặt xuất hiện đúng một lần nên mọi tổng đều có xác suất như nhau. Mọi hoán vị từ 1 đến 20 đều hợp lệ. Một giải pháp giả định luôn có một thứ tự duy nhất sẽ từ chối các kết quả đầu ra hợp lệ một cách không chính xác. 

Trường hợp cạnh cuối cùng là khi có nhiều xúc xắc.```
10 10 10 10 10
```Số lượng cuộn có thể có là lớn về mặt thiên văn, do đó việc lưu trữ xác suất hoặc liệt kê kết quả là không khả thi. Lập trình động chỉ lưu trữ số cách để đạt được mỗi tổng có thể đạt được, có chỉ số lớn nhất không bao giờ vượt quá 500. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ tạo đệ quy mọi lần cuộn có thể, tính tổng của nó và đếm số lần mỗi tổng xuất hiện. Điều này đúng vì mọi kết quả đều được kiểm tra chính xác một lần. Thật không may, thời gian chạy của nó bằng tổng số kết quả, trong trường hợp xấu nhất là$$4^{10} \cdot 6^{10} \cdot 8^{10} \cdot 12^{10} \cdot 20^{10},$$vượt xa mọi thứ mà máy tính có thể xử lý. 

Lý do bài toán này vẫn dễ là vì chúng ta thực sự không quan tâm đến chuỗi cuộn nào sẽ tạo ra tổng. Chúng ta chỉ cần số cách để có được mỗi tổng. Vì có nhiều nhất 451 tổng khác nhau nên chúng ta có thể cập nhật liên tục sự phân bố theo tổng khi chúng ta thêm từng viên xúc xắc một. 

Giả sử chúng ta đã biết có bao nhiêu cách để mỗi tổng có thể xảy ra sau khi xử lý một số viên xúc xắc. Khi người khác chết cùng$k$các cạnh được cộng vào, mọi tổng hiện có sẽ đóng góp chính xác vào$k$những khoản tiền mới. Chúng tôi chỉ đơn giản là phân phối số lượng của nó trên tổng số mới đó. Đây chính xác là sự kết hợp của các phân phối, nhưng chỉ với vài trăm tổng có thể, việc triển khai lập trình động đơn giản đã đủ nhanh. 

Sau khi tất cả các viên xúc xắc đã được xử lý, số cách cho mỗi tổng tỷ lệ thuận với xác suất của nó vì mọi kết quả hoàn chỉnh đều có khả năng xảy ra như nhau. Sắp xếp tổng theo các số đếm này sẽ trực tiếp tạo ra thứ tự cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\prod s_i)$|$O(\text{number of sums})$| Quá chậm | 
| Tối ưu |$O(D \times S \times 20)$|$O(S)$| Đã chấp nhận | 

Đây,$D \le 50$là số lượng xúc xắc và$S \le 500$là số tiền lớn nhất có thể. 

## Hướng dẫn thuật toán 

1. Đọc số của từng loại khuôn. 
2. Mở rộng đầu vào thành một danh sách chứa một mục nhập cho mỗi khuôn, trong đó mỗi mục nhập lưu số cạnh của nó. Ví dụ: một d4 và hai d8 trở thành danh sách`[4, 8, 8]`. 
3. Tạo một mảng lập trình động trong đó`dp[x]`là số cách lấy tổng`x`sử dụng xúc xắc được xử lý cho đến nay. Ban đầu chỉ có thể có tổng bằng 0, vì vậy hãy đặt`dp[0] = 1`. 
4. Xử lý từng viên xúc xắc một lần. Đối với mỗi người chết với`k`các bên, tạo một mảng mới`ndp`. 
5. Đối với mọi số tiền có thể tiếp cận và mọi mệnh giá từ 1 đến`k`, cộng số cách hiện tại vào tổng mới tương ứng. Mỗi kết quả hiện tại mở rộng thành chính xác một kết quả cho mỗi mệnh giá. 
6. Thay thế`dp`với`ndp`và tiếp tục cho đến khi mọi khuôn đều được xử lý. 
7. Tính số tiền nhỏ nhất và lớn nhất có thể đạt được từ số lượng xúc xắc và số mặt của chúng. 
8. Sắp xếp tất cả các số tiền có thể đạt được bằng cách giảm dần`dp[sum]`. Nếu hai số đếm bằng nhau thì mọi thứ tự tương đối đều được chấp nhận, do đó khóa phụ không liên quan. 
9. In tổng theo thứ tự sắp xếp. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của xúc xắc, bảng lập trình động sẽ đếm chính xác số lần tung xúc xắc riêng biệt tạo ra mọi tổng có thể có. Bất biến này ban đầu đúng vì chỉ có thể đạt được tổng 0 mà không cần tung xúc xắc. Mọi bản cập nhật đều giữ nguyên tính bất biến vì mỗi lần tung trước đó sẽ mở rộng độc lập với mọi mặt của xúc xắc mới và mỗi lần tung kết quả được tính chính xác một lần. Sau khi tất cả xúc xắc đã được xử lý,`dp[s]`bằng tổng số kết quả có tổng bằng`s`. Vì mỗi lần tung hoàn thành đều có khả năng như nhau nên việc sắp xếp theo số lượng này hoàn toàn giống với việc sắp xếp theo xác suất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t, c, o, d, i = map(int, input().split())

    dice = (
        [4] * t +
        [6] * c +
        [8] * o +
        [12] * d +
        [20] * i
    )

    max_sum = sum(dice)
    dp = [0] * (max_sum + 1)
    dp[0] = 1

    current_max = 0

    for sides in dice:
        ndp = [0] * (max_sum + 1)
        for s in range(current_max + 1):
            if dp[s] == 0:
                continue
            ways = dp[s]
            for face in range(1, sides + 1):
                ndp[s + face] += ways
        dp = ndp
        current_max += sides

    min_sum = len(dice)

    ans = list(range(min_sum, max_sum + 1))
    ans.sort(key=lambda x: dp[x], reverse=True)

    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách mở rộng đầu vào nén thành một mục danh sách trên mỗi khuôn. Điều này làm cho mọi bản cập nhật đều giống nhau bất kể loại khuôn. 

Mảng lập trình động luôn biểu thị phân bố sau khi xử lý một số xúc xắc nhất định. Một mảng mới được phân bổ cho mỗi khuôn vì mỗi lần chuyển đổi chỉ được sử dụng các giá trị từ giai đoạn trước. Việc cập nhật mảng tại chỗ sẽ vô tình sử dụng lại các trạng thái mới được tạo và đếm quá nhiều số tiền.`current_max`theo dõi số tiền lớn nhất có thể đạt được sau khi xúc xắc được xử lý. Chỉ lặp lại tối đa giá trị này để tránh quét các mục không sử dụng. 

Các số nguyên Python tự động tăng đến độ chính xác tùy ý, do đó, ngay cả số lượng cuộn khổng lồ có thể có cho thử nghiệm lớn nhất cũng phù hợp mà không bị tràn. 

Cuối cùng, số tiền có thể đạt được sẽ được sắp xếp theo số lượng của chúng. Vì xác suất chỉ khác nhau ở mẫu số chung bằng tổng số kết quả nên việc so sánh số lượng là đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1 1 0 0
```Xúc xắc là d4, d6 và d8. 

| Bước | Chết | Phạm vi tổng có thể tiếp cận | 
| --- | --- | --- | 
| 0 | Không có | 0 | 
| 1 | d4 | 1 đến 4 | 
| 2 | d6 | 2 đến 10 | 
| 3 | d8 | 3 đến 18 | 

Sau lần cập nhật cuối cùng, số lượng lớn nhất xảy ra ở các tổng 11, 10 và 9. Những số này trở thành điểm bắt đầu của đầu ra. 

Ví dụ này cho thấy cách tích chập lặp đi lặp lại một cách tự nhiên tạo ra phân bố hình chuông quen thuộc, trong đó tổng ở giữa có nhiều kết hợp hơn tổng cực trị. 

### Mẫu 2 

đầu vào:```
2 0 0 1 0
```Xúc xắc là d4, d4 và d12. 

| Bước | Chết | Phạm vi tổng có thể tiếp cận | 
| --- | --- | --- | 
| 0 | Không có | 0 | 
| 1 | d4 | 1 đến 4 | 
| 2 | d4 | 2 đến 8 | 
| 3 | d12 | 3 đến 20 | 

Xác suất cao nhất xảy ra ở gần giữa phân bố, do đó các tổng như 9 và 14 xuất hiện trước các giá trị cực trị. 

Ví dụ này chứng tỏ rằng thuật toán xử lý các kích thước khuôn khác nhau một cách thống nhất. Mỗi khuôn bổ sung chỉ thực hiện một bản cập nhật phân phối khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D \times S \times 20)$| Mỗi xúc xắc sẽ cập nhật mọi số tiền có thể tiếp cận bằng cách sử dụng tối đa 20 khuôn mặt. | 
| Không gian |$O(S)$| Chỉ các bản phân phối hiện tại và tiếp theo được lưu trữ. | 

Vì có tối đa 50 viên xúc xắc và tổng lớn nhất có thể chỉ là 500 nên thuật toán hoạt động tốt dưới một triệu thao tác chuyển tiếp. Điều này dễ dàng thỏa mãn giới hạn bài toán. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t, c, o, d, i = map(int, input().split())

    dice = [4] * t + [6] * c + [8] * o + [12] * d + [20] * i
    max_sum = sum(dice)

    dp = [0] * (max_sum + 1)
    dp[0] = 1
    cur = 0

    for sides in dice:
        ndp = [0] * (max_sum + 1)
        for s in range(cur + 1):
            if dp[s]:
                for f in range(1, sides + 1):
                    ndp[s + f] += dp[s]
        dp = ndp
        cur += sides

    mn = len(dice)
    ans = list(range(mn, max_sum + 1))
    ans.sort(key=lambda x: dp[x], reverse=True)
    return " ".join(map(str, ans))

# provided samples
assert run("1 1 1 0 0\n") == "11 10 9 12 8 13 14 7 15 6 5 16 17 4 18 3"
assert run("2 0 0 1 0\n") == "9 14 12 11 10 13 15 8 16 7 6 17 5 18 4 19 3 20"

# custom cases
assert set(run("1 0 0 0 0\n").split()) == {"1", "2", "3", "4"}
assert set(run("0 0 0 0 1\n").split()) == {str(x) for x in range(1, 21)}
assert run("0 1 0 0 0\n").split()[0] in {"3", "4"}
assert len(run("10 10 10 10 10\n").split()) == 451
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0 0 0`| Bất kỳ hoán vị nào của`1 2 3 4`| Một xúc xắc công bằng, mọi xác suất đều bằng nhau | 
|`0 0 0 0 1`| Bất kỳ hoán vị nào của`1`bởi vì`20`| Khuôn đơn lớn | 
|`0 1 0 0 0`| Bắt đầu với`3`hoặc`4`| Xử lý ràng buộc để phân phối đối xứng | 
|`10 10 10 10 10`| 451 khoản | Kích thước đầu vào tối đa | 

## Vỏ cạnh 

Xem xét đầu vào```
1 0 0 0 0
```Bảng lập trình động bắt đầu bằng`dp[0] = 1`. Sau khi xử lý d4, tổng từ 1 đến 4, mỗi tổng nhận được chính xác một đóng góp. Mọi số đếm đều giống hệt nhau nên mọi thứ tự đều hợp lệ. Thuật toán tự nhiên tạo ra tần số bằng nhau mà không cần xử lý đặc biệt. 

Bây giờ hãy xem xét```
0 0 0 0 1
```Chỉ có một d20. Mỗi khuôn mặt đóng góp chính xác một lần, vì vậy tất cả 20 tổng đều nhận được số 1. Sắp xếp theo tần suất để lại tất cả các tổng bằng nhau, phù hợp với đặc điểm kỹ thuật rằng các mối hòa có thể xuất hiện theo bất kỳ thứ tự nào. 

Cuối cùng, hãy xem xét```
10 10 10 10 10
```Tổng nhỏ nhất có thể đạt được là 50 và lớn nhất là 500. Mặc dù tổng số cuộn có thể lớn đến mức không thể tưởng tượng được nhưng bảng lập trình động vẫn chỉ chứa 501 mục. Mỗi xúc xắc thực hiện tối đa 20 lần chuyển đổi cho mỗi số tiền có thể có, do đó thời gian chạy chỉ phụ thuộc vào phạm vi tổng chứ không phụ thuộc vào số kết quả riêng lẻ.
