---
title: "CF 1869B - Du lịch 2D"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp mô tả một hệ thống di chuyển có trọng số hoàn chỉnh trên máy bay. Mỗi thành phố là một điểm có tọa độ nguyên và chúng ta được phép bay trực tiếp giữa bất kỳ cặp thành phố nào."
date: "2026-06-08T23:30:07+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math", "shortest-paths", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1869
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 896 (Div. 2)"
rating: 1100
weight: 1869
solve_time_s: 92
verified: true
draft: false
---

[CF 1869B - Du lịch 2D](https://codeforces.com/problemset/problem/1869/B) 

**Đánh giá:** 1100 
**Tags:** hình học, toán học, đường đi ngắn nhất, sắp xếp 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp mô tả một hệ thống di chuyển có trọng số hoàn chỉnh trên máy bay. Mỗi thành phố là một điểm có tọa độ nguyên và chúng ta được phép bay trực tiếp giữa bất kỳ cặp thành phố nào. Chi phí của một chuyến bay phụ thuộc vào việc cả hai điểm cuối có nằm trong k thành phố đầu tiên hay không. Nếu cả hai thành phố đều lớn, chuyến bay sẽ miễn phí. Nếu không, cái giá phải trả là khoảng cách Manhattan giữa chúng. 

Nhiệm vụ là tìm tổng chi phí tối thiểu để đi từ thành phố xuất phát a đến thành phố đích b, nơi chúng ta có thể thực hiện bất kỳ chuỗi chuyến bay và điểm dừng trung gian nào. Vì các chuyến bay tạo thành một biểu đồ hoàn chỉnh với các cạnh chi phí bằng 0 đặc biệt giữa các thành phố lớn, nên vấn đề cơ bản là tìm đường đi ngắn nhất trong biểu đồ có trọng số dày đặc với quy tắc chi phí rất có cấu trúc. 

Các ràng buộc tạo ra một cách tiếp cận đường đi ngắn nhất ngây thơ với các cạnh O(n^2) trên mỗi đường biên thử nghiệm nhưng có thể chấp nhận được với tổng số n 2·10^5 nếu được xử lý cẩn thận. Tuy nhiên, việc chạy Dijkstra với một biểu đồ hoàn chỉnh rõ ràng là không thể, vì nó đòi hỏi phải xem xét các cạnh O(n^2) cho mỗi trường hợp thử nghiệm. 

Một khó khăn tinh tế xuất hiện khi k lớn. Nếu có nhiều thành phố lớn, chúng sẽ tạo thành một nhóm chi phí bằng 0, nghĩa là chúng ta có thể di chuyển tự do giữa chúng. Điều này tạo ra một “xương sống dịch chuyển tức thời” có thể giảm đáng kể chi phí, nhưng chỉ khi chúng ta sử dụng nó một cách tối ưu. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ bao gồm các tình huống: 

Sự lựa chọn tham lam luôn sử dụng thành phố gần nhất đã thất bại vì dịch chuyển tức thời có thể vượt qua khoảng cách lớn. 

Ví dụ: nếu hai thành phố lớn cách xa nhau nhưng cả hai đều gần các khu vực khác nhau thì việc di chuyển qua các thành phố đó là miễn phí trong nội bộ nhưng việc đến được các thành phố đó thì không. Một chiến lược ngây thơ mà bỏ qua việc sử dụng các thành phố lớn làm trung tâm trung gian sẽ đánh giá quá cao chi phí. 

Một trường hợp thất bại khác là khi k = 0, nghĩa là không có cạnh tự do. Sau đó, bài toán giảm xuống còn đường đi ngắn nhất thuần túy trong đồ thị Manhattan hoàn chỉnh, trong đó đường đi tối ưu luôn đi thẳng từ a đến b. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý vấn đề dưới dạng biểu đồ có trọng số hoàn chỉnh và chạy thuật toán Dijkstra với tất cả các cạnh O(n^2). Đối với mỗi cặp thành phố, chúng tôi tính toán khoảng cách Manhattan trừ khi cả hai đều là chính, trong trường hợp đó trọng số cạnh bằng 0. Điều này đúng vì mọi chuyến bay có thể đều được xem xét rõ ràng. 

Tuy nhiên, điều này đòi hỏi phải đánh giá các chuyển đổi O(n^2) cho mỗi trường hợp thử nghiệm, điều này trở nên không thể thực hiện được khi n lớn. Với tổng số n lên tới 2·10^5, ngay cả một lần vượt qua biểu đồ dày đặc cũng sẽ vượt quá giới hạn thời gian. 

Quan sát quan trọng là cấu trúc của các cạnh không có chi phí sẽ thu gọn tất cả các thành phố lớn thành một thành phần được kết nối duy nhất với chi phí nội bộ bằng không. Khi bạn vào nhóm thành phố lớn, bạn có thể di chuyển miễn phí trong đó và thoát khỏi bất kỳ thành phố lớn nào. Điều này có nghĩa là thay vì xem xét tất cả các chuyển đổi từ chính sang chính, chúng tôi chỉ quan tâm đến việc liệu chúng tôi có thể sử dụng “trung tâm miễn phí” này để giảm khoảng cách giữa hai điểm hay không. 

Vì vậy, con đường ngắn nhất sẽ giảm xuống còn một số lượng nhỏ các chiến lược ứng cử viên. Hoặc chúng ta đi thẳng từ a đến b bằng khoảng cách Manhattan, hoặc chúng ta sử dụng cụm thành phố lớn làm cầu nối dịch chuyển tức thời: đi từ a đến một thành phố lớn nào đó, di chuyển tự do giữa các chuyên ngành, sau đó đi từ một thành phố lớn khác đến b. Vì sự di chuyển nội bộ giữa các chuyên ngành là tự do nên điều này tương đương với việc giảm thiểu: 

khoảng cách (a, u) + khoảng cách (v, b), trong đó u và v là bất kỳ thành phố lớn nào. 

Điều này làm giảm vấn đề kiểm tra một tập hợp nhỏ các điểm ứng viên thay vì tất cả các cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Dijkstra trên biểu đồ hoàn chỉnh) | O(n²) | O(n²) | Quá chậm | 
| Tối ưu (sử dụng giảm thành phố lớn) | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính khoảng cách trực tiếp từ Manhattan giữa thành phố a và thành phố b. Đây luôn là câu trả lời hợp lệ vì chúng tôi có thể bay trực tiếp giữa hai thành phố bất kỳ. 
2. Nếu không có thành phố lớn nào (k = 0), hãy trả về ngay khoảng cách trực tiếp này. Không có các cạnh tự do thì không có cấu trúc trung gian nào có thể cải thiện đường đi. 
3. Nếu có ít nhất một thành phố lớn, hãy tính cách tốt nhất để sử dụng nhóm thành phố lớn làm cầu nối. Đối với điều này, chúng tôi xem xét mức độ rẻ mà chúng tôi có thể kết nối a với bất kỳ thành phố lớn nào và b với bất kỳ thành phố lớn nào. 
4. Tính khoảng cách tối thiểu từ thành phố a đến thành phố lớn bất kỳ. Đây là cách rẻ nhất để vào mạng dịch chuyển tức thời miễn phí. 
5. Tính khoảng cách tối thiểu từ bất kỳ thành phố lớn nào đến thành phố b. Đây là cách rẻ nhất để thoát khỏi mạng miễn phí. 
6. Cộng hai giá trị này để tạo thành câu trả lời ứng viên bằng cách sử dụng tuyến đường thành phố lớn. 
7. Câu trả lời cuối cùng là mức tối thiểu giữa đường đi thẳng và đường cầu trong thành phố lớn. 

Lý do chúng tôi chỉ cần khoảng cách tối thiểu đến tập hợp chính là vì chi phí di chuyển bên trong tập hợp đó bằng 0, vì vậy chúng tôi không bao giờ cần theo dõi thành phố lớn cụ thể nào mà chúng tôi vào hoặc ra ngoài chi phí ra vào tốt nhất có thể. 

Tại sao nó hoạt động: mọi đường đi tối ưu đều có thể được chia thành ba đoạn, từ a đến thành phố lớn nào đó, bên trong thành phần chính và từ thành phố lớn đến b. Phân khúc trung bình có chi phí bằng 0 và do đó không ảnh hưởng đến tính tối ưu. Vì tất cả các thành phố lớn đều có thể truy cập lẫn nhau với chi phí bằng 0 nên chỉ có điểm vào và ra tốt nhất mới quan trọng, thu gọn toàn bộ biểu đồ con thành một nút có trọng số bằng 0 duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist(i, j, xs, ys):
    return abs(xs[i] - xs[j]) + abs(ys[i] - ys[j])

def solve():
    n, k, a, b = map(int, input().split())
    a -= 1
    b -= 1

    x = [0] * n
    y = [0] * n

    for i in range(n):
        x[i], y[i] = map(int, input().split())

    direct = abs(x[a] - x[b]) + abs(y[a] - y[b])

    if k == 0:
        print(direct)
        return

    majors = list(range(k))

    min_a_to_major = float('inf')
    min_b_to_major = float('inf')

    for i in majors:
        min_a_to_major = min(min_a_to_major,
                             abs(x[a] - x[i]) + abs(y[a] - y[i]))
        min_b_to_major = min(min_b_to_major,
                             abs(x[b] - x[i]) + abs(y[b] - y[i]))

    via_major = min_a_to_major + min_b_to_major

    print(min(direct, via_major))

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai tính toán trực tiếp khoảng cách Manhattan giữa các điểm liên quan. Sự đơn giản hóa chính là hạn chế sự chú ý chỉ vào k thành phố lớn khi đánh giá chi phí xuất nhập cảnh. Điều này tránh việc xây dựng bất kỳ cấu trúc biểu đồ nào. 

Phải cẩn thận khi lập chỉ mục vì đầu vào sử dụng nhãn thành phố dựa trên 1, nhưng mảng dựa trên 0. Một điểm tinh tế khác là xử lý riêng k = 0 để tránh việc lặp lại không cần thiết trên một tập chính trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 2, a = 3, b = 5
major cities: 1, 2
```Chúng tôi tính toán khoảng cách trực tiếp đầu tiên. 

| Bước | khoảng cách a-to-b | phút a→chính | phút b→chính | qua chính | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3→5 = được tính toán | thông tin | thông tin | thông tin | thông tin | 
| quét chuyên ngành 1 | - | cập nhật | cập nhật | - | - | 
| quét chuyên ngành 2 | - | cuối cùng | cuối cùng | tính toán | phút | 

Sau khi đánh giá cả hai thành phố lớn, chúng tôi tìm ra lối vào rẻ nhất từ ​​3 vào nhóm chính và lối ra rẻ nhất đến 5. Thuật toán xác nhận rằng việc sử dụng thành phố 1 làm trung tâm giúp giảm tổng chi phí so với di chuyển trực tiếp. 

Điều này chứng tỏ cách tập hợp chính hoạt động như một lối tắt ngay cả khi nó không nằm trên đường đi hình học ngắn nhất. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 1, a = 3, b = 1
```Chỉ có thành phố 1 là lớn. 

| Bước | trực tiếp | phút a→chính | phút b→chính | qua chính | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| tính toán | 3→1 | dist(3,1) | dist(1,1)=0 | tổng hợp | phút | 

Ở đây, tuyến đường tối ưu sử dụng thành phố lớn làm trục xoay với chi phí bằng 0. Mặc dù tồn tại khoảng cách trực tiếp, cấu trúc cho phép cấu trúc chi phí có thể khác nhau tùy thuộc vào hình học. 

Điều này cho thấy ngay cả một thành phố lớn cũng có thể thay đổi đáng kể hành vi định tuyến tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi quét ở hầu hết k thành phố lớn và tính toán khoảng cách theo thời gian không đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ tọa độ của tất cả các thành phố | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số thành phố, phù hợp thoải mái trong giới hạn 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, k, a, b = map(int, input().split())
        a -= 1
        b -= 1

        x = []
        y = []
        for _ in range(n):
            xi, yi = map(int, input().split())
            x.append(xi)
            y.append(yi)

        direct = abs(x[a] - x[b]) + abs(y[a] - y[b])

        if k == 0:
            return direct

        min_a = float('inf')
        min_b = float('inf')

        for i in range(k):
            d1 = abs(x[a] - x[i]) + abs(y[a] - y[i])
            d2 = abs(x[b] - x[i]) + abs(y[b] - y[i])
            min_a = min(min_a, d1)
            min_b = min(min_b, d2)

        return min(direct, min_a + min_b)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# provided samples
assert run("""5
6 2 3 5
0 0
1 -2
-2 1
-1 3
2 -2
-3 -3
2 0 1 2
-1000000000 -1000000000
1000000000 1000000000
7 5 4 2
154 147
-154 -147
123 456
20 23
43 20
998 244
353 100
3 1 3 1
0 10
1 20
2 30
4 3 2 4
0 0
-100 100
-1 -1
-1 0
""") == """4
4000000000
0
22
1"""

# custom cases
assert run("""2
2 0 1 2
0 0
5 0
3 1 1 3
0 0
1 0
2 0
""") == """5
2"""

assert run("""1
3 2 2 3
0 0
100 100
1 1
""") == """2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 0 trường hợp | khoảng cách trực tiếp | không được phép dịch chuyển tức thời | 
| chuỗi tuyến tính nhỏ | độ chính xác đường dẫn tối thiểu | sự đúng đắn không có chuyên ngành | 
| hình học hỗn hợp | tối ưu hóa trung tâm | sử dụng trục xoay của thành phố lớn | 

## Vỏ cạnh 

Khi k = 0, thuật toán không bao giờ đi vào vòng lặp thành phố lớn và trực tiếp trả về khoảng cách Manhattan giữa a và b. Điều này phù hợp với thực tế là không có cạnh nào có chi phí bằng 0 tồn tại, do đó mọi đường dẫn đều thu gọn thành một đường bay trực tiếp duy nhất. 

Khi k = n, mọi thành phố đều lớn nên mọi chi phí đi lại đều bằng 0. Thuật toán tính toán chính xác khoảng cách tối thiểu nhưng vẫn trả về 0 vì cả đầu vào và đầu ra thông qua chuyên ngành đều có giá bằng 0 trong mọi trường hợp. 

Khi a hoặc b là một thành phố lớn, chi phí ra vào sẽ tự động bằng 0. Khoảng thời gian tối thiểu được tính toán trên các thành phố lớn đương nhiên bao gồm chính thành phố đó, đảm bảo thuật toán không bỏ lỡ đường đi tối ưu nơi chúng tôi bắt đầu hoặc kết thúc bên trong thành phần chi phí bằng 0.
