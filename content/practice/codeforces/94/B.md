---
title: "CF 94B - Những Người Bạn"
description: "Chúng tôi có đúng năm người. Một số cặp người biết nhau và dữ liệu đầu vào liệt kê tất cả các mối quan hệ quen biết như vậy. Nhiệm vụ là xác định xem có tồn tại: 1. Ba người trong đó mỗi cặp đều biết nhau. 2. Ba người không có cặp nào quen biết nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 94
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 76 (Div. 2 Only)"
rating: 1300
weight: 94
solve_time_s: 97
verified: true
draft: false
---

[CF 94B - Những người bạn](https://codeforces.com/problemset/problem/94/B) 

**Đánh giá:** 1300 
**Tags:** đồ thị, cách thực hiện, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có đúng năm người. Một số cặp người biết nhau và dữ liệu đầu vào liệt kê tất cả các mối quan hệ quen biết như vậy. 

Nhiệm vụ là xác định xem có tồn tại: 

1. Ba người mà mỗi cặp đều biết nhau. 
2. Ba người không có cặp nào quen biết nhau. 

Đây tự nhiên là một vấn đề đồ thị. Mỗi người là một đỉnh và mối quan hệ quen biết là một cạnh vô hướng. Chúng tôi đang tìm kiếm: 

1. Một hình tam giác trong biểu đồ. 
2. Một tam giác trong đồ thị phần bù. 

Nếu có ít nhất một trong những cấu trúc đó tồn tại, chúng ta sẽ in`"WIN"`. Nếu không chúng tôi in`"FAIL"`. 

Những hạn chế là cực kỳ nhỏ. Chỉ có năm đỉnh và nhiều nhất là mười cạnh vì đồ thị vô hướng trên năm đỉnh có`5 * 4 / 2 = 10`các cặp có thể. Ngay cả việc tìm kiếm mạnh mẽ trên tất cả các bộ ba cũng là rất nhỏ. Tổng số bộ ba chỉ là`C(5, 3) = 10`. 

Khó khăn chính không phải là hiệu suất, mà là việc thực hiện đúng các điều kiện. 

Một sai lầm dễ mắc phải là chỉ kiểm tra các mối quan hệ bạn bè hình tam giác mà bỏ quên các bộ ba độc lập. Hãy xem xét đầu vào này:```
2
1 2
3 4
```Câu trả lời đúng là:```
WIN
```Mọi người`1, 3, 5`là cặp đôi không quen biết. Một giải pháp chỉ tìm kiếm hình tam giác tình bạn sẽ in sai`"FAIL"`. 

Một lỗi phổ biến khác là xử lý biểu đồ theo hướng dẫn. Mối quan hệ là đối xứng, vì vậy nếu`(1, 3)`thì xuất hiện`3`cũng biết`1`. Một cấu trúc kề bất cẩn có thể quên điều này và bỏ lỡ các bộ ba hợp lệ. 

Ví dụ:```
3
1 2
2 3
1 3
```Câu trả lời đúng là:```
WIN
```Đây là một tam giác tình bạn. Nếu việc triển khai chỉ lưu trữ một hướng của mỗi cạnh thì việc kiểm tra có thể thất bại. 

Một trường hợp cạnh tinh tế hơn là biểu đồ trống:```
0
```Câu trả lời đúng là:```
WIN
```Ba người bất kỳ tạo thành một bộ ba người lạ theo cặp. Một giải pháp giả định tồn tại ít nhất một cạnh có thể hoạt động không chính xác ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là dùng vũ lực đối với tất cả ba người. 

Chỉ có năm người nên chúng tôi có thể liệt kê mọi tổ hợp`(a, b, c)`Ở đâu`a < b < c`. Đối với mỗi bộ ba, chúng tôi kiểm tra hai điều kiện: 

1. Cả ba cặp đều là cạnh. 
2. Không có cặp nào trong ba cặp là cạnh. 

Nếu một trong hai điều kiện đúng với bất kỳ bộ ba nào, chúng ta sẽ trả về ngay lập tức`"WIN"``. 

Cách tiếp cận vũ phu này đã đủ nhanh rồi. Chỉ có mười bộ ba và mỗi lần kiểm tra sẽ kiểm tra ba cặp nên tổng công việc là không đổi. 

Lý do điều này hoạt động một cách tự nhiên là vì tài sản mà chúng tôi quan tâm hoàn toàn là tài sản cục bộ gấp ba lần. Chúng tôi không bao giờ cần cấu trúc biểu đồ lớn hơn, đường dẫn ngắn nhất, các thành phần được kết nối hoặc bất kỳ thứ gì mang tính toàn cầu. 

Một phiên bản đơn giản hơn của bạo lực sẽ xây dựng rõ ràng tất cả các tập hợp con người và sau đó kiểm tra tất cả các tập hợp con có kích thước ba. Điều đó vẫn hiệu quả vì biểu đồ rất nhỏ nhưng lại gây ra chi phí không cần thiết. 

Quan sát quan trọng là vấn đề giảm chính xác xuống việc kiểm tra từng bộ ba một lần. Vì kích thước biểu đồ được cố định ở mức 5 nên đây trở thành một vấn đề triển khai nhỏ gọn hơn là một vấn đề tối ưu hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con chung sức mạnh vũ phu | O(2^5) | O(1) | Đã chấp nhận | 
| Liệt kê ba | O(C(5,3)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một`5 x 5`ma trận kề được khởi tạo bằng`False`. 

Ma trận cho phép chúng ta kiểm tra xem hai người có biết nhau trong thời gian không đổi hay không. 
2. Đọc từng cặp người quen`(a, b)`. 

Chuyển đổi các chỉ mục thành lập chỉ mục dựa trên 0 và đặt cả hai`adj[a][b]`Và`adj[b][a]`ĐẾN`True`. 

Đồ thị không có hướng nên cả hai hướng đều phải được lưu trữ. 
3. Liệt kê từng bộ ba người riêng biệt`(i, j, k)`với`i < j < k`. 

Việc sử dụng bộ ba có thứ tự sẽ tránh trùng lặp và đảm bảo mỗi nhóm ba người đều được kiểm tra chính xác một lần. 
4. Đối với bộ ba hiện tại, hãy tính xem cả ba cặp có được kết nối hay không. 

Các cạnh cần thiết là:`i-j`,`i-k`, Và`j-k`. 
5. Tính xem có cả ba cặp vắng mặt hay không. 

Điều này có nghĩa là không có cạnh nào trong ba cạnh tồn tại. 
6. Nếu một trong hai điều kiện đúng, hãy in`"WIN"`và dừng lại. 

Chúng tôi chỉ cần một bộ ba hợp lệ. 
7. Nếu tất cả các bộ ba đều được chọn và không có bộ nào thỏa mãn điều kiện, hãy in`"FAIL"`. 

### Tại sao nó hoạt động 

Mọi câu trả lời hợp lệ chỉ phụ thuộc vào nhóm ba người. Thuật toán kiểm tra mọi bộ ba có thể có chính xác một lần. Với mỗi bộ ba, nó sẽ kiểm tra cả hai định nghĩa của bài toán: 

1. Làm quen theo cặp. 
2. Cặp đôi không quen biết. 

Nếu bộ ba như vậy tồn tại ở bất kỳ đâu trong biểu đồ thì phép liệt kê cuối cùng sẽ chạm đến nó và trả về`"WIN"`. Nếu thuật toán kết thúc mà không tìm được thì không tồn tại bộ ba đủ điều kiện, vì vậy`"FAIL"`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

def solve():
    m = int(input())

    adj = [[False] * 5 for _ in range(5)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1

        adj[a][b] = True
        adj[b][a] = True

    for i in range(5):
        for j in range(i + 1, 5):
            for k in range(j + 1, 5):

                ab = adj[i][j]
                ac = adj[i][k]
                bc = adj[j][k]

                all_friends = ab and ac and bc
                all_strangers = (not ab) and (not ac) and (not bc)

                if all_friends or all_strangers:
                    print("WIN")
                    return

    print("FAIL")

solve()
```Ma trận kề là cách biểu diễn đơn giản nhất ở đây vì đồ thị rất nhỏ và chúng ta thường đặt câu hỏi có dạng "không có cạnh".`(u, v)`có tồn tại không?". Mỗi lần tra cứu sẽ trở thành một truy cập mảng duy nhất. 

Các vòng lặp sử dụng`i < j < k`nên mỗi bộ ba xuất hiện đúng một lần. Điều này tránh công việc trùng lặp và ngăn chặn các trường hợp vô tình trong đó cùng một người xuất hiện nhiều lần. 

Các biến`ab`,`ac`, Và`bc`làm cho logic dễ đọc hơn và giảm các thao tác lập chỉ mục lặp đi lặp lại. Vì chỉ có ba cạnh liên quan cho mỗi bộ ba nên việc đặt tên rõ ràng cho chúng sẽ giúp điều kiện rõ ràng. 

Việc triển khai sẽ kiểm tra cả hai mẫu được yêu cầu một cách riêng biệt:```
all_friends = ab and ac and bc
all_strangers = (not ab) and (not ac) and (not bc)
```Việc trộn lẫn các điều kiện này vào một biểu thức thường tạo ra những sai lầm logic, đặc biệt là với các câu phủ định. 

Chương trình thoát ngay sau khi tìm được bộ ba hợp lệ vì một nhân chứng là đủ để xác định câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3
2 3
1 4
5 3
```Các cạnh kề là:`(1,3), (2,3), (1,4), (3,5)`Thuật toán kiểm tra bộ ba theo thứ tự. 

| Ba | Các cạnh hiện có | Tất cả bạn bè | Tất Cả Người Lạ | 
| --- | --- | --- | --- | 
| (1,2,3) | 1-3, 2-3 | Không | Không | 
| (1,2,4) | 1-4 | Không | Không | 
| (1,2,5) | không | Không | Có | 

Thuật toán in:```
WIN
```Dấu vết này cho thấy chúng ta không cần một mối tình tay ba. Bộ ba`(1,2,5)`tạo thành ba người lạ theo cặp, điều này đã thỏa mãn bài toán. 

### Ví dụ 2 

đầu vào:```
5
1 2
2 3
3 4
4 5
5 1
```Đồ thị này là một chu trình có độ dài năm. 

| Ba | Các cạnh hiện có | Tất cả bạn bè | Tất Cả Người Lạ | 
| --- | --- | --- | --- | 
| (1,2,3) | 1-2, 2-3 | Không | Không | 
| (1,2,4) | 1-2 | Không | Không | 
| (1,2,5) | 1-2, 1-5 | Không | Không | 
| (1,3,4) | 3-4 | Không | Không | 
| (1,3,5) | 1-5 | Không | Không | 
| (1,4,5) | 4-5, 1-5 | Không | Không | 
| (2,3,4) | 2-3, 3-4 | Không | Không | 
| (2,3,5) | 2-3 | Không | Không | 
| (2,4,5) | 4-5 | Không | Không | 
| (3,4,5) | 3-4, 4-5 | Không | Không | 

Không có bộ ba nào thỏa mãn cả hai điều kiện, nên thuật toán in ra:```
FAIL
```Ví dụ này là phản ví dụ nổi tiếng cho thấy câu nói trở thành sai đối với năm người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có 10 bộ ba được kiểm tra | 
| Không gian | O(1) | Kích thước ma trận kề được cố định ở mức 5 x 5 | 

Thời gian chạy thực tế là không đổi vì kích thước biểu đồ không bao giờ thay đổi. Ngay cả cách tiếp cận bạo lực đơn giản nhất cũng dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        m = int(input())

        adj = [[False] * 5 for _ in range(5)]

        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1

            adj[a][b] = True
            adj[b][a] = True

        for i in range(5):
            for j in range(i + 1, 5):
                for k in range(j + 1, 5):

                    ab = adj[i][j]
                    ac = adj[i][k]
                    bc = adj[j][k]

                    all_friends = ab and ac and bc
                    all_strangers = (not ab) and (not ac) and (not bc)

                    if all_friends or all_strangers:
                        return "WIN"

        return "FAIL"

    return solve()

# provided sample
assert run(
"""4
1 3
2 3
1 4
5 3
"""
) == "WIN", "sample 1"

# empty graph
assert run(
"""0
"""
) == "WIN", "all strangers"

# complete graph
assert run(
"""10
1 2
1 3
1 4
1 5
2 3
2 4
2 5
3 4
3 5
4 5
"""
) == "WIN", "all friendship triangles"

# 5-cycle Ramsey counterexample
assert run(
"""5
1 2
2 3
3 4
4 5
5 1
"""
) == "FAIL", "cycle of length 5"

# single friendship triangle
assert run(
"""3
1 2
2 3
1 3
"""
) == "WIN", "friendship triangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ trống | THẮNG | Phát hiện bộ ba độc lập | 
| Đồ thị hoàn chỉnh | THẮNG | Phát hiện tam giác tình bạn | 
| 5 chu kỳ | THẤT ​​BẠI | Phản ví dụ nổi tiếng | 
| Tam giác đơn | THẮNG | Phát hiện tình bạn cơ bản | 

## Vỏ cạnh 

Hãy xem xét biểu đồ trống:```
0
```Ma trận kề chỉ chứa`False`các giá trị. Bộ ba đầu tiên`(1,2,3)`không có cạnh nào cả. Thuật toán tính toán:```
all_friends = False
all_strangers = True
```Vì vậy, nó in chính xác:```
WIN
```Điều này xác nhận rằng thuật toán xử lý các đồ thị không có cạnh. 

Bây giờ hãy xem xét phản ví dụ năm chu kỳ:```
5
1 2
2 3
3 4
4 5
5 1
```Mỗi bộ ba đều chứa đúng một cạnh hoặc đúng hai cạnh. Không có cạnh nào chứa 0 cạnh hoặc ba cạnh. Thuật toán kiểm tra một cách có hệ thống tất cả mười bộ ba và không bao giờ tìm thấy một bộ ba hợp lệ, vì vậy nó in ra:```
FAIL
```Đây là trường hợp đúng đắn quan trọng vì nó chứng minh rằng tuyên bố này không đúng đối với năm người. 

Cuối cùng, hãy xem xét một tam giác tình bạn thuần túy:```
3
1 2
2 3
1 3
```Khi thuật toán đạt gấp ba`(1,2,3)`, tồn tại cả ba cạnh cần thiết:```
1-2 = True
1-3 = True
2-3 = True
```Vì thế`all_friends`trở thành`True`và thuật toán in ngay lập tức:```
WIN
```Điều này xác nhận rằng đồ thị được coi là tam giác vô hướng và tình bạn được phát hiện chính xác.
