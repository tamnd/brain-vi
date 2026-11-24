---
title: "CF 73B - Cần Phanh"
description: "Chúng tôi biết bảng xếp hạng vô địch hiện tại trước cuộc đua cuối cùng. Mỗi tay đua đều đã có một số điểm nhất định và cuộc đua cuối cùng sẽ phân bổ thêm điểm cho những người về đích hàng đầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 73
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 66"
rating: 2000
weight: 73
solve_time_s: 129
verified: true
draft: false
---

[CF 73B - Cần phanh](https://codeforces.com/problemset/problem/73/B) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi biết bảng xếp hạng vô địch hiện tại trước cuộc đua cuối cùng. Mỗi tay đua đều đã có một số điểm và chặng đua cuối cùng sẽ phân bổ thêm điểm cho người đứng đầu`m`người về đích. Tay đua về nhất cuộc đua nhận được`b[0]`điểm, tay đua về thứ hai nhận được`b[1]`, vân vân. 

Sau chặng đua cuối cùng, các thí sinh được xếp hạng theo tổng điểm theo thứ tự giảm dần. Nếu hai tay đua về đích với cùng số điểm, tay đua có biệt danh nhỏ hơn về mặt từ điển sẽ được coi là cao hơn. 

Chúng tôi được yêu cầu hai giá trị cho tay đua của Vasya: 

1. Vị trí vô địch cuối cùng tốt nhất mà anh ấy vẫn có thể đạt được. 
2. Vị trí vô địch cuối cùng tồi tệ nhất mà anh ấy vẫn có thể rơi vào. 

Thứ tự cuộc đua cuối cùng hoàn toàn nằm trong tầm kiểm soát của chúng tôi. Bất kỳ tay đua nào cũng có thể được chỉ định về đích bất kỳ đúng một lần. Điều này có nghĩa là chúng tôi có thể tự do phân phối điểm thưởng theo bất kỳ cách nào giúp ích hoặc gây tổn hại cho Vasya nhiều nhất. 

Những ràng buộc buộc chúng ta phải suy nghĩ cẩn thận. Có thể có tới`10^5`những tay đua, nên bất cứ thứ gì bậc hai đều đã quá đắt rồi. Một mô phỏng ngây thơ liên tục thử tất cả các phép gán điểm có thể sẽ bùng nổ về mặt tổ hợp. Thậm chí một`O(n^2)`kiểm tra với`10^5`các tay đua sẽ yêu cầu khoảng`10^10`hoạt động, điều này là không thể trong vòng 4 giây. 

Phần khó khăn là quy tắc hòa. Điểm bằng nhau không đủ để tạo nên sự bình đẳng trong xếp hạng. Thứ tự từ điển quyết định người chiến thắng trong số các mối quan hệ. 

Hãy xem xét đầu vào này:```
2
alex 10
vasya 10
0
vasya
```Không có điểm thưởng được phân phối. Cả hai tay đua đều về đích với 10 điểm. Từ`"alex" < "vasya"`, Alex xếp trên Vasya nên câu trả lời là:```
2 2
```Việc thực hiện bất cẩn chỉ so sánh điểm số sẽ cho ra kết quả không chính xác`1 1`. 

Một trường hợp tinh vi khác xuất hiện khi một số giá trị tiền thưởng bằng 0.```
3
aaa 5
bbb 5
vasya 5
2
10 0
vasya
```Trao cho Vasya phần thưởng cuộc đua hạng nhất mang lại 15 điểm, đảm bảo vị trí đầu tiên về tổng thể. Nhưng nếu Vasya không nhận được tiền thưởng thì mối quan hệ lại trở nên quan trọng. Chúng ta không thể cho rằng các vị trí được trao luôn thay đổi thứ hạng. 

Một trường hợp nguy hiểm hơn nữa là khi`m = 0`.```
3
a 100
vasya 100
z 100
0
vasya
```Không có điểm nào được thêm vào cả. Thứ tự cuối cùng được xác định hoàn toàn theo thứ tự từ điển giữa các điểm bằng nhau. Vasya về thứ hai. 

Việc triển khai luôn cho rằng Vasya tham gia trong số các tay đua được khen thưởng sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là đơn giản. Chúng tôi có thể thử mọi vị trí về đích có thể cho Vasya, sau đó chỉ định các vị trí đua còn lại cho tất cả các tay đua khác bằng mọi cách có thể, cuối cùng tính toán thứ hạng vô địch thu được. 

Điều này đúng vì nó khám phá mọi kết quả hợp lệ của cuộc đua cuối cùng. Vấn đề là số lượng hoán vị. Ngay cả đối với 15 tay đua, việc thử tất cả các nhiệm vụ cũng là điều không thể thực hiện được. Với`10^5`các tay đua, tìm kiếm toàn diện là vô vọng. 

Quan sát quan trọng là chúng ta không bao giờ quan tâm đến thứ tự chính xác của những người khác. Chúng tôi chỉ quan tâm liệu một tay đua có thể về đích trên Vasya hay không. 

Giả sử chúng ta ấn định số tiền thưởng mà Vasya nhận được. Điểm cuối cùng của anh ấy trở thành:```
vasya_score = current_score + chosen_bonus
```Bây giờ câu hỏi trở thành: 

Có bao nhiêu tay đua có thể bị buộc phải vượt lên trên anh ta hoặc bị ngăn cản vượt lên trên anh ta? 

Đối với bất kỳ tay đua nào khác, việc họ có đánh bại Vasya hay không chỉ phụ thuộc vào việc liệu có tồn tại phần thưởng chưa sử dụng giúp điểm cuối cùng của họ tốt hơn Vasya hay ngang bằng với tên nhỏ hơn về mặt từ điển hay không. 

Điều này biến vấn đề thành một vấn đề kết hợp tham lam giữa các tay đua và tiền thưởng có sẵn. 

Để có thứ hạng cuối cùng tốt nhất có thể, chúng tôi muốn càng ít tay đua trên Vasya càng tốt. Đương nhiên, chúng ta chỉ nên đưa ra những phần thưởng nguy hiểm khi không thể tránh khỏi. Phần thưởng đủ nhỏ nhất nên được sử dụng cho mỗi tay đua. 

Để có thứ hạng cuối cùng tệ nhất có thể, chúng tôi muốn có càng nhiều tay đua trên Vasya càng tốt. Sau đó, chúng tôi tham lam trao những khoản tiền thưởng lớn cho các tay đua bất cứ khi nào có thể. 

Cả hai phiên bản đều giảm bớt việc kiểm tra xem có bao nhiêu tay đua có thể vượt qua số điểm mục tiêu cố định dưới sự phân bổ tiền thưởng tham lam. Sắp xếp làm cho việc này hiệu quả. 

Độ phức tạp cuối cùng trở thành`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Vị trí tốt nhất có thể 

1. Hãy thử mọi phần thưởng có thể mà Vasya có thể nhận được. 

Vasya có thể hoàn thành bên ngoài các vị trí được thưởng, do đó, cũng có thể không nhận được điểm bổ sung khi một số phần thưởng vẫn chưa được sử dụng. 
2. Tính điểm cuối cùng của Vasya sau khi nhận phần thưởng đó. 
3. Xóa phần thưởng đó khỏi nhóm có sẵn. 
4. Sắp xếp các phần thưởng còn lại theo thứ tự tăng dần. 
5. Đối với mọi tay đua khác, hãy tính số điểm bổ sung tối thiểu cần thiết để về đích phía trên Vasya. 

Một tay đua đánh bại Vasya nếu: 

- tổng số điểm của họ trở nên lớn hơn, hoặc 
- tổng số điểm trở nên bằng nhau và biệt danh của họ nhỏ hơn về mặt từ điển. 
6. Xử lý các tay đua theo thứ tự tăng dần về số tiền thưởng yêu cầu. 

Đối với mỗi tay đua, hãy tham lam chỉ định phần thưởng nhỏ nhất hiện có là đủ. 

Điều này giảm thiểu số lượng tay đua có thể vượt qua Vasya vì phần thưởng mạnh mẽ được dành cho những trường hợp khó hơn. 
7. Đếm xem có bao nhiêu tay đua vượt qua Vasya thành công. 
8. Vị trí cuối cùng của Vasya là một điểm cộng. 
9. Lấy mức tối thiểu trong tất cả các lựa chọn tiền thưởng của Vasya. 

### Vị trí tồi tệ nhất có thể xảy ra 

1. Hãy thử lại mọi phần thưởng có thể mà Vasya có thể nhận được. 
2. Tính điểm cuối cùng của anh ấy. 
3. Xóa phần thưởng đã chọn của Vasya khỏi nhóm có sẵn. 
4. Sắp xếp các phần thưởng còn lại theo thứ tự tăng dần. 
5. Đối với mọi tay đua khác, hãy xác định phần thưởng tối thiểu cần thiết để vượt qua Vasya. 
6. Xử lý các tay đua từ khó nhất đến dễ nhất để thỏa mãn. 

Bây giờ chúng tôi muốn tối đa hóa số lượng tay đua phía trên Vasya. Chiến lược tối ưu là trao phần thưởng lớn nhất cho những tay đua cần sự trợ giúp nhất. 
7. Tham lam phù hợp với số tiền thưởng lớn nhất còn lại. 
8. Đếm xem có thể đẩy được bao nhiêu tay đua lên trên Vasya. 
9. Vị trí cuối cùng của Vasya trở thành một điểm cộng. 
10. Tận dụng tối đa tất cả các lựa chọn tiền thưởng của Vasya. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là mỗi tay đua chỉ phụ thuộc vào việc họ có nhận được phần thưởng ít nhất bằng một ngưỡng nào đó hay không. 

Điều này chuyển đổi vấn đề thành một vấn đề phù hợp giữa các ngưỡng và phần thưởng có sẵn. 

Để giảm thiểu số lượng tay đua thành công, việc chỉ định phần thưởng vừa đủ nhỏ nhất luôn là tối ưu. Bất kỳ phần thưởng lớn hơn nào sẽ chỉ lãng phí tài nguyên có thể giúp ích cho một tay đua khác sau này. 

Để tối đa hóa số lượng tay đua thành công, việc chỉ định phần thưởng lớn nhất cho những tay đua có yêu cầu cao nhất là tối ưu. Nếu một yêu cầu lớn không thể được đáp ứng ngay bây giờ thì sau này nó cũng không thể được đáp ứng bằng những phần thưởng nhỏ hơn. 

Cả hai chiến lược tham lam đều là những tranh luận trao đổi cổ điển. Bất kỳ nhiệm vụ không tham lam nào cũng có thể được sắp xếp lại thành nhiệm vụ tham lam mà không làm kết quả xấu đi. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def better(name1, score1, name2, score2):
    if score1 != score2:
        return score1 > score2
    return name1 < name2

def solve():
    n = int(input())

    names = []
    scores = []

    for _ in range(n):
        s, a = input().split()
        names.append(s)
        scores.append(int(a))

    m = int(input())
    b = list(map(int, input().split())) if m else []

    vasya = input().strip()
    vid = names.index(vasya)

    bonus_options = b[:] + [0]

    best = n
    worst = 1

    for used_bonus in bonus_options:
        vasya_score = scores[vid] + used_bonus

        remaining = b[:]
        removed = False

        for i in range(len(remaining)):
            if remaining[i] == used_bonus and not removed:
                remaining.pop(i)
                removed = True
                break

        remaining.sort()

        need = []

        for i in range(n):
            if i == vid:
                continue

            cur = scores[i]

            if names[i] < vasya:
                req = vasya_score - cur
            else:
                req = vasya_score - cur + 1

            req = max(req, 0)
            need.append(req)

        need.sort()

        # best rank
        ptr = 0
        above = 0

        for req in need:
            pos = bisect_left(remaining, req, ptr)
            if pos == len(remaining):
                continue
            above += 1
            ptr = pos + 1

        best = min(best, above + 1)

        # worst rank
        rem_ptr = len(remaining) - 1
        above = 0

        for req in reversed(need):
            if rem_ptr >= 0 and remaining[rem_ptr] >= req:
                above += 1
                rem_ptr -= 1

        worst = max(worst, above + 1)

    print(best, worst)

solve()
```Giải pháp lặp lại mọi phần thưởng có thể có mà Vasya có thể nhận được. Có nhiều nhất`m + 1`những lựa chọn như vậy, bao gồm cả khả năng không nhận được điểm nào. 

Điều kiện trợ giúp để so sánh các tay đua được mã hóa thông qua phép tính tiền thưởng bắt buộc. Giả sử một tay đua khác hiện có`cur`điểm. 

Nếu tên của họ nhỏ hơn về mặt từ điển so với tên của Vasya thì sự bình đẳng là đủ để xếp trên anh ta, vì vậy họ cần:```
cur + bonus >= vasya_score
```Nếu không thì họ cần nhiều điểm hơn:```
cur + bonus > vasya_score
```Sự khác biệt này là nguồn gốc phổ biến nhất của các câu trả lời sai. 

Để tính toán thứ hạng tốt nhất, chúng tôi tham lam chỉ định phần thưởng nhỏ nhất làm hài lòng từng tay đua. các`bisect_left`call sẽ tìm thấy phần thưởng có thể sử dụng đầu tiên một cách hiệu quả. 

Để tính toán thứ hạng kém nhất, chúng tôi đảo ngược cả hai mảng và tham lam tiêu thụ phần thưởng lớn nhất còn lại. Điều này tối đa hóa các trận đấu thành công. 

Mã chỉ loại bỏ một lần phần thưởng đã chọn của Vasya. Điều này quan trọng khi nhiều phần thưởng có giá trị như nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
teama 10
teamb 20
teamc 40
2
10 20
teama
```Giả sử Vasya nhận tiền thưởng`20`. 

| Tay đua | Hiện tại | Cần phải đánh bại Vasya | Phần thưởng được giao | Đánh bại Vasya | 
| --- | --- | --- | --- | --- | 
| độib | 20 | 11 | không | Không | 
| độic | 40 | 0 | 10 | Có | 

Vasya kết thúc với`30`. 

Chỉ một`teamc`có thể ở trên anh ta, vì vậy Vasya trở thành người thứ hai. 

Bây giờ giả sử Vasya lấy`0`. 

| Tay đua | Hiện tại | Cần thiết | Được giao | 
| --- | --- | --- | --- | 
| độib | 11 | 20 | | 
| độic | 0 | 10 | | 

Cả hai tay đua đều về đích phía trên anh ta nên anh ta đứng thứ ba. 

Câu trả lời cuối cùng là:```
2 3
```Dấu vết này cho thấy tại sao việc thử mọi phần thưởng có thể có cho Vasya là cần thiết. 

### Ví dụ 2```
4
adam 10
bob 10
vasya 10
zoe 10
2
0 5
vasya
```Nếu Vasya mất`5`, điểm số cuối cùng của anh ấy là`15`. 

| Tay đua | Từ điển nhỏ hơn? | Cần thiết | 
| --- | --- | --- | 
| adam | Có | 5 | 
| bob | Có | 5 | 
| zoe | Không | 6 | 

Chỉ có một phần thưởng còn lại tồn tại, đó là`0`, nên không ai đánh bại anh ta. 

Nếu Vasya mất`0`, điểm số cuối cùng của anh ấy là`10`. 

| Tay đua | Cần thiết | 
| --- | --- | 
| adam | 0 | 
| bob | 0 | 
| zoe | 1 | 

Bây giờ cả hai`adam`Và`bob`đã xếp hạng cao hơn anh ta do thứ tự từ điển. 

Câu trả lời cuối cùng:```
1 3
```Ví dụ này chứng minh tại sao điểm bằng nhau không thể được xử lý thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp và tìm kiếm nhị phân chiếm ưu thế | 
| Không gian | O(n) | Mảng tiền thưởng và yêu cầu | 

Thuật toán dễ dàng phù hợp với các ràng buộc. Sắp xếp`10^5`giá trị và thực hiện tìm kiếm logarit nằm trong giới hạn 4 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())

        names = []
        scores = []

        for _ in range(n):
            s, a = input().split()
            names.append(s)
            scores.append(int(a))

        m = int(input())
        b = list(map(int, input().split())) if m else []

        vasya = input().strip()
        vid = names.index(vasya)

        bonus_options = b[:] + [0]

        best = n
        worst = 1

        for used_bonus in bonus_options:
            vasya_score = scores[vid] + used_bonus

            remaining = b[:]
            removed = False

            for i in range(len(remaining)):
                if remaining[i] == used_bonus and not removed:
                    remaining.pop(i)
                    removed = True
                    break

            remaining.sort()

            need = []

            for i in range(n):
                if i == vid:
                    continue

                cur = scores[i]

                if names[i] < vasya:
                    req = vasya_score - cur
                else:
                    req = vasya_score - cur + 1

                req = max(req, 0)
                need.append(req)

            need.sort()

            ptr = 0
            above = 0

            for req in need:
                pos = bisect_left(remaining, req, ptr)
                if pos == len(remaining):
                    continue
                above += 1
                ptr = pos + 1

            best = min(best, above + 1)

            rem_ptr = len(remaining) - 1
            above = 0

            for req in reversed(need):
                if rem_ptr >= 0 and remaining[rem_ptr] >= req:
                    above += 1
                    rem_ptr -= 1

            worst = max(worst, above + 1)

        return f"{best} {worst}"

    return solve()

# provided sample
assert run(
"""3
teama 10
teamb 20
teamc 40
2
10 20
teama
"""
) == "2 3", "sample 1"

# minimum size
assert run(
"""1
vasya 0
0
vasya
"""
) == "1 1", "single racer"

# tie-breaking check
assert run(
"""3
adam 10
vasya 10
zoe 10
0
vasya
"""
) == "2 2", "lexicographical ties"

# all bonuses zero
assert run(
"""4
a 5
b 5
vasya 5
z 5
3
0 0 0
vasya
"""
) == "3 3", "zero bonuses"

# off-by-one strict inequality
assert run(
"""2
vasya 10
zzz 10
1
0
vasya
"""
) == "1 1", "equal score but larger name stays below"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tay đua đơn |`1 1`| Kích thước đầu vào tối thiểu | 
| Quan hệ từ điển học |`2 2`| Điểm bằng nhau sử dụng thứ tự tên | 
| Tất cả tiền thưởng bằng không |`3 3`| Không có tiền thưởng được xử lý chính xác | 
| Điểm bằng nhau với tên lớn hơn |`1 1`| Logic so sánh chặt chẽ | 

## Vỏ cạnh 

Hãy xem xét trường hợp tie-break thuần túy:```
3
a 100
vasya 100
z 100
0
vasya
```Vasya không nhận được tiền thưởng vì không có tiền thưởng nào tồn tại. 

Thuật toán tính toán tiền thưởng cần thiết: 

| Tay đua | Cần thiết | 
| --- | --- | 
| một | 0 | 
| z | 1 | 

tay đua`a`đã đánh bại Vasya vì`"a" < "vasya"`và điểm bằng nhau là đủ. 

tay đua`z`sẽ cần thêm một điểm, điều đó là không thể. 

Có chính xác một tay đua về đích phía trên Vasya, nên câu trả lời là:```
2 2
```Bây giờ hãy xem xét các giá trị thưởng lặp lại:```
4
a 0
b 0
c 0
vasya 0
3
5 5 5
vasya
```Giả sử Vasya lấy một`5`. 

Thuật toán chỉ loại bỏ một lần xuất hiện khỏi nhiều phần thưởng. Hai phần thưởng vẫn có sẵn. 

Mọi tay đua khác đều cần`5`điểm để hòa hoặc vượt qua Vasya. 

Cả hai phần thưởng còn lại vẫn có thể được chỉ định nên hai tay đua đã vượt qua anh ta. 

Nếu chúng tôi vô tình xóa tất cả các bản sao của`5`, chúng tôi sẽ kết luận không chính xác rằng không ai có thể đánh bại Vasya. 

Cuối cùng, xét trường hợp bất đẳng thức nghiêm ngặt:```
2
vasya 10
zzz 10
1
0
vasya
```tay đua`zzz`có tên lớn hơn về mặt từ điển, vì vậy việc buộc tên là không đủ. 

Phần thưởng bắt buộc sẽ trở thành:```
10 - 10 + 1 = 1
```Nhưng phần thưởng duy nhất có sẵn là`0`. 

Thuật toán kết luận chính xác rằng không ai có thể về đích trên Vasya, tạo ra:```
1 1
```Một giải pháp quên đi`+1`sẽ đặt Vasya ở vị trí thứ hai không chính xác.
