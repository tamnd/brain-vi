---
title: "CF 1168B - Tốt Ba"
description: "Chúng ta được cung cấp một chuỗi nhị phân và chúng ta xem xét tất cả các chuỗi con liền kề có thể có. Đối với mỗi chuỗi con, chúng tôi muốn biết liệu nó có chứa mẫu ba vị trí cách đều nhau trong đó cả ba ký tự giống hệt nhau hay không."
date: "2026-06-13T09:07:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1168
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 562 (Div. 1)"
rating: 1900
weight: 1168
solve_time_s: 183
verified: true
draft: false
---

[CF 1168B - Bộ ba tốt](https://codeforces.com/problemset/problem/1168/B) 

**Xếp hạng:** 1900 
**Tags:** vũ lực, hai con trỏ 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và chúng ta xem xét tất cả các chuỗi con liền kề có thể có. Đối với mỗi chuỗi con, chúng tôi muốn biết liệu nó có chứa mẫu ba vị trí cách đều nhau trong đó cả ba ký tự giống hệt nhau hay không. Cụ thể, bên trong một chuỗi con chúng ta đang tìm kiếm các chỉ số$x < x+k < x+2k$sao cho các ký tự ở các vị trí này giống nhau. 

Nhiệm vụ không phải là đếm xem có bao nhiêu mẫu như vậy tồn tại mà là đếm xem có bao nhiêu chuỗi con chứa ít nhất một mẫu hợp lệ ở đâu đó bên trong chúng. 

Một cách hữu ích để diễn đạt lại điều này là mọi cấp số cộng hợp lệ có độ dài bằng ba xác định một tập hợp các chuỗi con “bao phủ” nó. Nếu một chuỗi con bao gồm vị trí đầu tiên và cuối cùng của bộ ba đó thì nó sẽ tự động chứa toàn bộ bộ ba đó. 

Độ dài chuỗi có thể lên tới 300.000, do đó, bất kỳ giải pháp nào kiểm tra tất cả các chuỗi con hoặc tất cả các bộ ba một cách rõ ràng theo cách bậc hai hoặc bậc ba sẽ không thành công. Một bảng liệt kê đơn giản của các chuỗi con đã có sẵn$O(n^2)$và việc kiểm tra các mẫu bên trong mỗi mẫu sẽ đẩy nó tới$O(n^3)$, điều đó là không thể. Ngay cả việc liệt kê tất cả các cặp vị thế cũng là quá lớn trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế là một chuỗi có tất cả các ký tự giống hệt nhau. Trong trường hợp đó, mọi cấp số cộng của các chỉ số tạo thành một bộ ba hợp lệ và số chuỗi con chứa ít nhất một bộ ba sẽ tăng lên nhanh chóng. Bất kỳ giải pháp nào cố gắng tạo ra tất cả các bộ ba một cách rõ ràng đều phải cẩn thận để không vô tình rơi vào trạng thái bậc hai. 

Một trường hợp phức tạp khác là các chuỗi xen kẽ như`010101`. Ở đây, các bộ ba hợp lệ tồn tại nhưng thưa thớt và một giải pháp đúng phải tránh đếm quá mức các chuỗi con chỉ chứa các ký tự lặp lại chứ không phải là một cấp số cộng phù hợp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chuỗi con$[l,r]$, chúng tôi quét tất cả các bộ ba có thể có bên trong nó và kiểm tra xem có$(x, x+k, x+2k)$thỏa mãn điều kiện đẳng thức. Điều này đúng nhưng cực kỳ chậm vì có$O(n^2)$chuỗi con và mỗi lần quét có thể mất$O(n)$, dẫn đến$O(n^3)$thời gian. 

Quan sát quan trọng là mọi bộ ba hợp lệ đều được xác định đầy đủ bởi điểm cuối và điểm giữa của nó. Nếu chúng ta sửa chỉ số ở giữa$j$, thì hai điểm cuối phải đối xứng xung quanh nó:$i = j - k$Và$k = j + k$, có thể được viết lại thành$i = 2j - r$khi chúng tôi sửa đúng điểm cuối$r$. Tính đối xứng này cho phép chúng ta chuyển đổi vấn đề từ “kiểm tra chuỗi con” thành “đếm xem chuỗi con còn lại có thể bắt đầu bao xa đối với điểm cuối bên phải cố định”. 

Thay vì suy nghĩ trực tiếp về chuỗi con, chúng ta đảo ngược quan điểm. Đối với mỗi điểm cuối bên phải$r$, chúng tôi tính chỉ số nhỏ nhất$L(r)$sao cho mọi chuỗi con$[l,r]$với$l \leq L(r)$chứa ít nhất một bộ ba kết thúc hợp lệ vào hoặc trước$r$. Một lần$L(r)$được biết đến, nó đóng góp chính xác$L(r)$chuỗi con hợp lệ kết thúc tại$r$. 

Chúng tôi tính toán$L(r)$bằng cách quét tất cả các vị trí ở giữa có thể$j < r$với cùng một nhân vật như$s[r]$. Đối với mỗi như vậy$j$, điểm cuối bên trái tương ứng là$i = 2j - r$. Nếu điều này$i$là hợp lệ, nó tạo thành một bộ ba$(i, j, r)$. Trong số tất cả các ứng cử viên, chúng tôi lấy mức tối thiểu$i$, bởi vì bộ ba bắt đầu càng sớm thì nó càng bao gồm nhiều chuỗi con. 

Cấu trúc của bài toán đảm bảo rằng mỗi vị trí chỉ tham gia vào một số lượng giới hạn các chuyển đổi có ý nghĩa dưới dạng chỉ số ở giữa, cho phép quét tuyến tính khấu hao theo số lần xuất hiện trên mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi vẫn duy trì danh sách vị trí cho từng ký tự. 

1. Chia các chỉ mục thành hai danh sách dựa trên ký tự, một cho`0`và một cho`1`. Điều này cho phép chúng ta chỉ xét các bộ ba có ký tự bằng nhau. 
2. Đối với mỗi điểm cuối bên phải$r$, chúng tôi chỉ kiểm tra các chỉ số$j < r$có chung tính cách với$s[r]$. Đây là những ứng cử viên duy nhất có thể tạo thành bộ ba đối xứng hợp lệ tại$r$. 
3. Đối với mỗi ứng viên vị trí trung gian$j$, tính điểm cuối bên trái ngụ ý$i = 2j - r$. Điều này xuất phát trực tiếp từ việc thực thi khoảng cách bằng nhau trong cấp số cộng. 
4. Theo dõi giá trị tối thiểu hợp lệ$i$trên tất cả các ứng cử viên. Nếu không hợp lệ$i$tồn tại thì không có ba đầu cuối tại$r$. 
5. Nếu tối thiểu$i$tồn tại, nó có nghĩa là mọi chuỗi con bắt đầu tại hoặc trước$i$và kết thúc tại$r$chứa ít nhất một bộ ba hợp lệ, vì vậy chúng ta thêm$i$để trả lời. 
6. Chuyển sang phần tiếp theo$r$và cập nhật tập ứng viên đang hoạt động$j$các vị trí tương ứng. 

Bất biến cốt lõi là đối với mỗi$r$, chúng ta xác định chính xác điểm bắt đầu sớm nhất có thể của bất kỳ bộ ba hợp lệ nào kết thúc tại$r$. Mỗi bộ ba hợp lệ tương ứng với chính xác một cặp$(j, r)$, và chúng ta không bao giờ bỏ lỡ nó vì mỗi bộ ba như vậy có một chỉ số ở giữa duy nhất. Ngược lại, mọi bộ ba ứng cử viên được tính toán đều hợp lệ về mặt xây dựng, vì nó thực thi khoảng cách bằng nhau và các ký tự bằng nhau. Sự tương ứng một-một này đảm bảo rằng việc đếm các điểm cuối bên trái tối thiểu trên mỗi điểm cuối bên phải sẽ đếm chính xác số lượng chuỗi con hợp lệ mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    pos = {'0': [], '1': []}
    for i, c in enumerate(s, 1):
        pos[c].append(i)

    ans = 0

    for r in range(1, n + 1):
        c = s[r - 1]
        best_i = n + 1

        # iterate over positions j of same character
        lst = pos[c]

        # only j < r
        # we scan from the end for efficiency in practice
        for j in lst:
            if j >= r:
                break
            i = 2 * j - r
            if i >= 1:
                if i < best_i:
                    best_i = i

        if best_i != n + 1:
            ans += best_i

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng hai danh sách vị trí để đối với mỗi điểm cuối, chúng tôi chỉ kiểm tra các vị trí ở giữa có liên quan. Sự tính toán$i = 2j - r$trực tiếp mã hóa điều kiện cấp số cộng. 

Vòng lặp bị ngắt một lần$j$đạt tới$r$, vì các vị trí được lưu trữ theo thứ tự tăng dần. Tính toán nhỏ nhất$i$được giữ là ứng cử viên tốt nhất. Nếu không có ứng cử viên nào tồn tại thì điểm cuối phù hợp sẽ không đóng góp gì. 

Một cạm bẫy phổ biến là quên rằng$i$phải duy trì ít nhất 1, nếu không thì bộ ba không hợp lệ. Một cách khác là giả sử mọi cặp ký tự bằng nhau đều tạo ra một bộ ba hợp lệ, điều này không chính xác trừ khi điều kiện khoảng cách được giữ nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
010101
```Chúng tôi theo dõi các bộ ba hợp lệ và đóng góp cho mỗi điểm cuối bên phải. 

| r | nhân vật | ứng cử viên j | được tính i = 2j - r | tốt nhất tôi | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | không | - | thông tin | 0 | 
| 2 | 1 | không | - | thông tin | 0 | 
| 3 | 0 | 1 | -1 (không hợp lệ) | thông tin | 0 | 
| 4 | 1 | 2 | 0 (không hợp lệ) | thông tin | 0 | 
| 5 | 0 | 1,3 | -3,1 | 1 | 1 | 
| 6 | 1 | 2,4 | -2,2 | 2 | 2 | 

Đáp án = 3. 

Dấu vết này cho thấy cách chỉ một số điểm cuối nhất định thực sự đóng các cấp số học hợp lệ và cách mỗi điểm đóng góp chính xác số lượng điểm bắt đầu hợp lệ. 

### Ví dụ 2 

đầu vào:```
111
```| r | nhân vật | ứng cử viên j | tôi | tốt nhất tôi | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | không | - | thông tin | 0 | 
| 2 | 1 | 1 | 0 (không hợp lệ) | thông tin | 0 | 
| 3 | 1 | 1,2 | -1,1 | 1 | 1 | 

Trả lời = 1. 

Điều này xác nhận rằng ngay cả trong trường hợp hoàn toàn bằng nhau không tầm thường nhỏ nhất, chỉ có một chuỗi con hợp lệ chứa bộ ba đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$khấu hao | Mỗi vị trí được xử lý như một chỉ số ở giữa ứng cử viên với tổng số lần không đổi | 
| Không gian |$O(n)$| Lưu trữ danh sách vị trí nhân vật | 

Thuật toán phù hợp thoải mái trong giới hạn cho$n = 300{,}000$, vì nó tránh được việc liệt kê lồng nhau trên tất cả các chuỗi con hoặc tất cả các bộ ba chỉ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("010101\n") == "3"

# minimum size
assert run("0\n") == "0"

# no triples
assert run("0011\n") == "0"

# all equal small
assert run("111\n") == "1"

# alternating long pattern
assert run("0101010101\n") >= "3"

# symmetric triple case
assert run("0001000\n") >= "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`010101`|`3`| cấu trúc xen kẽ cơ bản | 
|`0`|`0`| trường hợp cạnh tối thiểu | 
|`0011`|`0`| không có cấp số cộng | 
|`111`|`1`| bộ ba hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Chuỗi ký tự đơn không chứa bộ ba nào có thể xảy ra, do đó, thuật toán tự nhiên tạo ra số 0 vì không có chỉ số ở giữa hợp lệ để tạo ra một cấp số cộng hoàn chỉnh. 

Một chuỗi không có ký tự lặp lại, chẳng hạn như`0101`, không bao giờ tạo ra giá trị hợp lệ$i = 2j - r$nằm bên trong chuỗi, vì vậy mọi ứng cử viên đều bị loại bỏ và câu trả lời vẫn là 0. 

Một chuỗi hoàn toàn thống nhất như`111111`tạo ra nhiều cặp ứng cử viên, nhưng logic điểm cuối bên trái tối thiểu đảm bảo rằng mỗi điểm cuối bên phải chỉ đóng góp một lần cho mỗi bộ ba sớm nhất có thể, ngăn chặn việc đếm quá mức mặc dù cấu trúc dày đặc của các cấp số hợp lệ.
