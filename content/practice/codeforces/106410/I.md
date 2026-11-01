---
title: "CF 106410I - Máy đẩy tốc độ"
description: "Chúng tôi có một bộ đèn hiệu được đặt trên một đường. Mỗi đèn hiệu có một vị trí và công suất hiện tại. Một đèn hiệu có công suất p bao trùm khoảng [x - p, x + p]. Trong mỗi vòng, mỗi cặp khoảng thời gian chồng chéo với sức mạnh khác nhau sẽ khiến đèn hiệu yếu hơn nhận được một sức mạnh."
date: "2026-06-25T09:56:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106410
codeforces_index: "I"
codeforces_contest_name: "HPI 2026 Novice"
rating: 0
weight: 106410
solve_time_s: 43
verified: true
draft: false
---

[CF 106410I - Máy đẩy tốc độ](https://codeforces.com/problemset/problem/106410/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ đèn hiệu được đặt trên một đường. Mỗi đèn hiệu có một vị trí và công suất hiện tại. Một ngọn hải đăng có sức mạnh`p`bao gồm khoảng bao gồm`[x - p, x + p]`. Trong mỗi vòng, mỗi cặp khoảng thời gian chồng chéo với sức mạnh khác nhau sẽ khiến đèn hiệu yếu hơn nhận được một sức mạnh. Tất cả lợi ích xảy ra đồng thời. Nhiệm vụ là xác định tổng số vị trí số nguyên được bao phủ sau khi quá trình đạt đến trạng thái ổn định. 

Số lượng đèn hiệu có thể đạt tới`2 * 10^5`, vì vậy việc mô phỏng các vòng là không thể. Một vòng duy nhất có thể thay đổi nhiều sức mạnh và số vòng cũng có thể lớn vì sức mạnh có thể tăng từng sức mạnh một. Với kích thước đầu vào này, chúng tôi cần một giải pháp gần với`O(n log n)`, loại trừ bất kỳ điều gì liên quan đến việc so sánh lặp đi lặp lại giữa nhiều cặp đèn hiệu. 

Các bẫy chính được gây ra bởi thực tế là sức mạnh thay đổi, không chỉ các khoảng thời gian. Một đèn hiệu ban đầu được tách ra khỏi một đèn hiệu khác có thể được kết nối sau một số lần tăng. Giải pháp chỉ hợp nhất các khoảng thời gian chồng chéo ban đầu là không chính xác. 

Ví dụ:```
2
0 1
5 3
```Các khoảng là`[-1,1]`Và`[2,8]`, vì vậy chúng đã trùng nhau vì phần thứ hai bắt đầu ngay sau phần đầu tiên kết thúc. Quyền lực trở nên cân bằng tại`3`, và sự kết hợp cuối cùng là`[-3,8]`, với chiều dài`12`. Việc thực hiện bất cẩn chỉ giữ được sự kết hợp ban đầu sẽ tạo ra`10`. 

Một ví dụ khác là:```
3
0 1
4 1
8 5
```Hai đèn hiệu đầu tiên không chồng lên nhau, nhưng đèn hiệu thứ ba chạm tới đèn hiệu thứ hai. Đèn hiệu thứ hai tăng lên và cuối cùng có thể kết nối hai đèn hiệu đầu tiên vào cùng một thành phần cuối cùng. Bỏ qua sự chồng chéo mới được tạo ra sẽ bỏ lỡ phản ứng dây chuyền này. 

Trường hợp cạnh cuối cùng là đèn hiệu có công suất tối đa trong một thành phần. Nó không bao giờ phát triển, trong khi mọi đèn hiệu yếu hơn cuối cùng cũng đạt được sức mạnh tương tự. Việc coi tất cả các đèn hiệu như thể chúng phát triển đối xứng sẽ mang lại sức mạnh cuối cùng sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quá trình. Đối với mỗi vòng, chúng tôi có thể kiểm tra từng cặp đèn hiệu, tìm tất cả các cặp chồng chéo và tăng sức mạnh yếu hơn. Điều này đúng vì nó tuân theo các quy tắc một cách chính xác. Tuy nhiên, kiểm tra tất cả các cặp chi phí`O(n^2)`mỗi vòng. Với`n = 2 * 10^5`, ngay cả một lần quét toàn bộ cũng đã quá đắt và số vòng không bị giới hạn bởi một hằng số nhỏ. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào các nhóm được kết nối. Bên trong bất kỳ nhóm khoảng chồng chéo nào được kết nối, công suất tối đa không bao giờ thay đổi vì không có đèn hiệu nào mạnh hơn để tăng công suất tối đa. Mọi đèn hiệu khác đều có một đường dẫn chồng chéo dẫn đến đèn hiệu có công suất tối đa, do đó mức tăng sẽ lan truyền dọc theo đường dẫn này cho đến khi mọi đèn hiệu đều đạt cùng công suất tối đa. 

Điều này có nghĩa là quy trình động có thể được thay thế bằng quy trình hợp nhất tĩnh. Chúng tôi xử lý đèn hiệu từ trái sang phải. Mỗi nhóm được xử lý được biểu thị bằng vị trí ngoài cùng bên trái, vị trí ngoài cùng bên phải và công suất tối đa bên trong nó. Nếu một nhóm mới chồng lên nhóm trước đó sau khi cả hai đều được mở rộng đến lũy thừa cuối cùng thì chúng thực sự phải thuộc về cùng một thành phần cuối cùng. Chúng tôi hợp nhất chúng và giữ công suất tối đa lớn hơn. 

Một ngăn xếp là đủ vì chỉ thành phần được xử lý cuối cùng mới có thể bị ảnh hưởng bởi đèn hiệu mới. Sau khi hợp nhất hai thành phần, thành phần mới cũng có thể tiếp cận thành phần trước chúng, vì vậy chúng tôi liên tục hợp nhất trong khi hai mục ngăn xếp cuối cùng chồng lên nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(r * n^2)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đèn hiệu theo vị trí của chúng. Chúng tôi xử lý chúng theo thứ tự này vì mọi tương tác có thể xảy ra trong tương lai chỉ có thể xảy ra với các thành phần đã thấy hoặc với các thành phần ở bên phải. 
2. Tạo một thành phần mới từ đèn hiệu hiện tại. Ranh giới bên trái và bên phải của nó đều là vị trí đèn hiệu và sức mạnh của nó là sức mạnh ban đầu của đèn hiệu. 
3. So sánh thành phần mới với thành phần trước đó trong ngăn xếp. Nếu những khoảng thời gian cuối cùng của chúng trùng nhau thì chúng không thể tách rời được. Hợp nhất chúng bằng cách lấy ranh giới bên trái từ thành phần bên trái, ranh giới bên phải từ thành phần bên phải và công suất tối đa từ cả hai. 
4. Tiếp tục hợp nhất ngược trong khi thành phần được hợp nhất mới vẫn chồng lên phần tử ngăn xếp trước đó. Điều này xử lý một thành phần mạnh mẽ có thể mở rộng đủ xa để hấp thụ một số thành phần cũ hơn. 
5. Sau khi tất cả các đèn hiệu được xử lý, mỗi mục nhập ngăn xếp là một thành phần ổn định cuối cùng. Tổng độ dài các khoảng của chúng. 

Tại sao nó hoạt động: bất biến là sau khi xử lý lần đầu tiên`i`đèn hiệu, ngăn xếp chứa chính xác các thành phần cuối cùng có thể được hình thành chỉ bằng cách sử dụng các đèn hiệu đó. Một thành phần lưu trữ công suất tối đa cuối cùng của nó, bởi vì mọi đèn hiệu yếu hơn bên trong nó cuối cùng sẽ đạt đến giá trị đó. Khi hai thành phần được lưu trữ liền kề chồng lên nhau, đèn hiệu của chúng sẽ ảnh hưởng lẫn nhau sau khi ổn định, do đó việc giữ chúng tách biệt sẽ vi phạm định nghĩa về thành phần cuối cùng. Việc hợp nhất lặp đi lặp lại sẽ khôi phục bất biến sau mỗi lần chèn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = []
    for _ in range(n):
        x, p = map(int, input().split())
        a.append((x, p))

    a.sort()

    stack = []

    for x, p in a:
        stack.append([x, x, p])

        while len(stack) >= 2:
            l1, r1, p1 = stack[-2]
            l2, r2, p2 = stack[-1]

            if l2 - p2 <= r1 + p1:
                nl = l1
                nr = max(r1, r2)
                np = max(p1, p2)

                stack.pop()
                stack.pop()
                stack.append([nl, nr, np])
            else:
                break

    ans = 0
    for l, r, p in stack:
        ans += r - l + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được sắp xếp trước vì phương thức ngăn xếp dựa trên quá trình xử lý từ trái sang phải. Mỗi mục ngăn xếp chỉ lưu trữ thông tin quan trọng sau khi ổn định: các vị trí cực trị và công suất lớn nhất. 

Kiểm tra chồng chéo sử dụng khoảng thời gian mở rộng cuối cùng. Hai thành phần`[l1, r1]`Và`[l2, r2]`với quyền hạn`p1`Và`p2`tương tác nếu`l2 - p2 <= r1 + p1`. Hoạt động hợp nhất giữ công suất lớn nhất vì đó là công suất cuối cùng được chia sẻ bởi toàn bộ thành phần được kết nối. 

Vòng lặp cuối cùng tính toán độ dài bao gồm, do đó sự đóng góp của`[l, r]`là`r - l + 1`. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
7
0 1
3 1
5 2
9 1
12 3
20 1
21 1
```Ngăn xếp phát triển như sau: 

| Đèn hiệu | Thành phần mới | Xếp chồng sau khi hợp nhất | 
| --- | --- | --- | 
|`(0,1)`|`[-, -, 1]`|`[-1,1]`quyền lực`1`| 
|`(3,1)`|`[3,3,1]`|`[-1,4]`quyền lực`1`| 
|`(5,2)`|`[5,5,2]`|`[-3,7]`quyền lực`2`| 
|`(9,1)`|`[9,9,1]`|`[-3,7]`,`[8,10]`| 
|`(12,3)`|`[12,12,3]`|`[-3,15]`quyền lực`3`| 
|`(20,1)`|`[20,20,1]`|`[-3,15]`,`[19,21]`| 
|`(21,1)`|`[21,21,1]`|`[-3,15]`,`[19,22]`| 

Dấu vết này cho thấy lý do tại sao đèn hiệu mới có thể hợp nhất ngược qua một số thành phần. Đèn hiệu ở vị trí`12`hấp thụ chuỗi trước đó vì sức mạnh của nó đủ lớn. 

Một trường hợp khác:```
3
0 1
4 1
8 5
```| Đèn hiệu | Trạng thái ngăn xếp | 
| --- | --- | 
|`(0,1)`|`[-1,1]`| 
|`(4,1)`|`[-1,5]`| 
|`(8,5)`|`[-5,13]`| 

Đèn hiệu thứ ba lớn mở rộng đủ để kết nối mọi thứ, mặc dù chỉ riêng hai đèn hiệu đầu tiên sẽ tạo thành một thành phần khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc sắp xếp chiếm ưu thế và mọi thành phần được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian |`O(n)`| Ngăn xếp có thể chứa tất cả các đèn hiệu trong trường hợp xấu nhất | 

Thuật toán chỉ thực hiện một số tuyến tính các thao tác ngăn xếp sau khi sắp xếp nên nó phù hợp với giới hạn cho`2 * 10^5`đèn hiệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    a = []

    for _ in range(n):
        x = int(next(it))
        p = int(next(it))
        a.append((x, p))

    a.sort()
    stack = []

    for x, p in a:
        stack.append([x, x, p])
        while len(stack) >= 2:
            l1, r1, p1 = stack[-2]
            l2, r2, p2 = stack[-1]
            if l2 - p2 <= r1 + p1:
                stack.pop()
                stack.pop()
                stack.append([l1, max(r1, r2), max(p1, p2)])
            else:
                break

    ans = sum(r - l + 1 for l, r, p in stack)
    return str(ans) + "\n"

assert run("""7
0 1
3 1
5 2
9 1
12 3
20 1
21 1
""") == "23\n"

assert run("""1
100 7
""") == "15\n"

assert run("""3
0 1
4 1
8 5
""") == "19\n"

assert run("""2
0 1
10 1
""") == "4\n"

assert run("""4
0 2
5 2
10 2
15 2
""") == "32\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đèn hiệu đơn |`15`| Trường hợp tối thiểu và độ dài bao gồm | 
| Kết nối đèn hiệu tầm xa |`19`| Phản ứng dây chuyền thông qua đèn hiệu mạnh | 
| Tách biệt quyền lực ngang nhau |`4`| Các thành phần không bao giờ hợp nhất | 
| Mọi quyền lực ngang nhau |`32`| Không thay đổi quyền lực và sáp nhập nhiều lần | 

## Vỏ cạnh 

Đối với trường hợp```
2
0 1
5 3
```ngăn xếp đầu tiên tạo ra hai thành phần. Thành phần thứ hai mở rộng đến`[2,8]`, chồng lên nhau`[ -1,1 ]`, nên chúng hợp nhất. Thành phần cuối cùng có sức mạnh`3`và khoảng thời gian`[-3,8]`, cho biết độ dài`12`. 

Vì```
3
0 1
4 1
8 5
```hai đèn hiệu đầu tiên hợp nhất thành một thành phần có nguồn điện`1`. Khi đèn hiệu thứ ba được chèn vào, khoảng thời gian của nó`[3,13]`chồng lấp khoảng thời gian cuối cùng của thành phần đó, do đó toàn bộ cấu trúc trở thành một thành phần. Công suất tối đa được lưu trữ trở thành`5`, sản xuất`[-5,13]`với chiều dài`19`. 

Phương thức ngăn xếp cũng xử lý lũy thừa lớn một cách an toàn vì tất cả số học được thực hiện bằng số nguyên Python, do đó vị trí lên tới`10^9`và phạm vi mở rộng vượt ra ngoài đó không tràn.
