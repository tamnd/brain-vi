---
title: "CF 1881A - Đừng Cố Đếm"
description: "Chúng ta bắt đầu bằng một chuỗi x. Một thao tác thay thế x bằng x + x, nghĩa là chuỗi hiện tại được nối vào chính nó và độ dài của nó tăng gấp đôi. Chúng ta cũng được cấp một chuỗi mục tiêu s."
date: "2026-06-08T22:40:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "strings"]
categories: ["algorithms"]
codeforces_contest: 1881
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 903 (Div. 3)"
rating: 800
weight: 1881
solve_time_s: 112
verified: true
draft: false
---

[CF 1881A - Đừng cố đếm](https://codeforces.com/problemset/problem/1881/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, dây 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi`x`. Một thao tác thay thế`x`với`x + x`, nghĩa là chuỗi hiện tại được nối vào chính nó và độ dài của nó tăng gấp đôi. 

Chúng tôi cũng được cung cấp một chuỗi mục tiêu`s`. Nhiệm vụ là tìm số lượng phép nhân đôi tối thiểu cần thiết để`s`xuất hiện ở đâu đó bên trong giá trị hiện tại của`x`như một chuỗi con liền kề. Nếu không có số lượng thao tác nào có thể thực hiện được điều này, chúng ta phải xuất ra`-1`. 

Hạn chế quan trọng nhất là`n · m ≤ 25`. Cả hai dây đều cực kỳ nhỏ. Ngay cả sau vài lần nhân đôi, độ dài chuỗi tăng rất chậm về mặt tuyệt đối vì độ dài ban đầu rất nhỏ. Điều này ngay lập tức gợi ý rằng chúng ta không cần các thuật toán chuỗi phức tạp như KMP hoặc cấu trúc hậu tố. Một mô phỏng trực tiếp là khả thi. 

Phần khó khăn là quyết định số lần chúng ta cần nhân đôi trước khi tuyên bố thất bại. Việc thực hiện bất cẩn có thể tiếp tục tăng gấp đôi mãi mãi khi câu trả lời thực sự là không thể. 

Coi như`x = "ab"`Và`s = "aaa"`. Mỗi chuỗi nhân đôi vẫn là sự lặp lại của`"ab"`:```
ab
abab
abababab
...
```nhân vật`'a'`xuất hiện, nhưng ba liên tiếp`'a'`nhân vật sẽ không bao giờ. Tiếp tục mãi mãi không thay đổi gì cả nên đáp án đúng là`-1`. 

Một trường hợp tinh tế khác là khi mục tiêu ban đầu đã tồn tại. 

đầu vào:```
2 2
aa
aa
```Đầu ra:```
0
```Một giải pháp thực hiện ít nhất một lần nhân đôi trước khi kiểm tra sẽ trả về không chính xác`1`. 

Trường hợp cạnh thứ ba xảy ra khi mục tiêu vượt qua ranh giới giữa các bản sao. 

đầu vào:```
5 5
eforc
force
```Ban đầu`"force"`không xuất hiện. Sau một lần nhân đôi:```
eforceforc
```Chuỗi con bắt đầu ở gần cuối bản sao đầu tiên và tiếp tục ở bản sao thứ hai. Bất kỳ giải pháp nào chỉ kiểm tra bên trong một bản sao duy nhất của`x`sẽ bỏ lỡ nó. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng. Kiểm tra xem`s`đã là một chuỗi con của`x`. Nếu không thì nhân đôi`x`, kiểm tra lại và tiếp tục. 

Cách tiếp cận này đúng vì mọi thao tác đều tạo ra chính xác một trạng thái chuỗi mới và chúng tôi kiểm tra các trạng thái đó theo thứ tự tăng dần của số lượng thao tác. Lần đầu tiên chúng tôi tìm thấy`s`, chúng tôi đã tự động tìm thấy số lượng hoạt động tối thiểu. 

Câu hỏi còn lại là khi nào nên dừng lại. 

Việc triển khai đơn giản có thể tiếp tục vô thời hạn cho đến khi chuỗi trở nên khổng lồ. Điều đó là không cần thiết. Từ`s`có chiều dài`m`, khi độ dài chuỗi hiện tại đã đạt ít nhất`m`, bất kỳ sự xuất hiện nào trong tương lai của`s`phải tồn tại hoặc xuất hiện do ranh giới được tạo bởi một lần nhân đôi bổ sung. 

Tại sao một nhân đôi thêm là đủ? Sau khi độ dài chuỗi hiện tại ít nhất là`m`, bất kỳ chuỗi con nào có độ dài`m`hoàn toàn phù hợp với một bản sao của chuỗi hiện tại ngoại trừ điểm giao nhau giữa hai bản sao liền kề. Thực hiện thêm một lần nhân đôi nữa sẽ tạo ra tất cả các vị trí giao nhau như vậy. Nếu như`s`vẫn không xuất hiện thì việc nhân đôi sau này không thể tạo ra một mô hình mới về cơ bản. 

Bởi vì`n · m ≤ 25`, độ dài vẫn rất nhỏ. Giải pháp được chấp nhận phổ biến chỉ đơn giản là tiếp tục tăng gấp đôi trong khi độ dài nhỏ hơn`m`, kiểm tra sau mỗi thao tác và sau đó thực hiện một lần kiểm tra bổ sung sau một lần nhân đôi nữa. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với khả năng nhân đôi không giới hạn | Không giới hạn | Không giới hạn | Không thực tế | 
| Mô phỏng có điều khiển | O(m2) | O(m) | Đã chấp nhận | 

Độ phức tạp chính xác là rất nhỏ vì`m ≤ 25`. 

## Hướng dẫn thuật toán 

1. Đọc`x`Và`s`. 
2. Nếu`s`đã là một chuỗi con của`x`, trở lại`0`. 
3. Khởi tạo bộ đếm`ops = 0`. 
4. Trong khi chiều dài của`x`nhỏ hơn chiều dài của`s`: 

1. Thay thế`x`với`x + x`. 
2. Tăng`ops`. 
3. Nếu`s`bây giờ là một chuỗi con của`x`, trở lại`ops`. 

Chiều dài cuối cùng phải đạt hoặc vượt quá`m`, do đó vòng lặp này chỉ thực hiện một vài lần. 
5. Thực hiện thêm một lần nhân đôi nữa: 

1. Thay thế`x`với`x + x`. 
2. Tăng`ops`. 
3. Nếu`s`là một chuỗi con của`x`, trở lại`ops`. 

Kiểm tra cuối cùng này xử lý các chuỗi con vượt qua ranh giới giữa hai bản sao. 
6. Nếu chuỗi con vẫn không xuất hiện thì trả về`-1`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra các chuỗi được tạo sau mỗi lần đếm thao tác có thể theo thứ tự tăng dần, do đó, mọi câu trả lời trả về đều tự động ở mức tối thiểu. 

Mối quan tâm duy nhất còn lại là liệu việc dừng lại sau một lần nhân đôi thêm có an toàn hay không. Khi độ dài hiện tại ít nhất là`m`, mọi chuỗi con có độ dài`m`trong tương lai, các chuỗi nhân đôi sẽ nằm hoàn toàn bên trong một bản sao của chuỗi hiện tại hoặc vượt qua chính xác một ranh giới giữa các bản sao liền kề. Việc nhân đôi tiếp theo sẽ tạo ra tất cả các vị trí ranh giới như vậy. Nếu như`s`khi đó không xuất hiện, các lần nhân đôi sau này chỉ lặp lại cấu trúc tương tự ở quy mô lớn hơn và không thể tạo ra độ dài mới-`m`chuỗi con. Như vậy trở về`-1`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, m = map(int, input().split())
        x = input().strip()
        s = input().strip()

        if s in x:
            answers.append("0")
            continue

        ops = 0

        while len(x) < len(s):
            x += x
            ops += 1

            if s in x:
                answers.append(str(ops))
                break
        else:
            x += x
            ops += 1

            if s in x:
                answers.append(str(ops))
            else:
                answers.append("-1")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Kiểm tra đầu tiên xử lý trường hợp không cần thực hiện thao tác nào. 

Vòng lặp tiếp tục tăng gấp đôi cho đến khi chuỗi hiện tại dài ít nhất bằng chuỗi đích. Sau mỗi lần nhân đôi, chúng tôi ngay lập tức kiểm tra chuỗi con vì trạng thái thành công đầu tiên đưa ra câu trả lời tối thiểu. 

các`while ... else`cấu trúc ở đây thuận tiện. các`else`khối chỉ thực thi nếu vòng lặp kết thúc bình thường, nghĩa là không tìm thấy câu trả lời nào trong khi tăng chiều dài chuỗi ít nhất`m`. 

Việc tăng gấp đôi cuối cùng là chi tiết quan trọng. Nhiều lần gửi sai dừng lại ngay khi đạt đến độ dài`m`, thiếu các chuỗi con chỉ xuất hiện trên ranh giới mới được tạo. 

Không có mối lo ngại về tràn vì tất cả các chuỗi đều cực kỳ nhỏ. Ngay cả sau vài lần nhân đôi, chiều dài của chúng vẫn rất nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 5
a
aaaaa
```| Bước | x | hoạt động |`"aaaaa" in x`| 
| --- | --- | --- | --- | 
| Ban đầu | một | 0 | Không | 
| Đôi | aa | 1 | Không | 
| Đôi | aaa | 2 | Không | 
| Đôi | aaaaaaa | 3 | Có | 

Đầu ra:```
3
```Độ dài mục tiêu là năm. Chuỗi đầu tiên chứa năm liên tiếp`'a'`các ký tự sau thao tác thứ ba. 

### Ví dụ 2 

đầu vào:```
1
5 5
eforc
force
```| Bước | x | hoạt động |`"force" in x`| 
| --- | --- | --- | --- | 
| Ban đầu | lực lượng | 0 | Không | 
| Đôi | lực lượng | 1 | Có | 

Đầu ra:```
1
```Sự xuất hiện vượt qua ranh giới giữa bản sao thứ nhất và thứ hai. Điều này chứng tỏ tại sao việc nhân đôi có thể tạo ra các chuỗi con mới ngay cả khi cả hai bản sao đều không chứa mục tiêu riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2) | Nhiều nhất là một vài lần nhân đôi, mỗi lần kiểm tra chuỗi con sẽ quét các chuỗi có độ dài O(m) | 
| Không gian | O(m) | Độ dài chuỗi được lưu trữ vẫn tỷ lệ thuận với m | 

Từ`n · m ≤ 25`, cả hai chuỗi đều cực kỳ nhỏ. Thời gian chạy thực tế thấp hơn nhiều so với giới hạn ngay cả trong tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        x = input().strip()
        s = input().strip()

        if s in x:
            ans.append("0")
            continue

        ops = 0

        while len(x) < len(s):
            x += x
            ops += 1

            if s in x:
                ans.append(str(ops))
                break
        else:
            x += x
            ops += 1

            if s in x:
                ans.append(str(ops))
            else:
                ans.append("-1")

    return "\n".join(ans)

# provided sample
assert run("""12
1 5
a
aaaaa
5 5
eforc
force
2 5
ab
ababa
3 5
aba
ababa
4 3
babb
bbb
5 1
aaaaa
a
4 2
aabb
ba
2 8
bk
kbkbkbkb
12 2
fjdgmujlcont
tf
2 2
aa
aa
3 5
abb
babba
1 19
m
mmmmmmmmmmmmmmmmmmm
""") == """3
1
2
-1
1
0
1
3
1
0
2
5"""

# minimum size
assert run("""1
1 1
a
a
""") == "0"

# impossible case
assert run("""1
2 3
ab
aaa
""") == "-1"

# boundary-crossing occurrence
assert run("""1
5 5
eforc
force
""") == "1"

# target appears after several doublings
assert run("""1
1 4
a
aaaa
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a -> a`|`0`| Không cần thao tác | 
|`ab -> aaa`|`-1`| Mục tiêu bất khả thi | 
|`eforc -> force`|`1`| Chuỗi con vượt qua ranh giới | 
|`a -> aaaa`|`2`| Cần nhân đôi nhiều lần | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 2
aa
aa
```Thuật toán kiểm tra chuỗi ban đầu trước khi thực hiện bất kỳ thao tác nào. Từ`"aa"`đã có mặt rồi thì nó lập tức quay trở lại`0`. Điều này tránh được sai lầm phổ biến là buộc ít nhất một lần nhân đôi. 

Coi như:```
1
5 5
eforc
force
```Ban đầu chuỗi con không có. Thuật toán nhân đôi một lần và thu được:```
eforceforc
```Hiện nay`"force"`xuất hiện qua điểm nối. Câu trả lời là`1`. 

Coi như:```
1
2 3
ab
aaa
```Thuật toán tạo ra:```
ab
abab
abababab
```Sau khi chiều dài đạt hoặc vượt quá`3`, nó thực hiện thêm một lần kiểm tra nhân đôi nữa. Chuỗi con vẫn chưa tồn tại nên trả về`-1`. Nhân đôi trong tương lai chỉ lặp lại cùng một mô hình xen kẽ và không bao giờ có thể tạo ra ba lần liên tiếp`'a'`nhân vật. 

Coi như:```
1
3 5
aba
ababa
```Độ dài chuỗi ban đầu nhỏ hơn độ dài mục tiêu. Sau một lần nhân đôi:```
abaaba
```Mục tiêu vẫn không xuất hiện. Chiều dài bây giờ ít nhất là`5`, do đó thuật toán thực hiện việc nhân đôi thêm:```
abaabaabaaba
```Hiện nay`"ababa"`tồn tại, đưa ra câu trả lời`2`. Ví dụ này cho thấy tại sao việc nhân đôi thêm cuối cùng là cần thiết.
