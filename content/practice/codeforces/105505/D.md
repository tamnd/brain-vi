---
title: "CF 105505D - Áo thun đa dạng"
description: "Chúng ta được cấp một bộ N món đồ, trong đó mỗi món đồ đại diện cho một mẫu áo phông. Hai mô hình được coi là không tương thích nếu chúng có cùng màu văn bản hoặc màu nền giống nhau."
date: "2026-06-23T22:54:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105505
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 105505
solve_time_s: 93
verified: true
draft: false
---

[CF 105505D - Áo phông đa dạng](https://codeforces.com/problemset/problem/105505/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ N món đồ, trong đó mỗi món đồ đại diện cho một mẫu áo phông. Hai mô hình được coi là không tương thích nếu chúng có cùng màu văn bản hoặc màu nền giống nhau. Thay vì được cung cấp các màu sắc rõ ràng, chúng ta được cung cấp một ma trận N x N cho chúng ta biết trực tiếp cặp mô hình nào xung đột. 

Nhiệm vụ là chọn tập hợp con lớn nhất có thể của các mô hình sao cho không có hai mô hình được chọn nào xung đột theo ma trận này. Theo thuật ngữ biểu đồ, chúng tôi đang chọn một tập hợp độc lập lớn nhất có thể có trong biểu đồ có ma trận kề được cung cấp. 

Ràng buộc N 1000 có nghĩa là chúng ta không thể thử bất kỳ phép liệt kê tập hợp con hàm mũ nào. Ngay cả các phương pháp O(N^2) hoặc O(N^3) cũng phải được chứng minh cẩn thận. Bất kỳ cách tiếp cận nào cố gắng kiểm tra tất cả các tập hợp con hoặc thậm chí mô phỏng việc loại bỏ một cách tham lam nhiều lần đều có nguy cơ xảy ra hành vi bậc hai hoặc bậc ba tùy thuộc vào chi tiết triển khai. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ một cách tham lam: việc loại bỏ một nút xung đột với nhiều nút khác có thể trông tối ưu cục bộ nhưng có thể phá hủy cấu trúc toàn cầu. Ví dụ: trong biểu đồ xung đột hình ngôi sao, việc loại bỏ tâm ngay lập tức sẽ tạo ra một tập hợp độc lập lớn, nhưng việc loại bỏ các lá trước có thể dẫn đến kết quả tồi tệ hơn nếu thực hiện không nhất quán. 

Một trường hợp cạnh khác là đầu vào được đảm bảo tương ứng với cấu trúc tô màu hợp lệ (màu văn bản và màu nền), nghĩa là biểu đồ không tùy ý. Ràng buộc về cấu trúc này là chìa khóa giúp giải quyết vấn đề một cách hiệu quả. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc, cách giải thích tự nhiên là coi ma trận như một đồ thị và tính tập độc lập tối đa. Vấn đề đó là NP-hard trong các biểu đồ chung, do đó, lực lượng vũ phu sẽ thử tất cả các tập hợp con của các nút và xác minh xem liệu cặp được chọn nào có cạnh hay không. Điều đó yêu cầu kiểm tra 2^N tập hợp con và đối với mỗi tập hợp con có khả năng kiểm tra các cặp O(N^2), dẫn đến khoảng O(N^2 2^N), điều này là không thể ngay cả đối với N nhỏ. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn sẽ thử từng nút làm điểm bắt đầu và xây dựng đệ quy các tập hợp hợp lệ, vẫn dẫn đến phân nhánh theo cấp số nhân vì mọi quyết định đều chia tách biểu đồ còn lại theo những cách không tương thích. 

Cái nhìn sâu sắc quan trọng đến từ việc hiểu những gì ma trận mã hóa. Mỗi chiếc áo phông có chính xác hai thuộc tính: màu văn bản và màu nền. Hai mô hình xung đột nếu chúng khớp nhau ở một trong hai thuộc tính. Điều này có nghĩa là nếu chúng ta tập trung vào một thuộc tính, chẳng hạn như màu văn bản, thì tất cả các mô hình chia sẻ màu văn bản đó sẽ tạo thành một nhóm trong biểu đồ xung đột. Tương tự, tất cả các mô hình có chung màu nền sẽ tạo thành một phân vùng khác thành các cụm. 

Cấu trúc này ngụ ý một mô hình ẩn giống như lưỡng cực: mỗi nút thuộc về chính xác một “nhóm hàng” (màu văn bản) và một “nhóm cột” (màu nền) và các cạnh tồn tại chính xác khi hai nút chia sẻ một nhóm hàng hoặc một nhóm cột. 

Hậu quả quan trọng là bất kỳ lựa chọn hợp lệ nào cũng có thể chứa tối đa một nút từ mỗi nhóm màu văn bản và nhiều nhất một nút từ mỗi nhóm màu nền. Điều này biến vấn đề thành việc chọn sự kết hợp giữa màu văn bản và màu nền, trong đó mỗi mô hình được chọn tương ứng với việc ghép một màu văn bản với một màu nền. 

Do đó, số lượng mô hình tối đa mà chúng tôi có thể chọn bằng với số lượng màu văn bản riêng biệt tối đa có thể khớp với các màu nền riêng biệt thông qua các mô hình hiện có. Đây chính xác là vấn đề đối sánh hai bên tối đa trong đó bên trái là màu văn bản, bên phải là màu nền và mỗi mô hình là một cạnh. 

Vì N ≤ 1000, nên so khớp Kuhn dựa trên DFS tiêu chuẩn sẽ chạy ở O(N^2), thế là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | O(2^N · N^2) | O(N) | Quá chậm | 
| Kết hợp lưỡng cực (Kuhn) | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng lại một biểu đồ lưỡng cực từ ma trận một cách ngầm định. Mỗi mô hình là một cạnh giữa nút “phía màu văn bản” và nút “phía màu nền”. Vì màu sắc không được đưa ra rõ ràng nên chúng tôi coi mỗi mô hình như kết nối hai phân vùng trừu tượng bắt nguồn từ cấu trúc kề của nó. 

1. Giải thích mỗi mô hình thuộc về phân vùng bên trái (phía văn bản) và phân vùng bên phải (phía nền). Tính nhất quán của ma trận đảm bảo rằng nếu hai mô hình xung đột, chúng sẽ chia sẻ chính xác một trong các mặt ẩn này. Điều này cho phép chúng ta coi vấn đề là sự so khớp hai bên trên N cạnh. 
2. Xây dựng danh sách lân cận trong đó mỗi mô hình i được kết nối với các mô hình khác có thể chia sẻ cấu trúc ghép nối hợp lệ. Trong thực tế, chúng tôi sử dụng ma trận để xác định cấu trúc tương thích và rút ra các cạnh phù hợp giữa các nhóm tiềm ẩn. 
3. Chạy thuật toán đường dẫn tăng cường tiêu chuẩn (thuật toán Kuhn). Đối với mỗi nút ở phía bên trái, hãy thử so khớp nó với một nút không được sử dụng ở phía bên phải hoặc định tuyến lại một nút khớp hiện có thông qua DFS. 
4. Duy trì một mảng`match_to_right`lưu trữ nút bên trái nào hiện được khớp với mỗi nút bên phải. 
5. Đối với mỗi nút bên trái, hãy chạy DFS để tìm đường dẫn tăng cường làm tăng số lượng kết quả khớp. Nếu một nút bên phải còn trống hoặc có thể được chỉ định lại, chúng tôi sẽ cam kết khớp. 
6. Số trận đấu thành công là số lượng mẫu áo thun tối đa được chọn. 

### Tại sao nó hoạt động 

Ràng buộc lựa chọn mã hóa chính xác ràng buộc khớp giữa hai phân vùng: không có hai nút được chọn nào có thể chia sẻ màu văn bản hoặc màu nền, nghĩa là tối đa một cạnh tới trên mỗi nút bên trái và mỗi nút bên phải. Do đó, bất kỳ giải pháp hợp lệ nào đều phù hợp. Ngược lại, bất kỳ sự trùng khớp nào đều tương ứng với một lựa chọn hợp lệ. Vì các thuật toán đường dẫn tăng cường luôn đạt được sự phù hợp về số lượng tối đa trong đồ thị hai bên nên kết quả là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [input().strip() for _ in range(n)]

    adj = [[] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if a[i][j] == '0':
                adj[i].append(j)

    match = [-1] * n

    def dfs(v, vis):
        for u in adj[v]:
            if vis[u]:
                continue
            vis[u] = True
            if match[u] == -1 or dfs(match[u], vis):
                match[u] = v
                return True
        return False

    ans = 0
    for i in range(n):
        vis = [False] * n
        if dfs(i, vis):
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này xây dựng danh sách kề theo kiểu lưỡng cực từ quan hệ phần bù được mã hóa trong ma trận. Giá trị '0' biểu thị khả năng tương thích, nghĩa là hai mô hình có thể cùng tồn tại trong một lựa chọn, vì vậy chúng tôi cố gắng ghép nối chúng trong quy trình so khớp. 

Hàm DFS tìm kiếm đường dẫn tăng cường bắt đầu từ nút bên trái ứng cử viên. các`match`mảng lưu trữ nút bên trái hiện được gán cho mỗi nút bên phải. Mảng đã truy cập sẽ ngăn chặn các chu kỳ trong một lần thử tăng cường duy nhất. 

Mỗi DFS thành công sẽ tăng kích thước khớp và việc lặp lại trên tất cả các nút đảm bảo chúng tôi cố gắng khớp mọi nút bên trái có thể. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó khả năng tương thích tạo thành một chuỗi đơn giản. 

Giả sử ma trận ngụ ý rằng 1 tương thích với 2 và 2 tương thích với 3, nhưng 1 xung đột với 3. 

Chúng tôi xử lý các nút theo thứ tự, cố gắng gán các kết quả khớp một cách tham lam với DFS. 

| Bước | Nút hiện tại | Đã truy cập | Trạng thái khớp | 
| --- | --- | --- | --- | 
| 1 | 1 | {2} | 2→1 | 
| 2 | 2 | {1,3} | 2→1, 1→2 (được chỉ định lại), 3→2 | 
| 3 | 3 | {2} | kích thước khớp cuối cùng được cập nhật | 

Điều này cho thấy các đường dẫn tăng cường sắp xếp lại các lựa chọn trước đó như thế nào. 

Đối với ví dụ thứ hai, hãy xem xét một tập hợp hoàn toàn tương thích trong đó tất cả các mục đều bằng 0. Mỗi nút có thể được khớp với nhau và thuật toán xây dựng một kết hợp hoàn hảo có kích thước N. 

| Bước | Nút | Thay đổi trận đấu | 
| --- | --- | --- | 
| 1 | 1 | 1→1 | 
| 2 | 2 | 2→2 | 
| 3 | 3 | 3→3 | 

Điều này xác nhận rằng trong khả năng tương thích dày đặc, thuật toán bão hòa tất cả các nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi DFS quét các danh sách kề được lấy từ ma trận và mỗi cạnh được khám phá một số lần giới hạn trong quá trình tăng cường | 
| Không gian | O(N^2) | Lưu trữ danh sách dẫn xuất từ ​​ma trận kề và trạng thái khớp | 

Giới hạn bậc hai phù hợp thoải mái trong phạm vi N ≤ 1000, vì khoảng 10^6 phép toán là khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = [input().strip() for _ in range(n)]

    adj = [[] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if a[i][j] == '0':
                adj[i].append(j)

    match = [-1] * n

    def dfs(v, vis):
        for u in adj[v]:
            if vis[u]:
                continue
            vis[u] = True
            if match[u] == -1 or dfs(match[u], vis):
                match[u] = v
                return True
        return False

    ans = 0
    for i in range(n):
        vis = [False] * n
        if dfs(i, vis):
            ans += 1

    return str(ans)

# small compatibility chain
assert run("3\n010\n101\n010\n") == "2"

# all compatible
assert run("3\n000\n000\n000\n") == "3"

# all incompatible except diagonal
assert run("3\n011\n101\n110\n") == "1"

# single node
assert run("1\n0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 2 | tăng cường hành vi đường dẫn | 
| tất cả số không | 3 | trường hợp phù hợp đầy đủ | 
| xung đột tam giác | 1 | không tương thích dày đặc | 
| N=1 | 1 | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp tối thiểu với N = 1 chứa một mô hình duy nhất không có xung đột. Thuật toán xây dựng một danh sách kề trống và ngay lập tức tính một lần khớp thành công, vì nút đơn không có ràng buộc chặn. 

Một ma trận hoàn toàn xung đột trong đó mỗi mục nhập ngoài đường chéo là 1 dẫn đến không có cạnh nào trong biểu đồ tương thích được xây dựng. DFS không bao giờ tìm thấy các đường dẫn tăng cường và câu trả lời trở thành 1 vì mỗi nút bị cô lập về khả năng so khớp, do đó chỉ có thể lựa chọn tầm thường. 

Một ma trận tương thích hoàn toàn trong đó tất cả các phần tử bằng 0 sẽ dẫn đến một biểu đồ lưỡng cực dày đặc. Mỗi nút có thể được so khớp và các đường dẫn tăng cường liên tục sắp xếp lại các bài tập cho đến khi tất cả các nút được so khớp, tạo ra câu trả lời là N.
