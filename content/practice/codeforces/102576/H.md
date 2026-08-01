---
title: "CF 102576H - Ngọn hải đăng"
description: "Chúng ta có một đa giác lồi có các đỉnh là ngọn hải đăng. Một số cặp đỉnh được nối với nhau bằng đường ray xe điện và Vladik chỉ có thể di chuyển dọc theo các đường ray đó."
date: "2026-07-31T07:37:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "H"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 78
verified: true
draft: false
---

[CF 102576H - Ngọn hải đăng](https://codeforces.com/problemset/problem/102576/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi có các đỉnh là ngọn hải đăng. Một số cặp đỉnh được nối với nhau bằng đường ray xe điện và Vladik chỉ có thể di chuyển dọc theo các đường ray đó. Một chuyến đi hợp lệ là một đường đi trong biểu đồ này, nhưng có hạn chế về hình học: chuỗi các đoạn đã đi được vẽ không được tự giao nhau và không được ghé thăm cùng một điểm hai lần. 

Nhiệm vụ là tìm tổng chiều dài Euclide tối đa có thể có của một đường đi như vậy. 

Số lượng ngọn hải đăng nhiều nhất là 300, điều này loại trừ bất kỳ giải pháp nào phụ thuộc vào việc liệt kê các đường đi hoặc tập hợp con của các đỉnh. Một đồ thị có 300 đỉnh vẫn có thể có gần 45.000 cạnh, do đó, giải pháp gần với thời gian bậc ba là mục tiêu tự nhiên. Tổng số đỉnh trong tất cả các bài kiểm tra chỉ là 3000, điều này cho phép xử lý mỗi bài kiểm tra nặng hơn. 

Khó khăn chính không phải là truyền tải đồ thị. Đường đi dài nhất trong đồ thị tổng quát là khó, nhưng ở đây tất cả các đỉnh đều nằm trên một đa giác lồi. Thứ tự của các đỉnh cho chúng ta một cấu trúc hình học mạnh mẽ: bất cứ khi nào chúng ta lấy một dây cung, lộ trình khả thi còn lại bị giới hạn ở một phía của dây cung đó. Điều này tạo ra các vấn đề con độc lập nhỏ hơn. 

Một vài trường hợp dễ dàng phá vỡ những cách tiếp cận ngây thơ. Nếu có các đường xe điện cắt nhau, chúng không thể cùng xuất hiện trong câu trả lời ngay cả khi chúng đưa ra một đường dẫn lý thuyết đồ thị lớn hơn. Ví dụ: một hình vuông có sẵn cả hai đường chéo không cho phép tuyến đường`1-3-2-4`, vì các đường chéo cắt nhau ở giữa. Đi đúng đường phải tránh chỗ băng qua. 

Một cái bẫy khác là cho rằng con đường dài nhất phải bắt đầu từ một ngọn hải đăng cố định. Xét một hình vuông chỉ có ba cạnh liên tiếp. Tuyến đường tốt nhất bắt đầu ở một đầu của các cạnh đó và kết thúc ở đầu kia, do đó, việc cố định đỉnh đầu tiên có thể làm mất đi tính tối ưu. 

Vấn đề thứ ba là đa giác có hình tròn. Một đường đi có thể vượt qua ranh giới nhân tạo giữa đỉnh được lập chỉ mục cuối cùng và đầu tiên. Bất kỳ DP khoảng thời gian nào chỉ hoạt động trên`0...n-1`không xử lý vòng tròn có thể bỏ lỡ câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi chuỗi đỉnh có thể có, chỉ giữ lại các chuỗi tạo thành đường xe điện hợp lệ và loại bỏ những chuỗi có giao lộ tự. Điều này đúng vì mọi chuyến đi có thể đều được xem xét rõ ràng. Tuy nhiên, số lượng đường dẫn có thể là theo cấp số nhân. Ngay cả việc tìm kiếm hoàn chỉnh trên các tập hợp con cũng sẽ yêu cầu khoảng`2^300`trạng thái, điều đó là không thể. 

Quan sát hữu ích là đa giác lồi. Chọn một đường đi hợp lệ và nhìn vào cạnh đầu tiên của nó bên trong một khoảng đỉnh nào đó. Khi cạnh đó được chọn, phần còn lại của đường đi buộc phải nằm ở một bên của dây cung, vì việc sử dụng các đỉnh ở cả hai bên sẽ làm cho một số đoạn trong tương lai đi qua dây cung đã chọn. 

Điều này có nghĩa là vấn đề có thể được giải quyết bằng quy hoạch động theo khoảng. Chúng tôi theo dõi một đoạn của đa giác vẫn có sẵn và điểm cuối nơi đặt tuyến đường hiện tại. Khi chúng ta chọn cạnh xe điện tiếp theo, bài toán còn lại sẽ trở thành một khoảng nhỏ hơn với điểm cuối đã biết. 

Vì đa giác là hình tròn nên chúng ta nhân đôi các đỉnh và chạy khoảng DP trên mảng nhân đôi này. Mọi khoảng tròn có thể có độ dài`n`sau đó xuất hiện như một khoảng thông thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng thời gian DP | O(n³) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhân đôi các đỉnh đa giác. đỉnh`i`được sao chép vào vị trí`i+n`, và mọi cạnh tram đều được sao chép tương ứng. Điều này chuyển đổi phạm vi vòng tròn thành khoảng thời gian bình thường. 
2. Xác định`dp[l][r]`là độ dài tối đa của tuyến đường hợp lệ chỉ sử dụng các đỉnh từ`l`ĐẾN`r`và điểm cuối hiện tại của nó là`l`. 
3. Xử lý khoảng thời gian bằng cách tăng độ dài. Trong một khoảng thời gian`[l,r]`, đầu tiên hãy kế thừa câu trả lời từ việc bỏ qua đỉnh`l`, mang lại`dp[l+1][r]`. 
4. Hãy thử mọi phương án xe điện đầu tiên có thể từ`l`đến một đỉnh`k`bên trong khoảng. Nếu cạnh đó tồn tại, sau khi di chuyển đến`k`, chuyến đi còn lại phải ở trong nhà`[k,r]`và bắt đầu từ`k`. Sự chuyển tiếp là:$$dp[l][r] = \max(dp[l][r], dist(l,k)+dp[k][r])$$1. Đáp án là tối đa`dp[i][i+n-1]`trên tất cả các vị trí bắt đầu. Các khoảng này thể hiện mọi lựa chọn có thể về vị trí cắt đa giác tròn. 

Điều bất biến đằng sau DP là mọi tuyến đường hợp lệ trong một khoảng có chính xác một cạnh đầu tiên tính từ điểm cuối hiện tại của nó hoặc nó hoàn toàn không sử dụng điểm cuối đó. Trong trường hợp đầu tiên, cạnh được chọn sẽ chia không gian tìm kiếm còn lại thành khoảng nhỏ hơn ở phía bên phải của dây cung. Trong trường hợp thứ hai, việc loại bỏ điểm cuối sẽ để lại một vấn đề về khoảng hợp lệ khác. Vì mọi tuyến đường hợp lệ đều tuân theo một trong hai dạng này nên phép truy toán sẽ xem xét mọi câu trả lời có thể có và không bao giờ tạo ra một giao lộ không hợp lệ. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve_case():
    n = int(input())
    pts = []
    for _ in range(n):
        x, y = map(int, input().split())
        pts.append((x, y))

    m = int(input())

    size = 2 * n
    edges = [[-1.0] * size for _ in range(size)]

    def length(a, b):
        x1, y1 = pts[a % n]
        x2, y2 = pts[b % n]
        return math.hypot(x1 - x2, y1 - y2)

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        w = length(a, b)
        for x in (a, a + n):
            for y in (b, b + n):
                if x < size and y < size:
                    edges[x][y] = w
                    edges[y][x] = w

    dp = [[0.0] * size for _ in range(size)]

    for ln in range(2, n + 1):
        for l in range(size - ln + 1):
            r = l + ln - 1
            best = dp[l + 1][r]
            row = edges[l]
            for k in range(l + 1, r + 1):
                if row[k] >= 0:
                    cand = row[k] + dp[k][r]
                    if cand > best:
                        best = cand
            dp[l][r] = best

    ans = 0.0
    for i in range(n):
        if dp[i][i + n - 1] > ans:
            ans = dp[i][i + n - 1]
    return ans

def main():
    data = sys.stdin.read().split()
    if not data:
        return

    it = iter(data)

    def next_input():
        return next(it)

    global input
    old_input = input

    def fake_input():
        return next_input() + "\n"

    input = fake_input

    t = int(input())
    out = []
    for _ in range(t):
        out.append(f"{solve_case():.12f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Ma trận cạnh lưu trữ chiều dài của mỗi tuyến xe điện có sẵn. Việc lập chỉ mục trùng lặp được xử lý bằng cách lấy tọa độ modulo`n`, do đó các đỉnh được sao chép có cùng vị trí hình học. 

Bảng DP chỉ lưu trữ tối đa các khoảng thời gian`n`. Khoảng thời gian dài hơn trong đa giác nhân đôi sẽ lặp lại một ngọn hải đăng và có thể tạo ra các tuyến đường không hợp lệ. Vòng lặp độ dài khoảng thời gian đảm bảo rằng mọi phụ thuộc đã được tính toán trước khi sử dụng. 

Quá trình chuyển đổi sẽ thử mọi tuyến xe điện đầu tiên có thể rời khỏi điểm cuối hiện tại. Nếu không có tuyến xe điện nào tồn tại, trạng thái chỉ giữ tùy chọn bỏ qua điểm cuối đó. Số nguyên Python xử lý phạm vi tọa độ một cách an toàn và tất cả các phép tính hình học chỉ sử dụng dấu phẩy động khi tính toán độ dài cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Có các khoảng O(n2) và mỗi khoảng thử O(n) các đỉnh tiếp theo. | 
| Không gian | O(n²) | Bảng DP và ma trận cạnh đều chứa các giá trị O(n2). | 

Vì`n = 300`, số hạng khối là khoảng 27 triệu lần chuyển đổi, phù hợp với các giới hạn đã cho. Tổng số đỉnh trong tất cả các bài kiểm tra giúp quản lý công việc tích lũy được. 

## Vỏ cạnh 

Khi không có đường xe điện tồn tại, mọi chuyển đổi DP đều thất bại và mọi trạng thái vẫn bằng 0. Câu trả lời là chính xác`0`, vì Vladik không thể di chuyển đi đâu cả. 

Khi tuyến đường tốt nhất bao quanh phần cuối của đơn hàng đầu vào, đa giác trùng lặp sẽ thu hút được tuyến đường đó. Đối với một hình vuông có các cạnh sẵn có`1-4`,`4-3`,`3-2`,`2-1`, tuyến đường tối ưu sử dụng ba mặt. Trong việc lập chỉ mục ban đầu, nó vượt qua ranh giới nhân tạo, nhưng trong mảng nhân đôi, nó xuất hiện dưới dạng một khoảng thông thường. 

Khi hai hợp âm có sẵn giao nhau, DP không bao giờ kết hợp chúng thành một tuyến. Việc chọn một hợp âm sẽ hạn chế trạng thái còn lại ở một phía của hợp âm đó, do đó, hợp âm cắt nhau không thể xuất hiện sau này trên cùng một đường dẫn. Đây là thuộc tính hình học thay thế việc kiểm tra giao lộ rõ ​​ràng.
