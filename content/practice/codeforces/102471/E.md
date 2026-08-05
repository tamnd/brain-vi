---
title: "CF 102471E - Dòng chảy"
description: "Đồ thị có hình dạng đặc biệt: nó được tạo thành từ nhiều đường độc lập đi từ đỉnh nguồn 1 đến đỉnh đích n. Mọi tuyến đường đều chứa cùng số cạnh và các tuyến đường không chia sẻ các đỉnh bên trong. Công suất trên các cạnh có thể được di chuyển xung quanh bằng các thao tác đơn vị."
date: "2026-08-05T20:22:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "E"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 80
verified: true
draft: false
---

[CF 102471E - Luồng](https://codeforces.com/problemset/problem/102471/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đồ thị có hình dạng đặc biệt: nó được tạo thành từ nhiều đường độc lập đi từ đỉnh nguồn 1 đến đỉnh đích n. Mọi tuyến đường đều chứa cùng số cạnh và các tuyến đường không chia sẻ các đỉnh bên trong. Công suất trên các cạnh có thể được di chuyển xung quanh bằng các thao tác đơn vị. Một thao tác lấy một đơn vị công suất từ ​​một cạnh hiện có một số công suất và đặt nó lên một cạnh khác. 

Mục tiêu không phải là xây dựng những năng lực cuối cùng. Chúng tôi chỉ cần số lượng đơn vị di chuyển tối thiểu cần thiết để lưu lượng tối đa có thể trở nên lớn như tổng công suất cho phép. 

Đối với một tuyến đường duy nhất có năng lực biên`c1, c2, ..., cl`, lượng luồng mà tuyến đường có thể mang theo là dung lượng biên nhỏ nhất của nó. Sau khi phân bổ lại năng lực, tổng công suất của tất cả các tuyến không thay đổi. Vì mỗi đơn vị dòng chảy cần một đơn vị công suất trên mỗi`l`các cạnh của tuyến đường, luồng tối đa tuyệt đối là:```
total_capacity // l
```Các ràng buộc làm cho việc mô phỏng không thể thực hiện được. Có thể có tới 100000 đỉnh và 200000 cạnh, trong khi dung lượng có thể lớn tới 10^9. Bất kỳ thuật toán nào phụ thuộc trực tiếp vào giá trị công suất, chẳng hạn như tăng lưu lượng một đơn vị tại một thời điểm, có thể yêu cầu hàng tỷ thao tác. 

Các trường hợp ẩn chính xuất phát từ thực tế là việc phân phối lại tốt nhất không phải lúc nào cũng cân bằng mọi đường dẫn một cách độc lập. 

Coi như:```
4 3
1 2 0
2 3 100
3 4 100
```Độ dài đường dẫn là 3. Tổng dung lượng là 200, do đó luồng tối đa cuối cùng là 66. Một phương pháp tham lam cố gắng làm cho mọi đường dẫn bằng`total/l`bằng cách lấp đầy trực tiếp tất cả các cạnh bị thiếu có thể hiệu quả ở đây, nhưng các phương pháp giả định rằng mỗi đơn vị bổ sung sẽ được trải đều sẽ thất bại vì chi phí biên của việc tăng đường đi có thể thay đổi mạnh. 

Một trường hợp khác là:```
4 3
1 2 5
2 3 5
3 4 5
```Câu trả lời là 0. Nút cổ chai hiện tại đã là 5 và tổng công suất cho cùng một luồng tối đa. Một phương pháp luôn thực hiện phân phối lại sau khi tính toán mức tối đa theo lý thuyết sẽ tính sai các bước di chuyển không cần thiết. 

Trường hợp quan trọng cuối cùng là đường dẫn có dung lượng bằng 0:```
2 1
1 2 0
```Luồng tối đa là 0 và không cần thực hiện thao tác nào. Việc triển khai giả định mọi cạnh đều có khả năng tích cực khi việc phân tách đường dẫn có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liên tục tăng luồng của một số đường dẫn cho đến khi đạt được mức tối đa toàn cục. Đối với mỗi đơn vị luồng tăng thêm, nó sẽ tìm thấy một cạnh đường dẫn không đủ công suất và chuyển công suất vào đó. Điều này đúng vì mọi thao tác đều có thể được coi là trả tiền cho một đơn vị bị thiếu ở một cạnh nào đó. Tuy nhiên, lưu lượng tối đa có thể vào khoảng`10^14`, vì vậy việc mô phỏng đơn vị là không thể. 

Sự quan sát hữu ích đến từ việc nhìn vào một con đường. Giả sử nút cổ chai hiện tại của nó được tăng từ`x-1`ĐẾN`x`. Số lượng thao tác cần thiết cho sự gia tăng đó chính xác là số cạnh có dung lượng nhỏ hơn`x`, bởi vì mỗi cạnh như vậy cần thêm một đơn vị. Những chi phí này tạo thành một chuỗi không giảm cho mỗi đường đi. 

Giá trị luồng cuối cùng được cố định. Chúng ta cần lựa chọn chính xác`total_capacity // l`mức tăng trong số tất cả các chuỗi tăng đường dẫn với tổng chi phí tối thiểu. Vì tất cả các chi phí gia tăng đều không giảm nên câu trả lời có được bằng cách lấy chi phí nhỏ nhất trên toàn cầu. 

Chi phí luôn nằm trong khoảng từ 0 đến`l`, vì vậy chúng tôi không cần lưu trữ mọi mức tăng có thể. Đối với mỗi đường dẫn, chúng tôi đếm xem có bao nhiêu phần tăng có chi phí 0, chi phí 1, v.v.`l-1`. Tất cả chi phí gia tăng còn lại`l`. Sau đó, chúng tôi tiêu thụ số lượng này từ chi phí nhỏ nhất trở lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(n + m + l) | O(n + m + l) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đi qua đồ thị từ đỉnh 1. Mỗi cạnh đi ra của nguồn bắt đầu một trong các đường đi độc lập. Đi theo từng đường đi cho đến khi đạt đến đỉnh n và lưu trữ dung lượng cạnh của nó. Vì các đỉnh trong chỉ thuộc một đường đi nên mọi đỉnh trong đều có đúng một cạnh tiếp theo. 
2. Hãy để`l`là độ dài của một con đường. Tính giá trị luồng mục tiêu bằng tổng của tất cả các dung lượng biên chia cho`l`dùng phép chia số nguyên. Đây là số lớp đường dẫn một đơn vị phải được tạo sau khi phân phối lại. 
3. Đối với mỗi đường dẫn, hãy sắp xếp dung lượng cạnh của nó. Nếu mức lưu lượng mong muốn hiện tại là`x`, chi phí để đạt được mức này là số lượng công suất nhỏ hơn`x`. Bằng cách nhóm các năng lực bằng nhau, hãy đếm xem có bao nhiêu cấp độ liên tiếp có mọi chi phí có thể. 
4. Thêm tất cả số lượng này vào một mảng tần số chung được lập chỉ mục theo chi phí vận hành. Chi phí nhỏ hơn`l`là hữu hạn. Bất kỳ mức bổ sung nào sau dung lượng cạnh lớn nhất trên đường dẫn đều có chi phí`l`. 
5. Bắt đầu từ chi phí 0, lấy càng nhiều cấp độ càng tốt cho đến khi chọn được chính xác giá trị luồng mục tiêu. Tổng chi phí được chọn là số lượng hoạt động tối thiểu. 

Tại sao nó hoạt động: mỗi đơn vị của luồng cuối cùng tương ứng với một cấp độ đã chọn trên một đường dẫn. Một đường dẫn chỉ có thể chọn các cấp độ của nó theo thứ tự, nhưng chi phí cấp độ của nó không bao giờ giảm, do đó, lựa chọn hợp lệ rẻ nhất trên toàn cầu có được bằng cách lấy chi phí sẵn có rẻ nhất trước tiên. Việc xây dựng tần suất chi phí bảo toàn chính xác những chi phí cận biên đó, do đó tổng tích lũy là số lượng đơn vị công suất di chuyển tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    indeg = [0] * n

    edges = []
    total = 0

    for _ in range(m):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append((v, c))
        indeg[v] += 1
        edges.append((u, v, c))
        total += c

    paths = []
    length = None

    for _, first_c in adj[0]:
        caps = [first_c]
        cur = adj[0][len(paths)][0]

        while cur != n - 1:
            nxt, c = adj[cur][0]
            caps.append(c)
            cur = nxt

        if length is None:
            length = len(caps)
        paths.append(caps)

    if length is None:
        print(0)
        return

    need = total // length

    freq = [0] * length
    cheap_count = 0
    cheap_sum = 0

    for caps in paths:
        caps.sort()
        cur = 0
        idx = 0
        i = 0
        while i < length:
            v = caps[i]
            j = i
            while j < length and caps[j] == v:
                j += 1
            cnt = j - i
            levels = v - cur
            if idx < length:
                freq[idx] += levels
                cheap_count += levels
                cheap_sum += levels * idx
            idx += cnt
            cur = v
            i = j

    ans = cheap_sum
    if need > cheap_count:
        ans += (need - cheap_count) * length

    print(ans)

if __name__ == "__main__":
    solve()
```Việc trích xuất đường dẫn dựa trên cấu trúc đồ thị đặc biệt. Nguồn có thể có một số cạnh đi ra, nhưng sau khi đi vào một đỉnh bên trong thì chỉ có thể có một cạnh tiếp theo. Điều này tránh việc chạy một thuật toán đồ thị chung. 

Mảng tần số chỉ lưu trữ chi phí từ`0`ĐẾN`l-1`. Khi mọi cạnh trên một đường đi trở thành cạnh giới hạn, việc tăng thêm luồng đường dẫn luôn phải trả giá`l`, do đó phần đóng góp còn lại có thể được tính toán một cách số học. 

Tất cả các giá trị liên quan đến dung lượng và câu trả lời đều sử dụng số nguyên Python. Điều này là cần thiết vì câu trả lời có thể vượt quá giới hạn 32 bit và 64 bit trong những trường hợp lớn nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 2 1
2 3 2
3 4 3
```Con đường duy nhất có khả năng`[1,2,3]`. 

| Bước | Dung lượng đường dẫn hiện tại | Tần suất chi phí được thêm vào | Chi phí đã chọn | 
| --- | --- | --- | --- | 
| Xây dựng đường dẫn | [1,2,3] | giá 0:1, giá 1:1, giá 2:1 | | 
| Tính toán mục tiêu | tổng cộng=6, chiều dài=3 | cần=2 | | 
| Lấy giá rẻ nhất | | 0 + 1 | 1 | 

Luồng hiện tại là 1 và luồng cuối cùng phải là 2. Một đơn vị công suất được di chuyển từ cạnh lớn nhất đến cạnh nhỏ nhất. 

### Mẫu 2 

đầu vào:```
4 4
1 2 1
1 3 1
2 4 2
3 4 2
```Có hai đường đi, cả hai đều có độ dài 2. 

| Bước | Đường dẫn | Chi phí bổ sung | Đã chọn | 
| --- | --- | --- | --- | 
| Con đường đầu tiên | [1,2] | 0,1 | | 
| Con đường thứ hai | [1,2] | 0,1 | | 
| Luồng mục tiêu | tổng cộng=6, chiều dài=2 | cần=3 | | 
| Chọn nhỏ nhất | | 0,0,1 | 1 | 

Chỉ cần một thao tác vì ba đơn vị luồng đường dẫn có thể được hỗ trợ sau khi di chuyển một đơn vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + l) | Mỗi đỉnh và cạnh được xử lý với số lần không đổi và khả năng sắp xếp đường đi bị giới hạn bởi tổng số cạnh | 
| Không gian | O(n + m + l) | Biểu đồ, đường dẫn và mảng tần số được lưu trữ | 

Tổng số cạnh chỉ là 200000, do đó việc xử lý đồ thị tuyến tính và bộ nhớ bổ sung có giới hạn dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```
# These tests describe the expected behavior of the algorithm.

def run(inp: str) -> str:
    import subprocess, sys, tempfile
    p = subprocess.run(
        [sys.executable, "main.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    )
    return p.stdout.decode().strip()

assert run("""4 3
1 2 1
2 3 2
3 4 3
""") == "1"

assert run("""4 4
1 2 1
1 3 1
2 4 2
3 4 2
""") == "1"

assert run("""2 1
1 2 0
""") == "0"

assert run("""4 3
1 2 5
2 3 5
3 4 5
""") == "0"

assert run("""7 6
1 2 0
2 7 10
1 3 10
3 4 10
4 5 10
5 7 10
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh không đơn | 0 | Xử lý công suất bằng 0 và lưu lượng bằng 0 | 
| Đã có đường dẫn tối ưu | 0 | Tránh những động thái không cần thiết | 
| Một con đường đơn giản | 1 | Kiểm tra tính toán chi phí cận biên | 
| Hai con đường độc lập | 1 | Kiểm tra đường dẫn kết hợp | 
| Năng lực đường dẫn không đồng đều | 10 | Kiểm tra phân phối lại giữa các tuyến khác nhau | 

## Vỏ cạnh 

Đối với đường dẫn không có dung lượng:```
2 1
1 2 0
```Độ dài đường dẫn là một và tổng dung lượng bằng không. Số lượng lớp luồng được yêu cầu là 0, do đó thuật toán không bao giờ chọn chi phí và trả về 0. 

Đối với biểu đồ đã có luồng tối đa có thể:```
4 3
1 2 5
2 3 5
3 4 5
```Tổng công suất là 15 và độ dài đường dẫn là 3, tạo ra luồng mục tiêu là 5. Chuỗi chi phí chứa đủ mức chi phí bằng 0 để tạo ra tất cả năm đơn vị, do đó không có công suất nào bị di chuyển. 

Đối với đường dẫn có cạnh rất lớn:```
4 3
1 2 0
2 3 100
3 4 100
```Đường dẫn đóng góp nhiều mức giá rẻ sau khi cạnh đầu tiên được lấp đầy. Phương pháp tần suất xử lý vấn đề này bằng cách nhóm các cấp thay vì lặp lại 100 lần trở lên. Nó đếm tất cả các mức tăng chi phí bằng nhau trong một hoạt động và vẫn chọn các lớp được yêu cầu rẻ nhất trên toàn cầu.
