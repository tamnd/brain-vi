---
title: "CF 103328C - Xương Rồng Hoàn Hảo"
description: "Chúng ta có một đồ thị vô hướng đơn giản được đảm bảo là một cây xương rồng, nghĩa là mọi cạnh đều thuộc nhiều nhất một chu trình đơn. Nhiệm vụ là quyết định xem biểu đồ này có phải là biểu đồ hoàn hảo hay không."
date: "2026-07-03T14:06:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103328
codeforces_index: "C"
codeforces_contest_name: "National Taiwan University NCPC Preliminary 2021"
rating: 0
weight: 103328
solve_time_s: 51
verified: true
draft: false
---

[CF 103328C - Cây xương rồng hoàn hảo](https://codeforces.com/problemset/problem/103328/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản được đảm bảo là một cây xương rồng, nghĩa là mọi cạnh đều thuộc nhiều nhất một chu trình đơn. Nhiệm vụ là quyết định xem biểu đồ này có phải là biểu đồ hoàn hảo hay không. 

Sự hoàn hảo ở đây không phải là thứ chúng ta có thể tính toán trực tiếp từ định nghĩa, vì việc kiểm tra tất cả các đồ thị con cảm ứng và so sánh số cụm với số màu là không khả thi. Thay vào đó, chúng tôi dựa vào Định lý đồ thị hoàn hảo mạnh, trong đó mô tả các đồ thị hoàn hảo chính xác là những đồ thị không có chu kỳ lẻ cảm ứng có độ dài ít nhất là 5 và không có phần bù của chúng. 

Vì vậy, vấn đề trở thành một câu hỏi về cấu trúc trên cây xương rồng: liệu nó có chứa bất kỳ chu kỳ lẻ cảm ứng nào có chiều dài ít nhất là 5, hoặc bất kỳ phản lỗ lẻ cảm ứng nào không. 

Ràng buộc xương rồng là sự đơn giản hóa quan trọng. Vì các cạnh thuộc về nhiều nhất một chu trình nên các chu trình được tách biệt rõ ràng và không chồng chéo theo những cách phức tạp. Điều này có nghĩa là bất kỳ hành vi không phải cây nào đều được định vị thành các chu trình đơn giản và chúng ta có thể suy luận về từng chu trình một cách độc lập chỉ với sự tương tác nhẹ thông qua các phần đính kèm của cây. 

Kích thước đầu vào lên tới 100000 đỉnh và cạnh, vì vậy mọi giải pháp đều phải gần với tuyến tính. Bất cứ thứ gì bậc hai hoặc thậm chí n log n với các hằng số nặng đều được, nhưng bất cứ thứ gì cố gắng liệt kê các sơ đồ con, chu trình một cách ngây thơ hoặc mô phỏng màu sắc thì quá chậm. 

Một điểm tinh tế là không phải mọi chu trình trong cây xương rồng đều tự động là chu trình cảm ứng trong biểu đồ. Nếu một chu trình có dây cung thì đó không phải là một chu trình đơn giản ở cây xương rồng, vì vậy chúng ta an toàn ở đó. Tuy nhiên, nhược điểm chính là các phần đính kèm vào một chu trình thông qua các cạnh của cây không ảnh hưởng đến cấu trúc đồ thị con cảm ứng trên chính chu trình đó, do đó mỗi chu trình có thể được coi là cảm ứng. 

Các trường hợp cạnh phát sinh khi đồ thị là một cây, khi có chính xác một chu trình hoặc khi nhiều chu trình chia sẻ các điểm khớp nối. Ví dụ, một cái cây luôn hoàn hảo vì nó có hai phần. Một chu kỳ 5 thì không hoàn hảo, nhưng một chu kỳ 4 thì hoàn hảo. Điều kiện bổ sung tinh tế hơn, nhưng đối với xương rồng, hóa ra mọi vi phạm đều xuất phát trực tiếp từ sự hiện diện của một chu kỳ lẻ có độ dài ít nhất là 5. 

Một cách tiếp cận ngây thơ cố gắng chạy một thuật toán nhận dạng đồ thị hoàn hảo nói chung sẽ thất bại vì những thuật toán đó phức tạp hơn nhiều và không cần thiết dưới sự ràng buộc của cây xương rồng. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu bắt đầu từ định nghĩa: đối với mọi sơ đồ con cảm ứng, chúng ta cần so sánh số cụm và số màu. Ngay cả khi chúng ta hạn chế sử dụng Định lý đồ thị hoàn hảo mạnh, chúng ta vẫn cần phát hiện các chu kỳ lẻ cảm ứng và các phản lỗ lẻ cảm ứng. 

Đối với một đồ thị tổng quát, việc phát hiện các chu trình cảm ứng ở mọi độ dài và phần bù của chúng là rất tốn kém. Việc liệt kê tất cả các chu trình đơn giản đã mất thời gian theo cấp số nhân trong trường hợp xấu nhất. Ngay cả ở cây xương rồng, việc liệt kê các chu kỳ là có thể quản lý được, nhưng việc kiểm tra trực tiếp các phản lỗ thì không. 

Quan sát chính là cấu trúc xương rồng đã làm sụp đổ vấn đề. Mọi chu trình đều bị cô lập ngoại trừ tại các điểm khớp nối, do đó, bất kỳ chu trình cảm ứng nào có trong đồ thị đều chính xác là một trong những chu trình đơn giản trong cây xương rồng. Hơn nữa, các phản lỗ lẻ cảm ứng không thể xuất hiện ở cây xương rồng trừ khi chúng tương ứng với những trường hợp có cấu trúc rất nhỏ, và những phản lỗ lẻ này giảm xuống cùng điều kiện chu trình sau khi đơn giản hóa. 

Điều này có nghĩa là vấn đề giảm xuống còn việc quét tất cả các chu kỳ đơn giản trong cây xương rồng và kiểm tra xem có bất kỳ chu kỳ nào có độ dài ít nhất 5 là số lẻ hay không. 

Khi chúng tôi chấp nhận mức giảm này, nhiệm vụ trở nên đơn giản: tìm tất cả độ dài chu kỳ trong cây xương rồng bằng DFS, tính toán chẵn lẻ của chúng và kiểm tra xem có tồn tại bất kỳ chu kỳ bị cấm nào không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra sơ đồ con do lực lượng vũ phu gây ra | Hàm mũ | O(n) | Quá chậm | 
| Trích xuất chu kỳ ở cây xương rồng + kiểm tra chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi nhổ cây xương rồng ở bất cứ đâu và thực hiện DFS để phát hiện các cạnh sau hình thành chu kỳ. Vì đồ thị là một cây xương rồng nên mỗi cạnh thuộc nhiều nhất một chu trình nên mỗi cạnh sau xác định duy nhất một chu trình. 

1. Chạy DFS trong khi vẫn duy trì con trỏ gốc và độ sâu cho mỗi đỉnh. Điều này cho phép chúng tôi xây dựng lại bất kỳ chu trình nào gặp phải thông qua cạnh sau. 
2. Khi chúng ta gặp một cạnh sau từ nút u đến nút tổ tiên v, chúng ta xây dựng lại chu trình bằng cách đi từ u trở lại qua các con trỏ cha cho đến v. Độ dài chu trình là số đỉnh được truy cập trong quá trình quay lui này cộng với một cho cạnh đóng. Điều này hoạt động rõ ràng vì biểu đồ không có chu kỳ chồng chéo. 
3. Với mỗi chu kỳ được phát hiện, hãy tính độ dài của nó. Nếu độ dài ít nhất là 5 và lẻ, chúng ta kết luận ngay rằng đồ thị không hoàn hảo. 
4. Nếu không tồn tại chu trình như vậy sau khi khám phá tất cả các cạnh, chúng ta kết luận đồ thị là hoàn hảo. 

Bước tái thiết là an toàn vì trong cây xương rồng, cây DFS đảm bảo rằng mọi cạnh không phải là cây đều tương ứng với chính xác một chu trình đơn giản và không có sự mơ hồ trong cấu trúc chu trình. 

### Tại sao nó hoạt động 

Ở cây xương rồng, mỗi chu kỳ đều bị cô lập theo nghĩa là nó có chung các cạnh không có chu kỳ nào khác. Do đó, bất kỳ chu trình nào được phát hiện thông qua các cạnh sau của DFS đều tương ứng chính xác với một chu trình đơn giản duy nhất trong biểu đồ. Vì các đồ thị con cảm ứng trên các chu kỳ được giữ nguyên (không tồn tại hợp âm bổ sung), nên cách duy nhất để vi phạm tính hoàn hảo là chứa một chu kỳ lẻ có độ dài ít nhất là 5, được phát hiện trực tiếp bằng cách quét độ dài chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
g = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * n
depth = [0] * n
vis = [0] * n

bad = False

def dfs(u, p):
    global bad
    vis[u] = 1
    for v in g[u]:
        if v == p:
            continue
        if not vis[v]:
            parent[v] = u
            depth[v] = depth[u] + 1
            dfs(v, u)
        else:
            if depth[v] < depth[u]:
                length = 1
                cur = u
                while cur != v:
                    length += 1
                    cur = parent[cur]
                if length >= 5 and length % 2 == 1:
                    bad = True

for i in range(n):
    if not vis[i]:
        dfs(i, -1)

print("No" if bad else "Yes")
```DFS duy trì một cây con trỏ gốc để khi gặp cạnh sau, chúng ta có thể xây dựng lại chu trình bằng cách đi lên trên. Việc kiểm tra độ sâu đảm bảo chúng ta chỉ đếm các cạnh ngược về phía tổ tiên và tránh tính hai lần các cạnh chéo trong quá trình truyền tải vô hướng. 

Quá trình tái tạo chu kỳ là tuyến tính theo độ dài chu kỳ và vì mỗi cạnh trong cây xương rồng thuộc về nhiều nhất một chu kỳ nên toàn bộ công việc tái thiết qua tất cả các chu kỳ là tuyến tính tổng thể. 

Một chi tiết triển khai tinh tế là bỏ qua cạnh gốc trực tiếp, nếu không mọi cạnh không được định hướng sẽ bị phân loại sai thành cạnh sau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2
2 3
3 4
4 5
5 1
```Điều này tạo thành một 5 chu kỳ duy nhất. 

| Bước | Sự kiện | Đã tìm thấy chu kỳ | Chiều dài | Cờ Xấu | 
| --- | --- | --- | --- | --- | 
| Truyền tải DFS | Khám phá chu kỳ | Có | 5 | Đúng | 

Khi cạnh sau đóng chu trình, chúng ta xây dựng lại tất cả 5 đỉnh. Độ dài là 5, là số lẻ và ít nhất là 5, do đó đồ thị ngay lập tức được phân loại là không hoàn hảo. 

Đầu ra là:```
No
```Điều này chứng tỏ rằng các chu kỳ lẻ có độ dài 5 bị cấm và bị phát hiện trực tiếp. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```Đây là một cái cây. 

| Bước | Sự kiện | Đã tìm thấy chu kỳ | Chiều dài | Cờ Xấu | 
| --- | --- | --- | --- | --- | 
| Truyền tải DFS | Không có cạnh sau | Không | - | Sai | 

Không có chu kỳ nào được phát hiện nên không có vi phạm nào xảy ra. 

Đầu ra là:```
Yes
```Điều này xác nhận rằng cây cối là hoàn hảo trong bối cảnh này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một lần trong DFS và việc tái cấu trúc chu trình trên tất cả các chu kỳ là tuyến tính tổng cộng do cấu trúc xương rồng | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng sổ sách kế toán DFS | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn 100000 đỉnh và cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    depth = [0] * n
    vis = [0] * n
    bad = False

    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        nonlocal bad
        vis[u] = 1
        for v in g[u]:
            if v == p:
                continue
            if not vis[v]:
                parent[v] = u
                depth[v] = depth[u] + 1
                dfs(v, u)
            else:
                if depth[v] < depth[u]:
                    length = 1
                    cur = u
                    while cur != v:
                        length += 1
                        cur = parent[cur]
                    if length >= 5 and length % 2 == 1:
                        bad = True

    for i in range(n):
        if not vis[i]:
            dfs(i, -1)

    return "No" if bad else "Yes"

# sample 1
assert run("""5 5
1 2
2 3
3 4
4 5
5 1
""") == "No"

# sample 2
assert run("""5 4
1 2
2 3
3 4
4 5
""") == "Yes"

# triangle (should be perfect)
assert run("""3 3
1 2
2 3
3 1
""") == "Yes"

# square (perfect)
assert run("""4 4
1 2
2 3
3 4
4 1
""") == "Yes"

# single edge
assert run("""2 1
1 2
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 chu kỳ | Không | phát hiện chu kỳ lẻ bị cấm | 
| con đường | Có | vỏ cây | 
| tam giác | Có | cho phép chu kỳ lẻ nhỏ | 
| 4 chu kỳ | Có | chu kỳ chẵn vẫn ổn | 
| cạnh đơn | Có | đồ thị tối thiểu | 

## Vỏ cạnh 

Đầu vào dạng cây không chứa chu trình, do đó DFS không bao giờ kích hoạt quá trình tái thiết cạnh sau. Thuật toán rời đi`bad`là sai và kết quả đầu ra chính xác là Có. 

Một 4 chu kỳ thuần túy sẽ kích hoạt chính xác một quá trình tái tạo chu kỳ có độ dài 4. Vì điều kiện yêu cầu độ dài ít nhất là 5 nên nó không đặt cờ xấu, phù hợp với thực tế là 4 chu kỳ là lưỡng cực và do đó hoàn hảo. 

Chu kỳ 5 là cấu trúc bị cấm tối thiểu duy nhất trong cài đặt này. Thời điểm cạnh sau đóng chu trình, việc tái thiết sẽ đi đúng năm nút và ngay lập tức đánh dấu biểu đồ là không hoàn hảo.
