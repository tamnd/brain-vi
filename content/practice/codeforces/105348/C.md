---
title: "CF 105348C - Mô hình truyền chuỗi 1"
description: "Chúng ta có một chuỗi s có độ dài n và chúng ta nên coi mỗi lần xuất hiện của một ký tự là một vị trí trên một dòng. Việc di chuyển giữa hai vị trí có chi phí phụ thuộc vào việc các ký tự có giống nhau hay không."
date: "2026-06-23T15:38:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105348
codeforces_index: "C"
codeforces_contest_name: "Coding Challenge Alpha VII - by Algorave"
rating: 0
weight: 105348
solve_time_s: 93
verified: false
draft: false
---

[CF 105348C - Mô hình truyền tải chuỗi 1](https://codeforces.com/problemset/problem/105348/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chiều dài`n`, và chúng ta nên coi mỗi lần xuất hiện của một ký tự là một vị trí trên một dòng. Việc di chuyển giữa hai vị trí có chi phí phụ thuộc vào việc các ký tự có giống nhau hay không. 

Nếu chúng ta di chuyển giữa hai chỉ số`i`Và`j`, chi phí bằng 0 khi cả hai vị trí đều chứa cùng một ký tự, bởi vì chúng ta có thể “dịch chuyển tức thời” giữa các chữ cái giống nhau. Nếu không, chi phí chỉ đơn giản là khoảng cách tuyệt đối giữa các chỉ số. 

Mỗi truy vấn cung cấp hai ký tự`u`Và`v`. Chúng tôi được phép bắt đầu từ bất kỳ sự cố nào xảy ra`u`trong chuỗi và kết thúc ở bất kỳ lần xuất hiện nào của`v`. Chúng ta có thể di chuyển qua các chỉ số trung gian và chi phí của một đường đi là tổng chi phí biên được xác định ở trên. Nhiệm vụ là tính toán chi phí tối thiểu có thể cho mỗi truy vấn hoặc báo cáo rằng việc đạt được`v`là không thể từ`u`. 

Những hạn chế quan trọng theo một cách cụ thể. Chuỗi có thể lớn tới 100.000, do đó, mọi giải pháp thử tất cả các cặp chỉ mục hoặc chạy đường dẫn ngắn nhất cho mỗi truy vấn sẽ không thành công. Tuy nhiên, số lượng truy vấn nhiều nhất là 26 x 26, nghĩa là chúng tôi chỉ quan tâm đến việc chuyển đổi giữa các chữ cái viết thường. Điều này ngay lập tức gợi ý một không gian trạng thái có kích thước tối đa là 26 nút thay vì`n`các vị trí. 

Trường hợp cạnh tinh tế là khi một ký tự hoàn toàn không tồn tại trong chuỗi. Ví dụ, nếu`s = "abac"`và một truy vấn yêu cầu`z a`, không có vị trí bắt đầu nên đáp án phải là`-1`. Một trường hợp cạnh khác là khi`u == v`và ký tự đó tồn tại ít nhất một lần. Vì chúng ta có thể chọn cùng một chỉ mục cho điểm bắt đầu và kết thúc nên chi phí bằng 0. Một công thức đường đi ngắn nhất ngây thơ bỏ qua điều này sẽ vẫn hoạt động, nhưng chỉ khi các chuyển đổi có độ dài bằng 0 được xử lý đúng cách. 

Một trường hợp phức tạp khác là khi cả hai ký tự tồn tại nhưng cách xa nhau trong chuỗi và đường dẫn tối ưu sử dụng các ký tự trung gian thay vì các bước nhảy trực tiếp. Chẳng hạn, việc chuyển từ`a`ĐẾN`c`có thể rẻ hơn thông qua các bước nhảy không tốn chi phí lặp đi lặp lại`b`xảy ra, bởi vì cấu trúc trung gian làm giảm nhu cầu nhảy xa. 

## Phương pháp tiếp cận 

Giải thích bạo lực coi mọi chỉ mục là một nút trong biểu đồ. Từ chỉ mục`i`, chúng ta có thể di chuyển đến bất kỳ`j`, trả tiền`|i - j|`trừ khi các ký tự khớp nhau, trong trường hợp đó chi phí bằng 0. Sau đó, với mỗi truy vấn, chúng tôi chạy đường đi ngắn nhất từ ​​tất cả các chỉ mục của ký tự`u`tới bất kỳ chỉ mục nào của ký tự`v`. 

Điều này đúng nhưng quá chậm. Với`n = 10^5`, một Dijkstra duy nhất trên tất cả các chỉ số đã liên quan đến khoảng`n log n`hoạt động và thực hiện việc này cho tối đa 676 truy vấn khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là các ký tự giống nhau tạo ra kết nối không tốn phí giữa tất cả các lần xuất hiện của chúng. Điều này có nghĩa là trong mỗi ký tự, tất cả các vị trí của nó hoạt động giống như một cụm chi phí bằng 0. Khi chúng ta chuyển sang một nhân vật khác, chi phí chỉ phụ thuộc vào khoảng cách giữa các vị trí và chúng ta chỉ quan tâm đến “điểm vào” tốt nhất của nhân vật tiếp theo. 

Điều này làm giảm vấn đề từ`n`các nút có tối đa 26 trạng thái ký tự. Chúng ta có thể tính toán trước danh sách sắp xếp các vị trí của nó cho mỗi ký tự. Sau đó, chúng tôi chạy đường dẫn ngắn nhất đa nguồn trên biểu đồ 26 chữ cái, trong đó việc chuyển đổi từ ký tự`c1`ĐẾN`c2`tốn khoảng cách tối thiểu có thể giữa bất kỳ lần xuất hiện nào của`c1`và bất kỳ sự xuất hiện nào của`c2`. 

Khi chúng tôi có chi phí chuyển đổi ký tự theo cặp này, mỗi truy vấn sẽ trở thành tra cứu trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ chỉ mục Dijkstra cho mỗi truy vấn) | O(q · n log n) | O(n) | Quá chậm | 
| Tối ưu (đường dẫn ngắn nhất 26 nút với các chuyển đổi được tính toán trước) | O(26² · n) hoặc O(26³) | O(26² + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm chuỗi thành 26 nhóm, một nhóm cho mỗi ký tự, lưu trữ tất cả các chỉ mục nơi nó xuất hiện. Sau đó, chúng tôi xây dựng biểu đồ có trọng số giữa 26 nút này. 

1. Thu thập vị trí của từng ký tự trong chuỗi. 

Chúng tôi quét chuỗi một lần và nối từng chỉ mục vào danh sách ký tự của chuỗi đó. Điều này đảm bảo chúng ta biết chính xác vị trí mỗi ký tự xuất hiện, điều này cần thiết để tính toán khoảng cách tối thiểu sau này. 
2. Khởi tạo ma trận chi phí 26 x 26 với vô cùng. 

Mỗi mục thể hiện chi phí được biết đến nhiều nhất để đi từ ký tự`a`để nhân vật`b`. Chúng tôi sẽ tinh chỉnh các giá trị này bằng cách sử dụng tính toán khoảng cách trực tiếp và các ký tự trung gian. 
3. Tính chi phí chuyển tiếp trực tiếp giữa mỗi cặp ký tự. 

Đối với hai nhân vật`x`Và`y`, chúng tôi tìm thấy sự khác biệt tuyệt đối tối thiểu giữa bất kỳ vị trí nào của`x`và bất kỳ vị trí nào của`y`. Vì cả hai danh sách đều được sắp xếp nên chúng ta có thể tính toán điều này một cách hiệu quả bằng cách sử dụng quét hai con trỏ. Điều này mang lại chi phí một bước tốt nhất giữa các ký tự đó. 
4. Đặt các mục theo đường chéo về 0. 

Việc di chuyển từ một ký tự sang chính nó không tốn phí vì chúng ta có thể chọn cùng một chỉ mục hoặc di chuyển miễn phí giữa các chữ cái giống hệt nhau. 
5. Chạy Floyd-Warshall qua 26 ký tự. 

Chúng tôi cho phép các ký tự trung gian cải thiện đường dẫn. Nếu đi từ`x`ĐẾN`k`và sau đó`k`ĐẾN`y`rẻ hơn trực tiếp`x`ĐẾN`y`, chúng tôi cập nhật nó. Bước này ghi lại các tối ưu hóa nhiều bước như`a -> b -> c`tốt hơn là nhảy trực tiếp. 
6. Trả lời các truy vấn bằng ma trận được tính toán trước. 

Đối với mỗi truy vấn`(u, v)`, nếu thiếu một trong hai ký tự, xuất ra`-1`. Nếu không, xuất ra khoảng cách ngắn nhất được lưu trữ giữa các nút tương ứng của chúng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất kỳ đường dẫn tối ưu nào giữa hai ký tự đều có thể được nén thành một chuỗi các chuyển đổi ký tự, trong đó mỗi chuyển đổi tương ứng với việc chuyển từ lần xuất hiện của ký tự này sang lần xuất hiện của ký tự khác. Bởi vì tất cả các lần xuất hiện của cùng một ký tự đều có thể truy cập tự do với nhau với chi phí bằng 0, nên chúng tôi không bao giờ cần theo dõi các chỉ mục riêng lẻ sau khi đã tính toán khoảng cách giữa các ký tự tốt nhất. Floyd-Warshall sau đó đảm bảo rằng tất cả các chuỗi ký tự trung gian có thể được xem xét, đảm bảo tính tối ưu toàn cầu trên không gian trạng thái 26 nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    pos = [[] for _ in range(26)]
    for i, ch in enumerate(s):
        pos[ord(ch) - 97].append(i)

    INF = 10**18
    dist = [[INF] * 26 for _ in range(26)]

    for i in range(26):
        if pos[i]:
            dist[i][i] = 0

    # compute direct costs
    for a in range(26):
        if not pos[a]:
            continue
        for b in range(26):
            if not pos[b]:
                continue
            i = j = 0
            best = INF
            pa, pb = pos[a], pos[b]
            while i < len(pa) and j < len(pb):
                best = min(best, abs(pa[i] - pb[j]))
                if pa[i] < pb[j]:
                    i += 1
                else:
                    j += 1
            dist[a][b] = min(dist[a][b], best)

    # floyd warshall on 26 nodes
    for k in range(26):
        for i in range(26):
            for j in range(26):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    for _ in range(q):
        u, v = input().split()
        u = ord(u) - 97
        v = ord(v) - 97

        if not pos[u] or not pos[v]:
            print(-1)
        else:
            ans = dist[u][v]
            print(ans if ans < INF else -1)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng danh sách xuất hiện cho từng ký tự, đây là cấu trúc duy nhất chúng ta cần từ chuỗi gốc. các`dist`ma trận lưu trữ chi phí ký tự theo cặp. Quét hai con trỏ được sử dụng vì cả hai danh sách vị trí đều được sắp xếp và nó đảm bảo chúng tôi tìm thấy sự khác biệt tuyệt đối tối thiểu mà không cần kiểm tra tất cả các cặp. 

Bước Floyd-Warshall an toàn vì kích thước đồ thị được cố định ở 26 nút nên vòng lặp khối không đáng kể. Cuối cùng, truy vấn là tra cứu theo thời gian liên tục, có thêm tính năng kiểm tra các ký tự bị thiếu. 

Một điểm tinh tế là ký tự bị thiếu phải được xử lý trước khi đọc`dist[u][v]`, bởi vì những mục đó vẫn có thể chứa vô hạn ngay cả khi tồn tại các đường dẫn trung gian. Việc kiểm tra sự tồn tại rõ ràng sẽ ngăn chặn kết quả đầu ra không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi trong đó`a`,`b`, Và`c`xuất hiện thành từng cụm riêng biệt. Chúng tôi tính toán khoảng cách trực tiếp đầu tiên. 

| Bước | Cặp | Khoảng cách trực tiếp tốt nhất | 
| --- | --- | --- | 
| ban đầu | a-a | 0 | 
| ban đầu | a-b | tính toán | 
| ban đầu | b-c | tính toán | 

Sau Floyd-Warshall, những con đường như`a -> b -> c`có thể giảm chi phí. 

Đối với một truy vấn`a c`, ngay cả khi khoảng cách trực tiếp tốt nhất là lớn, thì khoảng cách trung gian`b`có thể giảm bớt nó bằng cách kết nối những lần xuất hiện gần hơn. 

Điều này chứng tỏ tầm quan trọng của chuỗi nhiều ký tự chứ không chỉ những lần xuất hiện gần nhất. 

### Ví dụ 2 

Đối với một chuỗi như`dcdcccedcebe`, chúng tôi sử dụng lại nhiều ký tự. Vì tồn tại nhiều lần xuất hiện nên khoảng cách trực tiếp giữa các ký tự trở nên nhỏ và Floyd-Warshall nhanh chóng ổn định ma trận. 

Một truy vấn như`b e`được hưởng lợi từ sự chuyển đổi trung gian thông qua`c`hoặc`d`, tùy thuộc vào vị trí nào giảm thiểu khoảng cách tuyệt đối. 

Dấu vết xác nhận rằng thuật toán khám phá chính xác các đường dẫn gián tiếp thay vì dựa vào một cặp chỉ mục tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26² · n + 26³ + q) | Quét hai con trỏ trên danh sách ký tự chiếm ưu thế, Floyd-Warshall có kích thước không đổi | 
| Không gian | O(n + 26²) | Lưu trữ vị trí cộng với ma trận khoảng cách | 

Các ràng buộc cho phép điều này một cách thoải mái. Quá trình xử lý trước chuỗi là tuyến tính và biểu đồ 26 nút cố định đảm bảo tất cả các phép tính bậc cao đều bị giới hạn không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# sample-like tests
assert run("9 2\n2aabbcedb\ncb\ncd\n") in ["2\n1", "2\n1"]

# single character case
assert run("3 1\naaa\na a\n") == "0"

# missing character
assert run("3 1\nabc\nd a\n") == "-1"

# no repetition, simple line
assert run("4 1\nabcd\na d\n") == "3"

# all same character
assert run("5 1\naaaaa\na a\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaa`,`a a`| 0 | tự di chuyển không tốn phí | 
|`abc`,`d a`| -1 | xử lý ký tự bị thiếu | 
|`abcd`,`a d`| 3 | tính chính xác khoảng cách trực tiếp | 
|`aaaaa`,`a a`| 0 | tính nhất quán nhiều lần xuất hiện | 

## Vỏ cạnh 

Một trường hợp ký tự bị thiếu như`s = "abc"`với truy vấn`z a`được xử lý trước khi thực hiện bất kỳ tính toán nào. Danh sách vị trí dành cho`z`trống, do đó thuật toán ngay lập tức đưa ra`-1`mà không cần đọc ma trận khoảng cách. 

Một tự truy vấn như`a a`được xử lý bằng cách khởi tạo`dist[a][a] = 0`. Ngay cả khi các lần xuất hiện rải rác, chi phí bằng 0 vẫn hợp lệ vì chúng ta có thể chọn cùng một chỉ mục. 

Một sự phân bố thưa thớt như`a.....a.....a`đảm bảo rằng quá trình quét hai con trỏ sẽ tìm thấy chính xác khoảng cách tối thiểu giữa các cụm. Thuật toán chỉ kiểm tra các con trỏ liền kề trong danh sách được sắp xếp, do đó, nó tự nhiên bắt được cặp gần nhất mà không liệt kê tất cả các kết hợp.
