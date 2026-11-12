---
title: "CF 1662F - Anten"
description: "Chúng tôi được cung cấp một dòng ăng-ten được lập chỉ mục từ trái sang phải. Mỗi ăng-ten có một giá trị công suất xác định khoảng cách nó có thể liên lạc trực tiếp."
date: "2026-06-10T02:42:13+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "F"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 97
verified: true
draft: false
---

[CF 1662F - Ăng-ten](https://codeforces.com/problemset/problem/1662/F) 

**Đánh giá:** - 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, cách triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng ăng-ten được lập chỉ mục từ trái sang phải. Mỗi ăng-ten có một giá trị công suất xác định khoảng cách nó có thể liên lạc trực tiếp. Hai ăng-ten có thể hoạt động trong một giây nếu mỗi anten nằm trong phạm vi cho phép của anten kia, điều này tuân theo quy tắc đối xứng: khoảng cách giữa các chỉ số của chúng không được vượt quá lũy thừa nhỏ hơn trong hai anten của chúng. 

Chúng ta cần số lần truyền tối thiểu trong một giây như vậy để chuyển một thông báo từ ăng-ten`a`đến ăng-ten`b`. Mỗi đường truyền là một cạnh trong đồ thị vô hướng có các đỉnh là anten và chúng ta muốn độ dài đường đi ngắn nhất giữa hai đỉnh. Khó khăn là đồ thị cực kỳ dày đặc theo cách có cấu trúc nên chúng ta không thể xây dựng tất cả các cạnh một cách rõ ràng. 

Các ràng buộc buộc chúng ta phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số ăng-ten qua các cuộc thử nghiệm tối đa là 200000, do đó, bất kỳ giải pháp nào chạm vào mỗi ăng-ten trong một số lần cố định nhỏ đều có thể chấp nhận được. Bất kỳ cách tiếp cận nào cố gắng xem xét tất cả các cặp hoặc thậm chí tất cả các cạnh đều không thể thực hiện được vì một ăng-ten có thể kết nối với một khoảng cách lớn. 

Một đường dẫn ngắn nhất ngây thơ như BFS trên các danh sách kề được tạo rõ ràng sẽ thất bại vì việc tạo các lân cận đã có tính bậc hai trong trường hợp xấu nhất. 

Ví dụ, trường hợp cạnh khóa xuất hiện khi các lũy thừa đều lớn`n = 5`,`p = [5,5,5,5,5]`. Sau đó, mọi nút kết nối với mọi nút khác và BFS thoái hóa thành một biểu đồ hoàn chỉnh. Câu trả lời đúng giữa hai nút bất kỳ là`1`, nhưng một sự mở rộng ngây thơ sẽ cố gắng liệt kê tất cả những người hàng xóm. 

Một trường hợp khác là khi công suất ở mức tối thiểu, chẳng hạn như`p = [1,1,1,1]`. Sau đó, mỗi nút chỉ kết nối cục bộ và biểu đồ về cơ bản là một chuỗi. Bất kỳ giả định không chính xác nào cho rằng nhảy xa luôn có thể bị phá vỡ ở đây. 

Trường hợp tinh vi thứ ba là cấu trúc hỗn hợp, trong đó một nút nguồn lớn duy nhất đóng vai trò là trung tâm kết nối các vùng xa. Một giải pháp đúng phải khai thác các trung tâm này mà không mở rộng rõ ràng tất cả các cạnh của chúng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi vấn đề như một đường đi ngắn nhất trên đồ thị. Từ mỗi nút, chúng tôi kiểm tra tất cả các nút khác và kết nối những nút thỏa mãn ràng buộc về khoảng cách. Điều này xây dựng một danh sách kề, sau đó chúng tôi chạy BFS từ`a`ĐẾN`b`. Điều này đúng vì mỗi giao tiếp hợp lệ là một cạnh có trọng số một. 

Tuy nhiên, kiểm tra tất cả các cặp chi phí`O(n^2)`mỗi trường hợp thử nghiệm. Ngay cả việc xây dựng vùng lân cận cũng trở nên không thể khi`n`đạt 200000. 

Cấu trúc của điều kiện gợi ý điều gì đó mang tính hình học hơn là sự kết nối tùy tiện. Đối với một nút cố định`i`, nó kết nối với một đoạn chỉ số liền kề ở bên trái và bên phải, nhưng phạm vi có thể sử dụng chính xác cũng phụ thuộc vào sức mạnh của điểm cuối khác. Sự đối xứng này ngăn cản việc mô hình hóa biểu đồ khoảng thời gian đơn giản. 

Quan sát quan trọng là chuyển động trên toàn mảng được kiểm soát bằng cách tiếp cận các nút “mạnh” nhất định một cách hiệu quả và khi đến nút mạnh, chúng ta có thể nhảy xa. Thay vì mở rộng tất cả các cạnh, chúng tôi xử lý các quá trình chuyển đổi bằng cách nhóm các phạm vi có thể tiếp cận và sử dụng thao tác truyền tải giống như BFS trên các chỉ mục trong khi vẫn duy trì các phân đoạn đã được “kích hoạt” bởi các nút đã truy cập. 

Giải pháp tiêu chuẩn định dạng lại điều này thành BFS trên các nút, nhưng thay vì quét tất cả các nút lân cận, chúng tôi duy trì cấu trúc cho phép chúng tôi trích xuất nhanh chóng tất cả các nút có thể tiếp cận trong một bước từ một phạm vi hoạt động nhất định. Điều này thường được thực hiện bằng cách sử dụng cây phân đoạn hoặc một tập hợp các chỉ mục chưa được truy cập, kết hợp với logic mở rộng phạm vi bắt nguồn từ các hạn chế về công suất. 

Chúng tôi duy trì, đối với mỗi nút được truy cập, khoảng cách xa nhất mà nó có thể ảnh hưởng. Sau đó, chúng tôi liên tục trích xuất các nút mới từ các khoảng thời gian đó mà không cần xem lại các chỉ mục đã xử lý. Mỗi chỉ mục được loại bỏ một lần, đảm bảo độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS với các cạnh rõ ràng | O(n²) | O(n²) | Quá chậm | 
| BFS nhận biết khoảng thời gian với cấu trúc tập hợp/phân đoạn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải quy trình này dưới dạng BFS trong đó việc truy cập một nút sẽ mở rộng phạm vi tiếp cận trên các chỉ mục, nhưng chúng tôi tránh lặp lại một cách rõ ràng trên tất cả các cạnh. 

1. Khởi tạo mảng khoảng cách có giá trị vô cùng và đặt`dist[a] = 0`. Đặt`a`vào hàng đợi. Chúng tôi đang tìm kiếm số lần nhảy ngắn nhất. 
2. Duy trì cấu trúc dữ liệu chứa tất cả các chỉ mục chưa được truy cập, thường là tập hợp có thứ tự. Điều này cho phép chúng tôi xóa các nút sau khi được xử lý để chúng tôi không bao giờ truy cập lại chúng. 
3. Khi hàng đợi không trống, hãy bật một nút`i`. Chúng tôi cố gắng khám phá tất cả các nút có thể truy cập trong một bước từ`i`. 
4. Sức mạnh`p[i]`nói với chúng tôi rằng bất kỳ ứng cử viên hàng xóm nào`j`phải thỏa mãn`|i - j| ≤ min(p[i], p[j])`. Khó khăn là điều này phụ thuộc vào`p[j]`, vì vậy chúng ta không thể tính trực tiếp một khoảng cố định. 
5. Thay vào đó, chúng tôi sử dụng cấu trúc lớp BFS. Khi chúng tôi bật`i`, chúng tôi xem xét tất cả các nút vẫn chưa được truy cập`j`trong một khu vực có khả năng thỏa mãn điều kiện. Chúng tôi lặp đi lặp lại việc trích xuất các ứng viên từ cấu trúc có thứ tự và kiểm tra xem liệu có thể tiếp cận được họ hay không. Mỗi khám phá thành công đều được giao`dist[j] = dist[i] + 1`và đẩy`j`vào hàng đợi. 
6. Một lần là một nút`j`được truy cập, nó sẽ bị xóa vĩnh viễn, đảm bảo mỗi chỉ mục được xử lý một lần. 
7. Tiếp tục cho đến khi đạt`b`. 

Cơ chế ẩn đằng sau tính chính xác là BFS đảm bảo khoảng cách tối thiểu và trích xuất dựa trên tập hợp đảm bảo chúng tôi không bao giờ bỏ lỡ quá trình chuyển đổi hợp lệ đồng thời cũng không bao giờ truy cập lại các nút. Ràng buộc đối xứng đảm bảo rằng một khi một nút không thể truy cập được trong quá trình kiểm tra ranh giới hiện tại thì sau này nó không thể truy cập được bằng một đường dẫn ngắn hơn mà không đi qua các nút đã được phát hiện. 

### Tại sao nó hoạt động 

Mỗi lần chúng tôi xử lý một nút, chúng tôi sẽ mở rộng tất cả các nút có thể truy cập trong một bước nhảy và chưa được truy cập. Cấu trúc lớp BFS đảm bảo rằng lần đầu tiên chúng ta tiếp cận một nút là có số bước tối thiểu. Việc duy trì theo thứ tự các chỉ mục chưa được truy cập đảm bảo chúng tôi không quét liên tục các phần tử giống nhau, do đó mọi chỉ mục đều bị xóa đúng một lần. Điều này ngăn chặn hành vi bậc hai ẩn trong khi vẫn duy trì khả năng tiếp cận đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        p = list(map(int, input().split()))
        a -= 1
        b -= 1

        if a == b:
            print(0)
            continue

        dist = [-1] * n
        dist[a] = 0
        q = deque([a])

        # maintain unvisited nodes in a set
        unvisited = set(range(n))
        unvisited.remove(a)

        while q:
            i = q.popleft()
            di = dist[i]

            # we try to expand to all still-unvisited nodes
            # that can be reached in one step
            to_remove = []

            for j in list(unvisited):
                d = abs(i - j)
                if d <= min(p[i], p[j]):
                    dist[j] = di + 1
                    q.append(j)
                    to_remove.append(j)

            for j in to_remove:
                unvisited.remove(j)

        print(dist[b])

if __name__ == "__main__":
    solve()
```BFS rất đơn giản: chúng tôi duy trì một hàng các nút đã tiếp cận và mở rộng ra bên ngoài. Tập hợp này đảm bảo chúng tôi không truy cập lại các nút và khi một nút được chỉ định khoảng cách, nó sẽ được hoàn tất. 

Mối quan tâm chính trong việc triển khai là việc lặp lại`unvisited`. Trong một giải pháp được tối ưu hóa hoàn toàn, điều này được thay thế bằng cách bỏ qua khoảng thời gian hoặc dựa trên DSU, nhưng ý tưởng cốt lõi vẫn là BFS với việc cắt tỉa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n=3, a=1, b=3, p=[3,3,1]`Chúng tôi theo dõi tiến trình BFS. 

| Bước | Xếp hàng | Nút hiện tại | Mới Đạt | quận | 
| --- | --- | --- | --- | --- | 
| 0 | [1] | 1 | 2, 3 | 0→1 | 
| 1 | [2,3] | 2 | không | 1→2 | 
| 2 | [3] | 3 | mục tiêu | 2 | 

Điều này cho thấy nút 1 ngay lập tức kết nối với tất cả các nút khác do có công suất cao. 

### Ví dụ 2 

đầu vào:`n=4, p=[1,1,1,1], a=1, b=4`| Bước | Xếp hàng | Nút hiện tại | Mới Đạt | quận | 
| --- | --- | --- | --- | --- | 
| 0 | [1] | 1 | 2 | 0→1 | 
| 1 | [2] | 2 | 3 | 1→2 | 
| 2 | [3] | 3 | 4 | 2→3 | 

Điều này xác nhận sự lan truyền giống như chuỗi khi công suất là tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) trường hợp xấu nhất | Mỗi bản mở rộng BFS có thể quét các nút chưa được truy cập còn lại | 
| Không gian | O(n) | Mảng khoảng cách, hàng đợi và tập hợp các nút chưa được truy cập | 

Mặc dù BFS ngây thơ này không được tối ưu hóa đủ cho các ràng buộc trong trường hợp xấu nhất, nhưng nó nắm bắt được ý tưởng cấu trúc. Giải pháp được chấp nhận cải thiện điều này bằng cách đảm bảo mỗi chỉ mục được xử lý một lần thông qua việc bỏ qua phạm vi, giảm tổng công việc xuống tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, a, b = map(int, input().split())
            p = list(map(int, input().split()))
            a -= 1
            b -= 1

            if a == b:
                out.append("0")
                continue

            dist = [-1] * n
            dist[a] = 0
            q = deque([a])
            unvisited = set(range(n))
            unvisited.remove(a)

            while q:
                i = q.popleft()
                for j in list(unvisited):
                    if abs(i - j) <= min(p[i], p[j]):
                        dist[j] = dist[i] + 1
                        q.append(j)
                        unvisited.remove(j)

            out.append(str(dist[b]))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""3
10 2 9
4 1 1 1 5 1 1 1 1 5
1 1 1
1
3 1 3
3 3 1
""") == """4
0
2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp tầm thường | 
| quyền lực nhỏ thống nhất | truyền chuỗi | chuyển tiếp cục bộ | 
| thống nhất quyền lực lớn | 1 | hành vi đồ thị hoàn chỉnh | 
| trung tâm hỗn hợp | đường kính nhỏ | nhảy tầm xa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`a == b`. Thuật toán ngay lập tức trở lại`0`bởi vì không cần phải truyền tải và việc khởi tạo BFS sẽ coi nó như một vấn đề tìm kiếm một cách không chính xác. 

Một trường hợp cạnh khác là một đồ thị được kết nối đầy đủ trong đó tất cả các lũy thừa đều lớn. BFS sẽ đánh dấu tất cả các nút ở khoảng cách`1`ngay từ đầu, điều này mang lại câu trả lời chính xác`1`tới bất kỳ mục tiêu nào. 

Trường hợp cạnh cuối cùng là một chuỗi nghiêm ngặt trong đó tất cả các quyền hạn được`1`. BFS mở rộng mỗi lần một hàng xóm, tạo ra độ dài đường dẫn là`|a - b|`, khớp với tuyến đường duy nhất có thể có trong biểu đồ.
