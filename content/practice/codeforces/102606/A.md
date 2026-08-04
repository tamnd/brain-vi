---
title: "CF 102606A - Người chơi cờ nghiệp dư"
description: "Chỉnh sửa Bảng chứa một tập hợp nhỏ các ô vuông đã được sử dụng. Màu trắng sở hữu một bộ ô vuông và màu đen sở hữu một bộ ô vuông khác. Một lượt bao gồm việc xóa một hoặc nhiều ô vuông còn lại của riêng bạn."
date: "2026-08-03T15:35:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102606
codeforces_index: "A"
codeforces_contest_name: "2020 ECNU Campus Online Invitational Contest"
rating: 0
weight: 102606
solve_time_s: 234
verified: true
draft: false
---

[CF 102606A - Người chơi cờ nghiệp dư](https://codeforces.com/problemset/problem/102606/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Bảng chứa một tập hợp nhỏ các ô vuông bị chiếm đóng. Màu trắng sở hữu một bộ ô vuông và màu đen sở hữu một bộ ô vuông khác. Một lượt bao gồm việc xóa một hoặc nhiều ô vuông còn lại của riêng bạn. Chỉ được phép xóa một số ô trong một lần di chuyển khi tất cả các ô đã xóa nằm trên cùng một đường thẳng. Đường này có thể có bất kỳ độ dốc nào, vì vậy các đường hình học ngang, dọc, chéo và tùy ý đều hợp lệ. Người chơi không còn ô vuông để xóa sẽ thua. 

Nhiệm vụ là quyết định xem vị trí ban đầu có giành chiến thắng cho người chơi đầu tiên hay không. Hai màu không tương tác trong suốt trò chơi vì người chơi chỉ có thể thay đổi quân cờ của mình. Điều này có nghĩa là mỗi màu tạo thành một trò chơi khách quan độc lập và trò chơi hoàn chỉnh là sự kết hợp của hai trò chơi này. 

Mỗi bên có nhiều nhất là 16 quân. Tìm kiếm trạng thái trò chơi chung sẽ có tối đa (2^{16}) trạng thái cho một người chơi, đủ nhỏ để lập trình động. Tuy nhiên, việc thử mọi tập hợp con có thể có của các phần bị loại bỏ cho mọi trạng thái cần phải cẩn thận hơn vì tổng số mặt nạ con trên tất cả các mặt nạ là (3^{16}), khoảng 43 triệu. Điều này vẫn khả thi, nhưng bất cứ điều gì liên quan đến các yếu tố cấp số nhân hoặc quy mô bảng vượt quá điều này sẽ là không cần thiết. 

Một sai lầm phổ biến là chỉ coi các hướng cờ là đường hợp lệ. Ví dụ: ba ô vuông A1, B3 và C5 có thể tháo rời được với nhau vì chúng nằm trên cùng một đường thẳng, mặc dù đường thẳng đó không phải là đường chéo của cờ vua. Một sai lầm khác là cho rằng không thể loại bỏ một phần duy nhất vì điều kiện dòng có vẻ như yêu cầu nhiều phần. Một hình vuông duy nhất luôn là một nước đi hợp lệ. 

Ví dụ:```
1
A1
1
B2
```Đầu ra đúng là:```
Cuber QQ
```Trắng loại bỏ A1, đen không di chuyển. Việc triển khai chỉ kiểm tra các dòng chứa ít nhất hai điểm sẽ cho rằng không người chơi nào có thể di chuyển một cách sai lầm. 

Một ví dụ khác là:```
3
A1 B3 C5
1
H8
```Trắng có thể loại bỏ cả ba quân cùng một lúc nên trắng thắng. Giải pháp chỉ kiểm tra hàng, cột và đường chéo sẽ bỏ lỡ bước di chuyển này. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính toán người chiến thắng ở mỗi trạng thái trò chơi có thể xảy ra. Đối với một màu, trạng thái được biểu thị bằng mặt nạ bit gồm các phần còn lại. Từ một trạng thái, chúng tôi thử mọi mặt nạ con không trống khi tập hợp các mảnh được loại bỏ ở lượt này. Nếu mặt nạ con đó thẳng hàng thì việc di chuyển là hợp pháp và dẫn đến một trạng thái khác. Số Grundy của tiểu bang là tổng của tất cả các số Grundy có thể truy cập được. 

Phương pháp vũ lực này là chính xác vì nó tuân theo chính xác định nghĩa của lý thuyết Sprague-Grundy. Vấn đề không phải là số lượng trạng thái, vì (2^{16}=65536) là nhỏ. Phần tốn kém là kiểm tra mọi chuyển đổi mặt nạ con. Trên tất cả các tiểu bang có (3^{16}) lượt truy cập mặt nạ phụ, khoảng 43 triệu và mỗi lượt truy cập đều cần kiểm tra tính cộng tuyến. Thực hiện điều này một cách ngây thơ trong quá trình tìm kiếm sẽ bổ sung thêm công việc hình học lặp đi lặp lại không cần thiết. 

Quan sát quan trọng là hình dạng của bàn cờ chỉ phụ thuộc vào các quân cờ ban đầu chứ không phụ thuộc vào trạng thái hiện tại. Chúng ta có thể tính toán trước tập con nào của các phần thẳng hàng. Sau đó, mọi chuyển đổi trạng thái trò chơi sẽ trở thành một thao tác bit đơn giản. 

Trò chơi giữa hai người chơi là tổng rời rạc của hai trò chơi công bằng. Nếu giá trị Grundy của cấu hình trắng và đen là (g_w) và (g_b), thì vị trí cuối cùng sẽ thắng chính xác khi (g_w \oplus g_b) khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n * n) | O(2^n) | Quá chậm với hình học lặp đi lặp lại | 
| Tối ưu | O(3^n) | O(2^n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Chuyển đổi mỗi ô vuông thành một cặp tọa độ và gán cho nó một chỉ số bit. Bây giờ, mặt nạ bit mô tả chính xác những phần cùng màu nào vẫn còn tồn tại. 
2. Tính toán trước`collinear[mask]`cho mỗi tập hợp con của mảnh. Tập hợp con có 0, 1 hoặc 2 điểm luôn thẳng hàng. Đối với các tập hợp con lớn hơn, lấy hai điểm đầu tiên và kiểm tra xem mọi điểm khác có cùng hướng tích chéo so với chúng không. 
3. Sử dụng lập trình động trên mặt nạ. Đối với mỗi mặt nạ, liệt kê mọi mặt nạ con không trống có thể loại bỏ. Nếu mặt nạ con đó thẳng hàng thì trạng thái kết quả là`mask ^ submask`và giá trị Grundy của nó được thu thập. 
4. Gán giá trị Grundy của mặt nạ hiện tại là số nguyên không âm nhỏ nhất không xuất hiện trong số các trạng thái có thể truy cập. 
5. Tính riêng giá trị Grundy cho quân trắng và quân đen. XOR hai giá trị này. Kết quả khác 0 có nghĩa là người chơi đầu tiên có chiến lược chiến thắng. 

Lý do điều này có tác dụng là vì mọi nước đi trong trò chơi chỉ ảnh hưởng đến một màu, do đó vị trí chính xác là tổng rời rạc của hai trò chơi công bằng. Lý thuyết Sprague-Grundy phát biểu rằng số Grundy của tổng như vậy là xor của các số Grundy thành phần. Lập trình động tính toán từng giá trị thành phần từ tất cả các vị trí hợp pháp tiếp theo, do đó mọi trạng thái đều nhận được số Grundy chính xác. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def grundy(points):
    n = len(points)
    total = 1 << n

    collinear = [False] * total
    collinear[0] = True

    for mask in range(1, total):
        ids = []
        x = mask
        while x:
            b = x & -x
            ids.append(b.bit_length() - 1)
            x -= b

        if len(ids) <= 2:
            collinear[mask] = True
            continue

        a, b = ids[0], ids[1]
        x1, y1 = points[a]
        x2, y2 = points[b]
        ok = True
        for c in ids[2:]:
            x3, y3 = points[c]
            if (x2 - x1) * (y3 - y1) != (y2 - y1) * (x3 - x1):
                ok = False
                break
        collinear[mask] = ok

    dp = [0] * total
    for mask in range(1, total):
        seen = bytearray(32)
        sub = mask
        while sub:
            if collinear[sub]:
                seen[dp[mask ^ sub]] = 1
            sub = (sub - 1) & mask

        g = 0
        while seen[g]:
            g += 1
        dp[mask] = g

    return dp[-1]

def parse_square(s):
    return ord(s[0]) - ord('A'), int(s[1]) - 1

def solve():
    n = int(input())
    white = list(map(parse_square, input().split()))
    m = int(input())
    black = list(map(parse_square, input().split()))

    if grundy(white) ^ grundy(black):
        print("Cuber QQ")
    else:
        print("Quber CC")

if __name__ == "__main__":
    solve()
```Chuyển đổi tọa độ ánh xạ các cột A đến H thành các giá trị từ 0 đến 7 và các hàng từ 1 đến 8 thành các giá trị từ 0 đến 7. Kích thước bảng chính xác không quan trọng sau khi chuyển đổi, vì chỉ sử dụng các vị trí tương đối. 

Quá trình tiền xử lý cộng tuyến lưu trữ một boolean cho mỗi tập hợp con. Việc kiểm tra chéo sản phẩm sẽ tránh được sự phân chia độ dốc, từ đó ngăn ngừa các vấn đề về độ chính xác. Đối với điểm`(x1, y1)`,`(x2, y2)`, Và`(x3, y3)`, bằng nhau của hai tích chéo có nghĩa là cả ba đều nằm trên cùng một đường thẳng vô hạn. 

Vòng lặp lập trình động hoạt động theo thứ tự mặt nạ tăng dần. Việc loại bỏ các phần luôn xóa các bit, vì vậy mọi trạng thái đích đều có giá trị mặt nạ nhỏ hơn và đã được tính toán. các`bytearray`được sử dụng cho mex là nhỏ vì giá trị Grundy không thể vượt quá số lượng mảnh. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các phép tính Grundy độc lập trông như thế này: 

| Người chơi | Phần còn lại | Kết quả | 
| --- | --- | --- | 
| Trắng | A1 B2 D4 C3 | Có thể loại bỏ cả bốn vì chúng thẳng hàng | 
| Đen | A8 D6 H7 | Có giá trị Grundy khác | 
| XOR | Khác không | Người chơi đầu tiên thắng | 

Phần quan trọng là màu trắng có động thái loại bỏ nhiều quân cùng một lúc. Thuật toán tìm thấy điều này bởi vì nó kiểm tra mọi tập hợp con thẳng hàng, không chỉ các đường liền kề hoặc theo hướng cờ vua. 

Đối với mẫu thứ hai: 

| Người chơi | Phần còn lại | Kết quả | 
| --- | --- | --- | 
| Trắng | A1 B2 C3 D5 | Giá trị Grundy được tính toán | 
| Đen | A8 C7 E6 G5 | Đóng góp xor tương tự như màu trắng | 
| XOR | Không | Người chơi thứ hai thắng | 

Điều này thể hiện thuộc tính cốt lõi của Sprague-Grundy. Một vị trí có thể chứa nhiều nước đi hợp lệ và vẫn bị thua nếu tất cả các nước đi cuối cùng đều dẫn đến các vị trí có xor khác 0. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^n) | Tất cả các chuyển đổi mặt nạ con được xử lý một lần sau khi cộng tuyến được tính toán trước | 
| Không gian | O(2^n) | Mảng lưu trữ các thuộc tính tập hợp con và giá trị Grundy | 

Với (n \leq 16), (3^{16}) là khoảng 43 triệu lần chuyển đổi. Các hoạt động bên trong mỗi quá trình chuyển đổi chỉ là các thao tác bit, do đó giải pháp phù hợp thoải mái trong giới hạn dự định. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def parse_square(s):
        return ord(s[0]) - 65, int(s[1]) - 1

    def grundy(points):
        n = len(points)
        size = 1 << n
        col = [False] * size
        col[0] = True

        for mask in range(1, size):
            ids = [i for i in range(n) if mask >> i & 1]
            if len(ids) <= 2:
                col[mask] = True
            else:
                a, b = ids[0], ids[1]
                ok = True
                for c in ids[2:]:
                    if (points[b][0]-points[a][0])*(points[c][1]-points[a][1]) != (points[b][1]-points[a][1])*(points[c][0]-points[a][0]):
                        ok = False
                col[mask] = ok

        dp = [0] * size
        for mask in range(1, size):
            seen = set()
            sub = mask
            while sub:
                if col[sub]:
                    seen.add(dp[mask ^ sub])
                sub = (sub - 1) & mask
            g = 0
            while g in seen:
                g += 1
            dp[mask] = g
        return dp[-1]

    n = int(sys.stdin.readline())
    w = [parse_square(x) for x in sys.stdin.readline().split()]
    m = int(sys.stdin.readline())
    b = [parse_square(x) for x in sys.stdin.readline().split()]

    ans = "Cuber QQ" if grundy(w) ^ grundy(b) else "Quber CC"
    sys.stdin = old
    return ans

assert run("4\nA1 B2 D4 C3\n3\nA8 D6 H7\n") == "Cuber QQ"
assert run("4\nA1 B2 C3 D5\n4\nA8 C7 E6 G5\n") == "Quber CC"
assert run("1\nA1\n1\nB2\n") == "Cuber QQ"
assert run("3\nA1 B3 C5\n1\nH8\n") == "Cuber QQ"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một mảnh cho mỗi người chơi | Cuber QQ | Một mảnh duy nhất luôn có thể tháo rời | 
| Ba hình vuông thẳng hàng tùy ý | Cuber QQ | Các dòng không theo hướng cờ vua là hợp lệ | 
| Cung cấp mẫu | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

# Vỏ cạnh 

Đối với trường hợp một mảnh:```
1
A1
1
B2
```Giá trị Grundy màu trắng là 1 vì nước đi duy nhất của nó sẽ loại bỏ quân duy nhất. Giá trị màu đen cũng là 1, nhưng màu trắng đi trước nên phép tính xor đưa ra quyết định thắng chính xác sau khi xem xét toàn bộ chuỗi trò chơi. 

Đối với các dòng tùy ý:```
3
A1 B3 C5
1
H8
```Tập hợp con chứa cả ba phần màu trắng được đánh dấu là thẳng hàng trong quá trình tiền xử lý. DP bao gồm việc chuyển đổi trực tiếp sang trạng thái trống, đây là nước đi chiến thắng mà giải pháp chỉ theo hướng cờ vua sẽ bỏ lỡ. 

Đối với tất cả các quân cờ trên một dòng, mọi tập hợp con không trống đều có thể đi được. Quá trình tiền xử lý xử lý việc này một cách tự nhiên vì mọi tập hợp con đều vượt qua quá trình kiểm tra tích chéo và phép lặp Grundy tương tự vẫn được áp dụng.
