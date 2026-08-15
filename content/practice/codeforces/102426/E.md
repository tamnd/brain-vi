---
title: "CF 102426E - \u9f99\u8bed\u9b54\u6cd5"
description: "Ta có dãy n số nguyên dương. Mỗi cặp chỉ số l <= r xác định một mảng con liền kề và giá trị của nó là tổng của tất cả các phần tử từ l đến r."
date: "2026-08-12T19:23:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "E"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 91
verified: true
draft: false
---

[CF 102426E - \u9f99\u8bed\u9b54\u6cd5](https://codeforces.com/problemset/problem/102426/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`n`số nguyên dương. Mỗi cặp chỉ số`l <= r`định nghĩa một mảng con liền kề và giá trị của nó là tổng của tất cả các phần tử từ`l`bởi vì`r`. Có chính xác`n(n+1)/2`các mảng con như vậy và các tổng bằng nhau được tính riêng vì các mảng con khác nhau tương ứng với các kết quả khác nhau của phép thuật. 

Nhiệm vụ là tìm ra`k`-tổng mảng con nhỏ nhất sau khi tất cả các tổng này được sắp xếp theo thứ tự không giảm. Ví dụ, với`2 3 1 4`, mười mảng con tạo ra chuỗi được sắp xếp`1, 2, 3, 4, 4, 5, 5, 6, 8, 10`, Vì thế`k = 6`cho`5`. 

Độ dài mảng có thể đạt tới`10^5`. Điều đó có nghĩa là có thể có khoảng`5 * 10^9`mảng con. Ngay cả một thuật toán thực hiện công việc liên tục trên mỗi mảng con cũng đã quá chậm và việc lưu trữ rõ ràng tất cả các tổng của mảng con là không thể trong giới hạn bộ nhớ. Các phần tử có thể lớn bằng`10^9`, do đó tổng của mảng con có thể đạt tới khoảng`10^14`, cũng yêu cầu số học số nguyên 64 bit trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Tính tích cực của mỗi`ai`là hạn chế cơ cấu quan trọng. Do đó, các tổng tiền tố tăng lên một cách chặt chẽ, điều này cho phép chúng ta đếm xem có bao nhiêu tổng của mảng con có giá trị tối đa là một giá trị nhất định trong thời gian tuyến tính. Nếu không có tính tích cực, đối số đếm hai con trỏ sẽ không hoạt động. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Vì`n = 1`, kết quả duy nhất có thể là phần tử đơn lẻ. Ví dụ,`1 1`theo sau là`7`có câu trả lời`7`; mã vô tình tìm kiếm trong phạm vi mảng con trống có thể bị lỗi ở đây. 

Tổng các mảng con bằng nhau phải được tính theo bội số. Vì`3 4`theo sau là`2 2 2`, tổng được sắp xếp là`2, 2, 2, 4, 4, 6`, vậy câu trả lời là`4`. Một giải pháp xử lý các kết quả dưới dạng các giá trị riêng biệt sẽ nghĩ sai rằng chỉ có ba kết quả có thể xảy ra. 

Thứ hạng nhỏ nhất và lớn nhất có thể cũng có ý nghĩa. Vì`3 1`theo sau là`1 5 2`, tổng mảng con nhỏ nhất là`1`, trong khi đối với`3 6`câu trả lời là tổng của toàn bộ mảng,`8`. Tìm kiếm nhị phân sử dụng bất đẳng thức nghiêm ngặt trong hàm đếm có thể bị sai lệch một ở một trong hai ranh giới. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi mảng con liền kề. Đối với mỗi điểm cuối bên trái, hãy mở rộng điểm cuối bên phải từng vị trí một và duy trì tổng hiện tại, tính tổng của mọi mảng con trong`O(1)`công việc bổ sung. Điều này đúng vì mỗi cặp`(l, r)`xuất hiện đúng một lần. có`n(n+1)/2`những cặp như vậy, đó là`5,000,050,000`khi`n = 100000`. Chỉ tạo ra nhiều giá trị như vậy đã vượt quá giới hạn thời gian và việc sắp xếp chúng sẽ khiến tình hình trở nên tồi tệ hơn. 

Cách tiếp cận bạo lực hoạt động vì nó xây dựng rõ ràng bộ sưu tập có`k`-phần tử thứ chúng ta muốn. Vấn đề là bản thân bộ sưu tập có kích thước bậc hai. Sự thay đổi quan điểm hữu ích là ngừng hỏi tổng mỗi mảng con và thay vào đó hãy hỏi một câu hỏi đếm: cho một số`x`, có bao nhiêu mảng con có tổng nhiều nhất`x`? 

Khi thao tác đếm đó diễn ra nhanh, bài toán lựa chọn ban đầu sẽ trở thành tìm kiếm nhị phân. Nếu chúng ta có thể tính toán`count(x)`, số lượng tổng của mảng con không lớn hơn`x`, sau đó`count(x)`là đơn điệu. Đối với nhỏ`x`, một số tiền phù hợp; BẰNG`x`tăng lên thì số lượng chỉ có thể tăng lên. Đáp án chính xác là nhỏ nhất`x`vì cái gì`count(x) >= k`. 

Để tính toán`count(x)`, xác định tổng tiền tố`p[0] = 0`Và`p[i] = a1 + a2 + ... + ai`. 

Tổng của mảng con`l..r`là`p[r] - p[l-1]`. Bởi vì mọi phần tử mảng đều dương,`p`đang gia tăng nghiêm trọng. Đối với chỉ mục tiền tố bên phải cố định`r`, chúng tôi cần`p[r] - p[i] <= x`tương đương với`p[i] >= p[r] - x`. 

Vì tổng tiền tố đã được sắp xếp nên chúng ta có thể tìm được số tiền phù hợp đầu tiên`i`bằng cách tìm kiếm nhị phân cho mọi`r`, cho`O(n log n)`đếm. Chúng ta có thể làm tốt hơn. BẰNG`r`di chuyển sang phải,`p[r]`tăng nên ngưỡng`p[r] - x`cũng tăng lên. hợp lệ đầu tiên`i`không bao giờ có thể lùi lại được. Do đó, một con trỏ di chuyển sẽ đếm tất cả các lần bắt đầu hợp lệ trong`O(n)`thời gian. 

Nhu cầu tìm kiếm nhị phân bên ngoài`O(log S)`lặp đi lặp lại, ở đâu`S`là tổng mảng. Từ`S <= 10^14`, đây là ít hơn 50 lần lặp. Độ phức tạp thu được là`O(n log S)`, điều này thiết thực cho`n = 10^5`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n²) | Quá chậm | 
| Tối ưu | O(n log S) | O(n) | Đã chấp nhận | 

Đây`S`biểu thị tổng của toàn bộ mảng. 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố`p`. Bộ`p[0] = 0`, sau đó`p[i] = p[i-1] + a[i]`. Bởi vì tất cả`ai`là dương, tổng tiền tố tăng nghiêm ngặt. 
2. Xác định hàm`count(x)`trả về số mảng con liền kề có tổng lớn nhất`x`. Đối với mỗi điểm cuối bên phải được biểu thị bằng chỉ mục tiền tố`r`, chúng ta cần đếm tất cả`i < r`thỏa mãn`p[r] - p[i] <= x`. 
3. Viết lại điều kiện dưới dạng`p[i] >= p[r] - x`. Duy trì một con trỏ`left`đến tổng tiền tố đầu tiên thỏa mãn điều kiện này. Trong khi`left < r`Và`p[left] < p[r] - x`, nâng cao`left`. 
4. Sau khi con trỏ dừng lại, mọi chỉ số tiền tố từ`left`bởi vì`r-1`đưa ra một mảng con hợp lệ kết thúc tại`r-1`. có`r - left`trong số đó, vì vậy hãy cộng số đó vào số đếm. 

Con trỏ không bao giờ cần phải di chuyển về phía sau. Khi`r`tăng, ngưỡng`p[r] - x`tăng vì tổng tiền tố ngày càng tăng. Do đó, tổng tiền tố vốn đã quá nhỏ sẽ không bao giờ có giá trị sau này. 
5. Tìm kiếm nhị phân câu trả lời giữa`1`Và`p[n]`, tổng số mảng. Đối với một điểm giữa`mid`, tính toán`count(mid)`. Nếu số lượng ít nhất là`k`, sau đó`mid`đủ lớn để chứa`k`-tổng nhỏ nhất nên tìm nửa bên trái. Nếu không, hãy tìm kiếm nửa bên phải. 
6. Khi tìm kiếm nhị phân kết thúc, giới hạn dưới là giá trị nhỏ nhất có số đếm đạt`k`. In giá trị đó. 

### Tại sao nó hoạt động 

Đối với mỗi cố định`x`, hàm đếm sẽ xem xét mọi điểm cuối bên phải có thể có. Đối với điểm cuối đó, các chỉ số tiền tố bên trái hợp lệ tạo thành một hậu tố của các chỉ mục tiền tố trước đó vì tổng tiền tố đang tăng lên. Con trỏ di chuyển xác định thành viên đầu tiên của hậu tố đó, vì vậy`r - left`đếm chính xác tất cả các mảng con hợp lệ kết thúc ở đó. Như vậy`count(x)`chính xác là số lượng tổng của mảng con nhiều nhất`x`. 

Vị ngữ`count(x) >= k`là đơn điệu vì tăng`x`chỉ có thể biến các tổng mảng con bổ sung từ quá lớn thành tổng hợp lệ. Do đó, tìm kiếm nhị phân tìm thấy giá trị nhỏ nhất`x`chứa ít nhất`k`tổng mảng con. Vì tổng của mảng con là số nguyên nên giá trị đó chính xác là`k`- tổng nhỏ nhất thứ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + a[i - 1]

    total = prefix[n]

    def count_at_most(x):
        left = 0
        count = 0

        for right in range(1, n + 1):
            threshold = prefix[right] - x

            while left < right and prefix[left] < threshold:
                left += 1

            count += right - left

            if count >= k:
                return count

        return count

    lo = 1
    hi = total

    while lo < hi:
        mid = (lo + hi) // 2

        if count_at_most(mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Cấu trúc tiền tố lưu trữ tổng của giá trị đầu tiên`i`phần tử ở vị trí`i`. Số 0 thêm ở`prefix[0]`đại diện cho một tiền tố trống, cần thiết cho các mảng con bắt đầu tại chỉ mục`1`. 

Bên trong`count_at_most`,`right`là chỉ số tiền tố kết thúc. Mảng con tương ứng bắt đầu sau một số chỉ mục tiền tố`left`. Sự bất bình đẳng`prefix[left] >= prefix[right] - x`chính xác là điều kiện mà tổng mảng con thu được không vượt quá`x`. 

Điều kiện ở`while`sử dụng vòng lặp`<`, không`<=`. Nếu như`prefix[left] == prefix[right] - x`, mảng con kết quả có tổng chính xác`x`và phải được tính. Việc loại trừ đẳng thức sẽ biến hàm số thành một số tổng nhỏ hơn rất nhiều so với`x`, điều này sẽ làm thay đổi câu trả lời tìm kiếm nhị phân. 

các`left < right`điều kiện ngăn cản việc sử dụng cùng một chỉ mục tiền tố ở cả hai bên, điều này sẽ biểu thị một mảng con trống. Mỗi mảng con hợp lệ có một cặp chỉ số tiền tố riêng biệt với`left < right`. 

Sự trở lại sớm khi`count >= k`là không cần thiết để đảm bảo tính chính xác, nhưng nó tránh việc quét các tổng tiền tố còn lại một khi tìm kiếm nhị phân đã biết rằng điều này`x`là đủ lớn. Số nguyên Python tự động xử lý các tổng vượt quá phạm vi 64 bit, mặc dù các ràng buộc thực tế chỉ yêu cầu các giá trị xung quanh`10^14`. 

Việc tìm kiếm nhị phân sử dụng`lo = 1`bởi vì mọi phần tử mảng đều dương, nên mọi mảng con khác trống đều có tổng dương.`hi = total`hợp lệ vì bản thân toàn bộ mảng có tổng đó và đó là tổng của mảng con lớn nhất có thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`4 6`với mảng`2 3 1 4`, tổng tiền tố là`0, 2, 5, 6, 10`. Câu trả lời đúng là`5`. 

Một phần hữu ích của tìm kiếm nhị phân có thể được theo dõi như sau. 

|`lo`|`hi`|`mid`|`count(mid)`| Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 5 | 6 |`6 >= 6`, tìm kiếm bên trái | 
| 1 | 5 | 3 | 3 |`3 < 6`, tìm kiếm bên phải | 
| 4 | 5 | 4 | 5 |`5 < 6`, tìm kiếm bên phải | 
| 5 | 5 | 5 | 6 | dừng lại | 

Vì`x = 5`, sáu mảng con đủ điều kiện là tổng`1, 2, 3, 4, 4, 5`. Vì`x = 4`, chỉ có năm mảng con đủ điều kiện. Điều này chứng tỏ rằng`5`là ngưỡng đầu tiên chứa ít nhất sáu kết quả. 

### Ví dụ tùy chỉnh 2 

Hãy xem xét:```
3 4
2 2 2
```Tổng tiền tố là`0, 2, 4, 6`. Tổng sáu mảng con là`2, 2, 2, 4, 4, 6`, vậy đáp án thứ tư là`4`. 

Vì`x = 3`, mỗi mảng con có độ dài một có tổng`2`, trong khi mọi mảng con có độ dài hai hoặc độ dài ba đều quá lớn. Như vậy`count(3) = 3`. 

Vì`x = 4`, ba mảng con có độ dài một và hai mảng con có độ dài hai đủ điều kiện, cho`count(4) = 5`. 

|`lo`|`hi`|`mid`|`count(mid)`| Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 3 | 3 |`3 < 4`, tìm kiếm bên phải | 
| 4 | 6 | 5 | 5 |`5 >= 4`, tìm kiếm bên trái | 
| 4 | 5 | 4 | 5 |`5 >= 4`, tìm kiếm bên trái | 
| 4 | 4 | 4 | 5 | dừng lại | 

Câu trả lời là`4`. Ví dụ này giải thích tại sao số tiền trùng lặp phải được tính riêng. giá trị`2`xảy ra ba lần và giá trị`4`xảy ra hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log S) | Mỗi`count_at_most`cuộc gọi di chuyển`left`Và`right`chỉ chuyển tiếp nên tốn O(n) và tìm kiếm nhị phân sử dụng lệnh gọi O(log S). | 
| Không gian | O(n) | Mảng tổng tiền tố chứa`n + 1`các giá trị. | 

Với`n <= 10^5`Và`S <= 10^14`, tìm kiếm nhị phân thực hiện ít hơn 50 lượt đếm. Mỗi lượt là tuyến tính, do đó thuật toán chỉ thực hiện vài triệu thao tác con trỏ thay vì hàng tỷ thao tác mảng con. Mảng tiền tố cũng sử dụng bộ nhớ tuyến tính, vừa vặn thoải mái trong giới hạn 64 MB đã nêu trong Python chỉ với một số lưu ý; việc triển khai chỉ lưu trữ mảng đầu vào và mảng tiền tố và không có tập hợp bậc hai của tổng mảng con. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(data)
    sys.stdout = io.StringIO()

    n, k = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + a[i - 1]

    def count_at_most(x):
        left = 0
        count = 0

        for right in range(1, n + 1):
            threshold = prefix[right] - x

            while left < right and prefix[left] < threshold:
                left += 1

            count += right - left

            if count >= k:
                return count

        return count

    lo, hi = 1, prefix[n]

    while lo < hi:
        mid = (lo + hi) // 2
        if count_at_most(mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    result = str(lo)

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample
assert solve_data("4 6\n2 3 1 4\n") == "5", "sample 1"

# Minimum-size input
assert solve_data("1 1\n7\n") == "7", "single element"

# All values equal, with duplicate sums
assert solve_data("3 4\n2 2 2\n") == "4", "duplicate sums"

# Smallest possible rank
assert solve_data("4 1\n5 1 3 2\n") == "1", "k = 1"

# Largest possible rank, which must be the whole-array sum
assert solve_data("4 10\n5 1 3 2\n") == "11", "k = n(n+1)/2"

# Maximum-size structural test, all values equal.
# The expected value can be computed directly without enumerating subarrays.
n = 100000
a = "1 " * (n - 1) + "1"
k = n * (n + 1) // 2
expected = n
assert solve_data(f"{n} {k}\n{a}\n") == str(expected), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7`| Kích thước tối thiểu và mảng con duy nhất có thể có | 
|`3 4 / 2 2 2`|`4`| Tổng trùng lặp và bội số | 
|`4 1 / 5 1 3 2`|`1`| Ranh giới dưới`k = 1`| 
|`4 10 / 5 1 3 2`|`11`| Ranh giới trên`k = n(n+1)/2`| 
|`n = 100000`, tất cả những cái |`100000`| Kích thước đầu vào tối đa và lớn`k`| 

## Vỏ cạnh 

Đối với một phần tử, đầu vào`1 1`theo sau là`7`chỉ đưa ra một sự khác biệt về tiền tố,`7 - 0 = 7`. Trong quá trình đếm,`right = 1`Và`left = 0`, do đó số đếm trở thành`1`. Tìm kiếm nhị phân có`lo = hi = 7`, và ngay lập tức quay trở lại`7`. 

Đối với số tiền trùng lặp, hãy xem xét`3 4`theo sau là`2 2 2`. Tại`x = 3`, con trỏ đếm chính xác ba mảng con có độ dài một, cho`3`, bên dưới`k = 4`. Tại`x = 4`, hai mảng con có độ dài hai cũng trở thành hợp lệ, do đó số đếm trở thành`5`. Do đó, ngưỡng nhỏ nhất có ít nhất bốn kết quả là`4`. Thuật toán không bao giờ loại bỏ tổng, do đó bội số được bảo toàn một cách tự nhiên. 

Đối với thứ hạng nhỏ nhất có thể,`4 1`theo sau là`5 1 3 2`có tổng mảng con tối thiểu`1`. Vị ngữ`count(x) >= 1`đầu tiên trở thành sự thật tại`x = 1`. Tìm kiếm nhị phân giữ nửa dưới bất cứ khi nào số đếm đạt đến`k`, vì vậy nó ổn định chính xác trên`1`. 

Để có thứ hạng lớn nhất có thể,`4 10`theo sau là`5 1 3 2`yêu cầu tổng mảng con thứ mười và cuối cùng. Vì tất cả các phần tử đều dương nên mảng hoàn chỉnh có tổng lớn nhất,`5 + 1 + 3 + 2 = 11`. Tại`x = 10`, không phải tất cả mười mảng con đều đủ điều kiện, trong khi tại`x = 11`, cả mười đều làm được. Do đó tìm kiếm nhị phân trả về`11`. 

Bài kiểm tra tất cả một kích thước tối đa có`100000`các yếu tố và`k = n(n+1)/2`, do đó kết quả được yêu cầu là tổng mảng con lớn nhất, tức là`100000`. Hàm đếm không bao giờ xây dựng khoảng năm tỷ mảng con. Con trỏ của nó chỉ đơn giản là tiến qua các tổng tiền tố một lần trong mỗi lần lặp tìm kiếm nhị phân, chứng minh tại sao số bậc hai của các kết quả có thể có không bắt buộc phải có thuật toán bậc hai.
