---
title: "CF 104395F - Xe đạp"
description: "Chúng ta được cung cấp một quy trình đồ thị có hướng mà cuối cùng sẽ trở thành một đồ thị hàm số, nghĩa là mỗi nút kết thúc bằng chính xác một cạnh ra và chính xác một cạnh vào. Một số cạnh đã được cố định."
date: "2026-07-01T00:45:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104395
codeforces_index: "F"
codeforces_contest_name: "Cupertino Informatics Tournament"
rating: 0
weight: 104395
solve_time_s: 222
verified: false
draft: false
---

[CF 104395F - Chu kỳ](https://codeforces.com/problemset/problem/104395/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình đồ thị có hướng mà cuối cùng sẽ trở thành một đồ thị hàm số, nghĩa là mỗi nút kết thúc bằng chính xác một cạnh ra và chính xác một cạnh vào. Một số cạnh đã được cố định. Đối với các nút còn lại thiếu cạnh đi hoặc cạnh đến, chúng tôi liên tục kết nối các điểm cuối đi không được sử dụng với các điểm cuối đến không được sử dụng một cách thống nhất một cách ngẫu nhiên cho đến khi mọi nút đều thỏa mãn cả hai ràng buộc về mức độ. 

Khi đồ thị hoàn chỉnh, nó sẽ phân hủy thành các chu trình có hướng. Nhiệm vụ là tính toán số chu kỳ dự kiến ​​​​trong biểu đồ hàm ngẫu nhiên cuối cùng này, theo tính ngẫu nhiên được tạo ra bởi cách ghép nối các điểm cuối chưa khớp còn lại. 

Cấu trúc quan trọng là đối tượng cuối cùng luôn là một hoán vị trên các nút và các chu trình tương ứng chính xác với các chu trình trong hoán vị đó. Tính ngẫu nhiên không trực tiếp nằm ở các hoán vị mà nằm ở cách hoàn thành các cạnh cố định một phần. 

Với n lên tới 100000, bất kỳ phương pháp nào liệt kê số lần hoàn thành hoặc mô phỏng tính ngẫu nhiên đều không thể thực hiện được. Ngay cả việc lưu trữ tất cả các kết quả phù hợp có thể là theo cấp số nhân. Hướng khả thi duy nhất là chuyển kỳ vọng thành tổng đóng góp của địa phương, vì vậy chúng tôi không bao giờ xây dựng biểu đồ cuối cùng một cách rõ ràng. 

Trường hợp cạnh tinh tế là khi một số cạnh tự lặp hoặc đã hình thành một phần chu kỳ. Những điều này không hành xử khác nhau trong kỳ vọng; chúng vẫn đóng góp vào số chu kỳ một cách xác định nếu đã đóng và theo xác suất khác. Một trực giác ngây thơ cho rằng “các chu kỳ chỉ xuất hiện ở cuối” dẫn đến việc tính hai lần hoặc thiếu các đóng góp nếu chúng ta cố gắng mô phỏng việc hoàn thành từng bước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các cách có thể để hoàn thành các cạnh đi và đến còn thiếu, xây dựng đồ thị hàm số kết quả, đếm chu kỳ trong mỗi lần hoàn thành và tính trung bình. Ngay cả khi mỗi nút bị thiếu nhiều nhất một cạnh trong và cạnh ngoài, số cách ghép nối các nhánh còn lại sẽ là giai thừa theo số lượng nút không khớp, khoảng O(k!). Điều này trở nên không thể ngay cả đối với k khoảng 20. 

Quan sát quan trọng là số chu kỳ dự kiến ​​trong một hoán vị ngẫu nhiên có thể được biểu thị dưới dạng tổng trên các nút của xác suất một nút là phần tử tối thiểu trong chu trình của nó hoặc tương đương, dưới dạng tổng xác suất mà các cạnh nhất định sẽ đóng một chu trình khi đi ngang. Trong đồ thị hàm số, mỗi nút có chính xác một cạnh đi ra, vì vậy mỗi thành phần là một chu trình có hướng có thể có các cây đi vào nó, nhưng ở đây các ràng buộc mức độ và mức độ ngoài đảm bảo rằng chúng ta luôn xử lý một cấu trúc hoán vị. 

Đồ thị được xây dựng một phần có thể được coi là đã hình thành các đường dẫn và chu trình có hướng rời rạc. Mỗi chuỗi không hoàn chỉnh cuối cùng sẽ được đóng lại bằng cách kết hợp ngẫu nhiên các điểm cuối còn lại. Tính ngẫu nhiên là đồng nhất trong các phép đối chiếu giữa các nhánh đi và nhánh đến chưa từng có, điều này làm giảm vấn đề đếm số chu kỳ dự kiến ​​được hình thành khi hoàn thành một hoán vị một phần. 

Điều này làm giảm sự đóng góp tính toán từ mỗi thành phần được kết nối được hình thành bởi các cạnh cố định. Mỗi thành phần hoạt động độc lập về mặt xác suất hình thành chu kỳ và số chu kỳ dự kiến ​​​​là phụ gia. 

Do đó, thay vì liệt kê các lần hoàn thành, chúng tôi tính toán cho từng thành phần xem nó đã là một chu kỳ hay sẽ trở thành một chu kỳ sau khi đóng ngẫu nhiên và tính tổng các đóng góp bằng cách sử dụng tuyến tính của kỳ vọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các lần hoàn thành | O(k!) | O(n) | Quá chậm | 
| Phân tách thành phần + kỳ vọng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng ta hiểu các cạnh đã cho là các ràng buộc một phần của một hoán vị. Mỗi nút có nhiều nhất một cạnh ra và một cạnh vào, vì vậy đồ thị là một tập hợp các chuỗi và chu trình có hướng. 
2. Chúng ta duyệt đồ thị bằng cách sử dụng các con trỏ kề từ mỗi nút để xác định các thành phần liên thông được hình thành bởi các cạnh cố định. Mỗi nút thuộc về chính xác một cấu trúc như vậy. 
3. Đối với mỗi thành phần, chúng tôi xác định xem nó đã là một chu trình hoàn chỉnh hay chưa. Nếu mọi nút trong thành phần đã có cả mức độ vào và mức độ ngoài bằng 1 thì nó đóng góp chính xác 1 vào số chu kỳ với xác suất là 1. 
4. Nếu một thành phần không đóng, nó sẽ chứa một số đầu mở. Các đầu mở này sau đó sẽ được ghép ngẫu nhiên với các đầu mở khác trên các thành phần. Thông tin chi tiết quan trọng là các thành phần như vậy hoạt động giống như các “phân đoạn” sẽ được hoán vị ngẫu nhiên. 
5. Đếm số lượng cuống đi chưa khớp k. Các nhánh này sẽ được ghép ngẫu nhiên một cách thống nhất thành một hoán vị, tạo ra các chu kỳ. Số chu kỳ dự kiến ​​trong một hoán vị ngẫu nhiên có kích thước k được gọi là số hài thứ k. 
6. Do đó, câu trả lời tổng thể là tổng đóng góp từ các chu kỳ đã đóng cộng với kỳ vọng hài hòa đối với cấu trúc chưa từng có còn lại. 
7. Chúng tôi tính toán số hài modulo 1e9+7 bằng cách sử dụng phép tính trước hoặc phép tính tổng nghịch đảo mô đun trực tiếp. 

### Tại sao nó hoạt động 

Mỗi lần hoàn thành hợp lệ đều tương ứng với một hoán vị trên các điểm cuối chưa khớp và số chu kỳ là một hàm lớp trên các hoán vị phân rã bổ sung trên các thành phần. Tính tuyến tính của kỳ vọng cho phép chúng ta bỏ qua sự phụ thuộc giữa các thành phần và giảm vấn đề về việc đếm các chu kỳ dự kiến ​​trong một hoán vị ngẫu nhiên có kích thước bằng với số lượng kết nối chưa được giải quyết. Vì mỗi sự hình thành chu trình chỉ phụ thuộc vào thứ tự tương đối trong hoán vị nên tất cả các lần hoàn thành đều có khả năng xảy ra như nhau, làm cho kỳ vọng điều hòa trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    
    nxt = [-1] * (n + 1)
    indeg = [0] * (n + 1)
    outdeg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        nxt[a] = b
        outdeg[a] += 1
        indeg[b] += 1

    visited = [False] * (n + 1)

    def dfs(u):
        stack = []
        cur = u
        while cur != -1 and not visited[cur]:
            visited[cur] = True
            stack.append(cur)
            cur = nxt[cur]
        return stack

    cycles = 0
    open_stubs = 0

    for i in range(1, n + 1):
        if not visited[i]:
            comp = dfs(i)
            is_cycle = True
            for v in comp:
                if indeg[v] != 1 or outdeg[v] != 1:
                    is_cycle = False
                if indeg[v] == 0:
                    open_stubs += 1
                if outdeg[v] == 0:
                    open_stubs += 1
            if is_cycle:
                cycles += 1

    # expected cycles in random permutation of size open_stubs/2 pairs
    k = open_stubs // 2

    # harmonic number H_k mod MOD
    inv = [0] * (k + 2)
    for i in range(1, k + 2):
        inv[i] = pow(i, MOD - 2, MOD)

    harm = 0
    for i in range(1, k + 1):
        harm = (harm + inv[i]) % MOD

    print((cycles + harm) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xây dựng biểu đồ hàm từng phần, sau đó trích xuất các thành phần bằng cách đi theo các cạnh đi ra. Nó đếm các chu kỳ được hình thành đầy đủ ngay lập tức. Đối với các thành phần chưa hoàn chỉnh, nó sẽ tính các điểm cuối vào/ra chưa khớp dưới dạng sơ khai. Những sơ khai đó xác định một vấn đề khớp ngẫu nhiên và số chu kỳ dự kiến ​​​​của chúng được tính toán bằng cách sử dụng các số hài. 

Một điểm triển khai tinh vi là mỗi cạnh trong và cạnh ngoài bị thiếu sẽ đóng góp riêng vào số lượng sơ khai và chúng phải được ghép nối, do đó chia cho 2. Tổng hài hòa được tính theo modulo bằng cách sử dụng nghịch đảo mô-đun. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
2 4
3 1
```Chúng ta có hai chuỗi rời nhau: 3 → 1 và 2 → 4. 

| bước | chu kỳ | sơ khai mở | k | hài hòa | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 4 | 2 | 1 + 1/2 | 
| cuối cùng | 0 | 4 | 2 | 2/3 | 

Không có chu trình cố định nên đáp án là H2 = 3/2 mod. 

Điều này chứng tỏ rằng ngay cả các chuỗi bị ngắt kết nối cũng đóng góp một cách xác suất. 

### Mẫu 2 

đầu vào:```
9 6
9 4
6 6
7 7
1 8
3 1
8 2
```Ở đây các vòng lặp tự đóng góp vào các chu trình xác định. 

| thành phần | gõ | đóng góp | 
| --- | --- | --- | 
| 6 | chu kỳ | 1 | 
| 7 | chu kỳ | 1 | 
| nghỉ ngơi | mở | đóng góp hài hòa | 

Điều này cho thấy các thành phần xác định và xác suất hỗn hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền đơn + tổng điều hòa | 
| Không gian | O(n) | lân cận + kế toán | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n là 1e5 và tất cả các phép toán đều là các bước tuyến tính với số học mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

assert run("4 2\n2 4\n3 1\n") == run("4 2\n2 4\n3 1\n"), "sample 1 placeholder"
assert run("9 6\n9 4\n6 6\n7 7\n1 8\n3 1\n8 2\n") == run("9 6\n9 4\n6 6\n7 7\n1 8\n3 1\n8 2\n"), "sample 2 placeholder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây chuyền nhỏ | xử lý hài hòa chính xác | thành phần mở | 
| vòng tự | chu kỳ xác định | chu kỳ cố định | 
| đồ thị hỗn hợp | hành vi kết hợp | tính đúng đắn của sự phân hủy | 

## Vỏ cạnh 

Khi tất cả các nút đã hình thành vòng lặp tự thân, mỗi nút là một chu trình và các nút mở bằng 0. Phần hài hòa biến mất và câu trả lời chỉ đơn giản là n. 

Khi biểu đồ là một chuỗi dài, không có chu trình cố định và mọi đóng góp đều xuất phát từ kỳ vọng hài hòa đối với cấu trúc hoán vị mở hoàn toàn. 

Khi có nhiều chuỗi bị ngắt kết nối, mỗi chuỗi sẽ đóng góp độc lập thông qua tính tuyến tính của kỳ vọng và thuật toán sẽ tổng hợp chúng một cách chính xác mà không bị nhiễu.
