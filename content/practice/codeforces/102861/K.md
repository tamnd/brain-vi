---
title: "CF 102861K - Giữa chúng ta"
description: "Chúng ta được cung cấp một biểu đồ tình bạn vô hướng. Mỗi đỉnh đại diện cho một người chơi và mỗi cạnh đại diện cho một mối quan hệ hữu nghị. Chúng ta cần chia người chơi thành nhiều nhất hai nhóm sao cho với mỗi người chơi, số bạn bè trong cùng một nhóm là số lẻ."
date: "2026-07-25T14:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "K"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 46
verified: true
draft: false
---

[CF 102861K - Giữa chúng ta](https://codeforces.com/problemset/problem/102861/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ tình bạn vô hướng. Mỗi đỉnh đại diện cho một người chơi và mỗi cạnh đại diện cho một mối quan hệ hữu nghị. Chúng ta cần chia người chơi thành nhiều nhất hai nhóm sao cho với mỗi người chơi, số bạn bè trong cùng một nhóm là số lẻ. Tên thật của hai nhóm không quan trọng, chỉ quan trọng là hai người chơi có được xếp cùng nhau hay không. 

Đầu vào cung cấp tối đa 100 người chơi và tình bạn của họ. Câu trả lời là`Y`nếu một số người chơi được phân công vào hai nhóm có thể thỏa mãn điều kiện bạn lẻ cho mọi người chơi, và`N`nếu không thì. 

Giới hạn nhỏ về số lượng người chơi là quan sát chính về mức độ phức tạp dự định. Với 100 biến, các thuật toán dựa trên đại số tuyến tính rất thực tế. Việc gán bạo lực sẽ thử mọi phép phân chia có thể, nghĩa là kiểm tra tới (2^{100}) khả năng, vượt xa khả năng mà bất kỳ chương trình nào cũng có thể xử lý. Chúng ta cần chuyển đổi điều kiện đồ thị thành một hệ thống có tối đa 100 ẩn số, trong đó việc loại bỏ Gaussian đủ nhanh. 

Có một số trường hợp cách tiếp cận trực quan có thể thất bại. Một sai lầm phổ biến là nghĩ rằng việc xếp mọi người chơi vào một nhóm sẽ luôn hiệu quả nếu tất cả các độ đều là số lẻ. Điều này sai khi một số đỉnh có bậc chẵn. Ví dụ:```
3 2
1 2
2 3
```Người chơi ở giữa có hai người bạn, vì vậy nếu mọi người ở cùng một nhóm, người chơi đó sẽ nhìn thấy hai người bạn, tức là chẵn. Đầu ra đúng là`N`. Một phương pháp chỉ kiểm tra tổng bậc của đồ thị sẽ bỏ qua thực tế là mỗi đỉnh có điều kiện riêng. 

Một trường hợp phức tạp khác là hai nhóm không cần phải cùng trống. Ví dụ:```
2 1
1 2
```Việc đưa cả hai người chơi vào cùng một nhóm sẽ mang lại cho mỗi người chơi một người bạn trong nhóm. Đầu ra đúng là`Y`. Một giải pháp buộc nhóm thứ hai không trống sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp thứ ba là khi tồn tại một số phân vùng hợp lệ. Ví dụ:```
4 4
1 2
2 3
3 4
4 1
```Chu trình có thể được chia thành nhiều cách và thuật toán chỉ cần tìm một phép gán hợp lệ. Không nên cho rằng có một phân vùng duy nhất tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là quyết định nhóm của mọi người chơi một cách độc lập. Đối với mỗi (2^P) phép gán có thể có, đối với mỗi người chơi, chúng tôi đếm có bao nhiêu đỉnh liền kề có cùng một nhóm được gán. Nếu tất cả số đếm đều là số lẻ thì phân vùng đó tồn tại. 

Cách tiếp cận này đúng vì nó kiểm tra mọi phân vùng có thể. Tuy nhiên, với (P=100), số lượng bài tập là (2^{100}), xấp xỉ (1,27 \times 10^{30}). Ngay cả khi việc kiểm tra một bài tập chỉ mất một vài thao tác thì điều này cũng không thể thực hiện được. 

Cấu trúc quan trọng của bài toán là điều kiện chỉ phụ thuộc vào tính chẵn lẻ. Chúng ta không cần chính xác số lượng bạn bè trong cùng một nhóm, chỉ cần số đó là số lẻ hay số chẵn. Tính chẵn lẻ tự nhiên dẫn đến số học modulo 2, trong đó mọi giá trị đều bằng 0 hoặc một. 

Hãy để (x_i) mô tả nhóm người chơi (i). Chúng tôi sử dụng (0) và (1) cho hai nhóm. Đối với hàng xóm (j), giá trị cho biết (j) có cùng nhóm với (i) hay không có thể được viết là: 

[ 
1 + x_i + x_j 
] 

khi các phép tính được thực hiện theo modulo 2. Biểu thức là một khi hai người chơi có giá trị nhóm bằng nhau và bằng 0 nếu ngược lại. 

Đối với người chơi (i), việc thêm biểu thức này vào tất cả bạn bè sẽ mang lại số lượng bạn bè cùng nhóm tương đương: 

[ 
deg_i + \sum_{j \in N(i)} x_j + deg_i \cdot x_i 
] 

Tất cả các phép toán đều có modulo 2. Giá trị này phải bằng 1 vì số lượng bạn bè trong cùng nhóm phải là số lẻ. 

Sắp xếp lại cho một phương trình tuyến tính: 

[ 
\sum_{j \in N(i)} x_j + (deg_i \bmod 2)x_i = 1 + (deg_i \bmod 2) 
] 

Mỗi người chơi đóng góp một phương trình và mỗi người chơi là một biến. Bài toán trở thành kiểm tra xem một hệ gồm nhiều nhất 100 phương trình tuyến tính trên GF(2) có nghiệm hay không. Việc loại bỏ Gaussian hoạt động trực tiếp cho việc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^P · P2) | O(P2) | Quá chậm | 
| Tối ưu | O(P³) | O(P2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ma trận nhị phân biểu diễn hệ tuyến tính. Mỗi hàng tương ứng với một người chơi và mỗi cột tương ứng với biến nhóm của một người chơi. Cột cuối cùng lưu trữ giá trị cần thiết ở phía bên phải của phương trình. 

Hệ số của biến (x_j) là 1 nếu người chơi (j) là bạn của người chơi (i). Ngoài ra, nếu người chơi (i) có bậc lẻ, hệ số của (x_i) sẽ bị thay đổi do số hạng (deg_i \cdot x_i). 
2. Áp dụng phép loại trừ Gaussian theo modulo 2. Tìm một hàng có hàng trong cột hiện tại và hoán đổi nó sang vị trí trục xoay hiện tại. 

Việc hoán đổi là cần thiết vì một trục có thể xuất hiện bên dưới hàng hiện tại và nếu không di chuyển nó lên trên thì không thể loại bỏ cột một cách chính xác. 
3. Sử dụng hàng xoay để xóa biến hiện tại khỏi mọi hàng khác. 

Vì tất cả các thao tác đều là modulo 2 nên việc xóa một biến chỉ là XOR hàng trục với một hàng khác. Điều này giữ cho các giá trị ma trận ở dạng nhị phân. 
4. Sau khi loại bỏ, kiểm tra từng hàng. Nếu tất cả các hệ số biến bằng 0 nhưng giá trị cuối cùng là 1 thì phương trình không thể thực hiện được, do đó không tồn tại phân vùng hợp lệ. 

Một hàng như vậy thể hiện sự mâu thuẫn như (0 = 1). 
5. Nếu không tồn tại mâu thuẫn thì hệ thống có ít nhất một giá trị nhóm được gán, do đó câu trả lời là`Y`. 

Tại sao nó hoạt động: 

Việc chuyển đổi từ bài toán đồ thị sang phương trình bảo toàn chính xác điều kiện mà chúng ta cần. Bất kỳ phân vùng hợp lệ nào đều tạo ra các giá trị cho các biến thỏa mãn mọi phương trình và bất kỳ nghiệm nào của phương trình đều đưa ra các giá trị nhóm trong đó mọi người chơi có số lẻ bạn bè trong cùng một nhóm. Việc loại bỏ Gaussian trên GF(2) không làm thay đổi tập nghiệm mà chỉ chuyển các phương trình sang dạng dễ dàng hơn. Việc kiểm tra mâu thuẫn cuối cùng sẽ phân biệt các hệ thống không có khả năng phân vùng với các hệ thống có ít nhất một phân vùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    P, F = map(int, input().split())
    adj = [[0] * P for _ in range(P)]
    deg = [0] * P

    for _ in range(F):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        adj[a][b] = 1
        adj[b][a] = 1
        deg[a] += 1
        deg[b] += 1

    mat = [[0] * (P + 1) for _ in range(P)]

    for i in range(P):
        for j in range(P):
            mat[i][j] = adj[i][j]
        if deg[i] % 2 == 1:
            mat[i][i] ^= 1
        mat[i][P] = 1 ^ (deg[i] % 2)

    row = 0
    for col in range(P):
        pivot = row
        while pivot < P and mat[pivot][col] == 0:
            pivot += 1

        if pivot == P:
            continue

        mat[row], mat[pivot] = mat[pivot], mat[row]

        for i in range(P):
            if i != row and mat[i][col]:
                for j in range(col, P + 1):
                    mat[i][j] ^= mat[row][j]

        row += 1

    for i in range(P):
        all_zero = True
        for j in range(P):
            if mat[i][j]:
                all_zero = False
                break
        if all_zero and mat[i][P]:
            print("N")
            return

    print("Y")

if __name__ == "__main__":
    solve()
```Ma trận kề chỉ lưu trữ hai người chơi có phải là bạn bè hay không. Vì các ràng buộc nhỏ nên việc sử dụng ma trận (P \times P) giúp việc xây dựng phương trình trở nên đơn giản và tránh cần thêm logic truyền tải đồ thị. 

Việc xây dựng ma trận tuân theo phương trình dẫn xuất trực tiếp. Mỗi tình bạn đều đóng góp một hệ số cho biến bạn bè. Đối với người chơi có bậc lẻ, biến riêng của người chơi sẽ nhận được hệ số bổ sung. Vế phải được tính bằng (1 + deg_i) modulo 2. 

Việc loại bỏ sử dụng XOR vì phép cộng và phép trừ giống hệt nhau trong số học modulo 2. Tìm kiếm trục sẽ bỏ qua các cột hiện không thể được sử dụng làm trục, điều này cần thiết vì một số biến có thể là biến tự do. 

Lần quét cuối cùng chỉ kiểm tra các hàng không thể thực hiện được. Một hàng không chứa biến nhưng kết quả khác 0 có nghĩa là hệ thống yêu cầu một câu lệnh sai. Bất kỳ biểu mẫu cuối cùng nào khác vẫn có ít nhất một bài tập. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 4
4 2
1 3
2 3
1 4
```Hệ phương trình được xây dựng từ bốn người chơi. 

| Bước | Hoạt động hiện tại | Hàng xoay | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Xây dựng phương trình | Không có | Bốn phương trình chẵn lẻ được tạo | 
| 2 | Loại bỏ biến đầu tiên | Hàng 0 | Các hàng khác được XOR cập nhật | 
| 3 | Loại bỏ các biến còn lại | Hàng 1 đến 3 | Hệ thống đạt dạng rút gọn | 
| 4 | Kiểm tra mâu thuẫn | Không tìm thấy | đầu ra`Y`| 

Việc loại bỏ thành công mà không tạo ra một hàng biểu thị một phương trình không thể thực hiện được. Điều này xác nhận rằng có ít nhất một sự phân công người chơi vào các nhóm tồn tại. 

Đối với mẫu thứ hai:```
4 3
4 2
2 3
1 2
```| Bước | Hoạt động hiện tại | Hàng xoay | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Xây dựng phương trình | Không có | Bốn phương trình chẵn lẻ được tạo | 
| 2 | Chọn trục | Đã tìm thấy các cột có sẵn | Các biến bị ràng buộc | 
| 3 | Loại bỏ XOR | Đã xóa tất cả các cột trụ | Hệ tương đương thu được | 
| 4 | Kiểm tra mâu thuẫn | Không tìm thấy | đầu ra`Y`| 

Ví dụ này cho thấy biểu đồ không cần phải đều đặn hoặc có tính kết nối cao. Hệ thống tuyến tính xử lý tình trạng cục bộ của mỗi người chơi một cách độc lập trong khi vẫn giữ các phương trình nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(P³) | Việc loại bỏ Gaussian thực hiện tối đa P thao tác trục và mỗi thao tác có thể cập nhật P hàng có độ dài P | 
| Không gian | O(P2) | Ma trận phương trình chứa P hàng và P+1 cột | 

Với (P \le 100), thời gian chạy khối là khoảng một triệu phép tính cơ bản, dễ dàng nằm trong giới hạn thông thường. Việc sử dụng bộ nhớ cũng nhỏ vì toàn bộ hệ thống chỉ chứa khoảng mười nghìn giá trị được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""
    it = iter(data)
    P = int(next(it))
    F = int(next(it))

    adj = [[0] * P for _ in range(P)]
    deg = [0] * P

    for _ in range(F):
        a = int(next(it)) - 1
        b = int(next(it)) - 1
        adj[a][b] = adj[b][a] = 1
        deg[a] += 1
        deg[b] += 1

    mat = [[0] * (P + 1) for _ in range(P)]
    for i in range(P):
        for j in range(P):
            mat[i][j] = adj[i][j]
        if deg[i] % 2:
            mat[i][i] ^= 1
        mat[i][P] = 1 ^ (deg[i] % 2)

    row = 0
    for col in range(P):
        pivot = row
        while pivot < P and mat[pivot][col] == 0:
            pivot += 1
        if pivot == P:
            continue
        mat[row], mat[pivot] = mat[pivot], mat[row]
        for i in range(P):
            if i != row and mat[i][col]:
                for j in range(col, P + 1):
                    mat[i][j] ^= mat[row][j]
        row += 1

    for i in range(P):
        if all(mat[i][j] == 0 for j in range(P)) and mat[i][P]:
            return "N\n"
    return "Y\n"

assert solve_case("""4 4
4 2
1 3
2 3
1 4
""") == "Y\n", "sample 1"

assert solve_case("""4 3
4 2
2 3
1 2
""") == "Y\n", "sample 2"

assert solve_case("""5 5
3 5
3 1
1 4
2 5
2 4
""") == "N\n", "sample 3"

assert solve_case("""2 1
1 2
""") == "Y\n", "minimum size"

assert solve_case("""3 2
1 2
2 3
""") == "N\n", "even degree contradiction"

assert solve_case("""4 4
1 2
2 3
3 4
4 1
""") == "Y\n", "cycle graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai người chơi được kết nối | Y | Cho phép một nhóm không trống | 
| Con đường ba người chơi | N | Phát hiện mâu thuẫn do mức độ nội tại chẵn | 
| Chu kỳ bốn người chơi | Y | Xử lý nhiều bài tập hợp lệ | 
| Cung cấp mẫu | Y, Y, N | Khẳng định việc thực hiện đối với các trường hợp chính thức | 

## Vỏ cạnh 

Đối với đường đi của ba người chơi:```
3 2
1 2
2 3
```Người chơi ở giữa có cấp độ hai. Phương trình dành cho người chơi đó không thể được thỏa mãn bằng cách đơn giản đặt mọi người lại với nhau vì số lượng bạn bè trong cùng một nhóm sẽ là hai. Trong quá trình loại trừ, giá trị này xuất hiện dưới dạng một hàng có tất cả các hệ số biến đổi bằng 0 và vế phải bằng 1. Thuật toán trả về`N`. 

Đối với hai người chơi được kết nối:```
2 1
1 2
```Cả hai người chơi có thể thuộc cùng một nhóm. Mỗi người nhìn thấy chính xác một người bạn trong nhóm đó. Hệ tuyến tính có nghiệm nên phép loại trừ kết thúc không mâu thuẫn và trả về`Y`. 

Đối với biểu đồ có nhiều phân vùng hoạt động:```
4 4
1 2
2 3
3 4
4 1
```Thuật toán không cố gắng chọn một phân vùng đặc biệt. Nó chỉ xác minh rằng các phương trình tương ứng là nhất quán. Vì ma trận rút gọn không có hàng không thể, nên tồn tại một số phép gán và câu trả lời là`Y`. 

Tôi cũng có thể điều chỉnh bài xã luận theo định dạng kiểu Codeforces ngắn hơn nếu bạn muốn có phiên bản dài hơn để gửi bài dự thi.
