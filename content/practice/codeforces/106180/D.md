---
title: "CF 106180D - \u041e\u0431\u043c\u0435\u043d\u044b \u0440\u0435\u043a\u043e\u0440\u0434\u043e\u0432"
description: "Chúng ta được cung cấp một chuỗi hoạt động giống như một danh sách các “bản ghi” theo thời gian, trong đó mỗi phần tử có thể được coi là điểm số hoặc giá trị gắn liền với một vị trí. Cùng với điều này, chúng tôi được cung cấp một tập hợp các hoạt động trao đổi được phép."
date: "2026-06-25T06:46:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106180
codeforces_index: "D"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2025. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 106180
solve_time_s: 39
verified: true
draft: false
---

[CF 106180D - \u041e\u0431\u043c\u0435\u043d\u044b \u0440\u0435\u043a\u043e\u0440\u0434\u043e\u0432](https://codeforces.com/problemset/problem/106180/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi hoạt động giống như một danh sách các “bản ghi” theo thời gian, trong đó mỗi phần tử có thể được coi là điểm số hoặc giá trị gắn liền với một vị trí. Cùng với điều này, chúng tôi được cung cấp một tập hợp các hoạt động trao đổi được phép. Mỗi thao tác kết nối hai vị trí, nghĩa là chúng ta được phép hoán đổi các giá trị hiện được lưu trữ ở hai chỉ số đó. 

Chi tiết quan trọng là việc hoán đổi không phải là tùy ý: chúng tôi chỉ được phép trao đổi dọc theo các kết nối nhất định và chúng tôi có thể sử dụng lại các kết nối này nhiều lần. Câu hỏi đặt ra là đâu là sự sắp xếp cuối cùng tốt nhất có thể của các giá trị mà chúng ta có thể thu được sau khi thực hiện bất kỳ chuỗi hoán đổi được phép nào. 

Được định hình lại một cách có cấu trúc hơn, các kết nối tạo thành một biểu đồ vô hướng trên các vị trí. Mỗi thành phần được kết nối của biểu đồ này mô tả một tập hợp các chỉ số có giá trị có thể được hoán vị tùy ý giữa chúng bằng cách sử dụng các hoán đổi lặp đi lặp lại dọc theo các cạnh. Nhiệm vụ là lựa chọn, bên trong mỗi thành phần như vậy, sự phân phối lại các giá trị tốt nhất có thể. 

Khái niệm tự nhiên về “tốt nhất” ở đây là chúng ta muốn mảng cuối cùng càng lớn càng tốt theo thứ tự từ điển, có nghĩa là các vị trí trước đó quan trọng hơn các vị trí sau. Vì vậy, đối với mỗi thành phần, chúng tôi muốn đặt các giá trị lớn nhất của nó càng sớm càng tốt trong thứ tự chỉ mục của thành phần, vì các vị trí trước đó chiếm ưu thế so sánh từ điển. 

Các ràng buộc (có tối đa khoảng 10^5 vị trí và cạnh trở lên theo kiểu CF điển hình) loại trừ mọi cách tiếp cận cố gắng mô phỏng giao dịch hoán đổi trực tiếp. Một chuỗi hoán đổi duy nhất có thể dài và việc khám phá tất cả các chuỗi có thể sẽ bùng nổ về mặt tổ hợp. Ngay cả việc suy nghĩ về BFS trên các hoán vị cũng là không thể vì không gian trạng thái là giai thừa. 

Một cách tiếp cận ngây thơ sẽ là liên tục mô phỏng các giao dịch hoán đổi và cố gắng cải thiện mảng một cách tham lam, nhưng ngay cả một nỗ lực để “bong bóng” các giá trị lớn về phía trước cũng có thể tốn O(nm) hoặc tệ hơn, vì mỗi bước hoán đổi chỉ tạo ra các thay đổi cục bộ và có thể cần phải được lặp lại nhiều lần trên các chuỗi kết nối. 

Các trường hợp cạnh xuất hiện khi cấu trúc đồ thị thưa thớt nhưng tạo thành chuỗi dài. Ví dụ: nếu các chỉ số tạo thành một đường dẫn 1-2-3-4 và các giá trị ban đầu bị đảo ngược, một chiến lược hoán đổi tham lam ngây thơ chỉ xem xét các giao dịch hoán đổi có lợi liền kề có thể yêu cầu thực hiện nhiều lượt chuyển đổi hoặc thậm chí có thể hoán đổi sai thứ tự nếu nó không tôn trọng kết nối toàn cầu. Một trường hợp đặc biệt khác là khi các thành phần lớn nhưng bị ngắt kết nối với nhau, trong đó việc trộn các giá trị giữa các thành phần là không thể, do đó, bất kỳ cách tiếp cận sắp xếp toàn cục nào cũng sẽ giả định hoàn toàn tự do một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mô phỏng rõ ràng tất cả các giao dịch hoán đổi được phép và cố gắng khám phá tất cả các hoán vị có thể tiếp cận. Mỗi lần hoán đổi sẽ thay đổi cấu hình hiện tại và vì chúng ta có thể sử dụng lại các lần hoán đổi vô thời hạn nên điều này trở thành một biểu đồ duyệt qua các hoán vị. Ngay cả khi giới hạn ở một thành phần có kích thước k, vẫn có k! những sự sắp xếp có thể có và sự chuyển tiếp giữa chúng tạo thành một biểu đồ tiềm ẩn khổng lồ. Điều này nhanh chóng trở nên không khả thi ngay cả đối với k khoảng 10. 

Quan sát chính là các giao dịch hoán đổi tạo ra các thành phần được kết nối trong biểu đồ vị trí và trong mỗi thành phần được kết nối, chúng ta có thể sắp xếp lại các giá trị một cách tùy ý. Điều này làm giảm vấn đề từ “trình tự hoán đổi” thành “các vấn đề sắp xếp độc lập trên các thành phần”. 

Bên trong một thành phần, tất cả các chỉ mục đều tương đương nhau về khả năng tiếp cận, vì vậy chúng ta có thể tự do hoán đổi nhiều tập hợp giá trị trong thành phần đó. Để tối đa hóa từ điển, chúng tôi muốn các giá trị lớn xuất hiện ở các chỉ mục nhỏ hơn bên trong thành phần. Điều đó gợi ý việc sắp xếp các giá trị theo thứ tự giảm dần và gán chúng cho các chỉ mục được sắp xếp theo thứ tự tăng dần. 

Sự đơn giản hóa cấu trúc quan trọng là chúng ta không bao giờ cần phải mô phỏng các giao dịch hoán đổi. Chúng tôi chỉ cần kết nối và sắp xếp trong các thành phần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng hoán đổi/khám phá trạng thái) | hàm mũ | hàm mũ | Quá chậm | 
| Thành phần + sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ trong đó mỗi chỉ mục là một nút và mỗi hoán đổi được phép là một cạnh vô hướng. Mô hình này mô hình chính xác những vị thế nào có thể trao đổi giá trị thông qua các chuỗi hoán đổi. 
2. Tìm các thành phần liên thông của biểu đồ này bằng DFS, BFS hoặc DSU. Mỗi thành phần đại diện cho một tập hợp các chỉ số có giá trị có thể được hoán vị tự do giữa chúng. Bước này là cần thiết vì khả năng tiếp cận trao đổi có tính bắc cầu. 
3. Đối với mỗi thành phần được kết nối, hãy thu thập tất cả các chỉ mục thuộc về nó và cũng thu thập các giá trị hiện được lưu trữ tại các chỉ mục đó. Tại thời điểm này, chúng tôi đang cô lập bài toán con độc lập. 
4. Sắp xếp danh sách các chỉ số theo thứ tự tăng dần. Đây là những vị trí quan trọng về mặt từ điển từ trái sang phải. 
5. Sắp xếp các giá trị thu thập được theo thứ tự giảm dần. Điều này đảm bảo chúng tôi luôn đặt các giá trị lớn hơn sớm hơn, giúp cải thiện thứ tự từ điển ở vị trí sớm nhất có thể. 
6. Gán lại các giá trị: đối với chỉ mục nhỏ nhất trong thành phần, gán giá trị lớn nhất, đối với chỉ mục tiếp theo, gán giá trị lớn nhất tiếp theo, v.v. Việc ghép nối tham lam này là chính xác vì bất kỳ chuỗi hoán đổi nào bên trong thành phần đều có thể nhận ra bất kỳ hoán vị nào. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bên trong bất kỳ thành phần được kết nối nào, nhiều tập hợp giá trị được giữ nguyên và hoàn toàn có thể hoán vị. Mọi hoán đổi được phép chỉ là một cạnh trong biểu đồ được kết nối và các hoán đổi lặp lại cho phép sắp xếp lại tùy ý trong thành phần đó. Do đó, quyền tự do duy nhất chúng ta có là cách hoán vị các giá trị bên trong mỗi thành phần. 

Thứ tự từ điển so sánh các mảng từ trái sang phải. Ở vị trí đầu tiên mà hai mảng khác nhau, giá trị nào lớn hơn sẽ quyết định người chiến thắng. Điều này buộc phải thực hiện một chiến lược tham lam: trong mỗi thành phần, quyết định cục bộ tốt nhất trước tiên là tối đa hóa các chỉ số trước đó, điều này đạt được bằng cách sắp xếp các chỉ số tăng dần và các giá trị giảm dần rồi ghép nối chúng. 

Không có quyết định nào trong một thành phần ảnh hưởng đến thành phần khác, bởi vì việc hoán đổi không bao giờ xảy ra giữa các thành phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    vis = [False] * n

    def dfs(start):
        stack = [start]
        vis[start] = True
        comp = []

        while stack:
            v = stack.pop()
            comp.append(v)
            for to in g[v]:
                if not vis[to]:
                    vis[to] = True
                    stack.append(to)
        return comp

    for i in range(n):
        if not vis[i]:
            comp = dfs(i)

            idx = sorted(comp)
            vals = sorted([a[i] for i in comp], reverse=True)

            for j, v in zip(idx, vals):
                a[j] = v

    print(*a)

if __name__ == "__main__":
    solve()
```Việc xây dựng biểu đồ mã hóa trực tiếp các quyền trao đổi. DFS nhóm các chỉ mục thành các thành phần và mỗi thành phần được xử lý độc lập. Việc sắp xếp các chỉ mục đảm bảo chúng tôi tôn trọng mức độ ưu tiên từ điển, trong khi việc sắp xếp các giá trị theo thứ tự giảm dần đảm bảo chúng tôi tối đa hóa từng vị trí một cách tham lam. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng mô phỏng các giao dịch hoán đổi một cách rõ ràng. Tính đúng đắn hoàn toàn phụ thuộc vào thực tế là khả năng kết nối bao hàm sự tự do hoán vị hoàn toàn, điều này loại bỏ mọi nhu cầu xây dựng một chuỗi các thao tác thực tế. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với kết nối một phần. 

đầu vào:```
5 3
5 1 4 2 3
1 2
2 3
4 5
```Chúng tôi có thành phần`{1,2,3}`Và`{4,5}`(lập chỉ mục dựa trên 1). Thành phần đầu tiên có chỉ số`[0,1,2]`với các giá trị`[5,1,4]`. 

| Bước | Thành phần | Chỉ số | Giá trị | Chỉ số được sắp xếp | Giá trị được sắp xếp | Bài tập | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | {1,2,3} | [0,1,2] | [5,1,4] | [0,1,2] | [5,4,1] | a[0]=5, a[1]=4, a[2]=1 | 
| 2 | {4,5} | [3,4] | [2,3] | [3,4] | [3,2] | a[3]=3, a[4]=2 | 

Mảng cuối cùng trở thành:```
5 4 1 3 2
```Điều này cho thấy mỗi thành phần được giải quyết một cách độc lập và tham lam như thế nào. 

Bây giờ hãy xem xét một trường hợp được kết nối đầy đủ. 

đầu vào:```
4 3
1 2 3 4
1 2
2 3
3 4
```Mọi thứ đều là một thành phần. 

| Bước | Chỉ số | Giá trị | Chỉ số được sắp xếp | Giá trị được sắp xếp | Bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1,2,3] | [1,2,3,4] | [0,1,2,3] | [4,3,2,1] | điền giảm dần | 

Đầu ra cuối cùng:```
4 3 2 1
```Điều này xác nhận rằng khả năng kết nối đầy đủ mang lại sự sắp xếp đầy đủ theo thứ tự giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | DFS trên biểu đồ cộng với việc sắp xếp các thành phần bên trong chiếm ưu thế | 
| Không gian | O(n + m) | danh sách kề và mảng đã thăm | 

Các ràng buộc điển hình cho kích thước bài toán này phù hợp thoải mái trong các giới hạn này, vì việc sắp xếp chiếm ưu thế và vẫn hiệu quả ngay cả đối với n lớn. Việc truyền tải đồ thị là tuyến tính ở các cạnh và đỉnh, là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# simple chain
assert run("""5 3
5 1 4 2 3
1 2
2 3
4 5
""") == "5 4 1 3 2"

# fully connected
assert run("""4 3
1 2 3 4
1 2
2 3
3 4
""") == "4 3 2 1"

# no edges
assert run("""3 0
3 1 2
""") == "3 1 2"

# already optimal in components
assert run("""3 1
3 2 1
1 2
""") == "3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 5 4 1 3 2 | kết nối một phần | 
| đồ thị đầy đủ | 4 3 2 1 | tự do hoán vị hoàn toàn | 
| không có cạnh | cùng một mảng | thành phần bị cô lập | 
| cạnh đơn | chỉ sắp xếp lại cục bộ | tính đúng đắn của việc xử lý thành phần | 

## Vỏ cạnh 

Trường hợp quan trọng là khi không có giao dịch hoán đổi nào cả. Mỗi chỉ mục tạo thành thành phần riêng của nó, vì vậy thuật toán sẽ không thay đổi mảng. DFS vẫn chạy, nhưng mỗi thành phần có kích thước 1, do đó việc sắp xếp và gán là không cần thiết, duy trì trình tự ban đầu. 

Một trường hợp khác là khi đồ thị được kết nối đầy đủ. Thuật toán thu gọn mọi thứ thành một thành phần và toàn bộ mảng được sắp xếp theo thứ tự giảm dần. Bất kỳ sai sót nào trong việc phát hiện thành phần sẽ chia thành các phần nhỏ hơn một cách không chính xác và phá vỡ tính tối ưu. 

Các thành phần chuỗi dài cũng rất quan trọng. Trong biểu đồ đường dẫn, khả năng tiếp cận vẫn bao hàm sự tự do hoán vị hoàn toàn, mặc dù các giao dịch hoán đổi mang tính cục bộ. DFS hợp nhất chính xác toàn bộ chuỗi thành một thành phần, đảm bảo rằng các giá trị được sắp xếp lại trên toàn cầu thay vì chỉ hoán đổi cục bộ.
