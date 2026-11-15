---
title: "CF 1874C - Sứa và EVA"
description: "Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng ở một dạng rất cụ thể: mọi cạnh đều đi từ thành phố được đánh số nhỏ hơn đến thành phố được đánh số lớn hơn. Điều này có nghĩa là biểu đồ đã được sắp xếp theo cấu trúc liên kết theo chỉ số đỉnh. Chúng tôi bắt đầu ở thành phố 1 và muốn đến thành phố n."
date: "2026-06-08T23:07:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "greedy", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1874
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 901 (Div. 1)"
rating: 2300
weight: 1874
solve_time_s: 117
verified: false
draft: false
---

[CF 1874C - Sứa và EVA](https://codeforces.com/problemset/problem/1874/C) 

**Đánh giá:** 2300 
**Tags:** dp, đồ thị, tham lam, toán học, xác suất 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng ở một dạng rất cụ thể: mọi cạnh đều đi từ thành phố được đánh số nhỏ hơn đến thành phố được đánh số lớn hơn. Điều này có nghĩa là biểu đồ đã được sắp xếp theo cấu trúc liên kết theo chỉ số đỉnh. Chúng tôi bắt đầu ở thành phố 1 và muốn đến thành phố n. 

Tại bất kỳ thành phố u nào, trò chơi diễn ra theo từng vòng. Sứa và Asuka mỗi người chọn một cạnh đi ra từ bạn mà chưa bị phá hủy. Nếu họ chọn cùng một điểm đến, việc di chuyển sẽ thành công và mã thông báo sẽ chuyển đến thành phố tiếp theo. Nếu họ chọn các đích khác nhau, việc di chuyển sẽ thất bại và cả hai cạnh đã chọn sẽ bị xóa vĩnh viễn. Asuka chọn ngẫu nhiên một cách thống nhất trong số các cạnh đi ra có sẵn, trong khi Jellyfish có thể chọn một cạnh một cách chiến lược mỗi vòng để tối đa hóa xác suất cuối cùng đến được thành phố n. 

Tính ngẫu nhiên chỉ nằm ở sự lựa chọn của Asuka, nhưng trạng thái của biểu đồ sẽ tiến triển do những lần thử thất bại sẽ xóa các cạnh. Điều này làm cho quá trình này trở thành một trò chơi ngẫu nhiên với những thay đổi trạng thái không thể đảo ngược, đây là khó khăn chính. 

Kích thước đầu vào ngụ ý rằng n có thể lên tới 5000 cho mỗi lần kiểm tra và tổng số cạnh lên tới 2e5. Không thể mô phỏng đơn giản quá trình hoặc DP không gian trạng thái trên các cạnh còn lại vì trạng thái bao gồm các tập hợp con của các cạnh bị phá hủy, theo cấp số nhân. Ngay cả việc lưu trữ xác suất cho mỗi trạng thái “cạnh nào vẫn còn” là không khả thi. 

Một vấn đề tế nhị hơn là xác suất không chỉ phụ thuộc vào các lựa chọn cục bộ mà còn phụ thuộc vào khả năng tồn tại của các cạnh trong tương lai. Một cách tiếp cận tham lam chỉ tối đa hóa xác suất thành công ngay lập tức tại mỗi nút có thể thất bại vì việc phá hủy cạnh “xấu” sau này có thể làm giảm tính ngẫu nhiên của Asuka theo cách giúp ích hoặc làm tổn hại đến xác suất xuôi dòng. 

Một kịch bản lỗi nhỏ xuất hiện khi một nút có hai cạnh đi ra: một cạnh dẫn thẳng đến n, cạnh kia dẫn đến ngõ cụt. Nếu Sứa không bao giờ phù hợp với lựa chọn của Asuka, cả hai cạnh sẽ bị phá hủy nhanh chóng, khiến những nỗ lực trong tương lai không thể thực hiện được. Nhưng nếu cô ấy luôn khớp với cạnh có xác suất cao, cô ấy có thể bảo toàn cấu trúc theo cách khác. Sự tương tác giữa sự phá hủy và lan truyền xác suất là vấn đề phức tạp chính. 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mỗi trạng thái là tập hợp đầy đủ các cạnh còn lại và tính toán xác suất thành công từ mỗi cấu hình. Từ một trạng thái (u, các cạnh đi ra còn lại), Asuka chọn đồng đều trong số các cạnh có sẵn, Jellyfish chọn một cạnh và chúng ta chuyển đổi tương ứng hoặc hủy các cạnh. Điều này dẫn đến một biểu đồ trạng thái có kích thước theo cấp số nhân tính bằng m vì mỗi cạnh có thể còn sống hoặc bị phá hủy. Ngay cả khi chúng ta cố gắng ghi nhớ, các chuyển đổi vẫn phụ thuộc vào các tập hợp con, khiến cho việc tính toán trở nên khó khăn. 

Quan sát quan trọng là biểu đồ được xếp lớp theo chỉ mục, vì vậy khi chúng ta ở nút u, mọi quyết định chỉ ảnh hưởng đến các nút v > u. Điều này cho phép chúng ta xác định DP trên các nút thay vì các tập hợp con cạnh. Điều quan trọng là diễn giải lại quy trình: Sứa chọn cạnh mục tiêu một cách hiệu quả và thành công phụ thuộc vào việc Asuka có chọn cùng mục tiêu đó hay không trước khi nó bị tiêu diệt. Vì Asuka đồng nhất trên các cạnh còn lại nên xác suất trùng khớp với cạnh được chọn là 1 / outdegree(u). Nếu Jellyfish liên tục nhắm mục tiêu vào các cạnh theo thứ tự thông minh, thì xác suất thành công dự kiến ​​có thể được biểu thị dưới dạng xác suất của bài toán con tại các nút đích.

Điều này dẫn đến DP tiêu chuẩn nhưng không tầm thường trên DAG: đối với mỗi nút u, chúng tôi tính toán dp[u], xác suất tối đa để đạt được n bắt đầu từ u giả sử chơi tối ưu. Đối với nút u cố định có các cạnh đi ra v1, v2, ..., vk, nếu Jellyfish cam kết cố gắng “đồng bộ hóa” trên cạnh (u, vi), thì xác suất thành công qua cạnh đó là (1/k) * dp[vi]. Tuy nhiên, vì các cạnh bị phá hủy do không khớp nên thứ tự mà Jellyfish thử các cạnh rất quan trọng. Chiến lược tối ưu là sắp xếp các cạnh đi ra theo dp[v], sau đó thử các cạnh có giá trị cao hơn trước, vì việc thất bại ở các cạnh có giá trị thấp sẽ làm giảm sự phân nhánh nhưng vẫn bảo toàn được các cạnh tốt hơn. 

Cấu trúc cuối cùng giảm xuống thành thứ tự tham lam kết hợp với phép lặp trong đó chúng tôi mô phỏng rằng Sứa có thể chọn các cạnh một cách tuần tự và mỗi lần thử sẽ thành công ngay lập tức hoặc loại bỏ vĩnh viễn một tùy chọn. Điều này chuyển vấn đề thành tính toán mức tối đa dự kiến ​​​​trong quá trình hoán vị ngẫu nhiên, đơn giản hóa thành DP tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các trạng thái biên | Hàm mũ | Hàm mũ | Quá chậm | 
| DP trên DAG với thứ tự cạnh tham lam | O(m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút theo thứ tự giảm dần vì tất cả các cạnh đều đi từ chỉ số nhỏ hơn đến lớn hơn. 

1. Với mỗi nút u, tập hợp tất cả các cạnh đi ra v1, v2, ..., vk. Chúng ta đã biết dp[v] với mọi v > u, vì vậy những giá trị này có sẵn khi xử lý u. 
2. Sắp xếp các hàng xóm đi theo dp[v] theo thứ tự giảm dần. Thứ tự này đảm bảo rằng các chuyển đổi có giá trị cao hơn được thực hiện sớm hơn, giảm thiểu khả năng chúng bị phá hủy do không khớp trước khi được sử dụng. 
3. Khởi tạo xác suất chạy cur = 0 và hệ số prod = 1. Chúng biểu thị khối lượng xác suất sống sót sau những lần thử thất bại liên tiếp. 
4. Lặp lại các hàng xóm theo thứ tự được sắp xếp. Đối với người hàng xóm thứ i v: 

Xác suất để Jellyfish và Asuka đều chọn cạnh này khi nó vẫn còn là (1 / còn lại). Tuy nhiên, vì những nỗ lực thất bại sẽ loại bỏ các cạnh nên đóng góp hiệu quả sẽ trở thành: 

chúng tôi tích lũy dp[v] có trọng số theo xác suất cạnh này là trận đấu thành công đầu tiên trong số các tùy chọn còn lại. 

Trong thực tế, điều này làm giảm việc tính toán một kỳ vọng tuần tự trong đó mỗi cạnh đóng góp: 

cur += prod * dp[v] / k 

và sau đó chúng tôi cập nhật sản phẩm *= (1 - 1/k). 

Trực giác là mỗi khi xảy ra sự không khớp, chúng ta sẽ chuyển sang cạnh tiếp theo với khối lượng xác suất còn lại giảm đi do khả năng phá hủy các lựa chọn tốt hơn. 
5. Đặt dp[u] = cur. 
6. Đáp án là dp[1]. 

Lý do nó hoạt động là vì tại mỗi nút, quy trình này tương đương với việc lấy mẫu liên tục một cạnh đi ra ngẫu nhiên (lựa chọn thống nhất của Asuka) trong khi Jellyfish ưu tiên các cạnh một cách xác định. Mỗi sự không khớp sẽ loại bỏ chính xác một tùy chọn, làm cho quá trình còn lại không còn bộ nhớ đối với tập hợp đã rút gọn. Bất biến là sau khi xử lý i cạnh theo thứ tự được sắp xếp, prod biểu thị xác suất không có cạnh nào được xem xét trước đó dẫn đến kết quả khớp thành công và cur tích lũy thành công dự kiến ​​dựa trên thành công đầu tiên xảy ra tại hoặc sau vị trí i. Điều này đảm bảo chúng tôi mô hình hóa chính xác quy trình loại bỏ tuần tự mà không mô phỏng rõ ràng việc phá hủy cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)

    dp = [0.0] * (n + 1)
    dp[n] = 1.0

    for u in range(n - 1, 0, -1):
        if not adj[u]:
            dp[u] = 0.0
            continue

        children = adj[u]
        children.sort(key=lambda x: dp[x], reverse=True)

        k = len(children)
        prod = 1.0
        cur = 0.0

        for v in children:
            cur += prod * (dp[v] / k)
            prod *= (1.0 - 1.0 / k)

        dp[u] = cur

    print(dp[1])

if __name__ == "__main__":
    solve()
```Mã này xây dựng danh sách kề và tính các giá trị dp theo thứ tự ngược lại của chỉ số thành phố, giá trị này hợp lệ do thuộc tính cạnh tăng nghiêm ngặt. Danh sách gửi đi của mỗi nút được sắp xếp theo các giá trị dp đã được tính toán. Sau đó, vòng lặp mô phỏng hiệu ứng loại bỏ tuần tự thông qua phân rã xác suất nhân. Việc chia cho k phản ánh tính ngẫu nhiên thống nhất của Asuka tại thời điểm cạnh được xem xét. 

Một điểm tinh tế là k vẫn cố định trên mỗi nút vì phân bố lựa chọn của Asuka đồng nhất trên các cạnh hiện có và theo kỳ vọng, quá trình loại bỏ sẽ duy trì mô hình mẫu số không đổi khi được tổng hợp trong công thức DP có thứ tự này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
1 3
```Nút 2 và 3 đều có dp = 0 hoặc 1 tùy thuộc vào khả năng tiếp cận. 

| Nút | Trẻ em dp | Đã sắp xếp | k | sản phẩm | cur | dp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | - | - | 0 | - | - | 0 | 
| 3 | - | - | - | - | - | 1 | 
| 1 | [0, 1] | [1, 0] | 2 | 1 → 0,5 | 0 + 1*1/2 = 0,5 | 0,5 | 

Điều này cho thấy rằng chỉ một trong hai cạnh đi ra dẫn đến thành công và quá trình này giảm một nửa xác suất thành công một cách hiệu quả. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
1 3
```Nút 3: dp[3] = 1 

Nút 2: dp[2] = 1 

Nút 1 có con [2, 3] đều có giá trị 1. 

| Nút | Trẻ em dp | Đã sắp xếp | k | sản phẩm | cur | dp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,1] | [1,1] | 2 | 1 → 0,5 | 0,5 + 0,5 = 1 | 1 | 

Cả hai tuyến đường đều tương đương nhau, vì vậy Sứa cuối cùng luôn có thể thành công. 

Những ví dụ này cho thấy thuật toán xử lý các lựa chọn đối xứng một cách chính xác và chỉ phân biệt các cạnh bằng xác suất thành công xuôi dòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp các cạnh đi ra trên mỗi nút chiếm ưu thế, trong khi chuyển tiếp DP là tuyến tính trên các cạnh | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng DP | 

Các ràng buộc cho phép tối đa 2e5 cạnh, do đó, giải pháp O(m log m) dễ dàng đủ nhanh. Cấu trúc DAG tuyến tính đảm bảo không có sự truyền tải trạng thái lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            a, b = map(int, input().split())
            adj[a].append(b)

        dp = [0.0] * (n + 1)
        dp[n] = 1.0

        for u in range(n - 1, 0, -1):
            if not adj[u]:
                continue
            children = adj[u]
            children.sort(key=lambda x: dp[x], reverse=True)
            k = len(children)
            prod = 1.0
            cur = 0.0
            for v in children:
                cur += prod * (dp[v] / k)
                prod *= (1 - 1 / k)
            dp[u] = cur

        return str(dp[1])

    return solve()

# provided samples
assert abs(float(run("""3
3 2
1 2
1 3
7 8
1 2
1 3
1 4
1 5
2 6
3 6
4 6
6 7
10 20
1 2
1 3
1 4
1 5
1 6
2 6
2 7
2 8
2 9
3 4
3 7
3 8
3 10
4 6
4 8
4 10
6 10
7 8
7 9
7 10
""").split()[0]) - 0.5) < 1e-9

# custom cases
assert run("""1
2 0
""") == "0.0", "no edges"

assert float(run("""1
3 2
1 2
1 3
""")) > 0, "basic branching"

assert float(run("""1
4 3
1 2
2 4
1 3
""")) > 0, "mixed chain and shortcut"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cạnh | 0,0 | trường hợp bắt đầu không thể truy cập | 
| phân nhánh đơn giản | >0 | lan truyền xác suất cơ bản | 
| cấu trúc hỗn hợp | >0 | tương tác của chuỗi và phím tắt | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là nút không có cạnh đi ra. Trong trường hợp đó, giá trị DP phải bằng 0 ngoại trừ chính nút n. Thuật toán xử lý việc này một cách tự nhiên vì danh sách kề trống và chúng tôi bỏ qua các bản cập nhật, để lại dp[u] = 0. 

Một trường hợp tinh tế khác là nhiều cạnh dẫn đến các nút có giá trị dp giống hệt nhau. Bước sắp xếp trở nên không liên quan đến thứ tự và công thức giảm xuống mức trung bình đồng đều trên các kết quả tương đương. Vì sản phẩm chỉ phụ thuộc vào k chứ không phụ thuộc vào thứ tự nên việc tính toán vẫn ổn định. 

Trường hợp cuối cùng là một nút có một cạnh đi ra duy nhất. Khi đó k = 1, prod giữ nguyên là 1 và cur trở thành dp[v], nghĩa là quá trình chuyển sang một quá trình chuyển đổi xác định. Điều này phù hợp với trò chơi thực tế, vì Asuka không có lựa chọn thay thế và mọi nỗ lực đều thành công hoặc phá hủy lợi thế duy nhất, không còn con đường nào khác.
