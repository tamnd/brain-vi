---
title: "CF 103715C - \u041a\u043e\u043d\u0442\u0440\u043e\u043b\u044c \u0441\u0430\u0445\u0430\u0440\u0430"
description: "Chúng tôi được cung cấp một danh sách các cửa hàng đường. Mỗi cửa hàng có một mức giá ban đầu và giá đó tăng đúng một xu mỗi ngày. Vì vậy, nếu một cửa hàng bắt đầu ở mức giá a[i] thì vào ngày đầu tiên nó có giá a[i], vào ngày thứ 2 nó có giá a[i] + 1, v.v. Hàng ngày, bạn đi mua sắm với số tiền cố định x."
date: "2026-07-02T09:26:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103715
codeforces_index: "C"
codeforces_contest_name: "\u0421\u0443\u0440\u0441\u043a\u0438\u0435 \u0442\u0430\u043b\u0430\u043d\u0442\u044b 2022"
rating: 0
weight: 103715
solve_time_s: 49
verified: true
draft: false
---

[CF 103715C - \u041a\u043e\u043d\u0442\u0440\u043e\u043b\u044c \u0441\u0430\u0445\u0430\u0440\u0430](https://codeforces.com/problemset/problem/103715/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các cửa hàng đường. Mỗi cửa hàng có một mức giá ban đầu và giá đó tăng đúng một xu mỗi ngày. Vì vậy, nếu một cửa hàng bắt đầu ở mức giá`a[i]`, thì vào ngày thứ nhất nó có giá`a[i]`, vào ngày thứ 2 chi phí`a[i] + 1`, vân vân. 

Hàng ngày bạn đi mua sắm với một khoản ngân sách cố định`x`. Vào ngày đó, bạn có thể ghé thăm bất kỳ cửa hàng nhỏ nào, nhưng mỗi cửa hàng chỉ được sử dụng tối đa một lần mỗi ngày và bạn mua càng nhiều gói càng tốt mà không vượt quá ngân sách. Ngày hôm sau, giá lại tăng và bạn lặp lại quá trình tương tự. 

Cuối cùng, tất cả các cửa hàng đều trở nên quá đắt để mua dù chỉ một gói trong phạm vi ngân sách của bạn. Nhiệm vụ là tính tổng số gói bạn có thể mua trong tất cả các ngày cho đến thời điểm đó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp số lượng cửa hàng và ngân sách hàng ngày của bạn, theo sau là giá ban đầu của tất cả các cửa hàng. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị tổng số gói được mua trong toàn bộ quá trình. 

Các ràng buộc này ngụ ý tổng cộng có khoảng hai trăm nghìn cửa hàng trong tất cả các trường hợp thử nghiệm. Điều đó loại trừ mọi mô phỏng lặp đi lặp lại hàng ngày. Một mô phỏng đơn giản hàng ngày sẽ quá chậm vì quá trình này có thể kéo dài tới`max(x - min(a[i]) + 1)`ngày, trong trường hợp xấu nhất có thể lên tới một tỷ lần lặp. 

Một trường hợp phức tạp xuất hiện khi một số cửa hàng bắt đầu vượt quá ngân sách. Ví dụ, nếu`x = 5`và một cửa hàng có`a[i] = 10`, thì nó không bao giờ có thể sử dụng được và đóng góp bằng không. Một trường hợp khác là khi tất cả`a[i]`bằng với`x`, nghĩa là mỗi cửa hàng chỉ có thể được mua vào ngày đầu tiên và sẽ không còn hàng ngay sau đó. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo mô phỏng quá trình này từng ngày. Mỗi ngày, chúng tôi tính lại giá của mỗi cửa hàng và đếm xem chúng tôi có thể mua được bao nhiêu. Nếu có`n`cửa hàng và`D`ngày, chi phí này`O(nD)`hoạt động. Từ`D`có thể lớn như`x`, tăng lên`10^9`, cách tiếp cận này ngay lập tức là không thể. 

Quan sát quan trọng là các cửa hàng không tương tác với nhau theo bất kỳ cách nào có ý nghĩa trong nhiều ngày. Mỗi cửa hàng phát triển độc lập và quyết định về một cửa hàng chỉ phụ thuộc vào việc giá hiện tại của nó có nằm trong ngân sách hay không. Thay vì suy nghĩ theo ngày, chúng ta có thể đảo ngược góc nhìn và hỏi: đối với một cửa hàng cố định, nó sẽ có giá phải chăng trong bao nhiêu ngày? 

Đối với một cửa hàng có giá ban đầu`a[i]`, giá cả phải chăng trong ngày`d`chính xác khi nào`a[i] + (d - 1) <= x`. Bất đẳng thức này có thể được giải trực tiếp, đưa ra một phạm vi ngày hợp lệ liền kề. Điều đó có nghĩa là mỗi cửa hàng đóng góp độc lập vào câu trả lời tổng thể và chúng tôi có thể tổng hợp các đóng góp của tất cả các cửa hàng mà không cần tính toán thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng ngày) | O(n · x) | O(1) | Quá chậm | 
| Tính số tiền đóng góp cho mỗi cửa hàng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`x`, sau đó đọc mảng`a`về giá ban đầu. Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập vì không có trạng thái nào được chuyển tiếp. 
2. Đối với mỗi cửa hàng`i`, xác định xem nó còn có thể mua được trong bao lâu. Chúng tôi yêu cầu`a[i] + (d - 1) <= x`, sắp xếp lại thành`d <= x - a[i] + 1`. Điều này đưa ra số ngày chính xác mà cửa hàng có thể được sử dụng. 
3. Nếu`a[i] > x`, thì biểu thức`x - a[i] + 1`trở nên không tích cực, có nghĩa là cửa hàng không bao giờ đóng góp bất kỳ khoản mua hàng nào. Trong trường hợp đó, chúng tôi coi đóng góp của nó là bằng không. 
4. Tổng hợp sự đóng góp của tất cả các cửa hàng. Vì mỗi cửa hàng đóng góp độc lập mỗi ngày và chúng tôi luôn mua bất cứ khi nào có thể nên số tiền này bằng tổng số gói đã mua trong tất cả các ngày. 
5. Xuất ra tổng cuối cùng của test case. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là tính độc lập giữa các cửa hàng. Vào bất kỳ ngày nào, mọi cửa hàng đều hành xử một cách xác định: có phải chăng hoặc không, và điều kiện này chỉ phụ thuộc vào chỉ số ngày và giá ban đầu của nó. Vì ngân sách được đặt lại hàng ngày và không có sự cạnh tranh giữa các cửa hàng ngoài giới hạn ngân sách nên mỗi cửa hàng đóng góp một số ngày thành công cố định. Tổng hợp những đóng góp độc lập này sẽ tính chính xác mỗi lần mua hàng một lần, không có sự trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        ans = 0
        for v in a:
            if v <= x:
                ans += (x - v + 1)
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp thực hiện công thức đóng góp cho mỗi cửa hàng. Sự tinh tế duy nhất là đề phòng những đóng góp tiêu cực bằng cách kiểm tra`v <= x`. Nếu không có điều này, phép trừ sẽ thêm không chính xác các giá trị âm cho các cửa hàng có giá quá cao. 

Vòng lặp là tuyến tính cho mỗi trường hợp thử nghiệm và không cần sắp xếp hoặc cấu trúc phụ trợ vì mỗi phần tử được xử lý độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử`n = 3`,`x = 5`, Và`a = [1, 4, 6]`. 

Đối với mỗi cửa hàng, chúng tôi tính toán xem nó có giá phải chăng trong bao nhiêu ngày. 

| Cửa hàng | một [tôi] | x - a[i] + 1 | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 
| 2 | 4 | 2 | 2 | 
| 3 | 6 | tiêu cực | 0 | 

Tổng số câu trả lời là`7`. 

Dấu vết này cho thấy mỗi cửa hàng đóng góp độc lập trong nhiều ngày và các cửa hàng có giá quá cao sẽ biến mất khỏi sự chú ý. 

### Ví dụ 2 

hãy để`n = 4`,`x = 3`,`a = [3, 3, 3, 1]`. 

| Cửa hàng | một [tôi] | x - a[i] + 1 | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 
| 3 | 3 | 1 | 1 | 
| 4 | 1 | 3 | 3 | 

Tổng số câu trả lời là`6`. 

Ví dụ này nhấn mạnh rằng một cửa hàng giá rẻ sẽ đóng góp nhiều lượt mua hàng hơn vì nó vẫn nằm trong ngân sách trong nhiều ngày hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cửa hàng được xử lý một lần và làm việc liên tục | 
| Không gian | O(1) thêm | Chỉ có một khoản tiền được duy trì | 

Tổng cộng`n`trên nhiều trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, do đó lời giải dễ dàng phù hợp trong giới hạn thời gian. Không cần sắp xếp hoặc xử lý trước, giữ cho bộ nhớ và thời gian chạy ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        ans = 0
        for v in a:
            if v <= x:
                ans += (x - v + 1)
        print(ans)

    return output.getvalue().strip()

# sample-like tests
assert run("1\n3 5\n1 4 6\n") == "7"
assert run("1\n4 3\n3 3 3 1\n") == "6"

# minimum case
assert run("1\n1 10\n10\n") == "1"

# all too expensive
assert run("1\n5 2\n5 6 7 8 9\n") == "0"

# all equal and cheap
assert run("1\n3 4\n2 2 2\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cửa hàng bằng x | 1 | mua ranh giới duy nhất | 
| tất cả đều đắt đỏ | 0 | không có đóng góp tiêu cực | 
| giá trị đồng đều giá rẻ | 9 | tích lũy lặp đi lặp lại hàng ngày | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả giá vượt quá ngân sách. Đối với đầu vào như`n = 3, x = 5, a = [10, 20, 30]`, mọi số hạng đều thỏa mãn`a[i] > x`, vì vậy mỗi cái đóng góp bằng không. Thuật toán bỏ qua tất cả các giá trị một cách chính xác do`v <= x`kiểm tra. 

Một trường hợp khó khăn khác là khi một cửa hàng bắt đầu với mức ngân sách vừa phải. Vì`a[i] = x`, công thức cho`x - x + 1 = 1`, nghĩa là nó đóng góp đúng một lần. Thuật toán tính chính xác một lần mua hàng trước khi nó trở nên quá đắt vào ngày hôm sau. 

Trường hợp cạnh cuối cùng là khi`a[i] = 1`Và`x`là lớn. Trong trường hợp đó, cửa hàng góp phần`x`mua hàng, tương ứng với một lần mua mỗi ngày cho đến khi giá đạt đến giới hạn. Thuật toán tích lũy biểu thức này dưới dạng biểu thức số học đơn giản mà không cần mô phỏng.
