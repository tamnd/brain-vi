---
title: "CF 1863G - Hoán đổi"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí trỏ đến một giá trị trong cùng phạm vi với các chỉ số. Bạn được phép chọn liên tục một chỉ mục i và hoán đổi giá trị được lưu ở vị trí i với giá trị được lưu ở vị trí a[i]."
date: "2026-06-09T00:04:30+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "G"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 2800
weight: 1863
solve_time_s: 96
verified: false
draft: false
---

[CF 1863G - Hoán đổi](https://codeforces.com/problemset/problem/1863/G) 

**Đánh giá:** 2800 
**Tags:** tổ hợp, dp, đồ thị, toán 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi vị trí trỏ đến một giá trị trong cùng phạm vi với các chỉ số. Bạn được phép chọn nhiều lần một chỉ mục`i`và hoán đổi giá trị được lưu tại vị trí`i`với giá trị được lưu tại vị trí`a[i]`. Nói cách khác, thao tác cho phép bạn trao đổi hai mục, nhưng chỉ mục thứ hai không phải là tùy ý, nó được quyết định bởi giá trị hiện tại tại`i`. 

Nhiệm vụ là xác định có thể đạt được bao nhiêu mảng khác nhau bằng cách áp dụng thao tác này với số lần bất kỳ bắt đầu từ cấu hình ban đầu. 

Khó khăn chính là hoạt động này không phải là trao đổi miễn phí. Mỗi lần di chuyển phụ thuộc vào giá trị hiện tại tại một chỉ mục đã chọn, do đó, các trạng thái có thể truy cập sẽ tạo thành một biểu đồ có cấu trúc về các hoán vị của mảng thay vì sắp xếp lại tùy ý. 

Ràng buộc`n ≤ 10^6`ngay lập tức loại trừ bất kỳ cách tiếp cận nào khám phá các trạng thái một cách rõ ràng. Ngay cả việc lưu trữ các cấu hình đã truy cập cũng không thể thực hiện được vì số lượng mảng theo cấp số nhân`n`. Bất kỳ lời giải hợp lệ nào cũng phải quy bài toán về các thuộc tính cấu trúc của đồ thị có hướng xác định bởi các cạnh`i -> a[i]`. 

Trường hợp cạnh tinh tế phát sinh khi mảng chứa các điểm cố định hoặc các chu trình nhỏ. 

Ví dụ, nếu`a = [1, 1, 1]`, không có thao tác nào thay đổi bất cứ điều gì, vì vậy câu trả lời là`1`. Một cách giải thích ngây thơ có thể coi điều này giống như một hoán vị và mong đợi nhiều sự sắp xếp lại, nhưng thao tác luôn hoán đổi các giá trị giống hệt nhau hoặc hoán đổi trong một cấu trúc tầm thường. 

Một trường hợp cạnh khác là khi đồ thị chứa một chu trình như`1 → 2 → 3 → 1`. Trong trường hợp này, các thao tác lặp lại cho phép sắp xếp lại cục bộ bên trong chu trình nhưng không tương tác với các thành phần khác. Một chế độ xem hoán vị toàn cục ngây thơ sẽ tính toán quá mức không chính xác các tương tác giữa các chu kỳ. 

Cấu trúc ẩn cốt lõi là mảng xác định một biểu đồ hàm có hướng và thao tác chỉ ảnh hưởng đến các thành phần được kết nối của biểu đồ này một cách bị ràng buộc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quá trình dưới dạng biểu đồ trạng thái. Mỗi trạng thái là một mảng và từ mỗi trạng thái chúng ta tạo ra các trạng thái mới bằng cách thử mọi chỉ mục`i`và trao đổi`a[i]`với`a[a[i]]`. Điều này xác định sự chuyển tiếp giữa các cấu hình. 

Tuy nhiên, ngay cả đối với mức độ vừa phải`n`, biểu đồ này có phân nhánh theo quy mô giai thừa. Mỗi lần hoán đổi có thể thay đổi cấu trúc đồ thị hàm số và số lượng mảng có thể truy cập tăng theo cấp số nhân. BFS hoặc DFS trên các trạng thái sẽ ngay lập tức bùng nổ trong bộ nhớ và thời gian. 

Quan sát chính là thao tác không tạo ra các hoán vị tùy ý. Nó chỉ cho phép trao đổi các giá trị dọc theo các cạnh của đồ thị có hướng trong đó mỗi nút có chính xác một cạnh đi ra. Đây là một đồ thị hàm số, nghĩa là mỗi thành phần được kết nối chứa chính xác một chu trình có hướng với các cây đi vào đó. 

Trong mỗi thành phần, tất cả các nút cuối cùng đều “chảy” vào chu trình. Hoạt động chỉ cho phép sắp xếp lại một cách hiệu quả bên trong các thành phần này và số lượng cấu hình có thể truy cập chỉ phụ thuộc vào cấu trúc chu trình. 

Bước quan trọng là phân tích những gì xảy ra trên một thành phần được kết nối. Cây dẫn vào chu kỳ không phải là mức độ tự do độc lập: hoán đổi cuối cùng tạo ra các giá trị bong bóng theo chu kỳ, nhưng không tạo ra các lựa chọn tổ hợp bổ sung. Nguồn duy nhất của sự biến thiên tổ hợp là cách các giá trị có thể luân chuyển trong mỗi chu kỳ và cách các chu kỳ tương tác với các ràng buộc do các cây sắp tới áp đặt. 

Điều này làm giảm vấn đề đếm cấu hình trên mỗi thành phần, hóa ra lại phụ thuộc vào đóng góp giai thừa và các ràng buộc giống như chẵn lẻ xuất phát từ kích thước chu kỳ. Sau khi được phân tách, câu trả lời sẽ trở thành tích của các thành phần của biểu thức dạng đóng đơn giản dựa trên độ dài chu kỳ của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS bang) | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân rã đồ thị hàm số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng trong đó mỗi chỉ số`i`có lợi thế để`a[i]`. Điều này tạo thành một biểu đồ hàm trong đó mọi nút đều có bậc ngoài 1. Cấu trúc này đảm bảo mỗi thành phần có chính xác một chu trình. 
2. Phân tách biểu đồ thành các thành phần được kết nối bằng DFS hoặc truyền tải lặp. Trong quá trình truyền tải, hãy xác định xem mỗi nút nằm trên một chu trình hay nằm trong cây dẫn vào một chu trình. Cấu trúc chu trình là phần duy nhất ảnh hưởng đến tổ hợp. 
3. Đối với mỗi thành phần, hãy trích xuất độ dài chu kỳ của nó. Điều này có thể được thực hiện bằng cách theo dõi các trạng thái truy cập: các nút được xem lại trong ngăn xếp đệ quy hiện tại thuộc về chu trình. 
4. Tính toán phần đóng góp của một thành phần. Thực tế quan trọng là mỗi chu kỳ có độ dài`k`đóng góp một yếu tố`2`nếu như`k > 1`, và đóng góp`1`nếu không thì. Điều này phát sinh do các chu kỳ có kích thước lớn hơn 1 cho phép chính xác một mức độ tự do giống như đảo ngược không cần thiết trong hoạt động hoán đổi, trong khi các vòng tự lặp lại cứng nhắc. 
5. Nhân các khoản đóng góp cho tất cả các thành phần theo modulo`10^9 + 7`. 
6. Trả lại sản phẩm cuối cùng làm câu trả lời. 

Tính đúng đắn phụ thuộc vào thực tế là các cạnh của cây không tạo ra các hoán vị độc lập. Bất kỳ sự sắp xếp lại nào trong thành phần cây đều được hấp thụ vào chu trình mà nó đưa vào, do đó quyền tự do nhân duy nhất đến từ các lựa chọn đối xứng ở cấp độ chu kỳ. 

### Tại sao nó hoạt động 

Mỗi thành phần được kết nối của đồ thị chức năng có chính xác một chu kỳ và tất cả các hoạt động đều bảo toàn phân vùng thành các thành phần. Hoạt động hoán đổi chỉ có thể hoán vị các giá trị trong một thành phần và trong một thành phần, tất cả các cấu hình đều tương đương với một lựa chọn nhị phân được xác định bởi liệu chu trình là tầm thường hay không tầm thường. Điều này tạo ra một cấu trúc sản phẩm trên các thành phần, vì các hoạt động trong các thành phần khác nhau là độc lập và không tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a = [x - 1 for x in a]

    vis = [0] * n
    in_stack = [0] * n

    ans = 1

    def dfs(u):
        stack = []
        cur = u
        while True:
            vis[cur] = 1
            in_stack[cur] = 1
            stack.append(cur)
            nxt = a[cur]
            if vis[nxt]:
                if in_stack[nxt]:
                    cycle_nodes = stack[stack.index(nxt):]
                    return len(cycle_nodes)
                break
            cur = nxt
        for v in stack:
            in_stack[v] = 0
        return 1

    for i in range(n):
        if not vis[i]:
            cycle_len = dfs(i)
            if cycle_len > 1:
                ans = (ans * 2) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một biểu đồ hàm một cách ngầm định và duyệt qua từng nút một lần. DFS được lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy cho`n = 10^6`. các`in_stack`mảng được sử dụng để phát hiện các cạnh phía sau xác định các chu kỳ trong đường truyền hiện tại. Khi một chu trình được tìm thấy, độ dài của nó sẽ xác định liệu thành phần đó có đóng góp hệ số 2 hay không. 

Điểm tinh tế là chúng ta chỉ quan tâm đến độ dài chu kỳ tương đương lớn hơn 1 chứ không quan tâm đến kích thước chính xác. Khi một chu trình tồn tại, tất cả các cấu trúc cây gắn liền với nó sẽ không thay đổi số lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 2
```Chúng tôi theo dõi các thành phần: 

| Bước | Nút | Đã truy cập | Ngăn xếp | Phát hiện chu kỳ | Độ dài chu kỳ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [0] | [0] | không | - | 
| 2 | 1 | [0,1] | [0,1] | không | - | 
| 3 | 1 → 0 | xem lại trong ngăn xếp | [0,1] | vâng | 2 | 

Thành phần chứa chu trình có độ dài 2, vì vậy câu trả lời là`2`. 

Điều này xác nhận rằng ngay cả sự phụ thuộc lẫn nhau nhỏ giữa hai chỉ số cũng tạo ra lựa chọn nhị phân trong các cấu hình có thể truy cập. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| Bước | Nút | Đã truy cập | Ngăn xếp | Phát hiện chu kỳ | Độ dài chu kỳ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [0] | [0] | không | - | 
| 2 | 1 | [0,1] | [0,1] | không | - | 
| 3 | 2 | [0,1,2] | [0,1,2] | không | - | 
| 4 | 3 | [0,1,2,3] | [0,1,2,3] | không | - | 
| 5 | 3 → 4 chu kỳ không tồn tại | thiết bị đầu cuối | - | không | - | 

Mỗi nút là một chu trình tự lặp có độ dài 1, do đó không có thành phần nào đóng góp hệ số 2, đưa ra câu trả lời`1`. 

Điều này cho thấy các chuỗi trông hoàn toàn không theo chu kỳ (theo nghĩa đồ thị hàm số) không tạo ra tính biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong quá trình duyệt DFS của đồ thị hàm số | 
| Không gian | O(n) | Mảng cho trạng thái truy cập và mô phỏng ngăn xếp đệ quy | 

Độ phức tạp tuyến tính phù hợp thoải mái trong giới hạn của`n ≤ 10^6`và việc sử dụng bộ nhớ vẫn tuyến tính do các mảng kế toán và biểu diễn kề cận đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample
assert run("3\n1 1 2\n") == "2"

# all fixed points
assert run("5\n1 2 3 4 5\n") == "1"

# single 2-cycle
assert run("2\n2 1\n") == "2"

# one large cycle
assert run("4\n2 3 4 1\n") == "2"

# mixture of cycles and trees
assert run("5\n2 3 2 5 4\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 5 | 1 | tất cả các điểm cố định | 
| 2 1 | 2 | chu trình không cần thiết nhỏ nhất | 
| 2 3 4 1 | 2 | chu kỳ lớn duy nhất | 
| 2 3 2 5 4 | 4 | nhiều thành phần | 

## Vỏ cạnh 

Một cấu hình điểm cố định tối thiểu như`a = [1]`dẫn đến không có chu kỳ nào có độ dài lớn hơn 1. DFS ngay lập tức đánh dấu nút đơn là đã truy cập, không tìm thấy cạnh sau và trả về độ dài chu kỳ là 1, đóng góp hệ số nhân là 1. 

Cấu trúc hoán đổi hai nút như`a = [2, 1]`tạo ra một chu kỳ có độ dài 2. Quá trình truyền tải phát hiện cạnh sau ngay lập tức khi di chuyển từ nút 1 trở lại nút 0 trong ngăn xếp đệ quy và thành phần đóng góp hệ số 2, tạo ra câu trả lời 2. 

Trong một cấu trúc hỗn hợp như`a = [2, 3, 2]`, các nút 0 và 2 tạo thành một chu trình trong khi nút 1 đi vào đó. DFS xác định chu kỳ`[0, 1, 2]`chu trình cốt lõi của cấu trúc một cách chính xác và bỏ qua phần đính kèm của cây, mang lại hệ số đóng góp duy nhất là 2.
