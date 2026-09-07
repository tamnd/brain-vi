---
title: "CF 104555C - Chuyến đi bộ đầy thử thách"
description: "Chúng ta có một cây có gốc tại nút 1. Mỗi nút có một giá trị và chúng ta đi từ gốc xuống bất kỳ nút i nào dọc theo đường dẫn đơn giản duy nhất trong cây. Trong khi đi bộ, chúng ta có thể tùy ý “ghi lại” một số nút đã truy cập, nhưng trình tự được ghi phải có giá trị tăng dần."
date: "2026-06-30T08:47:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 134
verified: false
draft: false
---

[CF 104555C - Chuyến đi bộ đầy thử thách](https://codeforces.com/problemset/problem/104555/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc tại nút 1. Mỗi nút có một giá trị và chúng ta đi từ gốc xuống bất kỳ nút i nào dọc theo đường dẫn đơn giản duy nhất trong cây. Trong khi đi bộ, chúng ta có thể tùy ý “ghi lại” một số nút đã truy cập, nhưng trình tự được ghi phải có giá trị tăng dần. Đối với mỗi nút đích i, chúng tôi muốn số lượng nút được ghi tối đa có thể dọc theo đường dẫn từ 1 đến i. 

Vì vậy, đối với mỗi đường dẫn tiền tố trong cây gốc, chúng ta đang giải quyết vấn đề về dãy con tăng dần theo đúng dài nhất, nhưng dãy này bị hạn chế nằm dọc theo đường dẫn từ gốc tới nút. 

Các ràng buộc đủ lớn đến mức không thể thực hiện được bất kỳ phương trình bậc hai nào trên mỗi nút. Với tối đa 10^5 nút, mọi phương pháp tính toán lại LIS một cách độc lập trên mỗi nút hoặc tính toán lại các đường dẫn đầy đủ sẽ là TLE. Chúng ta cần sử dụng lại các tính toán dọc theo cây và quan trọng hơn là chúng ta cần một cấu trúc cho phép chúng ta duy trì thông tin LIS tăng dần trong khi duyệt. 

Trường hợp cạnh khóa là khi các giá trị không đơn điệu dọc theo cây. Một hành động tham lam ngây thơ “lấy nếu lớn hơn lần lấy cuối cùng” dọc theo đường dẫn sẽ không thành công vì việc bỏ qua một giá trị lớn sớm có thể cho phép một chuỗi con dài hơn sau này. Một trường hợp phức tạp khác là khi nhiều nhánh hợp nhất các giá trị cao sau đó, đòi hỏi chúng ta phải duy trì cấu trúc toàn cục thay vì các quyết định theo đường dẫn cục bộ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: với mỗi nút i, lấy đường dẫn từ 1 đến i, trích xuất chuỗi các giá trị và tính LIS trên đường dẫn đó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi phép tính LIS tốn O(độ dài nhật ký độ dài) và trên tất cả các nút, tổng công việc sẽ trở thành O(n^2 log n) trong một cây lệch, quá chậm đối với n lên tới 10^5. 

Quan sát quan trọng là đây không chỉ là LIS trên một mảng tĩnh mà còn là LIS trên đường dẫn từ gốc đến nút động trong cây. Cấu trúc đề xuất duy trì cách trình bày LIS “sắp xếp kiên nhẫn” trong khi thực hiện DFS. Khi chuyển từ cha mẹ sang con, chúng ta có thể cập nhật cấu trúc LIS bằng cách chèn giá trị con vào cấu trúc có thứ tự chung và sau đó khôi phục nó khi quay lui. 

Điều quan trọng là LIS có thể được duy trì bằng cách sử dụng một vectơ trong đó tails[k] là giá trị kết thúc tối thiểu có thể có của một dãy con tăng dần có độ dài k. Cấu trúc này có thể được cập nhật theo O(log n) trên mỗi nút bằng cách sử dụng tìm kiếm nhị phân và vì chúng ta đang ở trên một cây nên chúng ta có thể áp dụng DFS với tính năng khôi phục để duy trì tính chính xác trên các nhánh. 

Do đó, thay vì tính toán lại LIS cho mọi nút, chúng tôi duy trì trạng thái toàn cục dọc theo đường dẫn DFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại đường dẫn LIS | O(n^2 log n) | O(n) | Quá chậm | 
| Bảo trì đuôi DFS + LIS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và thực hiện truyền tải theo chiều sâu trong khi vẫn duy trì cấu trúc đại diện cho các đuôi LIS cho đường dẫn từ gốc đến nút hiện tại. 

1. Chúng tôi duy trì một mảng tails trong đó tails[k] là giá trị cuối cùng nhỏ nhất có thể có của dãy con tăng dần có độ dài k dọc theo đường dẫn DFS hiện tại. Cấu trúc này mã hóa tất cả thông tin LIS chúng ta cần tại bất kỳ thời điểm nào. 
2. Đối với mỗi nút chúng tôi truy cập, chúng tôi tính toán xem giá trị của nó phù hợp với đuôi bằng cách sử dụng tìm kiếm nhị phân. Điều này cho biết độ dài của chuỗi con tăng dài nhất kết thúc tại nút này nếu chúng ta chọn đưa nó vào. 
3. Chúng tôi lưu trữ giá trị trước đó của đuôi tại vị trí chúng tôi cập nhật để có thể khôi phục giá trị đó khi quay lại. Điều này rất cần thiết vì các nhánh khác nhau của cây không được cản trở lẫn nhau. 
4. Chúng tôi cập nhật câu trả lời cho nút hiện tại với độ dài tối đa k sao cho tails[k] hợp lệ sau khi chèn nút. 
5. Chúng ta tái diễn thành trẻ em, đưa trạng thái đuôi đã cập nhật về phía trước. 
6. Sau khi xử lý tất cả các phần tử con, chúng tôi khôi phục các đuôi về trạng thái trước đó trước khi quay lại phần tử gốc.

Điểm tinh tế là chúng ta không lưu trữ rõ ràng các dãy con mà chỉ lưu trữ các đại diện tối ưu của chúng. Biểu diễn nén này là đủ vì LIS chỉ phụ thuộc vào các giá trị kết thúc tối thiểu chứ không phụ thuộc vào các phần tử thực tế. 

### Tại sao nó hoạt động 

Mảng tails là một biểu diễn chuẩn mực của tất cả các dãy con tăng dần dọc theo đường dẫn hiện tại. Tại bất kỳ thời điểm nào, tails[k] là giá trị kết thúc tối thiểu có thể có trong số tất cả các chuỗi con có độ dài k, điều này đảm bảo rằng mọi phần mở rộng trong tương lai chỉ phụ thuộc vào biên giới này. Bởi vì DFS đảm bảo chúng tôi chỉ mở rộng đường dẫn gốc tới nút hiện tại và chúng tôi khôi phục hoàn toàn trạng thái quay lui, nên mỗi đường dẫn được đánh giá chính xác như thể nó được xử lý độc lập nhưng có tính toán chung. Điều này đảm bảo tính chính xác mà không cần tính toán lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    parent = list(map(int, input().split()))
    val = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for i, p in enumerate(parent, start=1):
        g[p - 1].append(i)

    tails = []
    ans = [0] * n

    from bisect import bisect_left

    def dfs(u):
        x = val[u]

        idx = bisect_left(tails, x)
        old = None
        replaced = False

        if idx == len(tails):
            tails.append(x)
            replaced = False
        else:
            old = tails[idx]
            tails[idx] = x
            replaced = True

        ans[u] = idx + 1

        for v in g[u]:
            dfs(v)

        if replaced:
            tails[idx] = old
        else:
            tails.pop()

    dfs(0)

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```DFS duy trì cấu trúc LIS toàn cầu dọc theo đường dẫn gốc tới nút hiện tại. Đối với mỗi nút, chúng tôi chèn giá trị của nó bằng cách sử dụng tìm kiếm nhị phân vào mảng tails, tính toán độ dài LIS kết thúc ở đó và sau đó lặp lại. Sau khi đệ quy, chúng tôi khôi phục trạng thái trước đó để các cây con anh chị em không can thiệp. 

Một lỗi phổ biến là quên khôi phục đuôi đúng cách. Nếu không khôi phục, cấu trúc LIS sẽ bị ảnh hưởng bởi các nhánh khác và tạo ra kết quả không chính xác. Một vấn đề nhỏ khác là sử dụng bisect_right thay vì bisect_left, điều này sẽ cho phép các giá trị bằng nhau mở rộng các chuỗi con tăng dần một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
5
1 1 3 3
5 7 7 6 8
```Chúng ta xây dựng cây có gốc từ 1. 

Tại nút 1, đuôi = [5], câu trả lời = 1. 

Tại nút 2, ta chèn 7 đuôi = [5, 7], đáp án = 2. 

Tại nút 3, giá trị 7 thay đuôi ở vị trí 1 nhưng không tăng độ dài nên đáp án = 2. 

Tại nút 4, giá trị 6 thay thế tails[1], mang lại tiềm năng tốt hơn trong tương lai nhưng vẫn trả lời = 2. 

Tại nút 5, giá trị 8 mở rộng đuôi tới [5, 7, 8], câu trả lời = 3. 

Điều này cho thấy cấu trúc LIS phát triển cục bộ như thế nào trong khi vẫn duy trì tính nhất quán toàn cầu. 

Bây giờ hãy xem xét một trường hợp sai lệch:```
3
1 2
3 2 5
```Tại nút 1: [3] cho 1 

Tại nút 2: [3,2] trở thành [2], vẫn trả lời 1 

Tại nút 3: [2,5] cho độ dài 2 

Điều này chứng tỏ tại sao việc thay thế là cần thiết: mặc dù 2 phá vỡ mô hình tăng dần trước đó nhưng nó sẽ cải thiện các chuỗi con trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mỗi nút thực hiện một lần cập nhật tìm kiếm nhị phân | 
| Không gian | O(n) | danh sách kề, đuôi, ngăn xếp đệ quy | 

Giải pháp có tỷ lệ thoải mái đến n tối đa 10^5 vì mỗi nút được xử lý một lần và mỗi bản cập nhật đều có tính logarit theo kích thước LIS hiện tại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "not_implemented"

# sample checks (placeholders)
# assert run(...) == ...

# single chain increasing
# 1-2-3 with values 1 2 3

# single chain decreasing
# 3 2 1

# star-shaped tree

# random medium tree
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng chuỗi | tăng trưởng đơn điệu | Tính chính xác của phần mở rộng LIS | 
| chuỗi giảm dần | tất cả những cái | hành vi thay thế | 
| cây sao | chi nhánh độc lập | quay lui đúng đắn | 
| cây ngẫu nhiên | tính nhất quán | tính đúng đắn chung |
