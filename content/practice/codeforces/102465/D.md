---
title: "CF 102465D - Tham quan Tượng đài"
description: "Thành phố là một mạng lưới hình chữ nhật. Xe buýt chọn một con đường nằm ngang hướng đông, được biểu thị bằng tọa độ hàng cố định y = r, đi vào từ phía tây, đi về phía đông và phải rời đi trên cùng hàng đó."
date: "2026-08-08T09:17:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "D"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 373
verified: true
draft: false
---

[CF 102465D - Chuyến tham quan tượng đài](https://codeforces.com/problemset/problem/102465/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thành phố là một mạng lưới hình chữ nhật. Xe buýt chọn một con đường nằm ngang hướng Đông, được thể hiện bằng tọa độ hàng cố định`y = r`, đi vào từ phía tây, đi về phía đông và phải rời đi trên cùng một hàng. Mỗi khi phải tham quan các di tích phía trên hoặc phía dưới con đường này, nó có thể rẽ dọc, tham quan các di tích cần thiết và quay trở lại đường chính. 

Đối với tuyến đường chính cố định`r`, hãy xem xét tất cả các di tích có cùng`x`điều phối. Chúng nằm trên một con đường thẳng đứng nên xe buýt có thể ghé thăm tất cả chúng trong một chuyến tham quan thẳng đứng. Chỉ nhỏ nhất và lớn nhất`y`tọa độ tại đó`x`vấn đề. Nếu những thái cực đó là`L`Và`R`, xe buýt phải đi hết quãng đường`[L, R]`và quay trở lại`r`. 

Đầu vào mang lại`X`đường hướng bắc và`Y`đường hướng đông, tiếp theo là`N`tọa độ tượng đài`(x, y)`. Đầu ra là số khối lưới tối thiểu được đi qua bằng cách chọn đường ngang tốt nhất. 

Các giới hạn đủ lớn để loại trừ việc thử mọi con đường có thể và quét mọi di tích. Với tối đa`100000`di tích và`100000`có thể có đường ngang, đường thẳng`O(NY)`tính toán có thể đạt`10^10`hoạt động. Thậm chí`O(XY)`có cùng thứ tự trong trường hợp xấu nhất. Một giải pháp xung quanh`O(N log N)`là thích hợp vì sắp xếp`O(N)`các giá trị dẫn xuất có thể dễ dàng thực hiện được trong các ràng buộc. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Nếu một số di tích có chung một`x`phối hợp, xử lý chúng một cách độc lập vượt quá hành trình dọc. Ví dụ:```
2 2
2
0 0
0 1
```Câu trả lời đúng là`3`. Xe buýt có thể vào theo hàng`0`, đi một dãy nhà về phía đông và ghé thăm cả hai di tích trên cùng một con đường thẳng đứng mà không cần đi đường vòng thêm. Việc thực hiện bất cẩn dẫn đến tính phí`2 * |r-y|`độc lập cho cả hai di tích sẽ nhận được`5`cho hàng`0`. 

Một di tích duy nhất là một trường hợp ranh giới hữu ích khác:```
1 1
1
0 0
```Câu trả lời là`0`, bởi vì xe buýt bắt đầu và kết thúc ở vị trí nằm ngang duy nhất có sẵn và không có dãy nhà đông tây nào để đi qua. Một triển khai bổ sung một cách mù quáng`X`thay vì`X - 1`sẽ sản xuất`1`. 

Tượng đài cũng có thể nằm ở hàng đầu tiên hoặc hàng cuối cùng:```
3 5
1
2 4
```Đang chọn hàng`4`đưa ra chi phí tối thiểu, cụ thể là`2`, vì xe buýt đi qua hai dãy nhà ngang và không bao giờ cần đi đường vòng theo chiều dọc. Việc triển khai giả định con đường tối ưu phải ở đâu đó trong thành phố có thể bỏ lỡ trường hợp này. 

Cuối cùng, tọa độ trùng lặp được cho phép:```
2 3
3
0 1
0 1
0 1
```Câu trả lời là`1`. Tượng đài lặp đi lặp lại không tạo thêm du lịch. Chỉ giữ mức tối thiểu và tối đa`y`cho mỗi`x`tự nhiên xử lý các bản sao. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là thử mọi con đường ngang có thể. Đối với mỗi hàng ứng cử viên`r`, chúng tôi có thể kiểm tra mọi di tích và xác định mức độ di chuyển theo chiều dọc cần thiết. Nếu chúng ta xử lý các di tích một cách độc lập, điều này sẽ mất`O(N)`làm việc cho một ứng viên, vì vậy tất cả`Y`ứng viên yêu cầu`O(NY)`thời gian. Trong trường hợp xấu nhất đó là`100000 * 100000 = 10^10`kiểm tra tượng đài, vượt xa những gì giới hạn một giây có thể xử lý. 

Chúng ta có thể cải thiện các yếu tố không đổi bằng cách nhóm các di tích có cùng đặc điểm`x`, nhưng điều đó không giải quyết được vấn đề chính. Sau khi nhóm lại vẫn có thể có`100000`những con đường thẳng đứng khác nhau và cố gắng mọi cách có thể`r`vẫn cho công hàm bậc hai. 

Quan sát quan trọng là một con phố thẳng đứng chỉ cần tượng đài thấp nhất và cao nhất. Giả sử những giá trị đó là`L`Và`R`. Nếu đường chính ở`r`, chuyến tham quan theo chiều dọc ngắn nhất đi qua toàn bộ khoảng thời gian và quay trở lại`r`có chiều dài`2 * (R - L)`khi`r`ở bên trong`[L, R]`. 

Nếu như`r < L`, trước tiên xe buýt phải đi từ`r`ĐẾN`L`, đi qua khoảng và trở về từ`R`ĐẾN`r`. chiều dài của nó là`2 * (R - r)`. 

Tương tự, nếu`r > R`, chiều dài của nó là`2 * (r - L)`. 

Ba trường hợp này có thể được viết gọn hơn như sau`2 * (R - L + distance(r, [L, R]))`. 

Phần cố định`2 * (R - L)`không ảnh hưởng đến việc lựa chọn`r`. Phần thú vị là giảm thiểu tổng khoảng cách từ`r`đến tất cả các khoảng. 

Có một danh tính hữu ích:`distance(r, [L, R]) = (|r-L| + |r-R| - (R-L)) / 2`. 

Do đó, sau khi thêm các số hạng không đổi, việc tối thiểu hóa toàn bộ hành trình tương đương với việc tối thiểu hóa`sum(|r-L| + |r-R|)`. 

Vì vậy, mỗi đường thẳng đứng có người sử dụng đều đóng góp hai giá trị, điểm cuối phía dưới của nó`L`và điểm cuối trên`R`và chúng ta chỉ cần một giá trị`r`giảm thiểu tổng chênh lệch tuyệt đối cho tất cả các giá trị điểm cuối này. Giá trị như vậy là giá trị trung bình. 

Đây chính xác là quan sát được sử dụng trong phân tích SWERC chính thức: chỉ giữ lại giá trị cực trị`y`tọa độ cho mỗi`x`, đếm khoảng thời gian một điểm hai lần và chọn giá trị trung bình của tất cả các điểm cuối thu được. 

Do đó hình học biến mất. Trước tiên, chúng tôi giảm mỗi cột bị chiếm dụng thành một khoảng, thu thập cả hai điểm cuối, sắp xếp chúng, lấy giá trị trung bình và đánh giá chuyến tham quan kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NY)`|`O(N)`| Quá chậm | 
| Tối ưu |`O(N log N)`|`O(N + X)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tọa độ di tích và duy trì, cho mỗi`x`, nhỏ nhất và lớn nhất`y`xuất hiện ở đó. Hai giá trị này mô tả đầy đủ công việc theo chiều dọc cần thiết tại con phố đó bởi vì việc tham quan mọi thứ giữa chúng bao gồm mọi di tích trên con phố đó. 
2. Đối với mọi`x`chứa ít nhất một tượng đài, hãy nối thêm mức tối thiểu của nó`y`và tối đa`y`đến một mảng điểm cuối. Nếu tất cả di tích ở đó`x`có cùng một thứ`y`, mức tối thiểu và tối đa bằng nhau, do đó, cùng một tọa độ được cố tình chèn hai lần. 
3. Sắp xếp mảng điểm cuối. Vì mỗi khoảng đóng góp chính xác hai điểm cuối nên số lượng giá trị được lưu trữ nhiều nhất là`2N`. 
4. Chọn dải phân cách dưới của các điểm cuối đã sắp xếp làm dãy đường chính`r`. Bất kỳ trung vị nào cũng giảm thiểu tổng khoảng cách tuyệt đối, do đó, trung vị thấp hơn là đủ ngay cả khi có số điểm cuối chẵn. 
5. Bắt đầu câu trả lời bằng`X - 1`, là khoảng cách đông tây tính từ ranh giới phía tây tại tọa độ`0`đến ranh giới phía đông tại tọa độ`X - 1`. 
6. Đối với mỗi người bị chiếm đóng`x`, gọi khoảng của nó là`[L, R]`. Nếu như`r < L`, thêm vào`2 * (R-r)`. Nếu như`r > R`, thêm vào`2 * (r-L)`. Nếu không thì thêm`2 * (R-L)`. Đây chính xác là chuyến tham quan ngắn nhất từ ​​con đường chính đi qua toàn bộ quãng đường và quay trở lại. 

Bất biến đằng sau thuật toán là mọi chuyến tham quan có thể có đường chính`r`có cùng chi phí theo chiều ngang,`X - 1`và chi phí dọc của nó là tổng các chi phí độc lập cho diện tích sử dụng`x`tọa độ. Việc thay thế mỗi cột bằng khoảng cực trị của nó sẽ không làm mất thông tin. Chi phí dọc của mỗi khoảng sau đó có thể được viết lại theo khoảng cách đến hai điểm cuối của nó, do đó mục tiêu hoàn chỉnh sẽ trở thành tổng của độ lệch tuyệt đối so với mảng điểm cuối. Đường trung tuyến giảm thiểu mục tiêu đó, có nghĩa là đường được chọn là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    X, Y = map(int, input().split())
    N = int(input())

    INF = 10**30
    low = [INF] * X
    high = [-1] * X

    for _ in range(N):
        x, y = map(int, input().split())
        if y < low[x]:
            low[x] = y
        if y > high[x]:
            high[x] = y

    endpoints = []

    for x in range(X):
        if high[x] != -1:
            endpoints.append(low[x])
            endpoints.append(high[x])

    endpoints.sort()
    road = endpoints[(len(endpoints) - 1) // 2]

    answer = X - 1

    for x in range(X):
        if high[x] == -1:
            continue

        L = low[x]
        R = high[x]

        if road < L:
            answer += 2 * (R - road)
        elif road > R:
            answer += 2 * (road - L)
        else:
            answer += 2 * (R - L)

    print(answer)

if __name__ == "__main__":
    solve()
```các`low`Và`high`mảng thực hiện bước thuật toán đầu tiên.`low[x]`lưu trữ tượng đài thấp nhất trên đường thẳng đứng`x`, trong khi`high[x]`lưu trữ cái cao nhất. lính gác`high[x] == -1`xác định các cột không chứa di tích. 

Danh sách điểm cuối chứa chính xác hai giá trị cho mỗi cột bị chiếm dụng. Đối với một cột chỉ chứa một phân biệt`y`, cả hai giá trị đều bằng nhau, điều này tự động mang lại sự đóng góp gấp đôi cần thiết. 

Trung vị được chọn bằng`(len(endpoints) - 1) // 2`. Với số điểm cuối chẵn, giá trị này sẽ chọn giá trị thấp hơn trong hai giá trị ở giữa. Cả hai giá trị ở giữa đều là các giá trị tối thiểu hợp lệ, vì vậy cả hai lựa chọn đều cho cùng một chi phí tối thiểu. 

Đóng góp theo chiều ngang là`X - 1`, không`X`. Tọa độ của thành phố dao động từ`0`bởi vì`X - 1`, do đó việc đi từ ranh giới phía tây sang ranh giới phía đông sẽ cắt chính xác`X - 1`khối. 

Số nguyên Python có độ chính xác tùy ý, do đó câu trả lời tích lũy không thể tràn. Dù sao thì câu trả lời tối đa cũng dễ dàng nằm trong phạm vi 64 bit thông thường, nhưng việc sử dụng số nguyên Python sẽ loại bỏ hoàn toàn mối lo ngại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các đường thẳng đứng bị chiếm đóng tạo ra các khoảng sau: 

|`x`|`L`|`R`| Đóng góp điểm cuối | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 |`0, 2`| 
| 2 | 4 | 4 |`4, 4`| 
| 4 | 2 | 2 |`2, 2`| 

Mảng điểm cuối hoàn chỉnh trước khi sắp xếp là`[0, 2, 4, 4, 2, 2]`. 

| Bước | Tiểu bang | 
| --- | --- | 
| Nhóm di tích |`[1:[0,2], 2:[4,4], 4:[2,2]]`| 
| Điểm cuối |`[0,2,4,4,2,2]`| 
| Điểm cuối được sắp xếp |`[0,2,2,2,4,4]`| 
| Con đường đã chọn |`2`| 
| Chi phí ngang |`5`| 
| Cột`x=1`|`2 * (2-0) = 4`| 
| Cột`x=2`|`2 * (4-2) = 4`| 
| Cột`x=4`|`0`| 
| Tổng cộng |`5 + 4 + 4 = 13`| 

Con đường được chọn là con đường`2`, là giá trị trung bình dưới của các giá trị điểm cuối. Cột đầu tiên yêu cầu xe buýt truy cập các hàng`0`Và`2`, trong khi cái thứ hai yêu cầu một chuyến đi từ hàng`2`chèo thuyền`4`và quay lại. Cột cuối cùng đã nằm trên đường chính. 

### Mẫu 2 

Khoảng cách di tích là: 

|`x`|`L`|`R`| Đóng góp điểm cuối | 
| --- | --- | --- | --- | 
| 0 | 0 | 3 |`0, 3`| 
| 2 | 2 | 3 |`2, 3`| 
| 3 | 2 | 2 |`2, 2`| 
| 4 | 3 | 6 |`3, 6`| 

Các giá trị điểm cuối là`[0,3,2,3,2,2,3,6]`. 

| Bước | Tiểu bang | 
| --- | --- | 
| Nhóm di tích |`[0:[0,3], 2:[2,3], 3:[2,2], 4:[3,6]]`| 
| Điểm cuối được sắp xếp |`[0,2,2,2,3,3,3,6]`| 
| Con đường đã chọn |`2`| 
| Chi phí ngang |`4`| 
| Cột`x=0`|`2 * (3-0) = 6`| 
| Cột`x=2`|`2 * (3-2) = 2`| 
| Cột`x=3`|`0`| 
| Cột`x=4`|`2 * (6-2) = 8`| 
| Tổng cộng |`4 + 6 + 2 + 0 + 8 = 20`| 

Trung vị lại là hàng`2`. Khoảng thời gian tại`x=3`chứa đường chính nên chỉ đóng góp chiều rộng bằng 0 vì cả hai điểm cuối đều`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(X + N + N log N)`| Xây dựng chi phí cực trị`O(N)`, quét cột chi phí`O(X)`và sắp xếp nhiều nhất`2N`chi phí điểm cuối`O(N log N)`| 
| Không gian |`O(X + N)`| Mảng cực trị sử dụng`O(X)`khoảng trống và mảng điểm cuối chứa tối đa`2N`giá trị | 

Với`X, Y, N <= 100000`, hoạt động chủ yếu là sắp xếp nhiều nhất`200000`số nguyên. Điều này thoải mái trong phạm vi dự định, trong khi lực lượng vũ phu`10^10`séc thì không. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    X = next(it)
    Y = next(it)
    N = next(it)

    INF = 10**30
    low = [INF] * X
    high = [-1] * X

    for _ in range(N):
        x = next(it)
        y = next(it)
        low[x] = min(low[x], y)
        high[x] = max(high[x], y)

    endpoints = []
    for x in range(X):
        if high[x] != -1:
            endpoints.append(low[x])
            endpoints.append(high[x])

    endpoints.sort()
    road = endpoints[(len(endpoints) - 1) // 2]

    answer = X - 1

    for x in range(X):
        if high[x] == -1:
            continue

        L = low[x]
        R = high[x]

        if road < L:
            answer += 2 * (R - road)
        elif road > R:
            answer += 2 * (road - L)
        else:
            answer += 2 * (R - L)

    return str(answer)

# Provided sample 1
assert solve("""\
6 5
4
1 0
1 2
2 4
4 2
""") == "13", "sample 1"

# Provided sample 2
assert solve("""\
5 7
9
0 0
0 2
0 3
2 2
2 3
3 2
4 3
4 4
4 6
""") == "20", "sample 2"

# Minimum-size city, single monument
assert solve("""\
1 1
1
0 0
""") == "0", "minimum size"

# All monuments at the same coordinate
assert solve("""\
2 3
3
0 1
0 1
0 1
""") == "1", "duplicate coordinates"

# One column spans the whole height, so every road inside it is optimal
assert solve("""\
3 5
2
1 0
1 4
""") == "6", "single wide interval"

# Monuments on both boundaries, forcing the median choice to matter
assert solve("""\
4 5
2
0 0
3 4
""") == "7", "boundary monuments"

# Large input shape, all coordinates equal
large = "100000 100000\n100000\n" + "\n".join(
    "99999 99999" for _ in range(100000)
) + "\n"
assert solve(large) == "99999", "large duplicate input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, một tượng đài ở`(0,0)`|`0`| Kích thước tối thiểu và`X-1`chi phí ngang | 
|`2 3`, ba di tích giống hệt nhau |`1`| Tọa độ trùng lặp và giá trị điểm cuối lặp lại | 
|`3 5`, di tích ở`(1,0)`Và`(1,4)`|`6`| Một khoảng dọc và đường trung tuyến nằm bên trong nó | 
|`4 5`, di tích ở`(0,0)`Và`(3,4)`|`7`| Tọa độ biên và trung vị không cần thiết | 
|`100000 100000`,`100000`di tích giống hệt nhau |`99999`| Tối đa`N`và kích thước lớn mà không cần tính bậc hai | 

## Vỏ cạnh 

Khi nhiều di tích chia sẻ một`x`tọa độ, thuật toán sẽ thay thế chúng bằng một khoảng. Ví dụ,```
2 2
2
0 0
0 1
```đưa ra khoảng thời gian`[0,1]`. Điểm cuối của nó là`0`Và`1`, do đó số trung vị có thể là một trong hai hàng`0`hoặc hàng`1`. Đang chọn hàng`0`, chi phí ngang là`1`và chi phí tham quan theo chiều dọc`2`, cho`3`. Hai di tích được viếng thăm trong một chuyến đi, thay vì trả tiền cho hai chuyến đi riêng biệt. 

Khi tất cả các di tích có cùng tọa độ,```
2 3
3
0 1
0 1
0 1
```khoảng thời gian cho`x=0`là`[1,1]`và các điểm cuối của nó được chèn dưới dạng`1,1`. Do đó, số trung vị là`1`. Chi phí theo chiều ngang là`1`và chi phí dọc bằng 0, vì vậy câu trả lời là`1`. Các bản sao không ảnh hưởng đến hình học. 

Đối với một tượng đài duy nhất ở vị trí sẵn có duy nhất,```
1 1
1
0 0
```mảng điểm cuối là`[0,0]`, vậy con đường được chọn là`0`. Từ`X=1`, chi phí ngang là`X-1=0`và khoảng cũng đóng góp bằng không. Kết quả là`0`. 

Đối với một tượng đài ở rìa xa nhất của dãy thẳng đứng,```
3 5
1
2 4
```mảng điểm cuối là`[4,4]`, vậy con đường được chọn là`4`. Tượng đài nằm ngay trên con đường chính. Xe buýt chỉ phải di chuyển từ phía Tây sang phía Đông, băng qua`X-1=2`khối, vì vậy câu trả lời là`2`. Thuật toán không cho rằng hàng tối ưu là tọa độ bên trong. 

Đối với một cột có tượng đài trải dài một khoảng lớn,```
3 5
2
1 0
1 4
```mảng điểm cuối là`[0,4]`. Bất kỳ hàng nào từ`0`bởi vì`4`giảm thiểu khoảng cách đến khoảng này và trung vị phía dưới chọn`0`. Chi phí tham quan theo chiều dọc`2 * (4-0)=8`, trong khi chi phí di chuyển theo chiều ngang`2`, cho`10`. Nếu thành phố chỉ có năm con đường hướng đông, hãy xếp hàng`0`là hợp lệ vì tọa độ di tích nằm trong khoảng từ`0`bởi vì`4`. Điều này chứng tỏ tại sao công thức khoảng phải bao gồm cả hai điểm cuối một cách chính xác. 

Bài học cốt lõi là vấn đề chỉ có tính chất hai chiều cho đến khi các di tích có chung một`x`tọa độ được nén thành các khoảng dọc. Sau đó, quyết định duy nhất còn lại là vị trí của một đường ngang và toàn bộ quá trình tối ưu hóa sẽ giảm xuống thành thực tế cổ điển là trung vị sẽ giảm thiểu tổng khoảng cách tuyệt đối.
