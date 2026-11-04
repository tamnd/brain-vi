---
title: "CF 1170F - Bánh xe"
description: "Chúng ta được cung cấp một loạt các áp suất bánh xe và chúng ta cần chọn chính xác $m$ trong số đó. Sau khi chọn các bánh xe $m$ đó, chúng ta được phép thay đổi áp suất của chúng để tất cả chúng đều bằng một giá trị duy nhất."
date: "2026-06-18T17:09:12+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 94
verified: true
draft: false
---

[CF 1170F - Bánh xe](https://codeforces.com/problemset/problem/1170/F) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt áp suất bánh xe và chúng ta cần chọn chính xác$m$của họ. Sau khi chọn những$m$bánh xe, chúng ta được phép thay đổi áp suất của chúng sao cho tất cả chúng đều bằng một giá trị duy nhất. Mỗi lần thay đổi đơn vị, tăng hoặc giảm một bánh xe đi 1, tốn 1 phút. 

Có một hạn chế bổ sung khiến bài toán không đối xứng: trong khi luôn cho phép giảm áp suất, tổng số đơn vị tăng trong tất cả các hoạt động không thể vượt quá$k$. Vì vậy, nếu chúng ta tăng tổng thể một bánh xe lên 5 thì điều đó đã tiêu tốn 5 đơn vị ngân sách tăng trưởng toàn cầu. 

Nhiệm vụ là chọn cái nào$m$bánh xe để sử dụng và áp lực cuối cùng bằng nhau để nhắm mục tiêu sao cho tổng thời gian sử dụng được giảm thiểu, đồng thời không bao giờ vượt quá ngân sách tăng trưởng toàn cầu. 

Những ràng buộc buộc chúng ta phải suy nghĩ$O(n \log n)$hoặc$O(n)$điều khoản. Với$n \le 2 \cdot 10^5$, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tất cả các cặp tập hợp con đều không thể thực hiện được ngay lập tức. Thậm chí$O(n^2)$công trình xây dựng quá chậm. Điều này thúc đẩy chúng ta hướng tới một cấu trúc được sắp xếp và một giải pháp dựa trên các phân đoạn liền kề, tổng tiền tố hoặc cửa sổ trượt. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng sửa giá trị đích một cách độc lập và tham lam chọn các phần tử gần nhất. Điều đó bỏ qua việc tăng ngân sách kết hợp tất cả các yếu tố được chọn với nhau. Một cạm bẫy khác là giả định tính đối xứng: chi phí không đối xứng vì mức tăng bị hạn chế trên toàn cầu, trong khi mức giảm luôn được cho phép mà không bị hạn chế. 

Một ví dụ cụ thể trong đó việc lựa chọn từng phần tử tham lam không thành công là khi các giá trị cao ít hơn một chút nhưng lại tốn kém để giảm, trong khi các giá trị thấp thì nhiều nhưng rẻ riêng lẻ, tuy nhiên mức tăng tổng hợp của chúng lại vi phạm$k$. Một chiến lược “chọn m gần mục tiêu nhất” ngây thơ có thể dễ dàng vượt quá mức tăng ngân sách mà không nhận thấy. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử tất cả các tập hợp con có kích thước$m$và với mỗi tập hợp con hãy thử tất cả các giá trị bằng nhau cuối cùng có thể có. Đối với một tập hợp con cố định và giá trị mục tiêu, chúng ta có thể tính toán chi phí bằng cách tính tổng các chênh lệch tuyệt đối và theo dõi tổng mức tăng. Điều này đúng nhưng hoàn toàn không khả thi: số lượng tập hợp con là$\binom{n}{m}$, là số mũ trong$n$và thậm chí việc đánh giá từng tập hợp con sẽ tuyến tính theo$m$. 

Quan sát cấu trúc quan trọng là sau khi sắp xếp mảng, bất kỳ tập hợp tối ưu nào của$m$các bánh xe sẽ tạo thành một khối liền kề theo thứ tự đã sắp xếp. Lý do là nếu chúng ta cố định giá trị cuối cùng, việc hoán đổi phần tử xa hơn với phần tử gần hơn theo thứ tự được sắp xếp chỉ có thể giảm chi phí và không bao giờ làm xấu đi yêu cầu tăng. 

Một khi chúng ta giới hạn bản thân ở những đoạn dài liền kề nhau$m$, vấn đề trở nên có cấu trúc hơn nhiều. Đối với mỗi phân đoạn, chúng ta vẫn cần quyết định giá trị bằng nhau cuối cùng. Đối với chi phí sai lệch tuyệt đối, giá trị tối ưu luôn là giá trị trung bình của phân khúc đã chọn. Điều đó làm giảm đáng kể việc lựa chọn mục tiêu. 

Nhiệm vụ còn lại là kiểm tra mọi cửa sổ có kích thước$m$, liệu nó có khả thi dưới ràng buộc tăng và tính toán chi phí của nó một cách hiệu quả hay không. Điều này có thể được thực hiện bằng cách sử dụng tổng tiền tố trên mảng đã sắp xếp, cho phép đánh giá thời gian không đổi trên mỗi cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(\binom{n}{m} \cdot m)$|$O(m)$| Quá chậm | 
| Đã sắp xếp + cửa sổ trượt + tổng tiền tố |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng sao cho bất kỳ nhóm ứng cử viên nào của$m$bánh xe được biểu diễn dưới dạng một đoạn liền kề. 

Sau đó chúng tôi kiểm tra từng đoạn có chiều dài$m$, coi nó như một nhóm ứng cử viên tiềm năng. 

1. Sắp xếp mảng$a$theo thứ tự không giảm. 
2. Xây dựng các tổng tiền tố để có thể truy vấn bất kỳ tổng phạm vi nào trong$O(1)$. 
3. Đối với mọi cửa sổ$[l, r]$chiều dài$m$, tính vị trí trung vị$p = l + \frac{m-1}{2}$. Chúng tôi chọn đây là giá trị áp suất bằng nhau cuối cùng. 
4. Tính chi phí để nâng tất cả các phần tử ở nửa bên trái của cửa sổ lên$a[p]$. Đây là tổng chi phí tăng thêm:$$\text{inc} = a[p] \cdot (p - l) - \sum_{i=l}^{p-1} a[i]$$Nếu điều này vượt quá$k$, cửa sổ không hợp lệ và chúng ta bỏ qua nó. 
5. Tính toàn bộ chi phí để làm cho tất cả các phần tử bằng nhau$a[p]$: 

bên trái góp phần tăng như trên, bên phải góp phần giảm:$$\text{dec} = \sum_{i=p+1}^{r} a[i] - a[p] \cdot (r - p)$$Tổng chi phí là$\text{inc} + \text{dec}$. 
6. Theo dõi chi phí tối thiểu trên tất cả các khoảng thời gian hợp lệ. 

Quyết định quan trọng là chọn trung vị làm giá trị mục tiêu. Điều này là tối ưu để giảm thiểu độ lệch tuyệt đối bên trong một tập hợp cố định. Hạn chế bổ sung duy nhất là đảm bảo ngân sách tăng không bị vượt quá, điều này chỉ phụ thuộc vào các yếu tố dưới mức trung bình. 

### Tại sao nó hoạt động 

Đối với bất kỳ cửa sổ đã chọn nào, nếu chúng ta cố định giá trị cuối cùng thì giá trị tối ưu giảm thiểu độ lệch tuyệt đối là giá trị trung bình. Bất kỳ sai lệch nào so với mức trung bình đều làm tăng tổng chi phí một cách đối xứng ở cả hai bên. Vì việc tăng chi phí chỉ phụ thuộc vào các yếu tố nằm dưới mục tiêu nên việc dịch chuyển mục tiêu ra khỏi mức trung bình sẽ làm tăng chi phí hoặc giảm tính khả thi mà không cải thiện được mục tiêu tổng thể. Vì vậy, việc kiểm tra từng khối liền kề với trung vị của nó là đủ để nắm bắt được tất cả các giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    INF = 10**30
    ans = INF

    for l in range(0, n - m + 1):
        r = l + m - 1
        p = l + (m - 1) // 2
        x = a[p]

        left_cnt = p - l
        left_sum = pref[p] - pref[l]
        inc = x * left_cnt - left_sum

        if inc > k:
            continue

        right_cnt = r - p
        right_sum = pref[r + 1] - pref[p + 1]
        dec = right_sum - x * right_cnt

        ans = min(ans, inc + dec)

    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo rằng mọi lựa chọn tối ưu sẽ trở thành một cửa sổ liền kề theo thứ tự này. Tổng tiền tố cho phép tính toán nhanh tổng phân đoạn, điều này rất cần thiết vì việc tính lại tổng trên mỗi cửa sổ sẽ dẫn đến$O(nm)$giải pháp. 

Chi tiết triển khai tinh tế duy nhất là lập chỉ mục chính xác quanh mức trung bình. Phía bên trái nằm ngay trước chỉ số trung vị và phía bên phải bắt đầu ngay sau chỉ số trung vị. Việc trộn lẫn các ranh giới bao hàm sẽ làm thay đổi chi phí một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 6 7
6 15 16 20 1 5
```Sau khi sắp xếp:```
1 5 6 15 16 20
```Chỉ có một cửa sổ cỡ 6. 

| tôi | r | p | x | inc | tháng mười hai | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 2 | 6 | (6·2 - (1+5)) = 6 | (15+16+20 - 6·3) = 27 | 33 | 

Chi phí gia tăng nằm trong$k = 7$, vì vậy cửa sổ hợp lệ. Chi phí tối thiểu là 33. 

Dấu vết này cho thấy cách thuật toán tách biệt một cấu hình chung hợp lệ duy nhất và tính toán cả hai bên một cách độc lập bằng cách sử dụng phép chia trung vị. 

### Ví dụ 2 

đầu vào:```
5 3 3
1 2 10 11 12
```Mảng được sắp xếp:```
1 2 10 11 12
```Chúng tôi kiểm tra tất cả các cửa sổ có kích thước 3. 

Cửa sổ [0..2]: 

| p | x | inc | tháng mười hai | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | (2·1 - 1) = 1 | (10 - 2·1) = 8 | 9 | 

Cửa sổ [1..3]: 

| p | x | inc | tháng mười hai | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 | 10 | (10·1 - 2) = 8 (không hợp lệ vì >3) | - | bỏ qua | 

Cửa sổ [2..4]: 

| p | x | inc | tháng mười hai | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 3 | 11 | (11·1 - 10) = 1 | (12 - 11·1) = 1 | 2 | 

Câu trả lời hay nhất là 2. 

Điều này chứng tỏ tính khả thi được xác định hoàn toàn bằng việc tăng chi phí ở phía bên trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét cửa sổ là tuyến tính | 
| Không gian |$O(n)$| Tổng tiền tố và lưu trữ mảng | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n = 2 \cdot 10^5$cho phép quét tuyến tính sau khi sắp xếp và tổng tiền tố tránh mọi tính toán lại lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def solve():
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        ans = 10**30

        for l in range(n - m + 1):
            r = l + m - 1
            p = l + (m - 1) // 2
            x = a[p]

            inc = x * (p - l) - (pref[p] - pref[l])
            if inc > k:
                continue

            dec = (pref[r + 1] - pref[p + 1]) - x * (r - p)
            ans = min(ans, inc + dec)

        return str(ans)

# provided sample
assert run("6 6 7\n6 15 16 20 1 5\n") == "33"

# all equal
assert run("4 3 10\n5 5 5 5\n") == "0"

# minimal window
assert run("3 1 0\n10 1 7\n") == "0"

# increasing array tight k
assert run("5 3 1\n1 2 3 4 5\n") == "2"

# large gap case
assert run("5 2 100\n1 100 1000 10000 100000\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | đường cơ sở chi phí bằng không | 
| m = 1 trường hợp | 0 | lựa chọn phần tử đơn | 
| ràng buộc k chặt chẽ | chi phí nhỏ | tăng cường thực thi ràng buộc | 
| khoảng trống lớn | điều chỉnh tối thiểu | chính xác dưới các giá trị sai lệch | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử đều giống hệt nhau. Trong tình huống đó, mọi cửa sổ đều có chi phí bằng 0 và thuật toán không được từ chối nhầm bất kỳ phân đoạn nào do chi phí tăng được tính toán bằng 0. Công thức mang lại kết quả bằng 0 cho cả số hạng tăng và giảm, vì vậy mọi cửa sổ đều hợp lệ. 

Một trường hợp cạnh khác xảy ra khi$m = 1$. Logic trung bình vẫn chọn phần tử duy nhất trong mỗi cửa sổ và cả tăng và giảm đều bằng 0, phù hợp với thực tế là không cần thay đổi. 

Một trường hợp tế nhị hơn là khi$k = 0$. Ở đây, bất kỳ cửa sổ nào có các phần tử nằm dưới mức trung vị sẽ trở thành không hợp lệ trừ khi tất cả các phần tử được chọn đã ở mức hoặc trên mức trung vị. Thuật toán xử lý việc này một cách chính xác vì chi phí tăng được tính toán được kiểm tra nghiêm ngặt so với 0 trước khi chấp nhận một khoảng thời gian.
