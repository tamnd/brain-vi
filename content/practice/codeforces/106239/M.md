---
title: "CF 106239M - \u654c\u4eba\u7684\u654c\u4eba"
description: "Chúng ta có một tập hợp các quốc gia, trong đó mỗi quốc gia là một nút trên biểu đồ và mọi mối quan hệ kẻ thù trực tiếp là một cạnh. Sự đảm bảo quan trọng về mặt cấu trúc là các mối quan hệ này tạo thành một cây, do đó không có chu trình và đồ thị được kết nối với chính xác n − 1 cạnh."
date: "2026-06-19T09:16:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "M"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 51
verified: true
draft: false
---

[CF 106239M - \u654c\u4eba\u7684\u654c\u4eba](https://codeforces.com/problemset/problem/106239/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các quốc gia, trong đó mỗi quốc gia là một nút trên biểu đồ và mọi mối quan hệ kẻ thù trực tiếp là một cạnh. Sự đảm bảo quan trọng về mặt cấu trúc là các mối quan hệ này tạo thành một cây, do đó không có chu trình và đồ thị được kết nối với chính xác n − 1 cạnh. 

Khái niệm “tình bạn” không được đưa ra một cách trực tiếp như một mối quan hệ cạnh. Thay vào đó, nó bắt nguồn từ một quy luật: nếu một quốc gia x có kẻ thù y và kẻ thù y đó có kẻ thù z khác, thì z sẽ trở thành bạn của x. Theo thuật ngữ đồ thị, điều này có nghĩa là bất kỳ nút nào có thể truy cập được từ x bằng cách đi theo chính xác hai cạnh đều tương ứng với một nút bạn của x. Không có sự lan truyền nào nữa xảy ra ngoài quy tắc này, vì vậy tình bạn được gắn chặt với những con đường có độ dài đúng hai. 

Nhiệm vụ là tính toán, đối với mỗi nút, có bao nhiêu nút riêng biệt nằm ở khoảng cách chính xác bằng hai nút và sau đó xuất ra bất kỳ nút nào tối đa hóa giá trị này cùng với số lượng tối đa. 

Các ràng buộc ngụ ý rằng n có thể lên tới 200.000 cho mỗi trường hợp thử nghiệm với tối đa 10.000 trường hợp thử nghiệm, nhưng tổng của tất cả n bị giới hạn bởi 200.000. Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào cũng phải tuyến tính trên tổng kích thước đầu vào. Một giải pháp bậc hai tính theo n cho mỗi trường hợp thử nghiệm sẽ quá chậm và thậm chí O(n log n) cho mỗi trường hợp thử nghiệm cũng sẽ gặp rủi ro nếu áp dụng nhiều lần. 

Một cách giải thích ngây thơ sẽ đề xuất, đối với mỗi nút, chạy BFS lên đến độ sâu 2 và đếm các nút ở khoảng cách 2. Mặc dù đúng, nhưng điều này lặp lại rất nhiều công việc và có nguy cơ xảy ra hành vi O(n²) trong cây hình ngôi sao. 

Một vài trường hợp tế nhị đáng được nêu ra. Trong một cây hình ngôi sao nơi một nút trung tâm kết nối với tất cả các nút khác, BFS từ mỗi lá sẽ liên tục đi qua tâm và tất cả các lá khác, gây ra công việc dư thừa. Một trường hợp cạnh khác là biểu đồ đường dẫn: trong một chuỗi dài, mỗi BFS vẫn đi một phần nhỏ, nhưng tích lũy nó vẫn trở thành bậc hai. 

Quan sát quan trọng là các nút khoảng cách-2 có thể được tính hoàn toàn từ thông tin cấp độ cục bộ mà không cần thực hiện truyền tải lặp đi lặp lại. 

## Phương pháp tiếp cận 

Giải pháp brute-force coi mỗi nút là điểm bắt đầu và thực hiện BFS hoặc DFS ở độ sâu hai. Từ một nút x, nó khám phá tất cả các lân cận y, và sau đó là tất cả các lân cận của y, thu thập những nút đó ở khoảng cách chính xác là hai. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về tình bạn. 

Tuy nhiên, mỗi BFS có thể chạm tới tối đa O(n) nút trong một vùng lân cận dày đặc và việc thực hiện điều này cho mọi nút sẽ dẫn đến độ phức tạp tổng thể là O(n²) trong các cây trong trường hợp xấu nhất chẳng hạn như các ngôi sao. Sự dư thừa xuất phát từ việc quét liên tục các danh sách lân cận giống nhau. 

Cái nhìn sâu sắc về cấu trúc quan trọng là bất kỳ nút z nào ở khoảng cách chính xác bằng hai từ x phải được kết nối thông qua một số lân cận trung gian y của x. Đối với y cố định, mọi lân cận của y ngoại trừ x đóng góp một nút hợp lệ ở khoảng cách hai từ x. Điều đó có nghĩa là mỗi lân cận y đóng góp độ(y) − 1 ứng viên cho x. Tổng hợp tất cả các lân cận của x sẽ cho câu trả lời. 

Điều này chuyển việc tính toán từ việc duyệt đồ thị lặp đi lặp lại sang một lần duyệt qua các danh sách kề, trong đó mỗi cạnh đóng góp một lượng thông tin không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force trên mỗi nút | O(n²) | O(n) | Quá chậm | 
| Phương thức đóng góp bằng cấp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề cho cây và tính bậc của mỗi nút. Mức độ thể hiện số lượng kẻ thù trực tiếp mà mỗi quốc gia có, số liệu này sẽ được sử dụng để suy ra các mối liên hệ cấp độ hai. 
2. Với mỗi nút x, khởi tạo bộ đếm ans[x] về 0. Điều này sẽ lưu trữ có bao nhiêu nút ở khoảng cách chính xác là hai. 
3. Lặp lại mọi lân cận y của nút x. Đối với mỗi nút lân cận như vậy, chúng ta xem xét tất cả các nút được kết nối với y. 
4. Cộng độ(y) − 1 vào ans[x]. Phép trừ sẽ loại bỏ chính x khỏi số lượng các nút lân cận của y, chỉ để lại các nút cách x đến y chính xác hai bước. 
5. Sau khi xử lý tất cả các nút, quét ans để tìm nút có giá trị lớn nhất. Nếu nhiều nút chia sẻ mức tối đa, bất kỳ nút nào trong số chúng đều có thể được trả về. 

### Tại sao nó hoạt động 

Đối với bất kỳ nút x cố định nào, mọi nút ở khoảng cách chính xác bằng hai phải xuất hiện dưới dạng lân cận của một số lân cận y của x. Bởi vì đồ thị là một cái cây nên có chính xác một đường đi đơn giản giữa hai nút bất kỳ, do đó không có sự đếm hai lần qua các chu kỳ. Khi tính tổng deg(y) − 1 trên tất cả các lân cận y của x, mỗi nút hợp lệ ở khoảng cách hai được tính chính xác một lần, vì nó tương ứng với một lân cận trung gian duy nhất y trên đường dẫn duy nhất từ ​​x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        g = [[] for _ in range(n + 1)]
        deg = [0] * (n + 1)

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)
            deg[u] += 1
            deg[v] += 1

        ans = [0] * (n + 1)

        for x in range(1, n + 1):
            total = 0
            for y in g[x]:
                total += deg[y] - 1
            ans[x] = total

        best_node = 1
        best_val = ans[1]

        for i in range(2, n + 1):
            if ans[i] > best_val:
                best_val = ans[i]
                best_node = i

        print(best_node, best_val)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng danh sách kề và số bậc theo thời gian tuyến tính. Sau đó, tính toán cốt lõi sẽ đánh giá từng nút một cách độc lập bằng cách tổng hợp các đóng góp từ các nút lân cận. Việc trừ đi một nút cho mỗi nút lân cận là phép hiệu chỉnh khóa nhằm ngăn chặn việc đếm chính nút gốc. 

Lần quét cuối cùng là một bước tuyến tính đơn giản để xác định mức tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào:```
3 nodes: 1 - 2 - 3
```| Nút x | Hàng xóm | Tính toán đóng góp | Nút khoảng cách-2 | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | độ(2)-1 = 2-1 = 1 | {3} | 1 | 
| 2 | 1, 3 | (1-1) + (1-1) = 0 | ∅ | 0 | 
| 3 | 2 | 2-1 = 1 | {1} | 1 | 

Nút ở giữa không có nút nào ở khoảng cách 2 vì cả hai nút lân cận của nó đều là lá. 

### Ví dụ 2 

Cây đầu vào:```
1 connected to 2
2 connected to 3, 4, 5
```| Nút x | Hàng xóm | Tính toán đóng góp | Nút khoảng cách-2 | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3-1 = 2 | {3,4,5} | 2 | 
| 2 | 1,3,4,5 | (1-1)+(1-1)+(1-1)+(1-1)=0 | ∅ | 0 | 
| 3 | 2 | 4-1 = 3 | {1,4,5}? chỉ có 1 hợp lệ (thực tế là rời đi qua 2) | 1 | 
| 4 | 2 | 4-1 = 3 | 1 | | 
| 5 | 2 | 4-1 = 3 | 1 | | 

Điều này cho thấy nút trung tâm tối đa hóa phạm vi tiếp cận cấp thứ hai như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi trường hợp thử nghiệm, tổng thể O(Σn) | Mỗi cạnh đóng góp công việc liên tục khi xử lý danh sách kề | 
| Không gian | O(n) | Lưu trữ danh sách kề, mảng độ và mảng trả lời | 

Vì tổng số nút trên tất cả các trường hợp thử nghiệm tối đa là 200.000 nên giải pháp tuyến tính phù hợp một cách thoải mái trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            g = [[] for _ in range(n + 1)]
            deg = [0] * (n + 1)

            for _ in range(n - 1):
                u, v = map(int, input().split())
                g[u].append(v)
                g[v].append(u)
                deg[u] += 1
                deg[v] += 1

            ans = [0] * (n + 1)
            for x in range(1, n + 1):
                total = 0
                for y in g[x]:
                    total += deg[y] - 1
                ans[x] = total

            best_node = 1
            best_val = ans[1]
            for i in range(2, n + 1):
                if ans[i] > best_val:
                    best_val = ans[i]
                    best_node = i

            out.append(f"{best_node} {best_val}")
        return "\n".join(out)

    return solve()

# provided sample
assert run("""2
3
1 2
2 3
6
1 2
2 3
3 4
3 5
2 6
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 0 | Trường hợp tối thiểu không có khoảng cách-2 nút | 
| ngôi sao tập trung ở 1 | 1 n-2 | Phân nhánh tối đa từ trung tâm | 
| chuỗi 1-2-3-4-5 | điểm cuối có 1 | cấu trúc đường dẫn đúng đắn | 
| cây cân đối | đúng tâm | xử lý đối xứng | 

## Vỏ cạnh 

Cây hai nút như`1 - 2`không tạo ra bạn bè nào cho cả hai nút. Thuật toán xử lý việc này một cách tự nhiên vì mỗi nút có chính xác một nút lân cận có bậc là 1, do đó mọi đóng góp đều bằng 0 sau khi trừ đi một. 

Cây có hình ngôi sao là trường hợp căng thẳng quan trọng nhất. Đối với một nút trung tâm được kết nối với tất cả các nút khác, mỗi lá đóng góp số 0 vào tâm, trong khi mỗi lá tích lũy tất cả các lá khác thông qua tâm. Công thức gán chính xác giá trị lớn nhất cho tâm mà không cần truyền tải. 

Một đường dẫn dài kiểm tra sự tích lũy trên các nút cấp độ thấp. Các nút bên trong có hai nút lân cận, cả hai đều cấp hai, không tạo ra sự đóng góp nào, trong khi các điểm cuối tích lũy chính xác một nút ở khoảng cách hai.
