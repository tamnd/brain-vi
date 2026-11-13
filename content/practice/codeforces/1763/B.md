---
title: "CF 1763B - Đốt"
description: "Chúng tôi đang chiến đấu với một nhóm quái vật. Mỗi quái vật đều có giá trị sức khỏe và giá trị sức mạnh. Genos liên tục thực hiện các cuộc tấn công. Nếu sức mạnh tấn công hiện tại là k, mọi quái vật vẫn còn sống sẽ đồng thời mất k máu."
date: "2026-06-09T13:44:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1763
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 840 (Div. 2) and Enigma 2022 - Cybros LNMIIT"
rating: 1200
weight: 1763
solve_time_s: 671
verified: false
draft: false
---

[CF 1763B - Thiêu đốt](https://codeforces.com/problemset/problem/1763/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, triển khai, toán học, sắp xếp 
**Thời gian giải:** 11m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang chiến đấu với một nhóm quái vật. Mỗi quái vật đều có giá trị sức khỏe và giá trị sức mạnh. 

Genos liên tục thực hiện các cuộc tấn công. Nếu sức mạnh tấn công hiện tại là`k`, mọi quái vật còn sống đều thua cuộc`k`sức khỏe cùng một lúc. Sau cuộc tấn công đó, chúng tôi nhìn những con quái vật vẫn còn sống. Trong số đó, tìm giá trị công suất tối thiểu. Công suất tối thiểu đó được trừ đi từ`k`, làm giảm sức mạnh của tất cả các cuộc tấn công trong tương lai. 

Quá trình tiếp tục cho đến khi mọi quái vật đều chết, nghĩa là câu trả lời là`"YES"`, hoặc sức mạnh tấn công trở nên không tích cực trong khi quái vật nào đó vẫn còn sống, điều đó có nghĩa là câu trả lời là`"NO"`. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết cường độ tấn công ban đầu`k`, sức khỏe của mọi quái vật và sức mạnh của mọi quái vật. Chúng ta phải xác định xem liệu cuối cùng tất cả quái vật có thể bị tiêu diệt hay không. 

Các ràng buộc là điều đầu tiên sẽ hướng dẫn giải pháp. Một sức khỏe hoặc sức mạnh có thể lớn bằng`10^9`, Và`k`cũng có thể`10^9`. Việc mô phỏng từng đòn tấn công một là không thể vì có thể có hàng tỷ đòn tấn công trước khi tất cả quái vật chết. Tổng số quái vật trong tất cả các trường hợp thử nghiệm chỉ là`2·10^5`, điều này gợi ý rõ ràng rằng giải pháp nên chi tiêu khoảng`O(n log n)`hoặc`O(n)`làm việc cho mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp xuất hiện khi nhiều quái vật chết trong cùng một đòn tấn công. Quái vật còn sống yếu nhất sau cuộc tấn công có thể khác hoàn toàn với quái vật yếu nhất trước cuộc tấn công. 

Coi như:```
n = 2, k = 5
h = [5, 100]
p = [1, 100]
```Sau đòn tấn công đầu tiên, quái vật đầu tiên sẽ chết ngay lập tức. Lần giảm tiếp theo phải sử dụng nguồn điện`100`, không phải sức mạnh`1`, bởi vì quái vật yếu nhất còn sống sót bây giờ đã có sức mạnh`100`. 

Một sai lầm dễ mắc phải khác là mô phỏng trực tiếp các giá trị sức khỏe. Giá trị sức khỏe có thể lớn bằng`10^9`, do đó, việc liên tục trừ đi sát thương từ mọi quái vật sẽ là quá chậm. 

Bẫy thứ ba là cho rằng quái vật phải được xử lý bằng máu. Các giá trị sức mạnh xác định các cuộc tấn công trong tương lai suy yếu nhanh như thế nào, do đó, việc chỉ sắp xếp theo sức khỏe sẽ làm mất thông tin quan trọng. 

Ví dụ:```
h = [10, 20]
p = [100, 1]
```Quái vật có sức mạnh thấp có tầm quan trọng về mặt chiến lược vì giết nó muộn sẽ giúp sức tấn công cao lâu hơn. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ liên tục thực hiện các cuộc tấn công. Trong mỗi cuộc tấn công, chúng tôi sẽ trừ đi sát thương hiện tại của mọi quái vật còn sống, xác định quái vật nào còn sống, tìm sức mạnh tối thiểu trong số chúng, giảm bớt`k`, và tiếp tục. 

Điều này đúng vì nó trực tiếp tuân theo các quy tắc. Thật không may, số lượng các cuộc tấn công không bị giới hạn bởi`n`. Sức mạnh tấn công và giá trị sức khỏe có thể đạt tới`10^9`, do đó quá trình này có thể yêu cầu số lần lặp rất lớn. Ngay cả khi mỗi cuộc tấn công chỉ mất`O(n)`, toàn bộ công việc sẽ hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là chỉ những khoảnh khắc quái vật chết mới thực sự quan trọng. 

Cho phép`damage`biểu thị tổng thiệt hại gây ra cho mỗi quái vật cho đến nay. Quái vật chết một lần`damage ≥ h_i`. 

Giả sử chúng ta biết thiệt hại tích lũy hiện tại`damage`. Sau đó mọi quái vật với`h_i ≤ damage`đã chết rồi. Trong số những quái vật còn lại, sức mạnh nhỏ nhất sẽ quyết định sức mạnh tấn công sẽ giảm bao nhiêu sau đòn tấn công tiếp theo. 

Điều này gợi ý việc sắp xếp quái vật theo sức khỏe. Sau khi được sắp xếp, chúng tôi có thể xác định một cách hiệu quả những quái vật nào đã chết khi tổng sát thương tích lũy tăng lên. 

Quan sát thứ hai là chúng ta chỉ cần biết sức mạnh tối thiểu của những con quái vật có sức khỏe vẫn lớn hơn sát thương tích lũy hiện tại. Điều này có thể được duy trì với một mảng hậu tố tối thiểu. 

Sau khi sắp xếp theo sức khỏe:```
(h1,p1), (h2,p2), ...
```định nghĩa```
suf[i] = min power among monsters i..n-1
```Nếu tất cả quái vật có chỉ số nhỏ hơn`ptr`đã chết thì con quái vật yếu nhất còn sống sẽ có sức mạnh`suf[ptr]`. 

Bây giờ quá trình trở nên rất nhỏ gọn: 

Chúng tôi duy trì tổng thiệt hại tích lũy`damage`. 

Mỗi lần lặp lại sẽ tăng thêm sức mạnh tấn công hiện tại`k`ĐẾN`damage`. 

Chúng tôi tiến một con trỏ qua mọi quái vật có lượng máu hiện tại nhiều nhất`damage`. 

Nếu tất cả quái vật đã chết, hãy trả lời`"YES"`. 

Nếu không, sức mạnh sống sót yếu nhất là`suf[ptr]`, vì vậy chúng tôi giảm`k`bằng số tiền đó. 

Số lần lặp nhiều nhất là`n`, bởi vì mỗi lần lặp lại sẽ giết chết ít nhất một quái vật mới hoặc tạo ra`k`không tích cực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn, có khả năng xảy ra hàng tỷ cuộc tấn công | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép sức khỏe và sức mạnh của mỗi quái vật vào`(h_i, p_i)`. 
2. Sắp xếp quái vật theo sức khỏe. 
3. Xây dựng một chuỗi quyền hạn tối thiểu. Đối với mọi vị trí`i`,`suf[i]`tích trữ sức mạnh tối thiểu của quái vật từ`i`đến cuối cùng. 
4. Khởi tạo: 

-`damage = 0`-`ptr = 0`5. Trong khi`k > 0`: 

1. Cộng sức tấn công hiện tại vào tổng sát thương:`damage += k`. 
2. Di chuyển`ptr`chuyển tiếp trong khi`health[ptr] ≤ damage`. 

Những con quái vật này đã chết rồi. 
3. Nếu`ptr == n`, mọi quái vật đều đã chết. đầu ra`"YES"`. 
4. Ngược lại, quái vật yếu nhất còn sống sót sẽ có sức mạnh`suf[ptr]`. Giảm sức mạnh tấn công:`k -= suf[ptr]`. 
6. Nếu vòng lặp kết thúc vì`k ≤ 0`và một số quái vật vẫn còn sống, xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Sát thương tích lũy sau vài đòn tấn công chính xác bằng tổng lượng máu của mỗi quái vật bị trừ đi. Một con quái vật còn sống chính xác khi sức khỏe của nó vượt quá mức sát thương tích lũy này. 

Tại bất kỳ thời điểm nào, việc giảm sức mạnh tấn công trong tương lai chỉ phụ thuộc vào sức mạnh tối thiểu của quái vật còn sống. Sau khi sắp xếp theo máu, quái vật còn sống luôn tạo thành hậu tố của mảng vì mọi quái vật có lượng máu nhỏ hơn sẽ chết không muộn hơn quái vật có lượng máu lớn hơn. 

Mảng hậu tố tối thiểu mang lại sức mạnh tối thiểu trong số tất cả quái vật còn sống trong thời gian không đổi. Mỗi lần lặp lại đều tái tạo chính xác quá trình chuyển đổi trạng thái giống như quy trình ban đầu nhưng không theo dõi các giá trị trạng thái riêng lẻ. Vì mô phỏng vẫn giống nhau về mặt toán học nên thuật toán tạo ra kết quả tương tự như trò chơi gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, k = map(int, input().split())
        h = list(map(int, input().split()))
        p = list(map(int, input().split()))

        monsters = sorted(zip(h, p))

        suf = [0] * n
        suf[-1] = monsters[-1][1]

        for i in range(n - 2, -1, -1):
            suf[i] = min(suf[i + 1], monsters[i][1])

        damage = 0
        ptr = 0
        cur_k = k

        while cur_k > 0:
            damage += cur_k

            while ptr < n and monsters[ptr][0] <= damage:
                ptr += 1

            if ptr == n:
                answers.append("YES")
                break

            cur_k -= suf[ptr]
        else:
            answers.append("NO")

    sys.stdout.write("\n".join(answers))

solve()
```Bước sắp xếp tạo ra thứ tự quái vật có thể biến mất. Khi tổng sát thương tích lũy đạt tới mức máu của quái vật, quái vật đó sẽ chết vĩnh viễn, vì vậy quái vật còn sống luôn chiếm một hậu tố của mảng đã được sắp xếp. 

Mảng hậu tố tối thiểu là tối ưu hóa quan trọng. Nếu không có nó, mỗi lần lặp lại sẽ phải quét tất cả quái vật còn sống sót để tìm ra sức mạnh yếu nhất. Với nó, sức mạnh sống sót yếu nhất sẽ có sẵn ngay lập tức. 

Con trỏ`ptr`chỉ tiến về phía trước. Mỗi quái vật được bỏ qua một lần, mang lại tổng cộng`O(n)`chuyển động của con trỏ trên toàn bộ trường hợp thử nghiệm. 

sử dụng`damage`thay vì cập nhật từng tình trạng riêng lẻ, hãy tránh xử lý các giá trị lớn bằng`10^9`lặp đi lặp lại và giữ cho mô phỏng hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 7

h = [18,5,13,9,10,1]
p = [2,7,2,1,2,6]
```Sau khi sắp xếp: 

| Chỉ mục | Sức khỏe | Quyền lực | 
| --- | --- | --- | 
| 0 | 1 | 6 | 
| 1 | 5 | 7 | 
| 2 | 9 | 1 | 
| 3 | 10 | 2 | 
| 4 | 13 | 2 | 
| 5 | 18 | 2 | 

Hậu tố quyền hạn tối thiểu:```
[1,1,1,2,2,2]
```| Lặp lại | Hiện tại k | Thiệt hại sau khi tấn công | ptr | Sức mạnh sống yếu nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | 2 | 1 | 
| 2 | 6 | 13 | 5 | 2 | 
| 3 | 4 | 17 | 5 | 2 | 
| 4 | 2 | 19 | 6 | chết | 

Tất cả quái vật đều đã chết nên câu trả lời là`"YES"`. 

Ví dụ này cho thấy nhiều quái vật có thể chết đồng thời như thế nào khi sát thương tích lũy vượt quá một số ngưỡng máu. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 4
h = [5,5,5]
p = [4,4,4]
```Thứ tự sắp xếp không thay đổi. 

| Lặp lại | Hiện tại k | Thiệt hại sau khi tấn công | ptr | Sức mạnh sống yếu nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 0 | 4 | 
| 2 | 0 | - | - | - | 

Sức mạnh tấn công trở thành 0 trước khi tổng sát thương đạt tới 5 máu, vì vậy quái vật vẫn còn sống và câu trả lời là`"NO"`. 

Ví dụ này thể hiện tình trạng thất bại khi sức mạnh tấn công cạn kiệt trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Sắp xếp mảng và hậu tố mảng tối thiểu | 

Tổng số quái vật trong tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`. MỘT`O(n log n)`giải pháp vừa vặn thoải mái trong giới hạn thời gian và bộ lưu trữ phụ trợ tuyến tính dễ dàng vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        h = list(map(int, input().split()))
        p = list(map(int, input().split()))

        monsters = sorted(zip(h, p))

        suf = [0] * n
        suf[-1] = monsters[-1][1]

        for i in range(n - 2, -1, -1):
            suf[i] = min(suf[i + 1], monsters[i][1])

        damage = 0
        ptr = 0
        cur_k = k

        while cur_k > 0:
            damage += cur_k

            while ptr < n and monsters[ptr][0] <= damage:
                ptr += 1

            if ptr == n:
                out.append("YES")
                break

            cur_k -= suf[ptr]
        else:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run(
"""3
6 7
18 5 13 9 10 1
2 7 2 1 2 6
3 4
5 5 5
4 4 4
3 2
2 1 3
1 1 1
"""
) == "YES\nNO\nYES"

# minimum size
assert run(
"""1
1 1
1
1
"""
) == "YES"

# immediate failure
assert run(
"""1
1 1
2
5
"""
) == "NO"

# all monsters die in first attack
assert run(
"""1
4 100
1 2 3 4
10 20 30 40
"""
) == "YES"

# power changes after weakest dies
assert run(
"""1
2 5
5 100
1 100
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quái vật đơn có sức khỏe 1 | CÓ | Trường hợp thành công ở quy mô tối thiểu | 
| Quái vật đơn có máu 2 và k=1 | KHÔNG | Sức tấn công cạn kiệt | 
| Cuộc tấn công đầu tiên rất lớn | CÓ | Nhiều quái vật chết cùng một lúc | 
| Quyền hạn`[1,100]`| KHÔNG | Sửa lỗi cập nhật yếu nhất sau khi chết | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả quái vật đều có sức khỏe giống nhau:```
1
3 5
5 5 5
1 2 3
```Sau đòn tấn công đầu tiên, sát thương tích lũy trở thành 5. Mọi quái vật chết đồng thời, con trỏ sẽ di chuyển thẳng đến`n`và kết quả đầu ra của thuật toán`"YES"`. Giải pháp cập nhật từng quái vật một có thể vô tình áp dụng mức giảm sức mạnh bổ sung sau khi tất cả mọi người đều đã chết. 

Hãy xem xét trường hợp sức mạnh tấn công đạt chính xác bằng 0:```
1
1 4
5
4
```Sau đòn tấn công đầu tiên, sát thương tích lũy là 4 và quái vật vẫn còn sống. Sức sống yếu nhất là 4, do đó sức tấn công trở thành 0. Vòng lặp dừng lại và câu trả lời là`"NO"`. Thuật toán kiểm tra chính xác xem quái vật có chết hay không trước khi áp dụng đòn tấn công tiếp theo. 

Hãy xem xét trường hợp có nhiều quái vật chết trong một lần lặp:```
1
4 10
3 4 5 100
1 1 1 1
```Sát thương tích lũy ngay lập tức trở thành 10, giết chết ba quái vật đầu tiên. Con trỏ bỏ qua cả ba trong một lần. Tập sống động vẫn là hậu tố của thứ tự được sắp xếp, đây chính xác là thuộc tính làm cho kỹ thuật tối thiểu hậu tố trở nên hợp lệ.
