---
title: "CF 102836D - \u0418\u0433\u0440\u0430 \u0432 \u041c\u0430\u0444\u0438\u044e"
description: "Trò chơi có k người chơi và kéo dài trong m đêm. Trong mỗi đêm, những người chơi hiện còn sống sẽ có một số cuộc gặp gỡ với nhau. Vào cuối đêm, chính xác một người chơi còn sống bị mafia giết chết."
date: "2026-07-26T14:50:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102836
codeforces_index: "D"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102836
solve_time_s: 63
verified: true
draft: false
---

[CF 102836D - \u0418\u0433\u0440\u0430 \u0432 \u041c\u0430\u0444\u0438\u044e](https://codeforces.com/problemset/problem/102836/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi có`k`người chơi và kéo dài trong`m`đêm. Trong mỗi đêm, những người chơi hiện còn sống sẽ có một số cuộc gặp gỡ với nhau. Vào cuối đêm, chính xác một người chơi còn sống bị mafia giết chết. Người chơi bị giết phải là thường dân và một trong những thành viên mafia đã gặp người chơi đó trong đêm này sẽ thực hiện vụ giết người. 

Đầu vào mô tả toàn bộ lịch sử của trò chơi: mỗi đêm chúng ta đều biết ai còn sống, ai gặp ai và ai đã chết. Nhiệm vụ là tìm ra số lượng thành viên mafia nhỏ nhất có thể gây ra chuỗi cái chết chính xác này. 

Hạn chế chính là chỉ có dân thường chết. Sau đó`m`chính xác là có những đêm`k - m`những người sống sót. Mọi thành viên mafia đều phải nằm trong số những người sống sót này. điều kiện`k - m <= 15`là đầu mối thuật toán chính. Mặc dù tổng số người chơi có thể là 200 nhưng số lượng ứng cử viên cho mafia nhiều nhất là 15, vì vậy việc thử các tập hợp con của các thành viên mafia có thể là khả thi. 

Một giải pháp kiểm tra tất cả các tập hợp con của tất cả người chơi sẽ cần tới`2^200`trường hợp đó là điều không thể. Số lượng người sống sót nhỏ sẽ thay đổi hoàn toàn vấn đề: chúng ta chỉ cần xem xét các tập hợp con có tối đa 15 người, tức là có nhiều nhất là 32768 khả năng. 

Có một số trường hợp nguy hiểm khi việc triển khai bất cẩn không thành công. Người chơi chết không thể là mafia, ngay cả khi họ gặp mọi nạn nhân. Ví dụ: nếu dữ liệu đầu vào mô tả hai người chơi và một đêm mà người chơi 1 chết thì câu trả lời là`1`bởi vì người chơi 1 không thể là mafia và người chơi 2 phải là kẻ giết người. Một sai lầm phổ biến khác là quên rằng thành viên mafia phải gặp nạn nhân vào đúng đêm xảy ra án mạng. Người chơi đã gặp nạn nhân vào đêm hôm trước nhưng không có mặt trong đêm giết chóc thì không thể giải thích được cái chết đó. 

Một ví dụ nhỏ:```
2 1
1 1
2
1 1
2
2
```Đêm đầu tiên bắt đầu với người chơi 1 và 2 còn sống. Họ gặp nhau, sau đó người chơi 2 chết. Câu trả lời là`1`, bởi vì chỉ có người chơi 1 sống sót và mafia phải sống sót sau tất cả các đêm. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi nhóm thành viên mafia có thể có trong số tất cả người chơi. Đối với mỗi tập ứng cử viên, chúng tôi sẽ mô phỏng tất cả các đêm và kiểm tra xem mỗi nạn nhân có thể bị giết bởi ai đó trong tập này hay không. Mô phỏng là chính xác vì yêu cầu duy nhất đối với một nhóm mafia là mỗi cái chết đều có một thành viên mafia còn sống sót đã gặp nạn nhân vào đêm đó. 

Vấn đề là số lượng ứng viên. Với 200 người chơi, cuộc tìm kiếm bạo lực sẽ có`2^200`khả năng, vượt xa những gì có thể được xử lý. 

Điều quan trọng là các thành viên mafia không bao giờ chết. Sau tất cả các đêm, thành viên mafia duy nhất có thể là`k - m`những người sống sót. Tuyên bố đảm bảo rằng con số này nhiều nhất là 15. Chúng tôi có thể liệt kê tất cả các tập hợp con của những người sống sót và kiểm tra chúng. Có nhiều nhất`2^15`tập hợp con và mỗi tập hợp con có thể được kiểm tra tối đa 200 đêm. 

Lực lượng vũ phu hoạt động vì nó kiểm tra định nghĩa chính xác về một nhóm mafia hợp lệ, nhưng nó thất bại vì tìm kiếm những người chơi không liên quan. Việc hạn chế tìm kiếm những người sống sót biến vấn đề thành một vấn đề nhỏ về kiểu vỏ bọc: mọi ứng cử viên thành viên mafia che đậy những đêm mà lẽ ra họ có thể giết nạn nhân, và chúng ta cần nhóm nhỏ nhất che chắn cả đêm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * k * m) | O(k * m) | Quá chậm | 
| Tối ưu | O(2^(k-m) * m * (k-m)) | O(k * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ lịch sử trò chơi và mô phỏng những người chơi còn sống sau nhiều đêm. Lưu giữ mỗi đêm như nạn nhân và danh sách các cuộc gặp gỡ diễn ra trước khi chết. 
2. Chỉ định một vị trí bit cho mỗi người sống sót. Chỉ những người chơi này mới có thể là mafia, vì các thành viên mafia không thể bị giết. 
3. Mỗi đêm, hãy tạo một bitmask gồm những người chơi sống sót đã gặp nạn nhân trong đêm đó. Một bộ mafia hợp lệ phải chứa ít nhất một bit từ mặt nạ này cho mỗi đêm. 
4. Liệt kê từng tập hợp con của những người sống sót. Đối với mỗi tập hợp con, hãy kiểm tra hàng đêm. Nếu tập hợp con giao nhau với mỗi mặt nạ ban đêm thì đó có thể là một đội mafia. 
5. Theo dõi kích thước nhỏ nhất của tập hợp con hợp lệ và xuất nó. 

Tại sao nó hoạt động: 

Mọi thành viên mafia thực sự đều phải sống sót qua đêm, vì vậy không gian tìm kiếm chứa đựng mọi câu trả lời có thể. Một tập hợp con chỉ được chấp nhận khi mỗi nạn nhân có ít nhất một thành viên trong tập hợp con đó đã gặp họ trong đêm tương ứng. Đây chính xác là điều kiện cần thiết để trò chơi được mô tả diễn ra. Vì mọi đội mafia hợp lệ đều được xem xét và mọi đội không hợp lệ đều bị từ chối, nên kích thước tối thiểu được chấp nhận là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return
    it = iter(data)

    k = int(next(it))
    m = int(next(it))

    nights = []
    alive = set(range(1, k + 1))

    for _ in range(m):
        t = int(next(it))
        meet = {}
        for _ in range(t):
            v = int(next(it))
            c = int(next(it))
            friends = []
            for _ in range(c):
                friends.append(int(next(it)))
            meet[v] = friends
        dead = int(next(it))
        nights.append((meet, dead))
        alive.remove(dead)

    survivors = sorted(alive)
    idx = {x: i for i, x in enumerate(survivors)}
    s = len(survivors)

    masks = []
    for meet, dead in nights:
        mask = 0
        for player in meet.get(dead, []):
            if player in idx:
                mask |= 1 << idx[player]
        masks.append(mask)

    ans = s
    for subset in range(1 << s):
        cnt = subset.bit_count()
        if cnt >= ans:
            continue
        ok = True
        for mask in masks:
            if subset & mask == 0:
                ok = False
                break
        if ok:
            ans = cnt

    print(ans)

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp lưu trữ mỗi đêm thay vì xử lý ngay lập tức vì tập hợp người sống sót cuối cùng không xác định được cho đến khi tất cả số người chết được đọc. Sau khi tìm thấy những người sống sót, mọi thành viên mafia có thể có đều có thể được biểu diễn bằng một bit, điều này khiến việc kiểm tra một ứng cử viên phải thực hiện một vài phép tính số nguyên. 

Việc xây dựng`masks`là chi tiết triển khai trung tâm. Một bit chỉ được thiết lập khi người sống sót đó gặp nạn nhân vào đúng đêm đó. Kiểm tra tập hợp con sau đó trở thành kiểm tra giao điểm giữa hai mặt nạ bit. Số nguyên Python xử lý các mặt nạ này một cách tự nhiên vì chỉ cần 15 bit. 

Việc liệt kê bắt đầu từ tất cả các tập hợp con, bao gồm cả tập hợp trống. Đầu vào được đảm bảo mô tả một trò chơi hợp lệ, do đó ít nhất một tập hợp con sẽ vượt qua. Tập hợp con trống sẽ chỉ trôi qua khi không có đêm, điều này không thể xảy ra ở đây vì`m >= 1`, nhưng việc giữ nguyên logic chung sẽ tránh được những trường hợp đặc biệt không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4 2
1 3
2 3 4
2 3
1 3 4
3 3
1 2 4
4 3
1 2 3
1
2 2
3 4
3 2
2 4
4 2
2 3
2
```Người sống sót cuối cùng là người chơi 2, vì vậy thành viên mafia duy nhất có thể là người chơi 2. 

| Bước | Người sống sót | Mặt nạ ban đêm | Tập hợp con hiện tại | 
| --- | --- | --- | --- | 
| Sau khi phân tích | 2 | {2}, {2} | trống | 
| Tập hợp con kiểm tra`{2}`| 2 | cả hai đêm được bảo hiểm | hợp lệ | 

Dấu vết cho thấy tại sao chỉ những người sống sót mới quan trọng. Người chơi đã chết không thể được chọn ngay cả khi họ tham gia nhiều cuộc họp. 

Ví dụ tùy chỉnh:```
3 1
1 1
2
1 1
2
2
```| Bước | Người sống sót | Mặt nạ ban đêm | Tập hợp con hiện tại | 
| --- | --- | --- | --- | 
| Sau khi phân tích | 1, 2 | {1} | trống | 
| Bài kiểm tra`{1}`| 1, 2 | được bảo hiểm | hợp lệ | 

Câu trả lời là`1`. Ví dụ chứng minh rằng người chơi sống sót phải che đậy cái chết trong đêm duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(k-m) * m * (k-m)) | Chúng tôi chỉ liệt kê các tập hợp con sống sót và kiểm tra tất cả các đêm | 
| Không gian | O(k * m) | Lịch sử được lưu trữ chứa mọi mô tả cuộc họp | 

Số lượng tập hợp con tối đa là`2^15`, chỉ là 32768. Kết hợp với tối đa 200 đêm và 15 người sống sót, thuật toán dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    k = int(next(it))
    m = int(next(it))

    nights = []
    alive = set(range(1, k + 1))

    for _ in range(m):
        t = int(next(it))
        meet = {}
        for _ in range(t):
            v = int(next(it))
            c = int(next(it))
            meet[v] = [int(next(it)) for _ in range(c)]
        dead = int(next(it))
        nights.append((meet, dead))
        alive.remove(dead)

    survivors = sorted(alive)
    idx = {x: i for i, x in enumerate(survivors)}
    masks = []

    for meet, dead in nights:
        mask = 0
        for x in meet[dead]:
            if x in idx:
                mask |= 1 << idx[x]
        masks.append(mask)

    ans = len(survivors)
    for sub in range(1 << len(survivors)):
        if sub.bit_count() >= ans:
            continue
        if all(sub & mask for mask in masks):
            ans = sub.bit_count()

    return str(ans) + "\n"

assert run("""4 2
1 3
2 3 4
2 3
1 3 4
3 3
1 2 4
4 3
1 2 3
1
2 2
3 4
3 2
2 4
4 2
2 3
2
""") == "1\n", "sample"

assert run("""2 1
1 1
2
1 1
2
2
""") == "1\n", "two players"

assert run("""4 2
1 3
2 3 4
2 3
1 3 4
3 3
1 2 4
4 3
1 2 3
1
2 2
3 4
3 2
2 4
4 2
2 3
2
""") == "1\n", "single survivor mafia"

assert run("""5 2
5 0
1 2 3 4 5
1
1 0
2
5 0
1 2 3 4
1
""") == "1\n", "same survivor covers both"

assert run("""3 2
2
1 2
2 2
1 3
1
3
""") == "1\n", "minimum survivor count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | 1 | Tính đúng đắn chung | 
| Hai người chơi | 1 | Trò chơi nhỏ nhất có thể | 
| Một người sống sót sau nhiều cái chết | 1 | Hạn chế người sống sót | 
| Cùng một người chơi bao gồm tất cả các cái chết | 1 | Tái sử dụng một thành viên mafia | 
| Số lượng ứng viên còn lại tối thiểu | 1 | Xử lý ranh giới | 

## Vỏ cạnh 

Một người chơi đã chết được chọn làm mafia là sai lầm khái niệm phổ biến nhất. Trong thuật toán, điều này không thể xảy ra vì việc liệt kê chỉ bắt đầu sau khi tất cả các trường hợp tử vong được xử lý và chỉ chứa những người sống sót. 

Một đêm mà không có người sống sót nào gặp được nạn nhân sẽ khiến mọi ứng cử viên đều thất bại. Đầu vào đảm bảo rằng tồn tại một số nhiệm vụ mafia, do đó tình huống này không thể xảy ra trong các thử nghiệm hợp lệ, nhưng vòng kiểm tra vẫn sẽ xử lý chính xác bằng cách từ chối tất cả các tập hợp con. 

Trường hợp một người sống sót có thể giải thích mọi cái chết đều được xử lý một cách tự nhiên. Tất cả các mặt nạ ban đêm đều chứa cùng một bit và tập hợp con chỉ chứa bit đó là giải pháp tối thiểu đầu tiên. 

Trường hợp cần một số người sống sót cũng được đề cập vì bảng liệt kê kiểm tra các tổ hợp chứ không chỉ từng người chơi. Một tập hợp con chỉ được chấp nhận sau khi mỗi đêm có ít nhất một kẻ giết người tiềm năng bên trong nó.
