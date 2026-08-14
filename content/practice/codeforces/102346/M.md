---
title: "CF 102346M - Maratona Brasileira de Popcorn"
description: "Chúng ta có dãy N túi bỏng ngô, trong đó P[i] là lượng bỏng ngô trong túi i. Có C đối thủ và mỗi đối thủ có thể ăn tối đa T bắp rang mỗi giây. Các túi phải được chia thành các đoạn liền kề nhau."
date: "2026-08-13T01:55:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "M"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 499
verified: true
draft: false
---

[CF 102346M - Maratona Brasileira de Popcorn](https://codeforces.com/problemset/problem/102346/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`N`túi bỏng ngô, ở đâu`P[i]`là lượng bỏng ngô trong túi`i`. có`C`đối thủ cạnh tranh và mọi đối thủ cạnh tranh đều có thể ăn nhiều nhất`T`bỏng ngô mỗi giây. 

Các túi phải được chia thành các đoạn liền kề nhau. Một đối thủ cạnh tranh nhận được một phân khúc và một đối thủ cạnh tranh có thể nhận được một phân khúc trống, do đó sử dụng ít hơn`C`phân đoạn không trống được cho phép. Vì một túi không thể được chia cho các đối thủ nên tổng số bắp rang của mỗi phần phải được một đối thủ ăn. Tất cả các thí sinh đều thực hiện đồng thời nên tổng thời gian thi đấu sẽ do thí sinh chậm nhất quyết định. 

Đối với một đoạn chứa`S`bỏng ngô, giờ ăn của nó là`ceil(S / T)`. Chúng ta cần chọn phân vùng liền kề để giảm thiểu thời gian lớn nhất và xuất ra số giây tối thiểu đó. 

Mảng có thể chứa tới`10^5`túi xách. Mỗi túi chứa nhiều nhất`10^4`bỏng ngô, vì vậy tổng số tiền có thể đạt tới`10^9`. Một thuật toán bậc hai đã thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa mức hợp lý. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(N log N)`. Số lượng đối thủ cạnh tranh cũng có thể lớn như`10^5`, do đó một cách tiếp cận phụ thuộc bậc hai vào`C`là không phù hợp. 

Có một số trường hợp ranh giới có thể cho thấy việc triển khai không chính xác. Nếu chỉ có một túi, chẳng hạn như```
1 3 4
5
```câu trả lời là`2`, bởi vì một đối thủ cạnh tranh cần`ceil(5 / 4) = 2`giây. Một giải pháp dựa trên phân vùng bất cẩn có thể cố gắng phân phối một túi duy nhất cho một số đối thủ cạnh tranh một cách không chính xác, điều này bị cấm vì một túi phải hoàn toàn thuộc về một đối thủ cạnh tranh. 

Một trường hợp khác là khi có nhiều đối thủ hơn túi:```
2 5 3
4 7
```Câu trả lời là`3`. Hai túi có thể được giao cho hai thí sinh và ba thí sinh còn lại không làm gì cả. Việc coi số lượng đối thủ cạnh tranh chính xác là số lượng các nhóm không trống được yêu cầu sẽ từ chối sự sắp xếp hợp lệ này. 

Trường hợp cạnh thứ ba xảy ra khi một túi lớn hơn mọi nhóm có thể có khác:```
3 2 1
1 1 5
```Câu trả lời là`5`. Chiếc túi chứa`5`bỏng ngô đã cần năm giây, bất kể các túi khác được phân phối như thế nào. Bất kỳ thuật toán nào chỉ cân bằng tổng số tiền mà không tôn trọng kích thước túi riêng lẻ đều có thể dự đoán sai câu trả lời nhỏ hơn. 

Cuối cùng, việc làm tròn phải diễn ra sau khi tính tổng điểm của đối thủ cạnh tranh. Vì```
2 2 4
5 5
```mỗi đối thủ cạnh tranh cần`ceil(5 / 4) = 2`giây, vậy câu trả lời là`2`, không`ceil(10 / (2 * 4)) = 2`một cách tình cờ. Trong các trường hợp khác, sự khác biệt rất quan trọng, vì vậy giải pháp nên suy luận trực tiếp về giới hạn thời gian bằng số nguyên thay vì lấy số bỏng ngô trung bình giữa các đối thủ cạnh tranh. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp xem xét mọi cách có thể để đặt ranh giới giữa các túi liên tiếp. có`N - 1`các ranh giới có thể có, và mỗi ranh giới có thể được chọn hoặc không, đưa ra`2^(N-1)`các phân vùng khác nhau. Đối với mỗi phần, chúng ta có thể tính tổng số bắp rang được giao cho mỗi thí sinh và giữ nguyên thời gian ăn yêu cầu lớn nhất. Lấy mức tối thiểu trên tất cả các phân vùng là đúng vì mọi phép gán liền kề hợp pháp đều tương ứng với chính xác một tập hợp các ranh giới như vậy. 

Vấn đề là số lượng phân vùng. Khi`N = 10^5`, số khả năng là`2^99999`, có giá trị lớn về mặt thiên văn. Ngay cả khi việc đánh giá từng phân vùng chỉ mất thời gian không đổi thì việc tìm kiếm cũng không thể thực hiện được. Với sự thẳng thắn`O(N)`đánh giá trên mỗi phân vùng, trường hợp xấu nhất là`O(N * 2^N)`. Cách tiếp cận này chỉ hữu ích cho các mảng nhỏ và cho chúng ta định nghĩa về thuật toán tối ưu chứ không phải thuật toán thực tế. 

Quan sát hữu ích là chúng ta không thực sự cần xây dựng phân vùng tối ưu ngay lập tức. Thay vào đó, giả sử chúng ta đoán rằng toàn bộ cuộc thi phải kết thúc trong vòng`X`giây. Trong trường hợp đó, mỗi thí sinh được ăn tối đa`X * T`bỏng ngô. Câu hỏi trở nên đơn giản hơn nhiều: mảng có thể được chia thành nhiều nhất`C`các nhóm liền kề, mỗi nhóm có tổng nhiều nhất`X * T`? 

Đối với một năng lực cố định, có một cách tham lam để trả lời câu hỏi đó. Quét mảng từ trái sang phải và liên tục bổ sung túi cho đối thủ hiện tại trong khi không vượt quá dung lượng. Khi túi tiếp theo có số tiền quá lớn, hãy bắt đầu với một đối thủ cạnh tranh mới. Bởi vì tất cả lượng bỏng ngô đều dương nên việc đưa thêm túi vào phân khúc hiện tại không bao giờ có thể làm giảm lượng đối thủ cạnh tranh cần sau này. Do đó, phân vùng tham lam sử dụng số lượng phân đoạn tối thiểu có thể cho dung lượng đó. 

Tính khả thi cũng đơn điệu. Nếu như`X`giây là đủ, thì số giây lớn hơn cũng là đủ, bởi vì mọi đối thủ đều có dung lượng lớn hơn. Nếu như`X`giây là không thể, mọi giá trị nhỏ hơn đều không thể. Thuộc tính đơn điệu đó cho phép tìm kiếm nhị phân trên câu trả lời. 

Chúng ta có thể tìm kiếm nhị phân giữa`1`thứ hai và`ceil(sum(P) / T)`giây. Đối với mỗi thời gian ứng cử viên, chúng tôi nhân nó với`T`để có được lượng bỏng ngô tối đa, một đối thủ cạnh tranh có thể xử lý và tiến hành kiểm tra tính khả thi một cách tham lam. Độ phức tạp thu được là`O(N log(sum(P) / T))`, đủ nhanh để`N = 10^5`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N * 2^N)`|`O(N)`| Quá chậm | 
| Tìm kiếm nhị phân + Tham lam |`O(N log(sum(P) / T))`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`,`C`,`T`và mảng số lượng bỏng ngô. Hãy tính tổng số bắp rang vì nó cho chúng ta giới hạn trên của câu trả lời. 
2. Đặt giới hạn dưới của tìm kiếm nhị phân thành`1`thứ hai và giới hạn trên của`ceil(sum(P) / T)`giây. Giới hạn trên tương ứng với việc đưa tất cả các túi cho một thí sinh. Mặc dù có thể có`C`đối thủ cạnh tranh, chỉ được phép có một đối thủ cạnh tranh không trống. 
3. Đối với thời gian ứng tuyển`mid`, hãy tính năng lực của một đối thủ cạnh tranh như`mid * T`. Bây giờ chúng ta hỏi liệu có thể chỉ định tối đa tất cả các túi hay không`C`các nhóm liền kề có tổng không vượt quá khả năng này. 
4. Quét mảng từ trái sang phải. Duy trì số lượng bỏng ngô được giao cho đối thủ cạnh tranh hiện tại. Nếu việc thêm túi tiếp theo vẫn nằm trong khả năng chứa, hãy giữ nó ở phân khúc hiện tại. Nếu vượt quá khả năng, hãy bắt đầu một phân khúc mới với túi đó và tăng số lượng đối thủ cạnh tranh được sử dụng. 
5. Nếu túi cá nhân nào lớn hơn sức chứa thì thời gian xét tuyển ngay lập tức không thể thực hiện được. Mặt khác, sau khi quét toàn bộ mảng, ứng cử viên khả thi chính xác khi số lượng phân đoạn không trống được yêu cầu nhiều nhất`C`. 
6. Nếu ứng viên khả thi, hãy tìm nửa dưới vì đáp án có thể nhỏ hơn. Nếu không khả thi, hãy tìm kiếm nửa trên vì cần nhiều thời gian hơn. 
7. Tiếp tục cho đến khi khoảng tìm kiếm nhị phân chứa một giá trị. Giá trị đó là số giây tối thiểu mà phân vùng hợp lệ tồn tại. 

### Tại sao nó hoạt động 

Đối với bất kỳ dung lượng cố định nào, quá trình quét tham lam sẽ tạo một phân đoạn lớn nhất có thể trước khi bắt đầu phân đoạn tiếp theo. Hãy xem xét phân đoạn đầu tiên của bất kỳ phân vùng hợp lệ nào. Vì tất cả số lượng túi đều là số dương, nên việc mở rộng phân đoạn đầu tiên đó bằng một túi khác trong khi vẫn đáp ứng đủ sức chứa không thể khiến hậu tố còn lại khó phân chia hơn là để lại túi đó cho thí sinh tiếp theo. Việc lặp lại đối số này có nghĩa là phương thức tham lam sẽ giảm thiểu số lượng phân đoạn cần thiết cho toàn bộ mảng. 

Như vậy việc kiểm tra tính khả thi là chính xác. Vị ngữ "có thể kết thúc trong vòng`X`giây" là đơn điệu vì tăng`X`chỉ làm tăng năng lực của mọi đối thủ cạnh tranh. Do đó, tìm kiếm nhị phân tìm thấy khả năng nhỏ nhất có thể`X`, đó chính xác là thời gian thi đấu tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c, t = map(int, input().split())
    p = list(map(int, input().split()))

    total = sum(p)

    def feasible(seconds):
        capacity = seconds * t
        competitors = 1
        current = 0

        for x in p:
            if x > capacity:
                return False

            if current + x <= capacity:
                current += x
            else:
                competitors += 1
                current = x

                if competitors > c:
                    return False

        return True

    lo = 1
    hi = (total + t - 1) // t

    while lo < hi:
        mid = (lo + hi) // 2

        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```các`feasible`hàm thực hiện phân vùng tham lam từ thuật toán.`capacity`là số tiền tối đa mà một thí sinh có thể ăn trong số giây của ứng cử viên. Biến`current`lưu trữ bỏng ngô được giao cho đối thủ cạnh tranh hiện tại, trong khi`competitors`đếm xem có bao nhiêu phân đoạn không trống đã được tạo. 

Bài kiểm tra`x > capacity`là cần thiết vì không có vách ngăn nào có thể chia đôi một chiếc túi. Ngay cả khi có nhiều đối thủ không được sử dụng, một túi lớn hơn sức chứa của ứng viên cũng không thể được chỉ định hợp pháp. 

Khi`current + x`vượt quá khả năng, phân đoạn hiện tại sẽ bị đóng và phân đoạn mới bắt đầu bằng`x`. Không có lý do gì để chuyển túi trước đó sang đối thủ cạnh tranh mới, vì điều đó sẽ chỉ làm cho phân khúc đầu tiên nhỏ hơn và không thể giảm số lượng phân khúc mà hậu tố còn lại yêu cầu. 

Giới hạn trên sử dụng phép chia trần số nguyên,`(total + t - 1) // t`. Giá trị thể hiện thời gian cần thiết nếu một thí sinh ăn hết bỏng ngô. Nó luôn là giới hạn trên hợp lệ vì các đối thủ cạnh tranh không được sử dụng vẫn được phép. 

Việc tìm kiếm nhị phân sử dụng`lo < hi`, vì vậy khi vòng lặp kết thúc, cả hai giới hạn đều biểu thị thời gian khả thi nhỏ nhất như nhau. Số nguyên Python có độ chính xác tùy ý, do đó phép nhân như`seconds * t`không thể tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 3 4
5 8 3 10 7
```Với ba đối thủ cạnh tranh và bốn bắp rang mỗi giây, hãy xem xét một ứng cử viên`4`giây. Mỗi thí sinh được ăn tối đa`16`bỏng ngô. 

Quá trình quét tham lam hoạt động như sau. 

| Túi | Tổng phân khúc hiện tại | Đối thủ cạnh tranh đã sử dụng | Quyết định | 
| --- | --- | --- | --- | 
| 5 | 5 | 1 | Thêm vào phân khúc hiện tại | 
| 8 | 13 | 1 | Thêm vào phân khúc hiện tại | 
| 3 | 16 | 1 | Thêm vào phân khúc hiện tại | 
| 10 | 10 | 2 | Bắt đầu phân khúc mới | 
| 7 | 7 | 3 | Bắt đầu phân khúc mới | 

Ba đối thủ là đủ rồi`4`giây là khả thi. 

Bây giờ hãy xem xét`3`giây. Năng lực trở thành`12`. 

| Túi | Tổng phân khúc hiện tại | Đối thủ cạnh tranh đã sử dụng | Quyết định | 
| --- | --- | --- | --- | 
| 5 | 5 | 1 | Thêm vào phân khúc hiện tại | 
| 8 | 8 | 2 | Bắt đầu phân khúc mới | 
| 3 | 11 | 2 | Thêm vào phân khúc hiện tại | 
| 10 | 10 | 3 | Bắt đầu phân khúc mới | 
| 7 | 7 | 4 | Bắt đầu phân khúc mới | 

Sẽ cần có bốn đối thủ cạnh tranh, vì vậy`3`giây là không thể. Kết quả tìm kiếm nhị phân trả về`4`. 

Dấu vết này cũng cho thấy tại sao không thể có được câu trả lời bằng cách chia tổng số bắp rang cho các đối thủ cạnh tranh. Hạn chế liền kề buộc mảng thành các nhóm và nhóm chứa`8 + 3`hoặc bị cô lập`10`ảnh hưởng đến thời gian tối đa. 

### Mẫu 2 

Đầu vào là```
3 2 1
1 5 1
```Với hai đối thủ cạnh tranh và một bắp rang mỗi giây, hãy kiểm tra`5`giây. công suất là`5`. 

| Túi | Tổng phân khúc hiện tại | Đối thủ cạnh tranh đã sử dụng | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Thêm vào phân khúc hiện tại | 
| 5 | 5 | 2 | Bắt đầu phân khúc mới | 
| 1 | 1 | 3 | Bắt đầu phân khúc mới | 

Cần có ba đối thủ cạnh tranh, vì vậy`5`giây là không thể thực hiện được. 

Bài kiểm tra`6`giây. công suất là`6`. 

| Túi | Tổng phân khúc hiện tại | Đối thủ cạnh tranh đã sử dụng | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Thêm vào phân khúc hiện tại | 
| 5 | 6 | 1 | Thêm vào phân khúc hiện tại | 
| 1 | 1 | 2 | Bắt đầu phân khúc mới | 

Chỉ cần có hai đối thủ cạnh tranh, vì vậy`6`giây là khả thi. Câu trả lời là`6`. 

Ví dụ này chứng minh tại sao một túi lớn ở giữa có thể tạo ra một vách ngăn không thuận lợi. Cách chia tốt nhất là`[1, 5] | [1]`, nhóm lớn nhất có sáu bắp rang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N log(sum(P) / T))`| Mỗi lần kiểm tra tính khả thi sẽ quét tất cả`N`túi và tìm kiếm nhị phân thực hiện nhiều lần kiểm tra theo logarit | 
| Không gian |`O(N)`| Mảng số lượng bỏng ngô được lưu trong bộ nhớ | 

Tổng số bỏng ngô nhiều nhất là`10^5 * 10^4 = 10^9`, do đó tìm kiếm nhị phân chỉ có khoảng ba mươi lần lặp. Mỗi lần lặp thực hiện quét tuyến tính nhiều nhất`10^5`túi, đưa ra khoảng vài triệu thao tác đơn giản. Điều này là thoải mái trong phạm vi dự định cho các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, c, t = map(int, input().split())
    p = list(map(int, input().split()))

    total = sum(p)

    def feasible(seconds):
        capacity = seconds * t
        competitors = 1
        current = 0

        for x in p:
            if x > capacity:
                return False

            if current + x <= capacity:
                current += x
            else:
                competitors += 1
                current = x

                if competitors > c:
                    return False

        return True

    lo = 1
    hi = (total + t - 1) // t

    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""5 3 4
5 8 3 10 7
""") == "4", "sample 1"

assert run("""3 2 1
1 5 1
""") == "6", "sample 2"

assert run("""3 2 1
1 1 5
""") == "5", "sample 3"

assert run("""1 1 1
1
""") == "1", "minimum-size input"

assert run("""1 5 4
5
""") == "2", "one bag with unused competitors"

assert run("""4 4 10
7 7 7 7
""") == "1", "one competitor per equal bag"

assert run("""5 2 1
1 1 1 1 1
""") == "3", "contiguous partition boundary"

assert run("""100000 100000 50
""" + " ".join(["10000"] * 100000) + "\n") == "200", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 4 / 5 8 3 10 7`|`4`| Cung cấp mẫu và phân vùng chung | 
|`3 2 1 / 1 5 1`|`6`| Túi lớn ở giữa và nhóm liền kề | 
|`3 2 1 / 1 1 5`|`5`| Túi chiếm ưu thế quyết định câu trả lời | 
|`1 1 1 / 1`|`1`| Đầu vào kích thước tối thiểu | 
|`1 5 4 / 5`|`2`| Nhiều đối thủ cạnh tranh hơn túi xách | 
|`4 4 10 / 7 7 7 7`|`1`| Giá trị ngang nhau và đủ đối thủ | 
|`5 2 1 / 1 1 1 1 1`|`3`| Phân vùng liền kề và xử lý ranh giới | 
|`100000 100000 50 / 10000 ...`|`200`| Tối đa`N`và giá trị lớn | 

Bài kiểm tra kích thước tối đa chứa`100000`túi, mỗi cái có`10000`bỏng ngô. Vì cũng có`100000`đối thủ cạnh tranh, mỗi túi có thể được giao cho đối thủ cạnh tranh của riêng mình. Mỗi túi mất`ceil(10000 / 50) = 200`giây, vì vậy câu trả lời mong đợi là`200`. 

## Vỏ cạnh 

### Một chiếc túi đơn 

Hãy xem xét```
1 3 4
5
```Tìm kiếm nhị phân cuối cùng sẽ kiểm tra`2`giây, cho công suất`8`. Chiếc túi duy nhất vừa vặn và tấm séc tham lam sử dụng một đối thủ cạnh tranh, nhiều nhất là ba người. Kết quả là`2`. 

Thuật toán không bao giờ cố gắng chia túi cho các đối thủ cạnh tranh. Toàn bộ túi được xử lý bởi cùng một phân đoạn tham lam, trực tiếp tôn trọng quy tắc không thể phân chia. 

### Nhiều đối thủ hơn túi 

Hãy xem xét```
2 5 3
4 7
```Tổng số bỏng ngô là`11`, đưa ra giới hạn trên của`4`giây. Tại`3`giây, mỗi đối thủ có thể xử lý`9`bỏng ngô. Phân vùng tham lam là`[4] | [7]`, yêu cầu hai đối thủ cạnh tranh. Từ`2 <= 5`, ứng cử viên là khả thi và câu trả lời là`3`. 

Việc kiểm tra cố tình sử dụng`competitors <= C`còn hơn là`competitors == C`. Đối thủ cạnh tranh trống là hợp pháp, vì vậy việc yêu cầu chính xác năm phân đoạn không trống sẽ từ chối trường hợp này một cách không chính xác. 

### Một túi lớn hơn sức chứa của ứng viên 

Hãy xem xét```
3 2 1
1 1 5
```Vì`4`giây, công suất là`4`. Khi quá trình quét tham lam đến túi cuối cùng chứa`5`, điều kiện`x > capacity`ngay lập tức khiến ứng viên không thể ứng tuyển được. Việc sắp xếp lại không thể giúp ích gì vì các túi phải giữ nguyên trật tự và bản thân túi không thể phân chia được. 

Tại`5`giây công suất trở thành`5`. Phân vùng tham lam là`[1, 1] | [5]`, yêu cầu chính xác hai đối thủ cạnh tranh, vì vậy`5`là khả thi và là tối thiểu. 

### Vấn đề về nhóm liền kề 

Hãy xem xét```
5 2 1
1 1 1 1 1
```Với hai đối thủ, ba giây là đủ vì mảng có thể được chia thành`[1, 1, 1] | [1, 1]`. Đoạn lớn nhất chứa ba bắp rang, vì vậy câu trả lời là`3`. 

Hai giây sẽ yêu cầu cả hai đối thủ phải xử lý chính xác hai bắp rang, không để lại một túi nào. Một nhiệm vụ không liền kề có thể xuất hiện để cân bằng
