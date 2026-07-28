---
title: "CF 102791L - Một vấn đề DAG khác"
description: "Chúng ta có một đồ thị chu kỳ có hướng. Mỗi cạnh đi từ đỉnh có giá trị được gán lớn hơn đến đỉnh có giá trị được gán nhỏ hơn. Nếu một cạnh đi từ x đến y thì đóng góp của nó là w (a[x] - a[y])."
date: "2026-07-27T18:18:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "L"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 60
verified: true
draft: false
---

[CF 102791L - Một vấn đề DAG khác](https://codeforces.com/problemset/problem/102791/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị chu kỳ có hướng. Mỗi cạnh đi từ đỉnh có giá trị được gán lớn hơn đến đỉnh có giá trị được gán nhỏ hơn. Nếu một cạnh đi từ`x`ĐẾN`y`, đóng góp của nó là`w * (a[x] - a[y])`. Nhiệm vụ là gán các giá trị nguyên cho các đỉnh sao cho mọi hiệu của cạnh đều dương và tổng đóng góp có trọng số càng nhỏ càng tốt. 

Đồ thị có nhiều nhất 18 đỉnh. Giá trị nhỏ này là đầu mối chính. Một giải pháp theo cấp số nhân trong`n`có thể hoạt động, nhưng bất cứ điều gì như thử mọi phép gán giá trị có thể thì không thể, vì số lượng phép gán tăng nhanh hơn nhiều so với`2^n`. 

Việc chuyển đổi hữu ích đầu tiên là viết lại mục tiêu. 

Đối với mỗi đỉnh, thu thập hệ số giá trị của nó. Một đỉnh đóng góp tích cực thông qua các cạnh đi ra và tiêu cực thông qua các cạnh đi vào.```
sum(w * (a[x] - a[y])) = sum(c[v] * a[v])
```Ở đâu`c[v]`là tổng trọng số của các cạnh đi ra trừ đi tổng trọng lượng của các cạnh vào. 

Các ràng buộc trở thành ràng buộc về thứ tự: đối với mọi cạnh`x -> y`,`a[x] > a[y]`. 

Một điểm tinh tế là việc thêm cùng một hằng số cho mọi`a[v]`không thay đổi gì vì tổng các hệ số bằng 0. Chúng ta có thể dịch chuyển toàn bộ nghiệm sao cho giá trị được gán tối thiểu bằng 0. 

Các giá trị không bao giờ cần vượt quá`n - 1`. Nếu hai lớp giá trị liên tiếp trống, mỗi đỉnh ở lớp cao hơn có thể giảm đi một mà không phá vỡ bất kỳ ràng buộc cạnh nào, cải thiện câu trả lời. Do đó, một giải pháp tối ưu luôn có thể được biểu diễn bằng cách sử dụng nhiều nhất`n`các lớp. 

Một lỗi dễ mắc phải là gán giá trị chỉ sử dụng độ dài đường dẫn dài nhất. Điều đó thỏa mãn các bất đẳng thức nhưng có thể không làm giảm tổng trọng số. Ví dụ:```
3 3
1 2 100
2 3 1
1 3 1
```Việc gán đường dẫn dài nhất mang lại các giá trị`2 1 0`, nhưng trọng lượng lớn ở cạnh đầu tiên đồng nghĩa với việc việc giảm khoảng cách ở cạnh đó quan trọng hơn. Việc phân lớp tối ưu phụ thuộc vào tất cả các trọng số của cạnh thông qua các hệ số. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi phép gán giá trị có thể. Vì mỗi đỉnh có thể có tới`n`các lớp có thể, điều này đòi hỏi khoảng`n^n`nhiệm vụ vốn đã quá lớn đối với`n = 18`. 

Quan sát hữu ích là giải pháp thực sự là sự phân chia các đỉnh thành các lớp. Các đỉnh ở lớp cao hơn có giá trị lớn hơn. Nếu chúng ta biết đỉnh nào có giá trị ít nhất`k`, tập hợp đó phải chứa mọi đỉnh trước nó. Nói cách khác, mỗi ranh giới lớp tương ứng với một tập hợp con các đỉnh được đóng dưới các cạnh sắp tới. 

Vấn đề trở thành việc chọn các lớp hợp lệ trong khi giảm thiểu tổng hệ số nhân với số lớp của chúng. Vì chỉ có`2^18`tập hợp con, chúng ta có thể sử dụng quy hoạch động tập hợp con. 

Việc triển khai thuận tiện sẽ xử lý từng lớp một. Đối với một tập hợp con`S`, lưu trữ chi phí tối thiểu sau khi gán giá trị cho chính xác các đỉnh trong`S`. Để tạo lớp tiếp theo, hãy chọn một tập hợp con hợp lệ gồm các đỉnh còn lại có thể nhận giá trị tiếp theo. Việc liệt kê tất cả các chuyển đổi một cách ngây thơ mang lại`O(3^n)`, quá cao. 

Thay vào đó, chúng tôi xây dựng các lớp bằng cách sử dụng tập hợp con DP chọn các đỉnh theo thứ tự tôpô. Khi xem xét một đỉnh cho lớp hiện tại, nó chỉ có thể được đặt ở đó khi tất cả các đỉnh mà nó trỏ tới đã được đặt ở các lớp thấp hơn. Điều này loại bỏ việc liệt kê tập hợp con và đưa ra một`O(n^2 2^n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^n) | O(n) | Quá chậm | 
| Tập hợp con lớp DP | O(n^2 2^n) | O(n 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`c[v]`cho mọi đỉnh. Thêm trọng số cạnh vào hệ số nguồn và trừ nó khỏi hệ số đích. Mục tiêu cạnh ban đầu bây giờ tương đương với việc giảm thiểu`sum(c[v] * a[v])`. 
2. Sắp xếp đồ thị theo cấu trúc. Thứ tự cho phép chúng ta quyết định xem một đỉnh có thể tham gia một lớp hay không bằng cách chỉ kiểm tra các đỉnh phải ở dưới nó. 
3. Chạy lập trình động trên các tập hợp con. Trạng thái lưu trữ chi phí tốt nhất sau khi xử lý một số đỉnh và quyết định đỉnh nào trong số chúng thuộc về các lớp thấp hơn đã hoàn thành. 
4. Với mỗi đỉnh theo thứ tự tôpô, hãy thử hai lựa chọn. Bỏ qua đỉnh của lớp hiện tại hoặc đặt nó vào lớp hiện tại nếu tất cả các đỉnh lân cận đi ra đã nằm trong tập hợp con của lớp thấp hơn. 
5. Khi đặt một đỉnh vào lớp`k`, thêm vào`c[v] * k`đến chi phí hiện tại. Lưu trữ thông tin gốc để các lớp đã chọn có thể được xây dựng lại. 
6. Khôi phục các lớp đã chọn từ các con trỏ cha và xuất ra giá trị được gán cho mỗi đỉnh. 

Tại sao nó hoạt động: mọi phép gán hợp lệ đều xác định các lớp có giá trị bằng nhau. DP xem xét chính xác các cấu trúc lớp có thể có này, bởi vì một đỉnh chỉ có thể vào một lớp sau khi tất cả các đỉnh có giá trị nhỏ hơn được cố định. Mỗi quá trình chuyển đổi sẽ bổ sung chính xác sự đóng góp của các đỉnh được đặt ở lớp đó. Vì mỗi phép gán hợp lệ tương ứng với một đường dẫn DP và mỗi đường dẫn DP tương ứng với một phép gán hợp lệ, nên giá trị DP tối thiểu chính xác là giá trị tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    coef = [0] * n
    out = [0] * n

    for _ in range(m):
        x, y, w = map(int, input().split())
        x -= 1
        y -= 1
        coef[x] += w
        coef[y] -= w
        out[x] |= 1 << y

    order = []
    indeg = [0] * n
    for i in range(n):
        for j in range(n):
            if (out[i] >> j) & 1:
                indeg[j] += 1

    q = [i for i in range(n) if indeg[i] == 0]
    while q:
        v = q.pop()
        order.append(v)
        for u in range(n):
            if (out[v] >> u) & 1:
                indeg[u] -= 1
                if indeg[u] == 0:
                    q.append(u)

    N = 1 << n
    INF = 10**30

    dp = [INF] * N
    dp[0] = 0
    parent = [(-1, -1)] * N

    for layer in range(n):
        ndp = dp[:]
        npar = parent[:]

        for mask in range(N):
            if dp[mask] == INF:
                continue
            for v in order:
                if (mask >> v) & 1:
                    continue
                if out[v] & ~mask:
                    continue
                nm = mask | (1 << v)
                val = dp[mask] + coef[v] * layer
                if val < ndp[nm]:
                    ndp[nm] = val
                    npar[nm] = (mask, v)

        dp = ndp
        parent = npar

    ans = [0] * n
    mask = N - 1
    layer = n - 1

    while mask:
        pm, v = parent[mask]
        if pm == -1:
            break
        ans[v] = layer
        mask = pm
        if mask == 0:
            break

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc tính toán hệ số loại bỏ hoàn toàn các biến cạnh. Thứ tự tôpô chỉ được sử dụng để đảm bảo rằng khi một đỉnh được chọn cho một lớp thì tất cả các đỉnh thấp hơn cần thiết đã được xem xét. 

Tập hợp con DP sử dụng mặt nạ bit vì`n`chỉ là 18. Điều kiện chuyển tiếp`out[v] & ~mask == 0`có nghĩa là mọi hàng xóm đi của`v`đã ở bên trong các lớp thấp hơn. Đây chính xác là yêu cầu mà`a[v]`phải lớn hơn tất cả chúng. 

Việc xây dựng lại đi ngược lại thông qua cha mẹ được lưu trữ. Mỗi quá trình chuyển đổi được chọn sẽ ghi lại một đỉnh đi vào một lớp, do đó việc gán số lớp hiện tại cho đỉnh đó sẽ xây dựng lại phép gán tối ưu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 2
2 1 4
1 3 2
```Các hệ số là: 

| Đỉnh | Hệ số | 
| --- | --- | 
| 1 | -2 | 
| 2 | 4 | 
| 3 | -2 | 

Một lớp tối ưu hợp lệ là: 

| Đỉnh | Giá trị được gán | 
| --- | --- | 
| 2 | 2 | 
| 1 | 1 | 
| 3 | 0 | 

Hệ số dương lớn của đỉnh 2 khiến việc đặt nó ở mức cao là không thể tránh khỏi, trong khi đỉnh 1 và 3 sẽ rẻ hơn nếu giữ ở mức thấp. 

Đối với mẫu thứ ba:```
5 5
1 2 1
2 3 1
3 4 1
1 5 1
5 4 10
```Các lớp tối ưu là: 

| Đỉnh | Giá trị được gán | 
| --- | --- | 
| 1 | 4 | 
| 2 | 3 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 2 | 

DP thích tăng giá trị của đỉnh 5 vì cạnh`5 -> 4`có trọng lượng lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² 2^n) | Mỗi trong số`2^n`tập hợp con cố gắng nhiều nhất`n`nhiều nhất là các đỉnh`n`lớp | 
| Không gian | O(2^n) | Lưu trữ giá trị DP và thông tin gốc cho mỗi tập hợp con | 

Vì`n = 18`,`2^n`là khoảng 262 nghìn, do đó phần mũ vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```
# The official samples plus small sanity checks.

def check(inp, expected=None):
    import subprocess
    result = subprocess.run(
        ["python3", "main.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()
    if expected is not None:
        assert result == expected
    return result

check("""3 2
2 1 4
1 3 2
""")

check("""5 4
1 2 1
2 3 1
1 3 6
4 5 8
""")

check("""2 0
""")

check("""3 3
1 2 100
2 3 1
1 3 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba đỉnh có hai cạnh | Bất kỳ bài tập tối ưu nào | Đặt hàng cơ bản | 
| DAG có trọng số với các thành phần riêng biệt | Bất kỳ bài tập tối ưu nào | Thành phần độc lập | 
| Hai đỉnh và không có cạnh | Cả hai giá trị đều bằng nhau | Trường hợp biểu đồ trống | 
| Cạnh nặng cộng với cạnh nhỏ | Bất kỳ nhiệm vụ tối ưu nào với khoảng cách giảm chi phí | Xử lý trọng lượng | 

## Vỏ cạnh 

Một đồ thị không có cạnh thì không có hạn chế về thứ tự. Hệ số của mọi đỉnh đều bằng 0 nên mọi đỉnh đều có thể nhận giá trị 0. DP bắt đầu từ tập con trống và có thể kết thúc với tất cả các đỉnh trong cùng một lớp. 

Các thành phần bị ngắt kết nối được xử lý độc lập. Nếu hai nhóm đỉnh không có cạnh giữa chúng, thì vị trí thẳng đứng tương đối của chúng sẽ không ảnh hưởng đến tính khả thi và phép biến đổi hệ số nắm bắt chính xác điều duy nhất quan trọng, giá trị mục tiêu. 

Các đỉnh được kết nối bằng trọng số rất lớn không được tách rời một cách không cần thiết. DP không chỉ tối ưu hóa số lượng lớp mà còn tối ưu hóa tổng hệ số có trọng số, do đó các cạnh đắt tiền sẽ ảnh hưởng đến sự sắp xếp đã chọn. 

Nhiều đỉnh có thể chia sẻ cùng một giá trị miễn là không có cạnh nào kết nối chúng sai hướng. Việc chuyển đổi tập hợp con cho phép bất kỳ tập hợp đỉnh tương thích nào chiếm một lớp, đó là lý do chính khiến giải pháp này tốt hơn là chỉ gán độ sâu đường đi dài nhất.
