---
title: "CF 106438C - Đường ở Laurasia"
description: "Vấn đề mô tả một cây làng được kết nối. Một cái cây có chính xác một tuyến đường nối giữa hai ngôi làng bất kỳ, vì vậy mỗi con đường đều là một điểm thất bại."
date: "2026-06-25T09:35:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106438
codeforces_index: "C"
codeforces_contest_name: "IUT Eid Salami Programming Contest 2026 - Powered by Okkhor Technology (Online Mirror)"
rating: 0
weight: 106438
solve_time_s: 37
verified: true
draft: false
---

[CF 106438C - Đường ở Laurasia](https://codeforces.com/problemset/problem/106438/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một cây làng được kết nối. Một cái cây có chính xác một tuyến đường nối giữa hai ngôi làng bất kỳ, vì vậy mỗi con đường đều là một điểm thất bại. Chúng ta cần thêm số lượng đường bổ sung nhỏ nhất để mỗi cặp làng có ít nhất hai tuyến đường đơn giản khác nhau giữa chúng. Tương tự, sau khi thêm đường, đồ thị không được có cầu, vì cầu chính xác là một con đường mà việc loại bỏ sẽ ngăn cách đồ thị và ngăn cản hai tuyến đường độc lập. 

Đầu vào đưa ra một số trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi nhận được số làng và`N - 1`đường hiện có tạo thành cây. Đầu ra là số lượng đường mới tối thiểu cần thiết. 

Ràng buộc về tổng số làng trong tất cả các thử nghiệm là lớn, do đó thuật toán phải gần tuyến tính. Giải pháp thử những con đường mới khả thi hoặc kiểm tra nhiều cặp làng sẽ nhanh chóng trở nên quá chậm. Với tối đa hàng trăm nghìn nút, chúng ta chỉ cần xử lý mọi cạnh và đỉnh với số lần không đổi. 

Phần khó khăn là hiểu được điều gì xảy ra ở những chiếc lá. Một chiếc lá chỉ có một con đường phụ nên con đường đó luôn là cây cầu trong cây ban đầu. Cách duy nhất để loại bỏ điểm yếu đó là nối chiếc lá với phần khác của cây, tạo ra một chu trình chứa cạnh của nó. Số đường thêm tối thiểu không phải là số lá, vì một đường mới có thể cố định hai lá cùng một lúc bằng cách nối hai nhánh lá lại với nhau. 

Xét một cây có ba đỉnh:```
3
1 2
2 3
```Có hai chiếc lá. Câu trả lời đúng là`1`, bởi vì việc thêm một cạnh giữa`1`Và`3`tạo một chu trình chứa cả hai con đường ban đầu. Một cách tiếp cận bất cẩn khi thêm một con đường vào mỗi lá sẽ tạo ra`2`. 

Một trường hợp cạnh khác là cây có số lá là số lẻ. Ví dụ:```
5
1 2
1 3
1 4
1 5
```Có 4 lá nên đáp án là`2`. Phương pháp ghép các lá một cách tham lam mà không xử lý lá chưa khớp cuối cùng có thể thất bại trên những cây có số lá lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn liên tục các cặp lá và thêm đường giữa chúng. Ý tưởng này hợp lý vì mỗi con đường được thêm vào sẽ tạo ra một vòng tròn, và vòng tròn chính là thứ giúp loại bỏ những cây cầu. Nếu chúng ta mô phỏng tác động của từng lựa chọn khả thi và tiếp tục tìm kiếm những con đường tốt nhất thì số khả năng sẽ tăng lên quá nhanh. Đối với một cây có nhiều lá, có quá nhiều cặp có thể xảy ra và việc tìm kiếm trở nên theo cấp số nhân. 

Quan sát hữu ích đến từ việc xem con đường mới thay đổi như thế nào. Nếu chúng ta nối hai lá, mọi cạnh trên đường đi giữa các lá đó sẽ trở thành một phần của một chu trình. Các cạnh duy nhất ban đầu không được bảo vệ là các cành kết thúc ở lá. Để làm cho toàn bộ cây không có cầu nối, mỗi mép lá cần phải thuộc một chu trình nào đó. Một con đường được thêm vào có thể bao gồm tối đa hai cạnh lá, vì hai điểm cuối của nó là nơi duy nhất mà nó có thể đóng trực tiếp hai nhánh bên ngoài. 

Điều này đưa ra giới hạn dưới: nếu cây có`L`lá, chúng ta cần ít nhất`ceil(L / 2)`những con đường mới. Chúng ta luôn có thể đạt được điều này bằng cách ghép các lá. Nếu như`L`chẵn, ghép mỗi lá với một lá khác. Nếu như`L`là số lẻ, hãy ghép tất cả trừ một lá và sử dụng lá còn lại trong kết nối cuối cùng. Cấu trúc cây đảm bảo rằng những phần bổ sung này bao phủ tất cả các mép lá. 

Toàn bộ vấn đề giảm xuống việc đếm lá. Không thể có một cây có một đỉnh theo các ràng buộc đã cho và mọi cây hợp lệ ở đây đều có ít nhất hai lá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lá | Phụ thuộc vào trạng thái tìm kiếm | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và đếm xem có đúng một đỉnh lân cận. Những đỉnh này là những chiếc lá, và những con đường tới của chúng là những con đường duy nhất ban đầu không thể thuộc về bất kỳ chu trình nào. 
2. Gọi số lá là`L`. Vì một con đường mới có thể nối hai lá và cố định cả hai nhánh tương ứng nên hãy chia các lá thành từng cặp. 
3. Nếu`L`chẵn, đáp án là`L / 2`. Mỗi cặp đóng góp một con đường mới. 
4. Nếu`L`là số lẻ, lá này không thể ghép với lá khác. Nó vẫn cần được kết nối bằng một con đường bổ sung, vì vậy câu trả lời trở thành`(L + 1) / 2`. 

Lý do điều này có hiệu quả là vì mọi con đường bên trong cây giữa hai đỉnh bên trong đều nằm trên một con đường giữa các lá. Khi tất cả các cạnh lá được đưa vào chu trình, mọi cạnh khác cũng nằm trên một chu trình, bởi vì các kết nối lá được thêm vào sẽ tạo ra các chu trình đi qua các đường dẫn bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    t = int(data[0])
    idx = 1
    ans = []

    for _ in range(t):
        n = int(data[idx])
        idx += 1

        deg = [0] * n

        for _ in range(n - 1):
            u = int(data[idx]) - 1
            v = int(data[idx + 1]) - 1
            idx += 2
            deg[u] += 1
            deg[v] += 1

        leaves = 0
        for d in deg:
            if d == 1:
                leaves += 1

        ans.append(str((leaves + 1) // 2))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã chỉ lưu trữ cấp độ của mỗi làng. Chúng ta không cần danh sách kề vì câu trả lời chỉ phụ thuộc vào số lượng lá tồn tại. 

Trong khi đọc từng con đường, cả hai điểm cuối đều tăng một độ. Sau khi tất cả các con đường được xử lý, một đỉnh có bậc 1 là một chiếc lá. Biểu thức cuối cùng`(leaves + 1) // 2`là dạng số nguyên lấy trần của một nửa số lá. 

Không có cạm bẫy lập chỉ mục trong logic chính vì các đỉnh chỉ được sử dụng để đếm độ. Đầu vào sử dụng một chỉ mục dựa trên một, nhưng mảng sử dụng chỉ mục dựa trên 0 sau khi trừ đi một. Số lượng làng lớn nhất có thể là lớn nên giải pháp sử dụng đầu vào được đệm. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
4
1 2
1 3
1 4
```Trạng thái thuật toán là: 

| Bước | Hành động hiện tại | Đếm lá | Trả lời | 
| --- | --- | --- | --- | 
| 1 | Đọc mọi con đường | 3 | Không được tính toán | 
| 2 | Đếm độ một đỉnh | 3 | (3 + 1) / 2 | 
| 3 | Kết thúc | 3 | 2 | 

Ba chiếc lá cần được che phủ. Hai con đường là đủ vì một lá có thể được xử lý bằng cách nối nó thông qua một chu trình được tạo bằng một con đường bổ sung khác. 

Mẫu 2: 

đầu vào:```
8
1 3
2 3
3 4
4 5
5 6
6 7
6 8
```Trạng thái thuật toán là: 

| Bước | Hành động hiện tại | Đếm lá | Trả lời | 
| --- | --- | --- | --- | 
| 1 | Đọc mọi con đường | 0 | Không được tính toán | 
| 2 | Đếm độ một đỉnh | 4 | (4 + 1) / 2 | 
| 3 | Kết thúc | 4 | 2 | 

Những chiếc lá là những ngôi làng`1`,`2`,`7`, Và`8`. Chúng có thể được ghép thành hai con đường mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi con đường được đọc một lần và mỗi cấp làng được kiểm tra một lần | 
| Không gian | O(N) | Mảng độ lưu trữ một giá trị cho mỗi làng | 

Tổng số làng trong tất cả các trường hợp thử nghiệm bị giới hạn, do đó cách tiếp cận tuyến tính dễ dàng phù hợp với giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        deg = [0] * n

        for _ in range(n - 1):
            u, v = map(int, input().split())
            deg[u - 1] += 1
            deg[v - 1] += 1

        leaves = sum(1 for x in deg if x == 1)
        out.append(str((leaves + 1) // 2))

    return "\n".join(out)

assert solution("""2
4
1 2
1 3
1 4
8
1 3
2 3
3 4
4 5
5 6
6 7
6 8
""") == """2
2""", "samples"

assert solution("""1
3
1 2
2 3
""") == "1", "path tree"

assert solution("""1
5
1 2
1 3
1 4
1 5
""") == "2", "star tree"

assert solution("""1
6
1 2
2 3
3 4
4 5
5 6
""") == "1", "long path"

assert solution("""1
7
1 2
1 3
1 4
1 5
1 6
1 7
""") == "3", "many leaves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường đi ba đỉnh | 1 | Một cạnh được thêm vào có thể cố định hai lá | 
| Ngôi sao bốn lá | 2 | Ghép nhiều lá | 
| Con đường dài | 1 | Chỉ có hai đầu mới quan trọng | 
| Ngôi sao sáu lá | 3 | Xử lý đúng số lượng lá chẵn lớn hơn | 

## Vỏ cạnh 

Đối với cây đường dẫn:```
3
1 2
2 3
```Những chiếc lá là`1`Và`3`. Thuật toán đếm hai lá và trả về`(2 + 1) // 2 = 1`. Việc thêm cạnh vào giữa chúng sẽ tạo ra một chu trình chứa cả hai cạnh ban đầu. 

Đối với một ngôi sao:```
5
1 2
1 3
1 4
1 5
```Có bốn lá. Thuật toán trả về`2`. Mỗi con đường mới có thể nối hai làng bên ngoài, như vậy hai con đường cũng đủ che hết bốn mép lá. 

Đối với cây có số lá lẻ như:```
6
1 2
1 3
1 4
1 5
5 6
```Những chiếc lá là`2`,`3`,`4`, Và`6`, vậy trường hợp cụ thể này có bốn lá và cần hai con đường. Nếu thêm một nhánh nữa từ nút`1`, số lá sẽ trở thành 5 và công thức trả về 3, điều này là cần thiết vì một lá không thể ghép với một lá khác.
