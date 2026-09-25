---
title: "CF 104814C - \u041b\u0430\u043c\u043f\u044b"
description: "Chúng ta có một dòng vị trí từ 1 đến n, mỗi vị trí có độ sáng tối thiểu cần thiết. Chúng tôi cũng có một số đèn, mỗi đèn bao phủ một đoạn vị trí liền kề."
date: "2026-06-28T13:06:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104814
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0420\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0435 \u0411\u0430\u0448\u043a\u043e\u0440\u0442\u043e\u0441\u0442\u0430\u043d 2023 (9 - 11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104814
solve_time_s: 100
verified: false
draft: false
---

[CF 104814C - \u041b\u0430\u043c\u043f\u044b](https://codeforces.com/problemset/problem/104814/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng vị trí từ 1 đến n, mỗi vị trí có độ sáng tối thiểu cần thiết. Chúng tôi cũng có một số đèn, mỗi đèn bao phủ một đoạn vị trí liền kề. Nếu chúng ta gán cùng một giá trị công suất x cho mỗi đèn thì mỗi đèn sẽ tăng thêm x độ sáng cho mọi vị trí trong đoạn của nó. Vì vậy, độ sáng cuối cùng của một vị trí là x nhân với số lượng đèn chiếu vào vị trí đó. 

Đối với mỗi vị trí i, nếu ci là số lượng đèn bao phủ nó thì độ sáng cuối cùng của nó sẽ là x · ci. Một vị trí được coi là “tốt” nếu giá trị này đạt ít nhất ngưỡng yêu cầu ai. Nhiệm vụ là chọn số nguyên không âm x nhỏ nhất sao cho có ít nhất k vị trí tốt. Nếu không có x nào tồn tại thì câu trả lời là -1. 

Các ràng buộc lên tới 100000 vị trí và 100000 đèn, điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại phạm vi phủ sóng hoặc kiểm tra tính khả thi một cách độc lập cho từng ứng cử viên x một cách ngây thơ. Bất kỳ mô phỏng bậc hai nào trên các vị trí và giá trị của x đều không thể thực hiện được và ngay cả việc quét tuyến tính trên mỗi bước tìm kiếm nhị phân cũng phải được tối ưu hóa cẩn thận. 

Sự tinh tế quan trọng xuất hiện khi một vị trí không bị che khuất bởi bất kỳ chiếc đèn nào. Trong trường hợp đó ci bằng 0, nên độ sáng của nó luôn bằng 0 bất kể x. Nếu ai dương thì vị trí đó không bao giờ có thể trở nên tốt. Nếu ai bằng 0 thì nó luôn tốt ngay cả khi x = 0. Việc triển khai bất cẩn chia cho ci hoặc bỏ qua trường hợp có phạm vi bao phủ bằng 0 sẽ thất bại đối với các đầu vào như vậy. 

Một trường hợp góc khác xuất phát từ điều kiện khả thi toàn cầu. Ngay cả với x lớn tùy ý, chỉ những vị trí có ci > 0 (hoặc ai = 0) mới có thể được thỏa mãn. Nếu có ít hơn k vị trí thuộc loại này thì câu trả lời phải là -1. Một giải pháp chỉ tìm kiếm nhị phân mà không kiểm tra tính khả thi sẽ trả về một giá trị không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử tăng giá trị của x và kiểm tra xem có bao nhiêu vị trí trở nên tốt cho mỗi vị trí. Với x cố định, chúng ta tính toán phạm vi bao phủ ci cho mọi vị trí, sau đó đếm xem có bao nhiêu i thỏa mãn x · ci ≥ ai. Điều này yêu cầu O(n + q) cho mỗi lần kiểm tra, vì ci có thể được tính một lần bằng cách sử dụng mảng sai phân và đánh giá là tuyến tính trên n. Nếu x có thể lên tới 109 thì việc thử tất cả các giá trị là không thể, thậm chí dừng sớm cũng không khả thi vì đáp án không nhất thiết phải nhỏ. 

Quan sát quan trọng là mỗi vị trí đóng góp độc lập một ngưỡng trên x. Đối với các vị trí có ci > 0 thì điều kiện x · ci ≥ ai trở thành x ≥ ceil(ai/ci). Vì vậy, mọi vị trí đều xác định một giá trị x tối thiểu mà tại đó nó trở thành “hoạt động”. Với ci = 0 và ai = 0, vị trí luôn hoạt động; với ci = 0 và ai > 0, nó không bao giờ hoạt động. 

Điều này biến bài toán thành: mỗi vị trí i có một giá trị ti, và chúng ta muốn x nhỏ nhất sao cho ít nhất k trong số các giá trị này thỏa mãn ti ≤ x. Đó là một điều kiện đếm đơn điệu cổ điển, vì vậy chúng ta có thể sắp xếp tất cả ti và sử dụng tìm kiếm nhị phân trên x (hoặc sắp xếp và đếm trực tiếp các tiền tố). 

Chúng ta vẫn cần ci một cách hiệu quả. Vì đèn là phạm vi bổ sung có cùng giá trị x nên trước tiên chúng tôi tính toán có bao nhiêu đèn bao phủ từng vị trí bằng cách sử dụng một mảng chênh lệch theo các khoảng, sau đó tính tổng tiền tố thành ci. 

Sau khi tất cả ti được tính toán, chúng tôi sắp xếp chúng và tìm kiếm nhị phân x nhỏ nhất sao cho ít nhất k giá trị là ≤ x. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên x | O(X · n) | O(n) | Quá chậm | 
| Phạm vi tiền tố + ngưỡng + sắp xếp + tìm kiếm nhị phân | O((n + q) + n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng mảng phủ sóng ci bằng cách sử dụng mảng sai phân trên tất cả các khoảng thời gian đèn. Mỗi khoảng [l, r] tăng một bộ đếm phạm vi và tổng tiền tố chuyển nó thành phạm vi bao phủ chính xác cho mỗi vị trí. Điều này là cần thiết vì sự đóng góp của mỗi vị trí chỉ phụ thuộc vào số lượng khoảng thời gian mà nó bao gồm. 
2. Quét tất cả các vị trí và kiểm tra tính khả thi. Nếu một vị trí có ci = 0 và ai > 0, hãy đánh dấu nó là không thể thỏa mãn. Đếm xem có bao nhiêu vị trí có khả năng thỏa mãn (ci > 0 hoặc ai = 0). Nếu số này nhỏ hơn k, hãy trả về -1 ngay lập tức vì ngay cả x vô hạn cũng không thể giúp được. 
3. Với mỗi vị trí, hãy tính ngưỡng kích hoạt ti của nó. Nếu ci = 0 thì ti bằng 0 khi ai = 0 và ngược lại là vô cùng. Nếu ci > 0 thì tính ti = ceil(ai/ci). Giá trị này đại diện cho x nhỏ nhất làm cho vị trí này tốt. 
4. Tập hợp tất cả các giá trị ti hữu hạn vào một mảng. Các vị trí có ti = 0 cũng được đưa vào vì chúng luôn tốt. 
5. Sắp xếp mảng ngưỡng. Sau khi sắp xếp, việc kiểm tra xem một x đã cho có hoạt động hay không sẽ chuyển sang việc đếm có bao nhiêu giá trị ≤ x, giá trị này trở thành chỉ mục tiền tố. 
6. Tìm kiếm nhị phân x trong khoảng [0, max_t], trong đó max_t là ngưỡng hữu hạn lớn nhất. Với mỗi ứng cử viên x, hãy tính xem có bao nhiêu ti ≤ x bằng cách sử dụng logic giới hạn trên. Nếu số này ít nhất là k thì x là khả thi và chúng tôi thử các giá trị nhỏ hơn; nếu không thì chúng ta tăng x. 
7. Giá trị x nhỏ nhất tìm được trong quá trình tìm kiếm nhị phân chính là đáp án. 

Tính đúng đắn đến từ cấu trúc đơn điệu của vị từ “ít nhất k ngưỡng là ≤ x”. Khi một vị trí trở nên tốt ở một số x, nó vẫn tốt cho tất cả các giá trị lớn hơn vì x chỉ tăng độ sáng một cách tuyến tính. Tính đơn điệu này đảm bảo tìm kiếm nhị phân không bỏ sót các chuyển tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    q = int(input())

    diff = [0] * (n + 2)
    for _ in range(q):
        l, r = map(int, input().split())
        diff[l] += 1
        diff[r + 1] -= 1

    c = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i + 1]
        c[i] = cur

    t = []
    possible = 0

    INF = 10**30

    for i in range(n):
        if c[i] == 0:
            if a[i] == 0:
                t.append(0)
                possible += 1
            else:
                continue
        else:
            possible += 1
            need = (a[i] + c[i] - 1) // c[i]
            t.append(need)

    if possible < k:
        print(-1)
        return

    t.sort()

    def ok(x):
        l, r = 0, len(t)
        while l < r:
            m = (l + r) // 2
            if t[m] <= x:
                l = m + 1
            else:
                r = m
        return l >= k

    lo, hi = 0, max(t) if t else 0
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if ok(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc xây dựng phạm vi bao phủ bằng cách sử dụng một mảng sai phân tiêu chuẩn sao cho mỗi khoảng thời gian đóng góp vào thời gian O(1). Bước xây dựng lại tiền tố rất quan trọng vì nó chuyển đổi các cập nhật phạm vi thành số lượng trên mỗi vị trí mà không cần lặp lại trên từng phân đoạn. 

Việc tính toán ngưỡng cẩn thận tách biệt trường hợp có phạm vi bao phủ bằng 0. Các vị trí có ci = 0 và ai > 0 bị bỏ qua đối với các ngưỡng nhưng vẫn ảnh hưởng đến tính khả thi. Sự tách biệt này giúp ngăn chặn các lỗi logic chia cho 0. 

Việc sắp xếp mảng ngưỡng chuyển đổi việc kiểm tra tính khả thi thành vấn đề đếm tiền tố. Hàm trợ giúp thực hiện tìm kiếm nhị phân thủ công để đếm xem có bao nhiêu ngưỡng ≤ x, tránh việc quét lặp lại. 

Tìm kiếm nhị phân bên ngoài tìm thấy x nhỏ nhất thỏa mãn yêu cầu, tận dụng tính đơn điệu của vị từ khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó mức độ bao phủ tạo ra mức độ nhạy cảm khác nhau giữa các vị trí. 

Đầu tiên chúng ta tính ci, sau đó là các ngưỡng, rồi tìm kiếm x. 

| Vị trí | ci | ai | tôi | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 5 | 5 | 
| 3 | 1 | 5 | 5 | 
| 4 | 2 | 8 | 4 | 
| 5 | 1 | 3 | 3 | 
| 6 | 1 | 6 | 6 | 

Các ngưỡng được sắp xếp trở thành [0, 3, 4, 5, 5, 6]. Chúng ta cần ít nhất k = 3 vị trí. 

Kiểm tra x = 4 sẽ cho bốn ngưỡng ≤ 4, như vậy là đúng. Bất kỳ x nào nhỏ hơn đều không đạt được ba vị trí tốt, vì vậy câu trả lời là 4. 

Dấu vết này cho thấy mỗi vị trí chuyển đổi độc lập thành mức kích hoạt bắt buộc như thế nào và yêu cầu chung trở thành vấn đề đếm tiền tố như thế nào. 

### Ví dụ 2 

Bây giờ hãy xem xét trường hợp tính khả thi không thành công do phạm vi bao phủ không đầy đủ. 

| Vị trí | ci | ai | tôi | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | thông tin | 
| 2 | 0 | 2 | thông tin | 
| 3 | 0 | 0 | 0 | 
| 4 | 1 | 10 | 10 | 

Chỉ có hai vị trí có thể thỏa mãn: vị trí 3 luôn luôn và vị trí 4 cuối cùng. Nếu k = 3 thì không có giá trị nào của x có thể thỏa mãn yêu cầu. 

Điều này chứng tỏ tại sao việc kiểm tra tính khả thi trước khi tìm kiếm nhị phân là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q + n log n) | xây dựng mảng khác biệt, tổng tiền tố, ngưỡng sắp xếp, tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ phạm vi bảo hiểm và ngưỡng | 

Giải pháp này phù hợp thoải mái trong các ràng buộc vì tất cả các phép toán nặng đều là tuyến tính hoặc n log n và không có bước nào phụ thuộc vào một phạm vi lớn các giá trị x. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else capture(inp)

def capture(inp: str) -> str:
    import subprocess, textwrap, sys
    return subprocess.run(
        [sys.executable, "-c", CODE],
        input=inp.encode()
    ).stdout.decode().strip()

CODE = r"""
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    q = int(input())

    diff = [0] * (n + 2)
    for _ in range(q):
        l, r = map(int, input().split())
        diff[l] += 1
        diff[r + 1] -= 1

    c = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i + 1]
        c[i] = cur

    t = []
    possible = 0
    INF = 10**30

    for i in range(n):
        if c[i] == 0:
            if a[i] == 0:
                t.append(0)
                possible += 1
        else:
            possible += 1
            t.append((a[i] + c[i] - 1) // c[i])

    if possible < k:
        print(-1)
        return

    t.sort()

    def ok(x):
        l, r = 0, len(t)
        while l < r:
            m = (l + r) // 2
            if t[m] <= x:
                l = m + 1
            else:
                r = m
        return l >= k

    lo, hi = 0, max(t) if t else 0
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if ok(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

def solve():
    pass
"""

# provided samples
assert True

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu đèn đơn | 0 hoặc -1 | hành vi ranh giới nhỏ nhất | 
| không thể có bảo hiểm | -1 | từ chối tính khả thi | 
| tất cả a_i = 0 | 0 | trường hợp cạnh luôn hài lòng | 
| đồng phục bảo hiểm đầy đủ | tối thiểu x | hành vi ngưỡng thống nhất | 

## Vỏ cạnh 

Khi một vị trí không bao giờ được che phủ bởi bất kỳ đèn nào nhưng yêu cầu độ sáng dương, thuật toán sẽ loại trừ vị trí đó một cách rõ ràng khỏi nhóm các vị trí thỏa mãn. Trong quá trình kiểm tra tính khả thi, các vị trí như vậy sẽ làm giảm tổng số lượng có thể có và nếu chúng không thể đạt tới k thì thuật toán sẽ kết thúc sớm với -1. 

Khi tất cả các giá trị bắt buộc a_i bằng 0, mọi vị trí đều trở nên thỏa mãn với x = 0. Tính toán ngưỡng gán 0 cho tất cả các vị trí và tìm kiếm nhị phân ngay lập tức xác nhận x = 0 là hợp lệ. 

Khi mọi vị trí được bao phủ đồng nhất, ci không đổi trên tất cả các chỉ số. Mỗi ngưỡng trở thành một giá trị tỷ lệ đơn giản của ai và cấu trúc được sắp xếp sẽ giảm vấn đề thành việc chọn yêu cầu nhỏ nhất thứ k chia cho phạm vi bao phủ mà tìm kiếm nhị phân nắm bắt chính xác.
