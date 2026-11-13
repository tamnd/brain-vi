---
title: "CF 176B - Cắt chữ"
description: "Mỗi thao tác chọn một tiền tố không trống và hậu tố không trống của từ hiện tại và hoán đổi thứ tự của chúng. Nếu từ hiện tại được viết là xy, trong đó cả x và y đều không trống thì thao tác sẽ chuyển nó thành yx. Một cách hữu ích để xem hoạt động này là xoay vòng theo chu kỳ."
date: "2026-06-02T17:10:11+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 176
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2012 - Round 2"
rating: 1700
weight: 176
solve_time_s: 128
verified: true
draft: false
---

[CF 176B - Cắt chữ](https://codeforces.com/problemset/problem/176/B) 

**Đánh giá:** 1700 
**Thẻ:** dp 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thao tác chọn một tiền tố không trống và hậu tố không trống của từ hiện tại và hoán đổi thứ tự của chúng. Nếu từ hiện tại được viết là`xy`, trong đó cả hai`x`Và`y`không trống, thao tác sẽ biến nó thành`yx`. 

Một cách hữu ích để xem hoạt động này là xoay vòng theo chu kỳ. Nếu độ dài từ là`n`và chúng tôi chia nhau theo vị trí`p`, hậu tố của độ dài`n-p`di chuyển về phía trước. Mỗi phép chia tương ứng với chính xác một phép quay không tầm thường và mọi phép quay không tầm thường tương ứng với chính xác một phép chia. 

Những lời nói`start`Và`end`có chiều dài bằng nhau`n`, Ở đâu`2 ≤ n ≤ 1000`. Chúng ta phải đếm chính xác có bao nhiêu chuỗi`k`chuyển đổi hoạt động phân chia`start`vào trong`end`. Các vị trí phân chia khác nhau tạo ra các hoạt động khác nhau, vì vậy hai chuỗi được coi là khác nhau nếu chúng chọn các điểm phân chia khác nhau ở một bước nào đó. 

Độ dài của từ nhiều nhất là 1000, nhưng`k`có thể đạt được`10^5`. Bất kỳ thuật toán nào theo dõi rõ ràng tất cả các chuỗi hoạt động có thể xảy ra đều là vô vọng. Ngay cả việc duy trì trạng thái DP cho mỗi vòng quay và mỗi bước cũng sẽ cần khoảng`1000 × 100000 = 10^8`chuyển đổi, đó là quá nhiều. 

Quan sát quan trọng là mọi thao tác đều bảo toàn thứ tự tuần hoàn của các ký tự. Bắt đầu từ một từ, chúng ta chỉ có thể đạt được các phép quay của nó. Nếu như`end`không phải là sự quay của`start`, câu trả lời ngay lập tức là số không. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
ab
ab
0
```Câu trả lời là`1`. Chúng tôi đã bắt đầu ở mục tiêu và không thực hiện hoạt động nào. Một giải pháp giả định có ít nhất một thao tác tồn tại sẽ trả về không chính xác`0`. 

Coi như:```
aaaa
aaaa
1
```Mỗi lần phân chia đều tạo ra cùng một chuỗi hiển thị`"aaaa"`, nhưng các thao tác vẫn khác nhau vì chúng sử dụng các vị trí phân chia khác nhau. có`3`trình tự một bước hợp lệ. Xử lý các trạng thái dưới dạng chuỗi mà không tính các phần tách riêng biệt sẽ bị tính thiếu. 

Coi như:```
abcd
bcda
1
```Có chính xác một phép chia hợp lệ. Xoay trái một vị trí tương ứng với việc tách sau ký tự đầu tiên. Giải pháp tính số lần xuất hiện của vòng quay mục tiêu thay vì đường dẫn vận hành thực tế có thể bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Ở mỗi bước, hãy thử tất cả`n-1`các vị trí có thể phân chia và tạo ra từ kết quả một cách đệ quy. Sau chính xác`k`hoạt động, kiểm tra xem từ cuối cùng có bằng không`end`. 

Điều này đúng vì nó khám phá mọi chuỗi hoạt động hợp pháp. Thật không may, yếu tố phân nhánh là`n-1`, vì vậy số lượng trạng thái được khám phá là khoảng`(n-1)^k`. Ngay cả đối với các giá trị nhỏ của`k`, điều này trở nên to lớn. 

Ý tưởng tiếp theo là quan sát xem thao tác phân tách thực sự làm gì. Tách sau vị trí`p`xoay chuỗi bằng cách`n-p`các vị trí. Việc phân tách lặp đi lặp lại không bao giờ thay đổi thứ tự tuần hoàn của các ký tự. Mỗi từ có thể truy cập chỉ đơn giản là một vòng quay của`start`. 

Giả sử chiều dài là`n`. Có nhiều nhất`n`trạng thái quay khác nhau. Chúng ta có thể xây dựng một đồ thị có các đỉnh là phép quay và các cạnh tương ứng với việc chọn một trong các`n-1`những phép quay không tầm thường. Từ bất kỳ vòng quay nào, mọi vòng quay khác đều có thể truy cập được trong chính xác một lần di chuyển, trong khi việc duy trì cùng một vòng quay chỉ có thể thực hiện được thông qua các vòng quay xảy ra để tạo ra cùng một chuỗi. 

Sự đơn giản hóa chính xuất phát từ việc nhóm các phép quay thành hai loại: 

Danh mục đầu tiên chứa các phép quay bằng`end`. 

Loại thứ hai chứa các phép quay không bằng`end`. 

Cho phép`cnt`là số vòng quay của`start`bằng nhau`end`. 

Khi đó, mỗi phép quay thuộc về một trong hai lớp và các chuyển đổi chỉ phụ thuộc vào lớp đó chứ không phụ thuộc vào phép quay cụ thể. Điều này biến vấn đề thành DP hai trạng thái có thể được xử lý trong tối đa`10^5`các bước. 

Để sử dụng DP này, chúng ta phải biết liệu việc xoay vòng có`0`của`start`đã bằng rồi`end`, và tổng số phép quay bằng bao nhiêu`end`. Cả hai đại lượng có thể thu được bằng cách tìm kiếm sự xuất hiện của`end`bên trong`(start + start)[:-1]`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-1)^k) | O(k) | Quá chậm | 
| Tối ưu | O(n + k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Đếm số lần quay của mục tiêu 

hãy để```
T = start + start
```Mỗi vòng quay của`start`tương ứng với vị trí bắt đầu trong`T`. 

Tìm kiếm tất cả các lần xuất hiện của`end`TRONG`T[:-1]`. Số lần xuất hiện chính xác là số lần quay của`start`bằng`end`. 

Gọi giá trị này`cnt`. 

Nếu như`cnt = 0`, sau đó`end`không phải là sự quay của`start`, và câu trả lời là`0`. 

### 2. Xác định trạng thái ban đầu 

hãy để```
good0 = 1 if start == end else 0
```Chúng tôi xác định hai trạng thái DP.`good[i]`là số cách để quay một vòng bằng`end`sau đó`i`hoạt động.`bad[i]`là số cách để quay một vòng không bằng`end`sau đó`i`hoạt động. 

Ban đầu:```
good = good0
bad = 1 - good0
```Có đúng một vòng quay bắt đầu. 

### 3. Tính số lần chuyển tiếp 

Giả sử chúng ta hiện đang trong một vòng quay tốt. 

có`n-1`có thể chia vị trí. 

Trong số tất cả các phép quay bằng`end`, một trong số đó là chính vòng quay hiện tại. Vì mọi hoạt động đều phải không hề tầm thường nên chúng ta không thể ở yên tại chỗ. 

Vậy số lần chuyển từ trạng thái tốt này sang trạng thái tốt khác là:```
cnt - 1
```Số lần chuyển từ trạng thái tốt sang trạng thái xấu là:```
n - cnt
```Bây giờ giả sử chúng ta đang ở trong một vòng quay tồi. 

Trong số`cnt`vòng quay tốt, tất cả đều có thể truy cập được trong một lần di chuyển. 

Đối với các đích xấu, một vòng quay chính là trạng thái hiện tại và không thể chọn được. Như vậy:```
good transitions = cnt
bad transitions = n - cnt - 1
```### 4. Cập nhật DP 

Đối với mỗi bước:```
new_good =
    good * (cnt - 1)
    + bad * cnt

new_bad =
    good * (n - cnt)
    + bad * (n - cnt - 1)
```Tất cả các tính toán được thực hiện modulo`10^9+7`. 

### 5. Lặp lại cho k thao tác 

Áp dụng chuyển tiếp`k`lần. 

Đáp án là giá trị cuối cùng của`good`. 

### Tại sao nó hoạt động 

Mỗi trạng thái của bài toán ban đầu là một phép quay của`start`. Thông tin duy nhất liên quan đến mục tiêu cuối cùng là liệu vòng quay đó có bằng`end`. 

Cho phép`cnt`là số vòng quay đại diện`end`. Từ bất kỳ vòng quay nào, việc chọn phần tách tương đương với việc chọn bất kỳ chỉ mục xoay nào khác. Số cạnh đi tới trạng thái tốt và trạng thái xấu chỉ phụ thuộc vào trạng thái hiện tại là tốt hay xấu. Nó không phụ thuộc vào vòng quay cụ thể. 

Bởi vì tất cả các trạng thái trong cùng một danh mục đều có hành vi chuyển tiếp giống hệt nhau, nên việc tổng hợp chúng thành hai trạng thái DP sẽ duy trì số lượng đường dẫn chính xác. Sự lặp lại đếm mọi chuỗi hợp lệ của các vị trí được phân chia một lần và chỉ một lần. Sau khi xử lý chính xác`k`hoạt động,`good`bằng số chuỗi thao tác kết thúc ở một vòng quay bằng`end`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def kmp_count(text, pattern):
    m = len(pattern)

    pi = [0] * m
    j = 0
    for i in range(1, m):
        while j > 0 and pattern[i] != pattern[j]:
            j = pi[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        pi[i] = j

    cnt = 0
    j = 0
    for ch in text:
        while j > 0 and ch != pattern[j]:
            j = pi[j - 1]
        if ch == pattern[j]:
            j += 1
        if j == m:
            cnt += 1
            j = pi[j - 1]

    return cnt

def solve():
    start = input().strip()
    end = input().strip()
    k = int(input())

    n = len(start)

    cnt = kmp_count((start + start)[:-1], end)

    if cnt == 0:
        print(0)
        return

    good = 1 if start == end else 0
    bad = 1 - good

    for _ in range(k):
        new_good = (good * (cnt - 1) + bad * cnt) % MOD
        new_bad = (good * (n - cnt) + bad * (n - cnt - 1)) % MOD

        good = new_good
        bad = new_bad

    print(good)

if __name__ == "__main__":
    solve()
```Phần đầu tính toán`cnt`, số vòng quay của`start`bằng`end`. Việc sử dụng KMP giữ cho bước này tuyến tính theo độ dài chuỗi. 

biểu thức`(start + start)[:-1]`là quan trọng. Nếu không loại bỏ ký tự cuối cùng, vòng quay ban đầu sẽ xuất hiện hai lần, gây ra thêm một ký tự trùng khớp. 

DP chỉ lưu trữ hai giá trị.`good`đếm các cách để kết thúc ở một vòng quay mục tiêu, trong khi`bad`đếm cách để kết thúc ở nơi khác. Các hệ số chuyển tiếp đến trực tiếp từ việc đếm số lượng hoạt động phân chia dẫn đến từng danh mục. 

Một điểm tinh tế là thuật ngữ`cnt - 1`trong quá trình chuyển đổi từ tốt sang tốt. Một trong những phép quay tốt là chính phép quay hiện tại, nhưng thao tác phân tách phải không trống ở cả hai bên, do đó không được phép giữ nguyên một phép quay như một thao tác. Quên phép trừ này là lỗi phổ biến nhất trong bài toán này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ab
ab
2
```Đây`n = 2`. 

Các phép quay là:```
ab
ba
```Chỉ một vòng quay bằng`end`, Vì thế`cnt = 1`. 

Trạng thái ban đầu:```
good = 1
bad = 0
```| Bước | tốt | tệ | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 0 | 1 | 
| 2 | 1 | 0 | 

Trả lời:```
1
```Dấu vết này cho thấy sự phân chia duy nhất có thể xen kẽ giữa hai phép quay. 

### Ví dụ 2 

đầu vào:```
ababab
ababab
1
```Phép quay bằng`"ababab"`xảy ra ở các vị trí`0`,`2`, Và`4`. 

Như vậy:```
cnt = 3
n = 6
```Trạng thái ban đầu: 

| Bước | tốt | tệ | 
| --- | --- | --- | 
| 0 | 1 | 0 | 

Sau một thao tác: 

| Bước | tốt | tệ | 
| --- | --- | --- | 
| 1 | 2 | 3 | 

Trả lời:```
2
```Hai thao tác hợp lệ chính xác là hai phép quay không tầm thường vẫn tạo ra cùng một chuỗi hiển thị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | KMP chạy trong O(n), DP thực hiện k lần lặp | 
| Không gian | O(n) | Bảng tiền tố KMP lưu trữ các giá trị O(n) | 

Với`n ≤ 1000`Và`k ≤ 100000`, khoảng một trăm nghìn bản cập nhật DP là chuyện nhỏ. Thuật toán phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def kmp_count(text, pattern):
        m = len(pattern)

        pi = [0] * m
        j = 0
        for i in range(1, m):
            while j > 0 and pattern[i] != pattern[j]:
                j = pi[j - 1]
            if pattern[i] == pattern[j]:
                j += 1
            pi[i] = j

        cnt = 0
        j = 0
        for ch in text:
            while j > 0 and ch != pattern[j]:
                j = pi[j - 1]
            if ch == pattern[j]:
                j += 1
            if j == m:
                cnt += 1
                j = pi[j - 1]

        return cnt

    start = input().strip()
    end = input().strip()
    k = int(input())

    n = len(start)
    cnt = kmp_count((start + start)[:-1], end)

    if cnt == 0:
        return "0"

    good = 1 if start == end else 0
    bad = 1 - good

    for _ in range(k):
        ng = (good * (cnt - 1) + bad * cnt) % MOD
        nb = (good * (n - cnt) + bad * (n - cnt - 1)) % MOD
        good, bad = ng, nb

    return str(good)

# provided sample
assert run("ab\nab\n2\n") == "1", "sample 1"

# custom cases
assert run("ab\nab\n0\n") == "1", "zero operations"
assert run("ab\nba\n1\n") == "1", "single valid rotation"
assert run("abcd\nabcd\n1\n") == "0", "cannot stay in place"
assert run("aaaa\naaaa\n1\n") == "3", "all rotations equal"
assert run("abcd\nacbd\n5\n") == "0", "target not a rotation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab ab 0`|`1`| Trường hợp cơ bản không có hoạt động | 
|`ab ba 1`|`1`| Vòng quay đơn không tầm thường | 
|`abcd abcd 1`|`0`| Ở cùng một vòng quay bị cấm | 
|`aaaa aaaa 1`|`3`| Nhiều phép quay tạo ra các chuỗi giống hệt nhau | 
|`abcd acbd 5`|`0`| Mục tiêu không phải là một vòng quay bắt đầu | 

## Vỏ cạnh 

Hãy xem xét:```
ab
ab
0
```chúng tôi có`cnt = 1`. Trạng thái ban đầu là`good = 1`,`bad = 0`. Từ`k = 0`, DP không thực hiện chuyển tiếp. Câu trả lời vẫn còn`1`. Điều này đếm chính xác trình tự hoạt động trống. 

Coi như:```
aaaa
aaaa
1
```Cả bốn phép quay đều bằng nhau`"aaaa"`, Vì thế`cnt = 4`. Ban đầu`good = 1`. 

Sau một bước:```
good = 1 * (4 - 1) = 3
```Có chính xác ba vị trí phân chia hợp pháp và tất cả chúng đều tạo ra chuỗi mục tiêu. Thuật toán đếm các hoạt động, không phải các chuỗi kết quả riêng biệt. 

Coi như:```
abcd
abcd
1
```Đây`cnt = 1`. Sự tái phát mang lại:```
good = 1 * (1 - 1) = 0
```Vòng xoay tốt duy nhất là vòng xoay hiện tại, nhưng thao tác phân tách không thể giữ nguyên vòng xoay. Thuật toán loại trừ chính xác quá trình chuyển đổi không thể thực hiện được này. 

Coi như:```
abcd
acbd
3
```

`acbd`không phải là sự quay của`abcd`, do đó KMP tìm thấy`cnt = 0`. Thuật toán ngay lập tức trở lại`0`, phản ánh thực tế rằng các hoạt động phân chia duy trì trật tự tuần hoàn và không bao giờ có thể tạo ra sự sắp xếp này.
