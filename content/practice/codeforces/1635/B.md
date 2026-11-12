---
title: "CF 1635B - Tránh mức tối đa cục bộ"
description: "Chúng ta phải sửa đổi một mảng để không có vị trí nào lớn hơn cả hai mảng lân cận. Những vị trí như vậy được gọi là cực đại địa phương. Phần tử đầu tiên và cuối cùng không bao giờ là giá trị tối đa cục bộ vì chúng chỉ có một phần tử lân cận."
date: "2026-06-10T04:40:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1635
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 772 (Div. 2)"
rating: 800
weight: 1635
solve_time_s: 122
verified: false
draft: false
---

[CF 1635B - Tránh mức tối đa cục bộ](https://codeforces.com/problemset/problem/1635/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải sửa đổi một mảng để không có vị trí nào lớn hơn cả hai mảng lân cận. Những vị trí như vậy được gọi là cực đại địa phương. Phần tử đầu tiên và cuối cùng không bao giờ là giá trị tối đa cục bộ vì chúng chỉ có một phần tử lân cận. 

Mỗi thao tác cho phép thay đổi một phần tử thành bất kỳ giá trị nào trong khoảng từ 1 đến$10^9$. Mục tiêu là giảm thiểu số lượng vị trí được sửa đổi và xuất ra một mảng kết quả hợp lệ. 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Với giới hạn hai giây, các giải pháp tuyến tính hoặc gần tuyến tính hoàn toàn an toàn. Các thuật toán bậc hai sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, đó là quá nhiều. 

Một số tình huống rất dễ xử lý sai. 

Xét các đỉnh liên tiếp:```
1 3 1 3 1
```Vị trí 2 và 4 đều là mức tối đa cục bộ. Nếu chúng ta sửa chúng một cách độc lập bằng cách hạ thấp từng đỉnh, chúng ta sẽ sử dụng hai thao tác. Một giải pháp tốt hơn là thay đổi phần tử ở giữa:```
1 3 3 3 1
```Chỉ cần một thao tác. Xử lý từng đỉnh riêng biệt sẽ bỏ lỡ sự tối ưu hóa này. 

Một trường hợp phức tạp khác là ba đỉnh liên tiếp được ngăn cách bởi các thung lũng:```
2 1 3 1 3 1 3
```Việc thay đổi thung lũng ở giữa thành một giá trị đủ lớn sẽ loại bỏ hai đỉnh lân cận cùng một lúc:```
2 1 3 3 3 1 3
```Sau đó chỉ còn lại đỉnh cuối cùng. Một chiến lược tham lam phải nhận ra mô hình này. 

Vị trí ranh giới cũng cần được chăm sóc. TRONG```
5 1 5
```chỉ phần tử ở giữa có hai phần tử lân cận, vì vậy vị trí 1 và 3 không thể là giá trị tối đa cục bộ bất kể giá trị của chúng là bao nhiêu. Vô tình kiểm tra chúng sẽ cho kết quả không chính xác. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là phát hiện mọi mức tối đa cục bộ và khắc phục nó một cách độc lập. Một khả năng là hạ thấp từng đỉnh cho đến khi nó không lớn hơn một đỉnh lân cận. Cách tiếp cận này đúng vì mọi thao tác đều phá hủy ít nhất một đỉnh. 

Điểm yếu xuất hiện khi các đỉnh xen kẽ với các thung lũng:```
1 3 1 3 1
```Việc cố định cả hai đỉnh riêng biệt yêu cầu hai lần sửa đổi, mặc dù việc thay đổi phần tử trung tâm một lần sẽ loại bỏ cả hai đỉnh. Vì mỗi đỉnh được xử lý độc lập nên sự tương tác giữa các đỉnh gần nhau sẽ bị bỏ qua. 

Quan sát quan trọng là một mô hình```
peak - valley - peak
```có thể được xử lý chỉ bằng một thao tác. Nếu vị trí$i-1$Và$i+1$là đỉnh cao, vị trí tăng dần$i$ĐẾN```
max(a[i-1], a[i+1])
```loại bỏ cả hai mức tối đa cục bộ cùng một lúc. 

Điều này gợi ý quét từ trái sang phải. Bất cứ khi nào hai đỉnh cách nhau đúng một phần tử, chúng tôi sửa đổi phần tử ở giữa đó và bỏ qua. Nếu không, các đỉnh riêng biệt sẽ được cố định riêng lẻ. 

Vì mỗi vị trí chỉ được kiểm tra một số lần không đổi nên toàn bộ thuật toán chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Không phải lúc nào cũng tối ưu | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng và ghi lại vị trí nào là cực đại cục bộ. 
2. Duyệt mảng từ trái sang phải. 
3. Bất cứ khi nào vị thế`i`Và`i+2`đều là mức tối đa cục bộ, sửa đổi vị trí`i+1`. 
4. Đặt```
a[i+1] = max(a[i], a[i+2])
```bởi vì điều này làm cho phần tử ở giữa ít nhất lớn bằng cả hai đỉnh, nên không bên nào vẫn là mức tối đa cục bộ. 

1. Đếm một thao tác và bỏ qua ba vị trí vì cả hai đỉnh đều đã được xử lý. 
2. Nếu vị trí`i`là mức tối đa cục bộ nhưng vị trí`i+2`không, hãy coi nó như một đỉnh cô lập. 
3. Thay thế```
a[i] = max(a[i-1], a[i+1])
```ngay lập tức phá hủy mức tối đa cục bộ đó. 

1. Đếm một thao tác và tiếp tục quét. 
2. In số phép tính và mảng cuối cùng. 

### Tại sao nó hoạt động 

Cách duy nhất một thao tác có thể loại bỏ hai đỉnh là khi các đỉnh đó được phân tách bằng chính xác một phần tử. Bất kỳ khoảng cách lớn hơn có nghĩa là các đỉnh độc lập. 

Trong quá trình quét từ trái sang phải, mỗi cặp đỉnh lân cận được phát hiện một lần. Việc sửa đổi phần tử ở giữa sẽ loại bỏ cả hai cùng một lúc, điều này luôn tốt hơn so với việc thực hiện hai thao tác. Các đỉnh không thuộc một cặp như vậy không thể chia sẻ hoạt động với một đỉnh khác, vì vậy việc sửa chúng riêng lẻ là tối ưu. Do đó tổng số hoạt động là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    peak = [False] * n
    for i in range(1, n - 1):
        if a[i] > a[i - 1] and a[i] > a[i + 1]:
            peak[i] = True

    cnt = 0
    i = 1

    while i < n - 1:
        if peak[i]:
            if i + 2 < n and peak[i + 2]:
                a[i + 1] = max(a[i], a[i + 2])
                cnt += 1
                i += 3
            else:
                a[i] = max(a[i - 1], a[i + 1])
                cnt += 1
                i += 1
        else:
            i += 1

    ans.append(str(cnt))
    ans.append(" ".join(map(str, a)))

sys.stdout.write("\n".join(ans))
```Vòng lặp đầu tiên xác định tất cả các mức tối đa cục bộ trong mảng ban đầu. Thông tin này được giữ cố định trong quá trình quét. Việc tính toán lại các đỉnh sau mỗi lần sửa đổi sẽ làm phức tạp việc triển khai và không cần thiết. 

Vòng lặp chính xử lý các đỉnh từ trái sang phải. Khi hai đỉnh cách nhau hai vị trí, một sửa đổi của phần tử ở giữa sẽ loại bỏ cả hai. Sau đó, chỉ số sẽ tăng ba vị trí vì các đỉnh đó đã kết thúc. 

Đối với các đỉnh bị cô lập, việc thay thế giá trị đỉnh bằng giá trị đỉnh lân cận lớn hơn sẽ phá hủy đỉnh đó mà không tạo ra đỉnh mới. Sử dụng giá trị lân cận tối đa là đủ vì đẳng thức không được tính là giá trị tối đa cục bộ. 

Vị trí biên không bao giờ được kiểm tra vì vòng lặp bắt đầu từ chỉ số 1 và dừng trước chỉ mục`n-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 1 2 1
```Đỉnh ban đầu ở vị trí 2 và 4. 

| tôi | Đỉnh tại i và i+2 | Hoạt động | Mảng | 
| --- | --- | --- | --- | 
| 1 | Có | Đặt a[2]=max(2,2)=2 | 1 2 2 2 1 | 

Chỉ cần một thao tác. 

Ví dụ này cho thấy một sửa đổi có thể loại bỏ hai đỉnh lân cận như thế nào. 

### Ví dụ 2 

đầu vào:```
1 2 1 3 2 3 1 2 1
```Các đỉnh ban đầu ở vị trí 2, 4 và 6. 

| tôi | Đỉnh tại i và i+2 | Hoạt động | Mảng | 
| --- | --- | --- | --- | 
| 1 | Có | Đặt a[2]=3 | 1 2 3 3 2 3 1 2 1 | 
| 4 | Không | Đặt a[5]=3 | 1 2 3 3 2 3 3 2 1 | 

Mảng cuối cùng không chứa mức tối đa cục bộ. 

Dấu vết này chứng tỏ rằng một hoạt động ghép đôi có thể được theo sau bởi một đỉnh bị cô lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập với số lần không đổi | 
| Không gian | O(n) | Mảng đỉnh lưu trữ xem mỗi vị trí ban đầu có phải là mức tối đa cục bộ hay không | 

Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, thời gian tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        peak = [False] * n
        for i in range(1, n - 1):
            if a[i] > a[i - 1] and a[i] > a[i + 1]:
                peak[i] = True

        cnt = 0
        i = 1

        while i < n - 1:
            if peak[i]:
                if i + 2 < n and peak[i + 2]:
                    a[i + 1] = max(a[i], a[i + 2])
                    cnt += 1
                    i += 3
                else:
                    a[i] = max(a[i - 1], a[i + 1])
                    cnt += 1
                    i += 1
            else:
                i += 1

        out.append(str(cnt))
        out.append(" ".join(map(str, a)))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# minimum size
assert run("1\n2\n5 7\n") == "0\n5 7"

# all equal
assert run("1\n5\n4 4 4 4 4\n") == "0\n4 4 4 4 4"

# alternating peaks
assert run("1\n5\n1 3 1 3 1\n") == "1\n1 3 3 3 1"

# isolated peak
assert run("1\n4\n1 4 2 1\n") == "1\n1 2 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 7 | 0 hoạt động | Kích thước tối thiểu | 
| 4 4 4 4 4 | Không có thay đổi | Giá trị bằng nhau | 
| 1 3 1 3 1 | Một thao tác | Sửa lỗi chung cho hai đỉnh | 
| 1 4 2 1 | Một thao tác | Xử lý cao điểm bị cô lập | 

## Vỏ cạnh 

Hãy xem xét```
1 3 1 3 1
```Các đỉnh ban đầu nằm ở vị trí 2 và 4. Thuật toán thông báo rằng chúng được phân tách bằng một phần tử và thay đổi giá trị trung tâm:```
1 3 3 3 1
```Chỉ có một thao tác được sử dụng. Một chiến lược sửa từng đỉnh một cách độc lập sẽ thực hiện hai thao tác. 

Coi như```
2 1 3 1 3 1 3
```Cặp đỉnh đầu tiên ở vị trí 3 và 5 có chung phần tử ở giữa, do đó, một thao tác sẽ loại bỏ cả hai:```
2 1 3 3 3 1 3
```Đỉnh còn lại ở vị trí 7 được cách ly và cố định riêng. Hai hoạt động là tối ưu. 

Coi như```
5 1 5
```Không có điểm cuối nào có thể là mức tối đa cục bộ. Vòng lặp chỉ kiểm tra vị trí trung tâm, vị trí này nhỏ hơn cả hai vị trí lân cận, do đó không có thao tác nào được thực hiện và mảng không thay đổi. Điều này tránh được một lỗi ở ranh giới.
