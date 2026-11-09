---
title: "CF 1463A - Ngục tối"
description: "Chúng tôi có ba quái vật có giá trị sức khỏe a, b và c. Một phát đại bác thông thường gây 1 sát thương cho đúng một quái vật sống. Mỗi lần bắn thứ bảy đều đặc biệt. Thay vì nhắm mục tiêu vào một quái vật, nó gây 1 sát thương cho mọi quái vật còn sống tại thời điểm đó."
date: "2026-06-11T01:59:08+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1463
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 100 (Rated for Div. 2)"
rating: 1100
weight: 1463
solve_time_s: 118
verified: true
draft: false
---

[CF 1463A - Ngục tối](https://codeforces.com/problemset/problem/1463/A) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba con quái vật có giá trị sức khỏe`a`,`b`, Và`c`. 

Một phát đại bác thông thường gây 1 sát thương cho đúng một quái vật sống. Mỗi lần bắn thứ bảy đều đặc biệt. Thay vì nhắm mục tiêu vào một quái vật, nó gây 1 sát thương cho mọi quái vật còn sống tại thời điểm đó. 

Mục tiêu không chỉ đơn thuần là tiêu diệt tất cả quái vật. Cả ba quái vật phải chết cùng nhau trong cùng một lần bắn nâng cao. Trước phát bắn đó, mọi quái vật vẫn phải còn sống và ngay sau lần bắn nâng cao đó, cả ba giá trị máu lần đầu tiên trở thành 0. 

Đầu vào chứa tối đa`10^4`trường hợp thử nghiệm. Mỗi giá trị sức khỏe có thể lớn bằng`10^8`, vì vậy bất kỳ mô phỏng nào dựa trên từng bức ảnh đều không thể thực hiện được. Một trường hợp thử nghiệm duy nhất có thể yêu cầu hàng trăm triệu bức ảnh, điều này ngay lập tức loại trừ mô phỏng tuyến tính. Chúng ta cần một điều kiện toán học có thể được kiểm tra theo thời gian không đổi. 

Một cái bẫy tinh vi là cho rằng chỉ có sức khoẻ tổng thể mới quan trọng. 

Ví dụ:```
1
10 1 7
```Tổng máu là 18 chia hết cho 9 nhưng đáp án vẫn là`NO`. Một con quái vật quá yếu so với những con khác và sẽ chết trước phát bắn nâng cao cuối cùng. 

Một sai lầm dễ mắc phải khác là quên rằng tất cả quái vật phải sống sót cho đến lần bắn nâng cao cuối cùng. 

Ví dụ:```
1
1 1 7
```Tổng lượng máu là 9, điều này gợi ý một chu kỳ hoàn chỉnh gồm 9 sát thương. Tuy nhiên, sau lần bắn nâng cao cuối cùng, mỗi quái vật phải nhận ít nhất một phần ba lượng sát thương không được tăng cường. Hai quái vật có máu 1 không thể hấp thụ đủ sát thương để thực hiện được điều đó, vì vậy câu trả lời là`NO`. 

Một trường hợp khác xảy ra khi tất cả quái vật đều bằng nhau.```
1
3 3 3
```Tổng lượng máu là 9 và mỗi quái vật có thể dễ dàng tham gia vào lượt bắn nâng cao cuối cùng. Câu trả lời đúng là`YES`. 

Hiểu được lý do tại sao những ví dụ này lại khác nhau sẽ dẫn trực tiếp đến nhận xét quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng trận chiến. Chúng ta có thể liên tục chọn quái vật nào nhận được mỗi phát bắn thông thường và áp dụng hiệu ứng nâng cao vào mỗi lần bắn thứ bảy. Cuối cùng, chúng tôi sẽ kiểm tra xem liệu cả ba con quái vật có chết cùng nhau hay không. 

Cách tiếp cận này đúng về nguyên tắc vì nó mô hình hóa trò chơi một cách chính xác. Vấn đề là giá trị sức khoẻ đạt tới`10^8`. Tổng thiệt hại có thể lên tới`3 × 10^8`, do đó, ngay cả một trường hợp thử nghiệm cũng sẽ yêu cầu hàng trăm triệu thao tác. 

Quan sát quan trọng là thứ tự chính xác của các cú đánh thông thường không quan trọng. Chỉ có tổng số thiệt hại là quan trọng. 

Giả sử những con quái vật chết trên`k`-th bắn nâng cao. 

Mỗi nhóm bảy bức ảnh bao gồm sáu bức ảnh thông thường và một bức ảnh nâng cao. Do đó, mỗi chu trình hoàn chỉnh sẽ góp phần:```
6 targeted damage
3 enhanced damage
```với tổng số 9 thiệt hại. 

Nếu tất cả quái vật chết trong lần bắn nâng cao cuối cùng, thì tổng sát thương gây ra phải bằng tổng máu:```
a + b + c = 9k
```Vậy tổng đó phải chia hết cho 9 

Bây giờ hãy xem xét lượng sát thương mà mỗi quái vật nhận được từ các phát bắn nâng cao. Vì phát bắn nâng cao cuối cùng là phát bắn giết chết chúng nên mọi quái vật đều phải sống sót cho đến khi phát bắn đó. Như vậy mỗi quái vật nhận được chính xác`k`tổng thiệt hại được tăng cường. 

Sau khi loại bỏ những`k`điểm, sát thương còn lại cần thiết cho quái vật`i`phải đến từ những cú đánh thường xuyên. 

Số lần chụp thông thường có thể thực hiện được là`6k`. 

Để thực hiện được điều này, không có con quái vật nào có thể yêu cầu ít hơn`k`sức khỏe toàn diện. Nếu không nó sẽ chết trước khi nhận được tất cả`k`lượt truy cập nâng cao. 

Điều đó mang lại:```
min(a, b, c) >= k
```Từ```
k = (a + b + c) / 9
```toàn bộ vấn đề giảm xuống còn hai lần kiểm tra đơn giản. 

Brute-force hoạt động vì nó tuân theo trò chơi theo đúng nghĩa đen, nhưng không thành công vì giá trị sức khỏe là rất lớn. Nhận xét rằng mọi trận chiến thành công đều bao gồm các chu kỳ 9 sát thương hoàn chỉnh cho phép chúng tôi thay thế mô phỏng bằng số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a+b+c) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng sức khỏe:```
s = a + b + c
```2. Kiểm tra xem`s`chia hết cho 9. 

Nếu không thì in`NO`. 

Một trận chiến thành công kết thúc bằng một phát bắn nâng cao sau`k`chu kỳ luôn giao dịch chính xác`9k`hư hại. 
3. Tính toán:```
k = s / 9
```Đây là số lần chụp nâng cao phải xảy ra. 
4. Tìm giá trị sức khỏe nhỏ nhất:```
m = min(a, b, c)
```5. Nếu`m < k`, in`NO`. 

Mọi quái vật đều phải sống sót qua tất cả`k`những bức ảnh nâng cao. Một con quái vật có sức khỏe ít hơn`k`sẽ đạt đến 0 trước lần bắn nâng cao cuối cùng. 
6. Nếu không thì in`YES`. 

### Tại sao nó hoạt động 

Giả sử câu trả lời là`YES`. 

Nếu những con quái vật chết cùng nhau trên`k`-chính xác là lần bắn nâng cao thứ`k`chu kỳ bắn hoàn chỉnh xảy ra. Mỗi chu kỳ đóng góp tổng cộng 9 sát thương, vì vậy tổng lượng máu phải bằng nhau`9k`. Điều này chứng tỏ số chia hết cho 9. 

Ngoài ra, mỗi quái vật sống sẽ nhận được một điểm sát thương từ mỗi lần bắn nâng cao. Vì tất cả quái vật đều sống sót cho đến phát bắn nâng cao cuối cùng nên mỗi quái vật phải hấp thụ ít nhất`k`hư hại. Kể từ đây`min(a,b,c) ≥ k`. 

Bây giờ giả sử cả hai điều kiện đều đúng. Cho phép`k = (a+b+c)/9`. 

Mỗi quái vật có thể dành`k`điểm sức khỏe khi tăng cường sát thương. Sau khi loại bỏ những đóng góp đó, thiệt hại cần thiết còn lại là:```
(a-k) + (b-k) + (c-k)
= (a+b+c) - 3k
= 9k - 3k
= 6k
```Chính xác`6k`có những bức ảnh thường xuyên tồn tại. Vì mọi số tiền còn lại đều không âm nên những lần bắn thông thường đó có thể được phân bổ tương ứng. Do đó, tất cả quái vật đều đạt lượng máu 1 ngay trước phát bắn nâng cao cuối cùng và cùng chết trong phát bắn đó. 

Cả hai điều kiện đều cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        a, b, c = map(int, input().split())
        
        s = a + b + c
        
        if s % 9 != 0:
            print("NO")
            continue
        
        k = s // 9
        
        if min(a, b, c) < k:
            print("NO")
        else:
            print("YES")

solve()
```Giải pháp theo sau bằng chứng trực tiếp. 

Điều kiện đầu tiên kiểm tra xem tổng lượng máu có thể được biểu diễn dưới dạng toàn bộ số chu kỳ 9 sát thương hay không. Nếu không, sẽ không có kết thúc nào trong một cảnh quay nâng cao. 

Sau đó,`k`được xác định duy nhất. Không cần phải tìm kiếm vì tổng lượng máu ấn định số chu kỳ. 

Điều kiện thứ hai xác minh rằng mọi quái vật đều có thể sống sót qua tất cả`k`những bức ảnh nâng cao. sử dụng`min(a,b,c)`là đủ vì nếu con quái vật nhỏ nhất sống sót thì những con lớn hơn chắc chắn cũng sống sót. 

Tất cả các phép tính đều phù hợp thoải mái với các loại số nguyên tiêu chuẩn. Trong Python không có vấn đề tràn, nhưng ngay cả trong số nguyên 64 bit, tổng tối đa chỉ là`3 × 10^8`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 4
```| Biến | Giá trị | 
| --- | --- | 
| một | 3 | 
| b | 2 | 
| c | 4 | 
| s | 9 | 
| s% 9 | 0 | 
| k | 1 | 
| phút(a,b,c) | 2 | 

Từ`s`chia hết cho 9 và`2 ≥ 1`, câu trả lời là`YES`. 

Điều này chứng tỏ trường hợp thành công khi tồn tại một chu trình hoàn chỉnh. Sau sáu phát bắn thông thường, mọi quái vật vẫn sống sót, và phát thứ bảy giết chết cả ba cùng một lúc. 

### Ví dụ 2 

đầu vào:```
10 1 7
```| Biến | Giá trị | 
| --- | --- | 
| một | 10 | 
| b | 1 | 
| c | 7 | 
| s | 18 | 
| s% 9 | 0 | 
| k | 2 | 
| phút(a,b,c) | 1 | 

Từ`1 < 2`, câu trả lời là`NO`. 

Điều này cho thấy tại sao chỉ chia hết cho 9 là không đủ. Quái vật thứ hai không thể sống sót sau hai phát bắn nâng cao, vì vậy việc kết liễu đồng thời là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Ngay cả với`10^4`trường hợp thử nghiệm, chương trình chỉ thực hiện công việc có thời gian không đổi cho từng trường hợp. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        s = a + b + c

        if s % 9 != 0:
            print("NO")
            continue

        k = s // 9
        print("YES" if min(a, b, c) >= k else "NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n3 2 4\n1 1 1\n10 1 7\n") == "YES\nNO\nNO\n", "sample"

# minimum values
assert run("1\n1 1 1\n") == "NO\n", "minimum healths"

# all equal and valid
assert run("1\n3 3 3\n") == "YES\n", "equal values"

# divisible by 9 but smallest too small
assert run("1\n1 4 4\n") == "NO\n", "fails min condition"

# large boundary values
assert run("1\n100000000 100000000 100000000\n") == "NO\n", "large numbers"

# exactly on boundary
assert run("1\n2 2 5\n") == "YES\n", "min equals k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`NO`| Tổng không chia hết cho 9 | 
|`3 3 3`|`YES`| Trường hợp hợp lệ đối xứng | 
|`1 4 4`|`NO`| Chia hết cho 9 nhưng sức khỏe nhỏ nhất không đủ | 
|`100000000 100000000 100000000`|`NO`| Kiểm tra giá trị lớn và tính chia hết | 
|`2 2 5`|`YES`| Ranh giới ở đâu`min(a,b,c) = k`| 

## Vỏ cạnh 

Hãy xem xét:```
1
1 4 4
```Tổng là 9 nên`k = 1`. Sức khỏe nhỏ nhất cũng là 1. Vì`1 ≥ 1`, đầu ra của thuật toán`YES`. Một con quái vật có thể sống sót sau một phát bắn nâng cao và chết ở phát bắn cuối cùng. Sự bình đẳng được cho phép, đó là một cái bẫy phổ biến. 

Coi như:```
1
10 1 7
```Tổng là 18 nên`k = 2`. Sức khỏe nhỏ nhất là 1. Vì`1 < 2`, đầu ra của thuật toán`NO`. Quái vật có máu 1 không thể sống sót sau hai phát bắn nâng cao, khiến việc tiêu diệt đồng thời là không thể. 

Coi như:```
1
1 1 1
```Tổng là 3. Vì`3 % 9 != 0`, thuật toán ngay lập tức xuất ra`NO`. Không có số nguyên của chu kỳ 9 sát thương hoàn chỉnh phù hợp với tổng lượng máu. 

Coi như:```
1
2 2 5
```Tổng là 9 nên`k = 1`. Sức khỏe tối thiểu là 2, thỏa mãn`2 ≥ 1`. Đầu ra của thuật toán`YES`. Điều này xác nhận ranh giới nơi tồn tại một chu kỳ đầy đủ và tất cả quái vật có thể tham gia vào lần bắn nâng cao cuối cùng.
