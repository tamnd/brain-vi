---
title: "CF 2067B - Hai Túi Lớn"
description: "Chúng ta bắt đầu với một dãy n số trong túi thứ nhất và túi thứ hai trống. Vì các túi cuối cùng phải giống nhau nên mỗi túi phải chứa đúng n/2 số. Chỉ có hai hoạt động được phép."
date: "2026-06-09T03:38:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2067
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1004 (Div. 2)"
rating: 1200
weight: 2067
solve_time_s: 136
verified: true
draft: false
---

[CF 2067B - Hai túi lớn](https://codeforces.com/problemset/problem/2067/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, dp, tham lam, phân loại 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một loạt`n`số ở túi thứ nhất và túi thứ hai trống. Vì các túi cuối cùng phải giống nhau nên mỗi túi phải chứa chính xác`n/2`những con số. 

Chỉ có hai hoạt động được phép. 

Thao tác đầu tiên di chuyển một số từ túi thứ nhất sang túi thứ hai. 

Thao tác thứ hai tăng một số trong túi thứ nhất lên một, nhưng chỉ khi giá trị hiện tại đã tồn tại trong túi thứ hai. 

Câu hỏi đặt ra là liệu chúng ta có thể thực hiện bất kỳ chuỗi thao tác nào như vậy để cuối cùng, hai túi chứa chính xác nhiều tập số giống nhau hay không. 

Các giá trị thỏa mãn`1 ≤ a[i] ≤ n`, Và`n ≤ 1000`. Tổng của`n²`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^6`, đó là một gợi ý mạnh mẽ rằng`O(n²)`giải pháp hoàn toàn có thể chấp nhận được. Chúng tôi không cần bất cứ thứ gì phức tạp như cấu trúc dữ liệu logarit hoặc lập trình động nâng cao. 

Phần khó khăn là hiểu được hoạt động tăng dần thực sự cho phép điều gì. 

Giả sử túi thứ hai chứa giá trị`x`. Sau đó bất kỳ sự xuất hiện của`x`còn lại trong túi đầu tiên có thể được chuyển thành`x+1`. Nếu túi thứ hai cũng chứa`x+1`, giá trị mới đó sau đó có thể được tăng trở lại. Điều này có nghĩa là một giá trị có thể được đẩy lên trên thông qua một chuỗi các giá trị liên tiếp, nhưng mỗi bước đều yêu cầu giá trị hiện tại đã tồn tại trong túi thứ hai. 

Một sai lầm phổ biến là chỉ nghĩ đến tính chẵn lẻ của tần số. Ví dụ:```
4
1 1 2 2
```Câu trả lời là`YES`. Di chuyển một`1`và một`2`sang túi thứ hai. Các túi đã giống hệt nhau. 

Nhưng chỉ tính chẵn lẻ thôi là chưa đủ:```
4
1 1 1 4
```Câu trả lời là`NO`. 

Mặc dù một số tần số là chẵn nhưng không có cách nào để tạo ra một tần số khác`4`. Việc tăng dần yêu cầu các giá trị trung gian tồn tại trong túi thứ hai và không có chuỗi kết nối`1`ĐẾN`4`. 

Một cạm bẫy dễ mắc phải khác là quên rằng mức tăng chỉ tăng lên. Coi như:```
4
2 2 3 4
```Câu trả lời là`NO`. 

Chúng ta có thể tạo ra những số lớn hơn từ những số nhỏ hơn, nhưng chúng ta không bao giờ có thể giảm đi`4`ĐẾN`3`hoặc`2`. 

Hiểu được những hạn chế này là chìa khóa cho giải pháp. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là suy nghĩ về mọi lựa chọn có thể có trong đó`n/2`các phần tử được chuyển vào túi thứ hai, sau đó mô phỏng tất cả các hoạt động gia tăng hợp pháp. Về nguyên tắc, điều này đúng vì mọi trạng thái cuối cùng hợp lệ đều tương ứng với một số chuỗi di chuyển và gia số. 

Vấn đề là số lượng khả năng. Lựa chọn`n/2`các phần tử ra khỏi`n`đã cho rồi$$\binom{n}{n/2}$$khả năng là rất lớn ngay cả đối với người vừa phải`n`. Vì`n = 1000`, điều này là hoàn toàn không thể. 

Để tiến bộ, chúng ta cần hiểu rõ cấu trúc của các chuyển đổi pháp lý. 

Sắp xếp mảng và chỉ nghĩ về tần số của từng giá trị. 

Giả sử một giá trị`x`chỉ xuất hiện một lần. Cuối cùng, một bản sao phải được đặt vào mỗi túi vì các túi cuối cùng giống hệt nhau. Chỉ có một bản duy nhất, điều này là không thể. Hy vọng duy nhất là tăng bản sao đơn độc đó thành một giá trị lớn hơn trước khi phân chia cuối cùng. 

Bây giờ hãy xem xét một giá trị xuất hiện ít nhất hai lần. Chúng tôi có thể đặt một bản sao vào túi thứ hai và giữ một bản khác vào túi thứ nhất. Điều này có hiệu quả "khóa" giá trị`x`, bởi vì một lần`x`tồn tại trong túi thứ hai, các bản sao còn lại của`x`có thể tăng lên`x+1`. 

Điều này gợi ý việc xử lý các giá trị từ trái sang phải. Bất cứ khi nào một giá trị xuất hiện ít nhất hai lần, chúng tôi có thể đặt trước một cặp và đẩy tất cả các bản sao bổ sung sang giá trị tiếp theo. Các bản sao được chuyển tiếp đại diện cho các số có thể tăng lên. 

Quan sát quan trọng là giá trị có tần số lẻ không thể cố định cục bộ. Một bản sao phải được chuyển tiếp. Một giá trị có tần số bằng một sẽ gây tử vong vì không có cặp nào có sẵn để neo giá trị đó vào cả hai túi. 

Điều này dẫn đến một mô phỏng tham lam rất đơn giản về tần số. 

Cho phép`cnt[x]`là số lần xuất hiện của giá trị`x`. 

Xử lý các giá trị theo thứ tự tăng dần. 

Nếu như`cnt[x] == 1`, câu trả lời là ngay lập tức`NO`. 

Mặt khác, sau khi hình thành càng nhiều cặp giá trị`x`càng tốt, mọi bản sao thừa có thể được đẩy lên giá trị`x+1`. Về mặt hoạt động, điều này tương đương với việc thêm$$cnt[x] - 2$$ĐẾN`cnt[x+1]`bất cứ khi nào`cnt[x] ≥ 2`. 

Điều này hoàn toàn phù hợp với tác dụng của việc giữ một cặp`x`và tăng dần mỗi bản sao còn lại. 

Cuối cùng, nếu không gặp phải tần số nào thì việc xây dựng sẽ thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tần số tham lam tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mọi giá trị. 
2. Xử lý các giá trị từ`1`lên đến`n`. 
3. Nếu tần số hiện tại chính xác`1`, trở lại`"NO"`. 

Một bản sao không thể được phân chia giữa hai túi và không có cặp nào có sẵn để neo giá trị này. 
4. Nếu tần số hiện tại ít nhất là`2`, giữ một cặp giá trị đó. 

Hai bản sao này thể hiện sự xuất hiện cuối cùng của giá trị này trong cả hai túi. 
5. Mọi bản sao còn lại sẽ được chuyển tiếp đến giá trị tiếp theo. 

Thêm vào`cnt[x] - 2`ĐẾN`cnt[x+1]`. 

Mô hình này tăng số lượng bản sao dư thừa của`x`vào trong`x+1`. 
6. Tiếp tục cho đến khi tất cả các giá trị được xử lý. 
7. Nếu không gặp tần số nào, hãy quay lại`"YES"`. 

### Tại sao nó hoạt động 

Khi xử lý giá trị`x`, cách duy nhất để làm`x`xuất hiện trong cả hai túi cuối cùng là phải đặt trước ít nhất hai bản sao của`x`. Một bản sao đi vào mỗi túi. 

Nếu chỉ có một bản sao thì điều này là không thể. Không có hoạt động nào trong tương lai có thể tạo ra một hoạt động khác`x`, bởi vì số gia chỉ tăng giá trị. 

Khi có ít nhất hai bản sao, việc giữ chính xác hai bản luôn là tối ưu. Bất kỳ bản sao bổ sung nào cũng không thể giúp tạo ra thêm`x`các giá trị, vì vậy hành động hữu ích duy nhất là tăng chúng. Tăng dần chuyển chúng vào số lượng tần số của`x+1`, đó chính xác là những gì mô phỏng thực hiện. 

Việc xử lý các giá trị theo thứ tự tăng dần sẽ bảo toàn tất cả các trạng thái có thể truy cập. Mọi bản sao dư thừa sẽ được đẩy sang phải nếu cần thông qua các giá trị liên tiếp. Nếu thuật toán không bao giờ gặp tần số bằng 1 thì mọi giá trị có thể được neo bởi một cặp và tất cả các bản sao dư thừa có thể được truyền về phía trước. Do đó tồn tại một công trình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * (n + n + 5)

        for x in a:
            cnt[x] += 1

        ok = True

        for x in range(1, n + 1):
            if cnt[x] == 1:
                ok = False
                break

            if cnt[x] >= 2:
                cnt[x + 1] += cnt[x] - 2

        print("YES" if ok else "NO")

solve()
```Phần đầu tiên xây dựng bảng tần số. 

Vòng lặp chính xử lý các giá trị từ nhỏ nhất đến lớn nhất. Tần suất của một bản ngay lập tức chứng tỏ là không thể, bởi vì một bản sao không thể xuất hiện trong cả hai túi cuối cùng. 

Khi tần số ít nhất là hai, hai bản sao được dự trữ và mọi bản sao còn lại sẽ được chuyển sang giá trị tiếp theo. Điều này tương ứng chính xác với việc tăng số lần xuất hiện dư thừa. 

Mảng tần số được phân bổ lớn hơn`n + 1`vì các bản sao dư thừa có thể bị đẩy vượt quá giá trị tối đa ban đầu. Sử dụng mảng hơi quá lớn sẽ tránh được các vấn đề về ranh giới. 

Một điểm tinh tế là chúng ta không bao giờ trừ hai một cách rõ ràng khỏi`cnt[x]`. Sau khi xử lý giá trị`x`, tần số đó không bao giờ được sử dụng nữa. Chỉ có số tiền dư`cnt[x] - 2`vấn đề, vì vậy việc thêm trực tiếp số lượng đó vào`cnt[x+1]`là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 3 4 5 3 3
```Tần số ban đầu: 

| Giá trị | Tần số | 
| --- | --- | 
| 3 | 4 | 
| 4 | 1 | 
| 5 | 1 | 

Xử lý: 

| x | cnt[x] trước | Hành động | cnt[x+1] đã thêm | 
| --- | --- | --- | --- | 
| 1 | 0 | Bỏ qua | 0 | 
| 2 | 0 | Bỏ qua | 0 | 
| 3 | 4 | Giữ hai bản | 2 | 
| 4 | 3 | Giữ hai bản | 1 | 
| 5 | 2 | Giữ hai bản | 0 | 

Không có tần số nào xuất hiện trong quá trình xử lý sau khi truyền, vì vậy câu trả lời là:```
YES
```Ví dụ này cho thấy các bản sao dư thừa của`3`tạo thêm bản sao của`4`, từ đó tạo thêm bản sao của`5`. 

### Ví dụ 2 

đầu vào:```
4
2 3 4 4
```Tần số ban đầu: 

| Giá trị | Tần số | 
| --- | --- | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 

Xử lý: 

| x | cnt[x] trước | Kết quả | 
| --- | --- | --- | 
| 1 | 0 | Tiếp tục | 
| 2 | 1 | Thất bại | 

Khoảnh khắc chúng ta gặp tần số một, câu trả lời sẽ trở thành:```
NO
```Điều này chứng tỏ sự cản trở chính. Một giá trị đơn độc không thể được biểu thị trong cả hai túi cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần truyền tần số và một lần quét tuyến tính | 
| Không gian | O(n) | Mảng tần số | 

Tổng công việc thoải mái trong giới hạn. Ngay cả trên tất cả các trường hợp thử nghiệm, đầu vào đảm bảo rằng tổng của`n²`nhiều nhất là`10^6`lớn hơn nhiều so với những gì giải pháp tuyến tính này yêu cầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * (2 * n + 5)

        for x in a:
            cnt[x] += 1

        ok = True

        for x in range(1, n + 1):
            if cnt[x] == 1:
                ok = False
                break

            if cnt[x] >= 2:
                cnt[x + 1] += cnt[x] - 2

        out.append("YES" if ok else "NO")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""9
2
1 1
2
2 1
4
1 1 4 4
4
3 4 3 3
4
2 3 4 4
6
3 3 4 5 3 3
6
2 2 2 4 4 4
8
1 1 1 1 1 1 1 4
10
9 9 9 10 10 10 10 10 10 10
"""
) == """YES
NO
YES
YES
NO
YES
NO
YES
YES
"""

# minimum size, possible
assert run(
"""1
2
1 1
"""
) == """YES
"""

# minimum size, impossible
assert run(
"""1
2
1 2
"""
) == """NO
"""

# all values equal
assert run(
"""1
8
3 3 3 3 3 3 3 3
"""
) == """YES
"""

# frequency one appears after propagation
assert run(
"""1
4
1 1 1 4
"""
) == """NO
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`| CÓ | Trường hợp thành công nhỏ nhất | 
|`2 / 1 2`| KHÔNG | Trường hợp lỗi nhỏ nhất | 
| Tám giá trị bằng nhau | CÓ | Tuyên truyền thặng dư lớn | 
|`1 1 1 4`| KHÔNG | Tần suất tắc nghẽn sau khi chuyển giao | 

## Vỏ cạnh 

Hãy xem xét:```
2
1 2
```Các tần số là`{1:1, 2:1}`. Thuật toán ngay lập tức nhìn thấy`cnt[1] = 1`và trả về`NO`. 

Điều này đúng vì một bản sao của`1`không thể đặt vào cả hai túi cuối cùng. 

Coi như:```
4
1 1 1 4
```Tần số bắt đầu như`{1:3, 4:1}`. 

Giá trị xử lý`1`chuyển tiếp một bản sao dư thừa, tạo ra tần số`1`ở giá trị`2`. 

Bảng trở thành: 

| Giá trị | Tần số | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 4 | 1 | 

Khi giá trị`2`được xử lý, thuật toán tìm tần số bằng 1 và trả về`NO`. 

Điều này phù hợp với thực tế. Mặc dù có rất nhiều bản sao`1`, không có chuỗi giá trị neo liên tục nào cho phép tạo ra một chuỗi giá trị khác`4`. 

Coi như:```
8
1 1 1 1 1 1 1 4
```Quá trình xử lý tiến hành như sau: 

| x | cnt[x] | 
| --- | --- | 
| 1 | 7 | 
| 2 | 5 | 
| 3 | 3 | 
| 4 | 2 | 

Mỗi tần số ít nhất là hai, do đó thuật toán trả về`YES`. 

Các bản sao dư thừa của`1`tạo đủ`2`s, những thứ đó tạo ra đủ`3`s, và những thứ đó tạo ra đủ`4`S. Đây chính xác là loại chuỗi mà hoạt động tăng dần cho phép.
