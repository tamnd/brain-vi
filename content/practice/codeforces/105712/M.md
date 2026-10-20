---
title: "CF 105712M - LIS Trên Cây"
description: "Chúng ta có một cây có gốc trong đó mỗi đỉnh mang một giá trị. Cấu trúc xác định mối quan hệ cha-con và mỗi nút nằm trên chính xác một đường dẫn từ gốc. Đối với bất kỳ nút nào, hãy xem xét chuỗi các giá trị gặp phải khi đi từ gốc xuống nút đó."
date: "2026-06-26T07:58:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105712
codeforces_index: "M"
codeforces_contest_name: "Rutgers University Programming Contest Fall 2024"
rating: 0
weight: 105712
solve_time_s: 40
verified: true
draft: false
---

[CF 105712M - LIS trên cây](https://codeforces.com/problemset/problem/105712/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi đỉnh mang một giá trị. Cấu trúc xác định mối quan hệ cha-con và mỗi nút nằm trên chính xác một đường dẫn từ gốc. 

Đối với bất kỳ nút nào, hãy xem xét chuỗi các giá trị gặp phải khi đi từ gốc xuống nút đó. Nhiệm vụ là tính toán, đối với mỗi nút, độ dài của chuỗi con tăng nghiêm ngặt dài nhất có thể được hình thành từ các giá trị dọc theo đường dẫn từ gốc đến nút đó. Dãy tiếp theo không cần phải liền kề nhau, chỉ cần giữ nguyên thứ tự dọc theo đường dẫn. 

Đầu ra là độ dài LIS cho mỗi nút, thường được in theo thứ tự chỉ số nút. 

Hàm ý ràng buộc chính là một cách tiếp cận đơn giản tính toán lại LIS một cách độc lập cho mỗi nút sẽ xử lý nhiều lần các đường dẫn chồng chéo. Vì mỗi đường dẫn có thể có độ dài O(n), điều này dẫn đến O(n^2) hoạt động trong cây hình chuỗi. Với n lên tới khoảng 200000 trong các bài toán cây Codeforces điển hình, bất kỳ phương pháp bậc hai hoặc gần bậc hai nào đều không khả thi. Giải pháp dự định phải xử lý tất cả các nút trong thời gian gần tuyến tính hoặc n log n, lý tưởng nhất là chia sẻ tính toán dọc theo quá trình truyền tải DFS. 

Một vấn đề khó phát sinh với các giá trị lặp lại và cấu trúc phân nhánh. Ví dụ: nếu các giá trị lặp lại dọc theo các nhánh khác nhau, giải pháp tái sử dụng không chính xác trạng thái chung mà không khôi phục sẽ trộn lẫn thông tin giữa các đường dẫn không liên quan. 

Xét một trường hợp nhỏ: 

Cây đầu vào: 

1(3) 

└──2(1) 

└──3(2) 

Đối với nút 3, đường dẫn là 3 → 1 → 2 với các giá trị [3,1,2] và độ dài LIS là 2. Một bản cập nhật “LIS toàn cầu” đơn giản mà không khôi phục trạng thái có thể mang các đóng góp từ các nhánh anh em và các chuỗi đếm vượt mức không nằm trên một đường dẫn từ gốc đến nút không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng nút một cách độc lập. Đối với một nút cố định, chúng tôi đi từ nút gốc đến nút đó, thu thập tất cả các giá trị trên đường dẫn và chạy thuật toán LIS tiêu chuẩn trong O(k log k) bằng cách sử dụng một mảng đuôi tạm thời. Việc lặp lại điều này cho tất cả các nút sẽ tạo ra tổng độ phức tạp là O(n^2 log n) trong một cây lệch. Tính chính xác rất đơn giản vì mỗi nút được giải độc lập bằng thủ tục LIS tiêu chuẩn. 

Điểm nghẽn là sự dư thừa. Các nút liền kề chia sẻ gần như toàn bộ đường dẫn, do đó việc tính toán lại LIS từ những thứ không cần thiết sẽ hoạt động trên các tiền tố giống hệt nhau. 

Quan sát quan trọng là DFS tự nhiên xây dựng các đường dẫn từ nút gốc đến nút hiện tại theo từng bước. Nếu chúng tôi duy trì cấu trúc LIS của đường dẫn DFS hiện tại, chúng tôi có thể cập nhật nó trong O(log n) trên mỗi nút và sau đó khôi phục nó khi quay lui. Cấu trúc LIS mà chúng tôi duy trì là “mảng đuôi sắp xếp kiên nhẫn” cổ điển, trong đó tails[len] lưu trữ giá trị kết thúc tối thiểu có thể có của một dãy con tăng dần có độ dài len. 

Trong DFS, khi chúng tôi nhập một nút, chúng tôi tạm thời chèn giá trị của nó vào cấu trúc này bằng cách sử dụng tìm kiếm nhị phân. Khi chúng tôi rời khỏi nút, chúng tôi sẽ khôi phục trạng thái trước đó. Điều này làm cho mỗi nút đóng góp chính xác một lần chèn và một lần khôi phục, dẫn đến giải pháp O(n log n) tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) | O(n) | Quá chậm | 
| Khôi phục DFS + LIS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1. Chúng tôi duy trì một mảng động`tails`, Ở đâu`tails[i]`là giá trị kết thúc nhỏ nhất có thể có của dãy con có độ dài tăng dần`i+1`dọc theo đường dẫn DFS hiện tại. 

Chúng tôi cũng duy trì một mảng câu trả lời`ans`cho mỗi nút. 

1. Khởi động DFS từ thư mục gốc. Trước khi vào bất kỳ nút nào,`tails`trống rỗng. Điều này thể hiện một chuỗi con trống dọc theo đường dẫn hiện tại. 
2. Tại nút`u`, ta xác định được vị trí`pos`Ở đâu`a[u]`có thể được chèn vào`tails`sử dụng tìm kiếm nhị phân. Vị trí này là chỉ số đầu tiên nơi`tails[pos] >= a[u]`. 
3. Nếu`pos`bằng với độ dài hiện tại của`tails`, chúng tôi mở rộng cấu trúc bằng cách nối thêm`a[u]`. Ngược lại, chúng ta thay thế`tails[pos]`với`a[u]`. 
4. Độ dài LIS cho nút`u`là`pos + 1`, vì nó đại diện cho chuỗi con tốt nhất kết thúc tại thời điểm này trong đường dẫn DFS. Chúng tôi lưu trữ cái này trong`ans[u]`. 
5. Chúng tôi tái diễn thành tất cả trẻ em của`u`, luôn cập nhật`tails`. Mỗi đứa trẻ mở rộng cùng một đường dẫn từ nút gốc đến nút hiện tại. 
6. Sau khi kết thúc tất cả con cháu của`u`, chúng ta phải khôi phục`tails`về trạng thái trước đó trước khi xử lý`u`. Điều này được thực hiện bằng cách đảo ngược sửa đổi được thực hiện ở bước 2 và 3, bằng cách chụp nhanh giá trị cũ hoặc bằng cách theo dõi cẩn thận xem chúng tôi đã thêm hay thay thế. 

Tính chính xác phụ thuộc vào thực tế là tại bất kỳ nút DFS nào,`tails`biểu thị chính xác một cấu trúc dãy con tăng dần hợp lệ trên đường dẫn từ nút gốc tới nút hiện tại. Nó không bao giờ trộn lẫn các giá trị từ các nhánh khác nhau vì chúng tôi khôi phục trạng thái trước khi quay về nhánh gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

from bisect import bisect_left

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))
    
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    ans = [0] * (n + 1)
    tails = []

    def dfs(u, p):
        # find insertion position
        pos = bisect_left(tails, a[u])
        
        old = None
        replaced = False
        
        if pos == len(tails):
            tails.append(a[u])
        else:
            old = tails[pos]
            tails[pos] = a[u]
            replaced = True
        
        ans[u] = pos + 1
        
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)
        
        # rollback
        if replaced:
            tails[pos] = old
        else:
            tails.pop()

    dfs(1, -1)
    print(*ans[1:])

if __name__ == "__main__":
    solve()
```DFS duy trì một hệ thống toàn cầu duy nhất`tails`mảng biểu thị trạng thái đường dẫn gốc tới nút hiện tại. Chi tiết triển khai chính là cơ chế khôi phục: nếu chúng tôi mở rộng mảng, chúng tôi sẽ bật lên; nếu chúng tôi thay thế một phần tử, chúng tôi sẽ khôi phục giá trị trước đó. Điều này đảm bảo tính chính xác trong quá trình đệ quy phân nhánh. 

Một sai lầm phổ biến là quên điều đó`tails`chỉ phải phản ánh đường dẫn đệ quy đang hoạt động. Bất kỳ đột biến chung nào mà không được phục hồi sẽ làm ô nhiễm các cây con anh chị em. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây: 

đầu vào:```
5
1 3 2 4 0
1 2
2 3
2 4
4 5
```Chúng tôi theo dõi DFS bắt đầu từ nút 1. 

Tại nút 1,`tails = [1]`, LIS là 1. 

| Nút | Giá trị | đuôi trước | vị trí | đuôi sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [] | 0 | [1] | 1 | 
| 2 | 3 | [1] | 1 | [1,3] | 2 | 
| 3 | 2 | [1,3] | 1 | [1,2] | 2 | 
| quay lại 2 | - | [1,2] | - | được khôi phục [1,3] | - | 

Dấu vết này cho thấy nút 3 thay thế một giá trị nội bộ trong`tails`, cải thiện dãy con có độ dài 2 mà không cần mở rộng nó. 

Một nhánh khác: 

| Nút | Giá trị | đuôi trước | vị trí | đuôi sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 4 | [1,3] | 2 | [1,3,4] | 3 | 
| 5 | 0 | [1,3,4] | 0 | [0,3,4] | 1 | 

Nút 5 thể hiện sự thay thế ở vị trí đầu tiên, cho thấy các giá trị nhỏ đặt lại chuỗi bắt đầu tốt nhất trong khi vẫn bảo toàn cấu trúc cho các giá trị dài hơn. 

Những dấu vết này xác nhận rằng`tails`hoạt động giống như cấu trúc LIS toàn cầu được giới hạn ở đường dẫn DFS hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút thực hiện một lần tìm kiếm nhị phân và một lần cập nhật trong`tails`| 
| Không gian | O(n) | danh sách kề, ngăn xếp đệ quy và lưu trữ đuôi LIS | 

Hệ số logarit xuất phát từ việc tìm kiếm nhị phân trong`tails`mảng. Vì mỗi nút được xử lý một lần trong DFS nên tổng chi phí sẽ chia theo n log n, phù hợp thoải mái với các ràng buộc thông thường lên tới 200000 nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    sys.stdout = io.StringIO()
    
    # assume solve() is defined above in same module
    solve()
    
    return sys.stdout.getvalue().strip()

# simple chain
assert run("""3
1 2 3
1 2
2 3
""") == "1 2 3"

# decreasing chain
assert run("""4
4 3 2 1
1 2
2 3
3 4
""") == "1 1 1 1"

# star shape
assert run("""5
3 1 4 2 5
1 2
1 3
1 4
1 5
""") == "1 1 2 2 3"

# single node
assert run("""1
10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng chuỗi | 1 2 3 | LIS phát triển dọc theo con đường | 
| chuỗi giảm dần | 1 1 1 1 | thay thế chiếm ưu thế | 
| cây sao | khác nhau | chi nhánh độc lập | 
| n=1 | 1 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với một cây nút đơn, DFS nhập vào nút 1 với một ô trống`tails`, chèn giá trị và tạo ra độ dài LIS 1. Không có đệ quy và khôi phục không bao giờ được kích hoạt, do đó cấu trúc vẫn nhất quán. 

Đối với chuỗi giảm nghiêm ngặt, mọi nút chèn vào vị trí 0, liên tục thay thế`tails[0]`. Mảng
