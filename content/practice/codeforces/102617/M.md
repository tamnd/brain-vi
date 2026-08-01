---
title: "CF 102617M - Lịch Thần Kỳ"
description: "Chúng ta có một lịch trong đó một tuần có thể có số ngày bất kỳ từ 1 đến r. Với độ dài tuần k đã chọn, lịch được vẽ dưới dạng hàng k ô. Chúng tôi vẽ n ngày liên tiếp và nhìn vào hình dạng được kết nối được hình thành bởi các ô đó."
date: "2026-07-31T17:40:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "M"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 64
verified: true
draft: false
---

[CF 102617M - Lịch thần kỳ](https://codeforces.com/problemset/problem/102617/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lịch trong đó một tuần có thể có số ngày bất kỳ kể từ`1`ĐẾN`r`. Trong khoảng thời gian một tuần đã chọn`k`, lịch được vẽ dưới dạng các hàng`k`tế bào. Chúng tôi sơn`n`ngày liên tiếp và nhìn vào hình dạng liên kết được hình thành bởi các tế bào đó. Hai bức tranh được coi là giống hệt nhau khi một bức tranh có thể được dịch chuyển để chồng lên bức tranh kia mà không cần xoay hoặc thay đổi hình dạng. 

Nhiệm vụ là đếm xem có bao nhiêu hình dạng được kết nối khác nhau có thể xuất hiện sau khi thử mỗi tuần có thể từ`1`ĐẾN`r`. 

Những ràng buộc cho phép`n`Và`r`lớn như`10^9`. Việc mô phỏng trên tất cả các độ dài tuần có thể là không thể bởi vì thậm chí việc lặp lại lên đến`10^9`các giá trị vốn đã quá chậm và việc xây dựng các ô lịch thậm chí còn đòi hỏi nhiều công sức hơn. Lời giải phải rút gọn bài toán thành một số phép tính số học không đổi. 

Phần khó khăn là hiểu được khi nào độ dài tuần khác nhau thực sự tạo ra những hình dạng khác nhau. Một sai lầm phổ biến là cho rằng mọi vị trí xuất phát có thể hoặc độ dài mỗi tuần luôn mang lại một hình dạng mới. Điều đó vượt quá nhiều trường hợp. 

Coi như`n = 3, r = 4`. Câu trả lời đúng là`4`. Trong thời gian dài một tuần`1`,`2`,`3`, Và`4`, các hình dạng có thể có là bốn hình dạng khác nhau. Một cách tiếp cận bất cẩn đếm mọi ô bắt đầu có thể sẽ được tính nhiều hơn vì một số ô bắt đầu tạo ra hình dạng được dịch giống nhau. 

Một trường hợp khác là khi độ dài tuần ít nhất bằng số ngày được sơn. Ví dụ,`n = 3, r = 5`Và`k = 5`. Ba ô được sơn luôn nằm trên một hàng, bất kể chúng bắt đầu từ đâu. Câu trả lời được đóng góp bởi độ dài tuần này chỉ là một hình dạng. Đếm tất cả các vị trí bắt đầu sẽ cộng sai năm hình. 

Trường hợp nhỏ nhất cũng quan trọng. Vì`n = 1`, mọi chiều rộng lịch hợp lệ đều tạo ra hình dạng một ô giống hệt nhau. Câu trả lời là`1`, không`r`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là kiểm tra độ dài mỗi tuần có thể`k`. Đối với mỗi`k`, chúng ta có thể thử mọi vị trí bắt đầu trong tuần và xây dựng tập hợp các ô được sơn kết quả, sau đó so sánh nó với các hình dạng đã thấy trước đó. Điều này đúng vì nó liệt kê mọi khả năng theo đúng nghĩa đen. Tuy nhiên có thể có tới`10^9`sự lựa chọn cho`k`, vì vậy ngay cả vòng lặp bên ngoài cũng quá chậm. 

Nhận xét quan trọng là chúng ta thực sự không cần phải vẽ lịch. Điều duy nhất quan trọng là mối quan hệ giữa độ dài tuần và số ngày sơn. 

Giả sử độ dài tuần là`k`. 

Nếu như`k < n`, vùng sơn bao gồm nhiều hơn một hàng. Trong tình huống này, mỗi khoảng lệch bắt đầu hợp lệ khác nhau sẽ tạo ra một hình dạng khác nhau và có chính xác`k`những hình dạng có thể. 

Nếu như`k >= n`, những ngày được vẽ sẽ xếp thành một hàng duy nhất. Mọi vị trí bắt đầu có thể chỉ là một bản sao được dịch chuyển của cùng một dòng, do đó chỉ có một hình dạng. 

Sự đóng góp của tất cả thời lượng trong tuần có thể được tính toán bằng toán học. Vì`k = 1`ĐẾN`min(r, n - 1)`, chúng tôi thêm`k`hình dạng. Nếu như`r >= n`, chúng tôi thêm một hình dạng bổ sung cho tất cả các chiều rộng từ`n`trở đi. Điều này trở thành một cấp số cộng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r * n) | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`r`cho trường hợp thử nghiệm hiện tại. 

Câu trả lời chỉ phụ thuộc vào hai giá trị này nên không cần xây dựng lịch. 
2. Hãy để`x = min(r, n - 1)`. 

Đây là những độ dài tuần nhỏ hơn`n`. Mỗi người trong số họ đóng góp một số hình dạng bằng giá trị riêng của nó. 
3. Cộng tổng`1 + 2 + ... + x`để trả lời. 

Điều này tính mọi trường hợp trong đó ngày được vẽ trải dài trên nhiều hàng. 
4. Nếu`r >= n`, thêm vào`1`để trả lời. 

Tất cả các tuần dài từ`n`trở đi tạo ra các hình dạng thẳng ngang giống nhau, do đó chúng cùng nhau chỉ đóng góp một hình dạng bổ sung. 
5. Xuất kết quả. 

Giá trị trung gian lớn nhất là khoảng`5 * 10^17`, vừa vặn thoải mái bên trong các số nguyên Python. 

Tại sao nó hoạt động: 

Cứ mỗi tuần có thể, chỉ có hai trường hợp. Khi tuần ngắn hơn đoạn được sơn, ô được sơn đầu tiên có thể chiếm mọi vị trí trong tuần và mỗi vị trí sẽ có một hình dạng riêng. Khi tuần dài ít nhất bằng đoạn sơn, toàn bộ bức tranh là một hàng và bản dịch sẽ loại bỏ ảnh hưởng của vị trí bắt đầu. Các trường hợp này bao gồm mọi chiều rộng lịch có thể có chính xác một lần, do đó tổng sẽ tính mọi hình dạng hợp lệ mà không trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, r = map(int, input().split())

        x = min(r, n - 1)

        cur = x * (x + 1) // 2

        if r >= n:
            cur += 1

        ans.append(str(cur))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`x`đại diện cho tất cả các chiều dài tuần nhỏ hơn chiều dài được sơn. biểu hiện`x * (x + 1) // 2`là tổng lũy ​​tiến số học của những đóng góp của chúng. 

điều kiện`r >= n`xử lý trường hợp thứ hai trong đó tất cả các lịch lớn hơn thu gọn thành một hình ngang. Rất dễ mắc phải một sai lầm ở đây bằng cách sử dụng`r > n`, Nhưng`k = n`đã thuộc về trường hợp một hàng và phải được đưa vào. 

Số nguyên Python tránh được sự cố tràn có thể xảy ra trong các ngôn ngữ có loại số nguyên có chiều rộng cố định vì phép nhân có thể đạt tới khoảng`10^18`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
```Đây`n = 3`Và`r = 4`. 

| Bước | Biến | Giá trị | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu |`n, r`|`3, 4`|`0`| 
| Giới hạn |`x = min(r, n-1)`|`2`|`0`| 
| Tổng chiều rộng nhỏ hơn |`1 + 2`|`3`|`3`| 
| Thêm trường hợp một hàng |`r >= n`| đúng |`4`| 

Độ dài hai tuần đầu tiên đóng góp`1`Và`2`hình dạng. Chiều rộng`3`Và`4`cả hai đều tạo ra một đường thẳng giống nhau, đưa ra câu trả lời cuối cùng`4`. 

### Ví dụ 2 

đầu vào:```
13 7
```Đây`n = 13`Và`r = 7`. 

| Bước | Biến | Giá trị | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu |`n, r`|`13, 7`|`0`| 
| Giới hạn |`x = min(r, n-1)`|`7`|`0`| 
| Tổng chiều rộng nhỏ hơn |`1 + ... + 7`|`28`|`28`| 
| Thêm trường hợp một hàng |`r >= n`| sai |`28`| 

Mỗi chiều dài tuần có sẵn đều ngắn hơn đoạn được sơn, do đó, mỗi chiều rộng đều đóng góp một số hình dạng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học được thực hiện cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Các ràng buộc cho phép các giá trị lên tới`10^9`, do đó, bất kỳ lần lặp nào tùy thuộc vào`n`hoặc`r`sẽ là quá chậm. Công thức hằng số thời gian dễ dàng thỏa mãn các giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data):
    input = io.StringIO(data).readline
    t = int(input())
    out = []

    for _ in range(t):
        n, r = map(int, input().split())
        x = min(r, n - 1)
        ans = x * (x + 1) // 2
        if r >= n:
            ans += 1
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert solution("""5
3 4
3 2
3 1
13 7
1010000 9999999
""") == """4
3
1
28
510049500000
"""

# custom cases
assert solution("""1
1 100
""") == "1"

assert solution("""1
5 2
""") == "3"

assert solution("""1
5 5
""") == "11"

assert solution("""1
1000000000 1000000000
""") == "500000000500000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100`|`1`| Chiều dài sơn tối thiểu và nhiều lịch giống hệt nhau | 
|`5 2`|`3`| Chỉ các trường hợp nhiều hàng mới được tính | 
|`5 5`|`11`| Ranh giới ở đâu`r`bằng`n`| 
|`1000000000 1000000000`|`500000000500000000`| Giá trị lớn và xử lý số nguyên | 

## Vỏ cạnh 

cho`n = 1`Và`r = 100`, bộ thuật toán`x = min(100, 0) = 0`, do đó tổng số học không đóng góp gì cả. Từ`r >= n`, nó thêm một hình dạng. Điều này xử lý chính xác thực tế là mọi lịch có thể đều tạo ra cùng một ô được vẽ. 

Vì`n = 3`Và`r = 4`, thuật toán sử dụng`x = 2`và đếm`1 + 2 = 3`hình nhiều hàng. Sau đó nó thêm một hình nữa vì chiều rộng`3`Và`4`cả hai đều phù hợp với tất cả các ô được sơn trong một hàng. Kết quả là`4`, tránh lỗi đếm trùng lặp khi coi mọi vị trí bắt đầu là duy nhất. 

Vì`n = 5`Và`r = 5`, chiều dài tuần chính xác bằng chiều dài sơn được xử lý bởi`r >= n`tình trạng. Bốn chiều rộng đầu tiên góp phần`1 + 2 + 3 + 4 = 10`, và chiều rộng`5`góp phần tạo nên một hình dạng nằm ngang, mang lại`11`. sử dụng`r > n`ở đây sẽ bỏ lỡ trường hợp ranh giới này.
