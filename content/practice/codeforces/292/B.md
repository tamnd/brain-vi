---
title: "CF 292B - Cấu trúc liên kết mạng"
description: "Chúng ta được cho một đồ thị vô hướng được kết nối đại diện cho một mạng máy tính. Nhiệm vụ không phải là phân tích cấu trúc đồ thị tùy ý mà là xác định xem đồ thị có khớp chính xác với một trong ba cấu trúc liên kết cụ thể hay không. Cấu trúc liên kết bus chỉ đơn giản là một đường dẫn."
date: "2026-06-05T17:19:26+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 292
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2013 - Round 1"
rating: 1200
weight: 292
solve_time_s: 111
verified: true
draft: false
---

[CF 292B - Cấu trúc liên kết mạng](https://codeforces.com/problemset/problem/292/B) 

**Đánh giá:** 1200 
**Tags:** đồ thị, thực hiện 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng được kết nối đại diện cho một mạng máy tính. Nhiệm vụ không phải là phân tích cấu trúc đồ thị tùy ý mà là xác định xem đồ thị có khớp chính xác với một trong ba cấu trúc liên kết cụ thể hay không. 

Cấu trúc liên kết bus chỉ đơn giản là một đường dẫn. Có chính xác hai đỉnh là điểm cuối có bậc 1 và mọi đỉnh còn lại đều có bậc 2. 

Cấu trúc liên kết vòng là một chu trình. Mỗi đỉnh đều có bậc 2. 

Cấu trúc liên kết sao có một đỉnh trung tâm được kết nối với tất cả các đỉnh khác. Trung tâm có bằng cấp$n-1$và mọi đỉnh còn lại đều có bậc 1. 

Biểu đồ được đảm bảo được kết nối nên chúng ta không cần phải tự mình xác minh kết nối. Chúng ta chỉ cần kiểm tra mô hình bậc của các đỉnh và quyết định cấu trúc liên kết nào phù hợp. 

Các ràng buộc cho phép lên đến$10^5$đỉnh và$10^5$các cạnh. Bất kỳ thuật toán nào đi qua biểu đồ nhiều lần hoặc thực hiện các kiểm tra theo cặp tốn kém đều không cần thiết. Vì toàn bộ đầu vào chỉ chứa$O(m)$thông tin cạnh, quét tuyến tính trên các cạnh và đỉnh là mục tiêu tự nhiên. MỘT$O(n+m)$nghiệm dễ dàng nằm trong giới hạn, trong khi bất kỳ phương trình bậc hai nào cũng sẽ quá lớn đối với$10^5$đỉnh. 

Một số trường hợp cạnh có thể gây ra sự phân loại không chính xác nếu chúng ta chỉ nhìn vào số cạnh. 

Coi như:```
4 3
1 2
1 3
1 4
```Đồ thị có$n-1$các cạnh, giống như một đường dẫn, nhưng trình tự mức độ của nó là$[3,1,1,1]$. Câu trả lời đúng là:```
star topology
```Một giải pháp chỉ kiểm tra xem$m=n-1$sẽ gọi sai đây là xe buýt. 

Một ví dụ khác là:```
4 4
1 2
2 3
3 4
4 1
```Mọi đỉnh đều có bậc 2 nên:```
ring topology
```Chỉ nhìn vào khả năng kết nối và số cạnh là không đủ. Chúng ta phải kiểm tra độ đỉnh. 

Một ví dụ cuối cùng:```
5 5
1 2
2 3
3 4
4 5
2 5
```Bằng cấp là$[1,3,2,2,2]$. Biểu đồ này được kết nối nhưng không khớp với cấu trúc nào được yêu cầu. 

Câu trả lời đúng là:```
unknown topology
```## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng nhận ra từng cấu trúc liên kết bằng cách tái cấu trúc rõ ràng hình dạng của nó. Ví dụ: chúng ta có thể bắt đầu từ mọi đỉnh, đi qua các đỉnh lân cận, xác minh xem biểu đồ tạo thành một đường đi, một chu trình hay một ngôi sao. Vì biểu đồ được kết nối nên các phép duyệt như vậy là chính xác nhưng chúng liên quan đến logic biểu đồ nhiều hơn mức cần thiết. 

Trong trường hợp xấu nhất, việc thực hiện liên tục các bước đi trên đồ thị hoặc kiểm tra cấu trúc liên kết có thể dẫn đến$O(n(n+m))$, lớn hơn nhiều so với mức cần thiết khi$n$đạt tới$10^5$. 

Quan sát quan trọng là cả ba cấu trúc liên kết đều được đặc trưng hoàn toàn bởi độ đỉnh. 

Đối với một vành, mọi đỉnh đều có bậc 2. 

Đối với một xe buýt, có đúng hai đỉnh có bậc 1 và mọi đỉnh còn lại đều có bậc 2. 

Đối với một ngôi sao, có đúng một đỉnh có bậc$n-1$và mọi đỉnh còn lại đều có bậc 1. 

Bởi vì đồ thị được đảm bảo liên thông nên các điều kiện bậc này là đủ. Chúng tôi không cần bất kỳ xác minh cấu trúc bổ sung nào. 

Điều này làm giảm toàn bộ vấn đề về việc đếm độ đỉnh trong khi đọc các cạnh và sau đó kiểm tra một vài mẫu đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O(n+m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. 
2. Tạo một mảng`deg`kích thước$n$được khởi tạo về 0. 
3. Cho mọi cạnh$(u,v)$, tăng`deg[u]`Và`deg[v]`. 

Bậc của một đỉnh chính xác là số cạnh liên quan, do đó sau khi xử lý tất cả các cạnh, mảng chứa chuỗi bậc hoàn chỉnh. 
4. Kiểm tra xem mọi đỉnh có bậc 2 hay không. 

Nếu đúng thì đồ thị này là một cấu trúc liên kết vòng. 
5. Đếm xem có bao nhiêu đỉnh có bậc 1 và bao nhiêu đỉnh có bậc 2. 

Nếu có đúng hai đỉnh bậc 1 và$n-2$đỉnh bậc 2, đồ thị là cấu trúc liên kết bus. 
6. Đếm xem có bao nhiêu đỉnh có bậc$n-1$. 

Nếu có đúng một đỉnh như vậy và đỉnh còn lại$n-1$các đỉnh có bậc 1 thì đồ thị là cấu trúc liên kết sao. 
7. Nếu không có điều kiện nào ở trên xảy ra, hãy xuất ra`"unknown topology"`. 

### Tại sao nó hoạt động 

Biểu đồ được kết nối, đó là sự đảm bảo quan trọng. 

Đồ thị liên thông trong đó mỗi đỉnh có bậc 2 phải là một chu trình đơn nên nó là một vành. 

Một đồ thị liên thông có đúng hai đỉnh bậc 1 và tất cả các đỉnh còn lại bậc 2 phải là một đường đi duy nhất, vì vậy nó là một bus. 

Đồ thị liên thông có một đỉnh bậc$n-1$và tất cả những ngôi sao khác cấp 1 chỉ có thể là một ngôi sao có tâm ở đỉnh bậc cao đó. 

Vì các điều kiện này loại trừ lẫn nhau và mô tả đầy đủ ba cấu trúc liên kết bắt buộc nên thuật toán luôn trả về phân loại chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    deg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        deg[u - 1] += 1
        deg[v - 1] += 1

    if all(d == 2 for d in deg):
        print("ring topology")
        return

    cnt1 = sum(d == 1 for d in deg)
    cnt2 = sum(d == 2 for d in deg)

    if cnt1 == 2 and cnt2 == n - 2:
        print("bus topology")
        return

    cnt_center = sum(d == n - 1 for d in deg)

    if cnt_center == 1 and cnt1 == n - 1:
        print("star topology")
        return

    print("unknown topology")

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. 

Mảng độ là thông tin duy nhất chúng ta cần từ biểu đồ. Mỗi cạnh đóng góp một độ cho mỗi điểm cuối, do đó việc xử lý tất cả các cạnh một lần là đủ. 

Thứ tự kiểm tra không đặc biệt quan trọng vì ba cấu trúc liên kết hợp lệ loại trừ lẫn nhau đối với các ràng buộc nhất định. Đầu tiên, mã sẽ kiểm tra vòng, sau đó là xe buýt, sau đó là ngôi sao. 

Một điểm tinh tế là chúng ta sử dụng số đỉnh bậc 1 đã được tính toán khi kiểm tra điều kiện sao. Một ngôi sao hợp lệ phải có chính xác một tâm độ$n-1$và mọi đỉnh khác đều phải có bậc 1. 

Một chi tiết khác là việc chuyển đổi từ đánh số đỉnh dựa trên 1 trong đầu vào sang lập chỉ mục dựa trên 0 trong mảng Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2
2 3
3 4
```Trình tự độ trở thành: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 

Đếm: 

| cnt1 | cnt2 | cnt_center | 
| --- | --- | --- | 
| 2 | 2 | 0 | 

Tình trạng xe buýt khớp chính xác, vì vậy câu trả lời là:```
bus topology
```Điều này thể hiện mẫu đường dẫn đặc trưng: hai điểm cuối và tất cả các đỉnh bên trong bậc 2. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
1 3
1 4
1 5
```Trình tự độ: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 4 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Đếm: 

| cnt1 | cnt2 | cnt_center | 
| --- | --- | --- | 
| 4 | 0 | 1 | 

Vì một đỉnh có bậc$n-1=4$và tất cả những người khác đều có độ 1, câu trả lời là:```
star topology
```Ví dụ này cho thấy tại sao chỉ thông tin về độ cũng đủ để xác định một ngôi sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần duyệt qua tất cả các cạnh và một vài lần quét mảng độ | 
| Không gian | O(n) | Lưu trữ bậc của từng đỉnh | 

Với nhiều nhất$10^5$đỉnh và$10^5$các cạnh, một giải pháp tuyến tính chỉ thực hiện vài trăm nghìn thao tác và phù hợp thoải mái trong giới hạn thời gian. Mảng độ chỉ yêu cầu$O(n)$bộ nhớ, thấp hơn nhiều so với giới hạn có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())
    deg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        deg[u - 1] += 1
        deg[v - 1] += 1

    if all(d == 2 for d in deg):
        return "ring topology"

    cnt1 = sum(d == 1 for d in deg)
    cnt2 = sum(d == 2 for d in deg)

    if cnt1 == 2 and cnt2 == n - 2:
        return "bus topology"

    cnt_center = sum(d == n - 1 for d in deg)

    if cnt_center == 1 and cnt1 == n - 1:
        return "star topology"

    return "unknown topology"

# provided sample
assert run(
"""4 3
1 2
2 3
3 4
"""
) == "bus topology", "sample 1"

# ring
assert run(
"""4 4
1 2
2 3
3 4
4 1
"""
) == "ring topology", "simple ring"

# star
assert run(
"""5 4
1 2
1 3
1 4
1 5
"""
) == "star topology", "simple star"

# unknown
assert run(
"""5 5
1 2
2 3
3 4
4 5
2 5
"""
) == "unknown topology", "extra edge"

# minimum valid ring
assert run(
"""4 4
1 2
2 3
3 4
4 1
"""
) == "ring topology", "minimum size ring"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường đi trên 4 đỉnh | cấu trúc liên kết xe buýt | Phát hiện xe buýt tiêu chuẩn | 
| 4 chu kỳ | cấu trúc liên kết vòng | Tất cả các đỉnh bậc 2 | 
| Ngôi sao có 5 đỉnh | cấu trúc liên kết sao | Trung tâm cấp cao đơn | 
| Đường dẫn cộng thêm cạnh | cấu trúc liên kết chưa biết | Từ chối các cấu trúc gần như hợp lệ | 
| Nhẫn cỡ tối thiểu | cấu trúc liên kết vòng | Giá trị biên của n | 

## Vỏ cạnh 

Hãy xem xét đồ thị ngôi sao:```
4 3
1 2
1 3
1 4
```Trình tự độ là$[3,1,1,1]$. Trong quá trình quét,`cnt1 = 3`,`cnt2 = 0`, và một đỉnh có bậc`n - 1 = 3`. The bus test fails because there are not exactly two degree-1 vertices. Thử nghiệm ngôi sao thành công, tạo ra:```
star topology
```Bây giờ hãy xem xét một chiếc nhẫn:```
4 4
1 2
2 3
3 4
4 1
```Mọi đỉnh đều có bậc 2. Lần kiểm tra đầu tiên ngay lập tức thành công và đưa ra:```
ring topology
```Không cần phân tích thêm. 

Cuối cùng, hãy xem xét một biểu đồ được kết nối không khớp với bất kỳ mẫu nào:```
5 5
1 2
2 3
3 4
4 5
2 5
```Trình tự độ là$[1,3,2,2,2]$. Điều kiện vòng không thành công vì không phải mọi bậc đều bằng 2. Điều kiện bus không thành công vì chỉ có một đỉnh bậc 1. Điều kiện ngôi sao không thành công vì không có đỉnh nào có bậc$n-1=4$. Thuật toán trả về chính xác:```
unknown topology
```Những ví dụ này bao gồm các tình huống trong đó việc chỉ dựa vào số cạnh sẽ tạo ra sự phân loại không chính xác, trong khi cách tiếp cận dựa trên mức độ xử lý chúng một cách chính xác.
