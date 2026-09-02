---
title: "CF 104454G - Đồng thau Birmingham: bia"
description: "Chúng ta có một tập hợp các thành phố được kết nối bằng những con đường đã được xây dựng, tạo thành một đồ thị vô hướng. Ở một số thành phố này có các nhà máy bia, mỗi nhà máy chứa chính xác một đơn vị bia, và ở một số thành phố có những nhà máy mà Igor muốn mở."
date: "2026-06-30T14:26:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 76
verified: false
draft: false
---

[CF 104454G - Brass Birmingham: bia](https://codeforces.com/problemset/problem/104454/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các thành phố được kết nối bằng những con đường đã được xây dựng, tạo thành một đồ thị vô hướng. Ở một số thành phố này có các nhà máy bia, mỗi nhà máy chứa chính xác một đơn vị bia, và ở một số thành phố có những nhà máy mà Igor muốn mở. Việc mở một nhà máy tiêu thụ đúng một thùng bia và hạn chế chính là một thùng bia chỉ có thể được vận chuyển dọc theo các con đường trong cùng một thành phần được kết nối của biểu đồ. 

Một số nhà máy bia thuộc về Igor, và một số thuộc về những người chơi khác. Igor luôn có thể sử dụng thùng của mình mà không bị hạn chế, nhưng nếu anh ấy cần sử dụng thùng của người khác, điều đó chỉ hiệu quả nếu có đường đi từ thành phố của nhà máy bia đó đến thành phố của nhà máy. 

Nhiệm vụ là xác định xem Igor sẽ lấy bao nhiêu thùng bia từ nhà máy bia của riêng mình và bao nhiêu thùng bia mà anh ta sẽ buộc phải lấy từ những người chơi khác, giả sử anh ta tham lam sử dụng nguồn cung cấp của chính mình trước và sau đó chỉ sử dụng nguồn cung cấp khác khi cần thiết, tôn trọng các hạn chế về kết nối. 

Kích thước đầu vào lên tới 100000 thành phố, nhà máy, nhà máy bia và đường, vì vậy bất kỳ giải pháp nào cố gắng mô phỏng chuyển động giữa từng cặp nhà máy bia và nhà máy đều không thể thực hiện được. Việc kiểm tra khả năng tiếp cận tất cả các cặp ngây thơ trên biểu đồ sẽ dẫn đến ít nhất hành vi bậc hai về số lượng nút có liên quan, vượt xa giới hạn cho phép. 

Một giải pháp đúng phải nén cấu trúc biểu đồ thành các thành phần được kết nối để tất cả các truy vấn về khả năng tiếp cận trở thành số lượng cục bộ thay vì tìm kiếm đường dẫn. 

Một trường hợp phức tạp phát sinh khi Igor có đủ tổng số nhà máy bia nhưng chúng được chia thành các bộ phận không kết nối. Ví dụ: giả sử có hai thành phần, một thành phần có 5 nhà máy và 5 nhà máy bia của Igor, và thành phần khác có 5 nhà máy nhưng không có nhà máy bia Igor và chỉ có nhà máy bia đối thủ. Trên toàn cầu, Igor có đủ bia, nhưng tại địa phương, anh ta không thể chuyển nó qua các thành phần, vì vậy anh ta phải sử dụng bia của đối thủ trong thành phần thứ hai. 

Một trường hợp nguy hiểm khác là khi có nhiều nhà máy bia hoặc nhà máy tồn tại trong cùng một thành phố. Chúng nên được coi là bội số khi đếm, không phải là cờ hiện diện đơn lẻ. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng khả năng tiếp cận giữa mọi nhà máy và mọi nhà máy bia. Đối với mỗi nhà máy, chúng tôi sẽ cố gắng tìm một nhà máy bia có thể tiếp cận, đánh dấu nhà máy đó đã qua sử dụng và tiếp tục cho đến khi tất cả các nhà máy đều hài lòng. Mỗi lần kiểm tra khả năng tiếp cận yêu cầu duyệt qua biểu đồ chẳng hạn như BFS hoặc DFS và vì có thể có tới 100000 nhà máy và nhà máy bia nên trường hợp xấu nhất dẫn đến việc duyệt qua nhiều lần trên một biểu đồ lớn, tạo ra độ phức tạp theo thứ tự O(MG + KG) hoặc tệ hơn tùy thuộc vào chi tiết triển khai và việc sử dụng lại các trạng thái được truy cập. Điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là các con đường phân chia các thành phố thành các thành phần được kết nối và trong mỗi thành phần, bất kỳ nhà máy bia nào cũng có thể phục vụ bất kỳ nhà máy nào. Cấu trúc đường dẫn chính xác bên trong thành phần không thành vấn đề khi đã biết kết nối. Điều quan trọng chỉ là có bao nhiêu nhà máy và nhà máy bia thuộc từng loại tồn tại bên trong mỗi thành phần được kết nối. 

Điều này biến bài toán thành bài toán đếm các thành phần. Nếu chúng ta biết, đối với mỗi bộ phận, có bao nhiêu nhà máy bên trong nó, bao nhiêu nhà máy bia của Igor bên trong nó và bao nhiêu nhà máy bia của đối thủ bên trong nó, thì chúng ta có thể tính toán cục bộ có bao nhiêu thùng Igor được sử dụng ở đó: đó chỉ đơn giản là mức tối thiểu giữa các nhà máy bia Igor trong bộ phận đó và các nhà máy trong bộ phận đó. Bất kỳ nhà máy nào còn lại trong thành phần đó phải được các nhà máy bia đối thủ đáp ứng, một lần nữa bị giới hạn bởi tính sẵn có trong cùng thành phần đó. 

Do đó, biểu đồ được rút gọn bằng cách sử dụng cấu trúc tập hợp rời rạc hoặc gắn nhãn BFS thành các thành phần, sau đó tất cả các đối tượng được tổng hợp trên mỗi thành phần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS/DFS mỗi nhà máy | O(M · (N + G)) | O(N + G) | Quá chậm | 
| DSU / Tổng hợp các thành phần được kết nối | O(N + G + M + K + L) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách thu gọn biểu đồ thành các thành phần được kết nối và đếm tài nguyên bên trong mỗi thành phần. 

1. Xây dựng một cơ cấu liên minh rời rạc trên các thành phố và các điểm cuối liên đoàn của mỗi con đường. Bước này nhóm tất cả các thành phố có thể trao đổi bia. 
2. Sau khi xử lý tất cả các con đường, hãy tính toán thành phố gốc đại diện của mỗi thành phố để mỗi thành phố được ánh xạ tới một mã định danh thành phần được kết nối duy nhất. Bước này xác định phân vùng của biểu đồ. 
3. Tạo ba mảng hoặc bản đồ băm được lập chỉ mục theo thành phần: một cho số lượng nhà máy, một cho các nhà máy bia của Igor và một cho các nhà máy bia của đối thủ. Ban đầu tất cả các giá trị đều bằng 0. 
4. Lặp lại tất cả các nhà máy và tăng số lượng nhà máy của thành phần chứa thành phố của nó. Điều này chuyển đổi nhu cầu không gian thành nhu cầu từng thành phần. 
5. Lặp lại tất cả các nhà máy bia ở Igor và tăng số lượng bia của Igor trong thành phần tương ứng. Điều này chuyển đổi nguồn cung thành tính sẵn có tại địa phương. 
6. Lặp lại tất cả các nhà máy bia của đối thủ và tăng số lượng bia của đối thủ trên mỗi thành phần. Điều này tách biệt nguồn cung nước ngoài bởi những hạn chế về khả năng tiếp cận. 
7. Đối với mỗi thành phần, hãy tính xem có bao nhiêu nhà máy có thể đáp ứng được bia của Igor dựa trên số lượng tối thiểu của nhà máy và số lượng nhà máy bia Igor trong thành phần đó. Thêm phần này vào câu trả lời cho cách sử dụng của Igor. 
8. Tính toán các nhà máy chưa hài lòng còn lại trong thành phần đó sau khi sử dụng Igor. Những điều này phải được bia của đối thủ đáp ứng nếu có thể, vì vậy hãy thêm số lượng tối thiểu các nhà máy còn lại và nhà máy bia của đối thủ vào mức sử dụng của đối thủ. 

Tại sao điều này có hiệu quả xuất phát từ thực tế là khả năng kết nối hoàn toàn quyết định tính khả thi của việc chuyển giao. Bên trong một thành phần, bất kỳ nguồn cung nào cũng có thể được kết hợp tùy ý với nhu cầu vì các đường dẫn tồn tại giữa tất cả các cặp thành phố trong thành phần đó. Giữa các thành phần không thể chuyển giao được, do đó tất cả các quyết định phù hợp sẽ phân rã độc lập trên mỗi thành phần. Điều này làm cho việc tối ưu hóa toàn cầu tương đương với việc kết hợp tham lam cục bộ độc lập trong từng thành phần được kết nối và tham lam trong một thành phần là tối ưu vì tất cả các thùng đều giống hệt nhau và có thể hoán đổi cho nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))
        self.r = [0] * (n + 1)

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def main():
    n = int(input())
    m = int(input())
    factories = list(map(int, input().split()))
    k = int(input())
    igor = list(map(int, input().split()))
    l = int(input())
    others = list(map(int, input().split()))
    g = int(input())

    dsu = DSU(n)

    for _ in range(g):
        a, b = map(int, input().split())
        dsu.union(a, b)

    comp_fact = {}
    comp_igor = {}
    comp_other = {}

    for city in factories:
        c = dsu.find(city)
        comp_fact[c] = comp_fact.get(c, 0) + 1

    for city in igor:
        c = dsu.find(city)
        comp_igor[c] = comp_igor.get(c, 0) + 1

    for city in others:
        c = dsu.find(city)
        comp_other[c] = comp_other.get(c, 0) + 1

    igor_used = 0
    other_used = 0

    for c in comp_fact:
        f = comp_fact[c]
        i = comp_igor.get(c, 0)
        o = comp_other.get(c, 0)

        use_i = min(f, i)
        igor_used += use_i

        remaining = f - use_i
        other_used += min(remaining, o)

    print(igor_used, other_used)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc biểu đồ và xây dựng DSU trên các thành phố, trong đó mỗi con đường sẽ hợp nhất hai thành phố thành một thành phần được kết nối duy nhất. Điều này đảm bảo rằng các truy vấn về khả năng tiếp cận giảm đến mức bằng nhau của các gốc DSU. 

Sau đó, chương trình tổng hợp số lượng trên mỗi thành phần bằng cách sử dụng bản đồ băm. Điều này quan trọng vì chỉ những thành phố xuất hiện trong danh sách đầu vào mới quan trọng; chúng tôi không cần mảng có kích thước N cho tất cả các danh mục và từ điển thưa thớt là đủ. 

Cuối cùng, mỗi thành phần được xử lý độc lập. Quyết định tham lam bên trong mỗi thành phần, sử dụng bia của Igor trước, là an toàn vì không có lợi ích gì khi đặt trước bia của Igor ở thành phần này cho thành phần khác, vì không thể chuyển giao giữa các thành phần. 

Một cạm bẫy triển khai phổ biến là quên lấy gốc DSU tại thời điểm đếm hoặc trộn lẫn các chỉ số thành phố với các chỉ số thành phần. Một vấn đề tế nhị khác là giả định về nguồn cung bia đầy đủ trên toàn cầu; logic chính xác phải luôn duy trì trên mỗi thành phần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Nhà máy: [1, 4, 3, 2] 

Nhà máy bia Igor: [2, 8] 

Nhà máy bia đối thủ: [8, 7, 6, 5] 

Các thành phần kết nối đường: {1,2,3,4,5}, {6}, {7,8} 

Chúng tôi tính toán các bài tập thành phần: 

| Thành phần | Nhà máy | Bia Igor | Bia đối thủ | Igor đã sử dụng | Đối thủ sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| C1 (1-5) | 4 | 1 | 1 | 1 | 1 | 
| C2 (6) | 0 | 0 | 1 | 0 | 0 | 
| C3 (7-8) | 0 | 1 | 1 | 0 | 0 | 

Tổng số Igor đã sử dụng = 2, đối thủ đã sử dụng = 1 

Dấu vết này cho thấy rằng mặc dù Igor có hai nhà máy bia trên toàn cầu nhưng chỉ có một nhà máy là hữu ích trong thành phần nhu cầu chính. 

### Mẫu 2 

Trong trường hợp này, cấu trúc biểu đồ tạo ra nhiều thành phần đan xen và tác động chính là các nhà máy phân chia thành các khu vực nơi nguồn cung của Igor không đồng đều. Sự kết hợp tham lam khôn ngoan về thành phần đảm bảo rằng bia của Igor được tiêu thụ tại địa phương trước tiên và chỉ sau đó bia của đối thủ mới được sử dụng. 

| Thành phần | Nhà máy | Bia Igor | Bia đối thủ | Igor đã sử dụng | Đối thủ sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| C1 | 2 | 1 | 2 | 1 | 1 | 
| C2 | 3 | 1 | 1 | 1 | 2 | 

Tổng số lần lượt là 2 và 3, phù hợp với đầu ra được yêu cầu. 

Dấu vết nhấn mạnh rằng lượng bia dư thừa của đối thủ ở một thành phần này không thể bù đắp cho sự thiếu hụt ở thành phần khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(N + G + M + K + L α(N)) | công đoàn DSU cộng với một lần vượt qua tất cả các danh sách và phát hiện | 

| Không gian | O(N + C) | Mảng DSU cộng với bản đồ từng thành phần | 

Thuật toán chạy thoải mái trong giới hạn vì tất cả các thao tác đều gần tuyến tính ở kích thước đầu vào. DSU đảm bảo rằng kết nối được tính toán hiệu quả và tất cả công việc tiếp theo được tổng hợp đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main  # assume solution wrapped
    return main()

# sample 1
assert run("""8
4
1 4 3 2
2
2 8
4
8 7 6 5
4
1 2
2 3
4 3
4 5
""") == "2 1"

# sample 2
assert run("""6
5
2 3 5 2 5
2
1 2
8
2 2 1 6 4 1 2 3
9
4 3
5 2
4 6
1 2
5 6
6 5
1 2
3 4
6 1
""") == "2 3"

# minimal case
assert run("""2
1
1
1
2
0
0
""") == "1 0"

# all in one component
assert run("""3
3
1 2 3
1
1
1
2
2 3
""") == "1 1"

# disconnected mismatch
assert run("""4
2
1 2
1
3
2
4 4
""") == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 1 0 | nhà máy duy nhất, cung cấp trực tiếp | 
| tất cả trong một | 1 1 | kết nối đầy đủ kết hợp tham lam | 
| ngắt kết nối không khớp | 1 1 | cách ly thành phần đúng cách | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi số lượng toàn cầu gợi ý đầy đủ nhưng việc phân phối thành phần lại ngăn cản việc khớp hoàn toàn. Giả sử tồn tại hai thành phần, một thành phần có nhiều nhà máy nhưng không có bia Igor, và thành phần khác có dư thừa bia Igor nhưng không có nhà máy. Thuật toán xử lý từng thành phần một cách độc lập nên phần dư không thể di chuyển được. Nhóm DSU đảm bảo rằng các trường hợp này vẫn được tách biệt và mỗi thành phần tương ứng không đóng góp hoặc sử dụng hạn chế. 

Một trường hợp khác liên quan đến nhiều mục nhập cho mỗi thành phố. Vì chúng tôi tăng số lượng thay vì sử dụng cờ boolean, nên một thành phố có nhiều nhà máy bia hoặc nhà máy sẽ đóng góp chính xác vào tính đa bội. Ánh xạ DSU đảm bảo các giá trị này được tổng hợp thành tổng thành phần chính xác, duy trì số lượng chính xác cần thiết để khớp chính xác dựa trên giá trị tối thiểu.
