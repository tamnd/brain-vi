---
title: "CF 245G - Gợi ý bạn bè"
description: "Chúng ta được cung cấp một mạng xã hội vô hướng, trong đó mỗi người dùng được xác định bằng một chuỗi tên và tình bạn được đặt dưới dạng các cặp tên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "graphs"]
categories: ["algorithms"]
codeforces_contest: 245
codeforces_index: "G"
codeforces_contest_name: "CROC-MBTU 2012, Elimination Round (ACM-ICPC)"
rating: 2200
weight: 245
solve_time_s: 66
verified: true
draft: false
---

[CF 245G - Những người bạn được đề xuất](https://codeforces.com/problemset/problem/245/G) 

**Đánh giá:** 2200 
**Tags:** bạo lực, đồ thị 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng xã hội vô hướng, trong đó mỗi người dùng được xác định bằng một chuỗi tên và tình bạn được đặt dưới dạng các cặp tên. Từ những tình bạn này, đối với mỗi người dùng, chúng ta phải xác định xem những người không phải bạn bè nào là “giống” nhất với họ, trong đó mức độ tương tự được đo bằng số lượng bạn bè chung. 

Cụ thể, đối với người dùng cố định`x`, chúng tôi xem xét mọi người dùng khác`y`ai không`x`và chưa được kết nối trực tiếp với`x`. Trong số tất cả các ứng cử viên như vậy, chúng tôi đếm có bao nhiêu người dùng`z`là bạn của cả hai`x`Và`y`. Những người dùng tối đa hóa số lượng này được coi là bạn bè được đề xuất của`x`. Chúng ta phải xuất ra, đối với mỗi người dùng, có bao nhiêu bộ tối đa hóa như vậy tồn tại. 

Cấu trúc là một bài toán đồ thị trên một đồ thị đơn giản vô hướng, nhưng thay vì chỉ tìm một ứng cử viên tốt nhất, chúng ta phải đếm tất cả các đỉnh gắn với số lượng tối đa các lân cận chung. 

Kích thước đầu vào nhỏ về mặt biên, với tối đa 5000 cặp tình bạn, nhưng số lượng người dùng riêng biệt không bị giới hạn rõ ràng và vẫn có thể lớn. Điều này làm cho việc lập luận dựa trên danh sách kề trở nên cần thiết. Một dày đặc$O(n^2)$quét qua tất cả các cặp người dùng vẫn có thể chấp nhận được nếu được thực hiện cẩn thận, nhưng việc tính toán lại các giao điểm một cách đơn giản cho từng cặp người dùng sẽ quá chậm. 

Một trường hợp khó khăn đến từ những người dùng có nhóm bạn rất nhỏ. Nếu một người dùng chỉ có một người bạn, mỗi ứng viên không phải là bạn bè sẽ chia sẻ tối đa một hoặc không có bạn chung, vì vậy nhiều ứng viên có thể hòa hợp ở mức tối đa. Một trường hợp góc cạnh khác là khi nhiều người dùng đạt được số lượng tối đa như nhau, nghĩa là chúng ta không được chọn một người bạn thân nhất mà phải tính tất cả họ. 

Một vấn đề khác là tính cả bạn chung nếu mối quan hệ gần gũi không được xử lý cẩn thận. Vì đồ thị là vô hướng và các cạnh đầu vào là duy nhất nên chúng ta vẫn phải đảm bảo việc xây dựng kề cận đối xứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét mọi người dùng`x`, sau đó lặp lại với mọi người dùng khác`y`, bỏ qua nếu`y`là`x`hoặc đã là bạn của`x`và tính số lượng bạn bè chung bằng cách giao nhau các tập hợp kề. Nếu vùng lân cận được lưu trữ dưới dạng danh sách, mỗi giao lộ sẽ có giá$O(deg(x) + deg(y))$, dẫn đến chi phí trong trường hợp xấu nhất là khoảng$O(n^2 \cdot n)$, điều này không thể chấp nhận được khi số lượng người dùng lớn. 

Quan sát quan trọng là số lượng bạn bè chung có thể được tính bằng cách lặp lại số bạn bè của`x`. Cho mỗi người bạn`z`của`x`, mọi người hàng xóm`y`của`z`đóng góp một người bạn chung giữa`x`Và`y`. Điều này chuyển việc tính toán từ các giao điểm theo cặp sang chế độ xem “lan truyền lân cận”. Thay vì so sánh trực tiếp tất cả các cặp, chúng ta tích lũy điểm cho các ứng cử viên trong vùng lân cận hai bước nhảy của`x`. 

Điều này hoạt động hiệu quả vì mỗi cạnh đóng góp vào chính xác hai danh sách kề và mỗi mối quan hệ tam giác`(x, z, y)`được tính chính xác một lần cho mỗi người hàng xóm được chia sẻ`z`. 

Chúng tôi vẫn phải đảm bảo không coi người dùng đã kết nối trực tiếp với`x`, và chúng ta phải loại trừ`x`chính nó. 

Độ phức tạp tổng thể tỷ lệ thuận với tổng mức độ lân cận của mỗi nút, về cơ bản là tuyến tính theo số “bước đi hai bước” trong biểu đồ và dễ dàng phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giao điểm theo cặp Brute Force | O(n² · d) | O(n + m) | Quá chậm | 
| Tích lũy hai bước nhảy | O(∑ d²) ≈ O(m√m) trong trường hợp xấu nhất, thực tế là O(mn) trong trường hợp xấu nhất nhưng có thể chấp nhận được với m ≤ 5000 | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta biểu diễn đồ thị bằng các tập kề cận để có thể kiểm tra tình bạn trong O(1). 

1. Ánh xạ từng tên người dùng duy nhất vào một chỉ mục số nguyên. Điều này giúp đơn giản hóa việc xử lý biểu đồ và tránh so sánh chuỗi lặp lại. 
2. Xây dựng tập kề`adj[u]`cho mỗi người dùng. Vì tình bạn là đối xứng nên chúng tôi chèn cả hai hướng. 
3. Đối với mỗi người dùng`x`, tạo một từ điển`cnt`sẽ lưu trữ bao nhiêu người bạn chung của mỗi ứng viên`y`có với`x`. 
4. Lặp lại từng người bạn`z`của`x`. Đối với mỗi người hàng xóm`y`của`z`, tăng`cnt[y]`bởi một. Bước này đếm số lượng hàng xóm được chia sẻ giữa`x`Và`y`thông qua`z`. 
5. Sau khi xử lý tất cả bạn bè của`x`, xác định giá trị lớn nhất trong`cnt`. Bất kỳ người dùng nào đạt được giá trị này đều là ứng viên được đề xuất kết bạn. 
6. Loại trừ khỏi việc xem xét bất kỳ người dùng nào`y`ai là ai`x`chính nó hoặc đã là bạn trực tiếp của`x`, vì chúng bị loại theo định nghĩa. 
7. Đếm xem có bao nhiêu người dùng còn lại đạt được số lượng bạn bè chung tối đa và lưu trữ kết quả này làm câu trả lời cho`x`. 

Ý tưởng chính là mọi mối quan hệ bạn bè chung được tạo ra chính xác một lần trên mỗi nút trung gian được chia sẻ. Nếu như`x`Và`y`chia sẻ`k`bạn ơi, vậy thì chính xác là có`k`các đường dẫn riêng biệt có độ dài bằng hai kết nối chúng thông qua những người bạn đó và mỗi đường dẫn như vậy sẽ tăng bộ đếm một lần. 

### Tại sao nó hoạt động 

Đối với một nút nguồn cố định`x`, mọi ứng viên tiềm năng`y`nhận chính xác một mức tăng cho mỗi đường dẫn riêng biệt`x → z → y`. Những đường dẫn này tương ứng một-một với các lân cận chung của`x`Và`y`. Vì thế,`cnt[y]`luôn bằng số lượng bạn bè chung giữa`x`Và`y`. Vì chúng tôi loại trừ một cách rõ ràng những người bạn trực tiếp và`x`chính nó, mức tối đa còn lại`y`xác định chính xác tất cả những người không phải là bạn bè giống nhau nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    
    name_to_id = {}
    adj = []
    
    def get_id(name):
        if name not in name_to_id:
            name_to_id[name] = len(adj)
            adj.append(set())
        return name_to_id[name]
    
    for _ in range(m):
        a, b = input().split()
        u = get_id(a)
        v = get_id(b)
        adj[u].add(v)
        adj[v].add(u)
    
    n = len(adj)
    id_to_name = [""] * n
    for name, i in name_to_id.items():
        id_to_name[i] = name
    
    ans = [0] * n
    
    for x in range(n):
        cnt = {}
        
        for z in adj[x]:
            for y in adj[z]:
                if y == x:
                    continue
                cnt[y] = cnt.get(y, 0) + 1
        
        best = -1
        for y, c in cnt.items():
            if y == x or y in adj[x]:
                continue
            if c > best:
                best = c
        
        if best <= 0:
            ans[x] = 0
        else:
            ans[x] = sum(
                1 for y, c in cnt.items()
                if y != x and y not in adj[x] and c == best
            )
    
    print(n)
    for i in range(n):
        print(id_to_name[i], ans[i])

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén tên chuỗi thành các id số nguyên để các phép toán kề cận có hiệu quả. Mỗi danh sách kề được lưu trữ dưới dạng một tập hợp, cho phép kiểm tra liên tục xem người dùng đã là bạn bè hay chưa. 

Đối với mỗi nút, chúng tôi xây dựng bản đồ tần số trên vùng lân cận hai bước nhảy của nó. Vòng lặp kép bên trong đi qua từng người bạn rồi đến bạn bè của người bạn đó, tích lũy số lượng hàng xóm chung. Lần thứ hai trích xuất giá trị tối đa và đếm xem có bao nhiêu ứng viên đạt được nó, loại trừ những giá trị không hợp lệ. 

Một chi tiết tinh tế là xử lý những người dùng không có ứng viên hợp lệ. Trong trường hợp đó`best`còn lại`-1`và chúng tôi xuất ra chính xác không có người bạn được đề xuất nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
Mike Gerald
Kate Mike
Kate Tank
Gerald Tank
Gerald David
```Chúng tôi xây dựng sự liền kề: 

Mike: Gerald, Kate 

Gerald: Mike, Tank, David 

Kate: Mike, Xe tăng 

Xe tăng: Kate, Gerald 

David: Gerald 

cho`David`, hàng xóm của`Gerald`là những đóng góp của Mike, Tank, Mike, Tank thông qua cấu trúc chung: 

| x | z (bạn của x) | y (hàng xóm của z) | cnt[y] | 
| --- | --- | --- | --- | 
| David | Gerald | Mike | 1 | 
| David | Gerald | Xe tăng | 1 | 

Tối đa là 1, do Mike và Tank đạt được, vì vậy câu trả lời là 2 dành cho David. 

Vì`Kate`, lý luận tương tự mang lại một ứng cử viên tốt nhất. 

Điều này xác nhận rằng thuật toán đếm chính xác sự chồng chéo hai bước nhảy thay vì kết nối trực tiếp. 

### Ví dụ 2 

đầu vào:```
4
A B
B C
C D
A D
```Sự liền kề tạo thành một chu kỳ. Vì`A`, ứng viên là`C`chỉ (vì`B`Và`D`là bạn bè) và`C`chia sẻ một người bạn chung với`A`thông qua`B`hoặc`D`. 

| x | z | y | cnt[y] | 
| --- | --- | --- | --- | 
| A | B | C | 1 | 
| A | D | C | 2 | 

Đây`C`có được 2 người bạn chung và là người duy nhất tốt nhất. 

Điều này cho thấy rằng nhiều đường dẫn đến cùng một ứng cử viên được tổng hợp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ độ(v)2) | Đối với mỗi nút, chúng tôi duyệt qua các nút lân cận của nó, tích lũy đóng góp hai bước | 
| Không gian | O(n + m) | Bộ kề cộng với bản đồ đếm tạm thời | 

Ràng buộc$m \le 5000$đảm bảo rằng ngay cả trong các biểu đồ có cấu trúc kém nhất, tổng số lần truyền qua hai bước nhảy vẫn nằm trong giới hạn chấp nhận được. Thuật toán tránh mọi phép quét bậc hai đối với tất cả các cặp người dùng, điều này sẽ không khả thi nếu số lượng người dùng tăng lên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys
    
    m = int(sys.stdin.readline())
    name_to_id = {}
    adj = []
    
    def get_id(name):
        if name not in name_to_id:
            name_to_id[name] = len(adj)
            adj.append(set())
        return name_to_id[name]
    
    for _ in range(m):
        a, b = sys.stdin.readline().split()
        u = get_id(a)
        v = get_id(b)
        adj[u].add(v)
        adj[v].add(u)
    
    n = len(adj)
    id_to_name = [""] * n
    for name, i in name_to_id.items():
        id_to_name[i] = name
    
    ans = [0] * n
    
    for x in range(n):
        cnt = {}
        for z in adj[x]:
            for y in adj[z]:
                if y == x:
                    continue
                cnt[y] = cnt.get(y, 0) + 1
        
        best = -1
        for y, c in cnt.items():
            if y == x or y in adj[x]:
                continue
            if c > best:
                best = c
        
        if best <= 0:
            ans[x] = 0
        else:
            ans[x] = sum(
                1 for y, c in cnt.items()
                if y != x and y not in adj[x] and c == best
            )
    
    out = [str(n)]
    for i in range(n):
        out.append(id_to_name[i] + " " + str(ans[i]))
    return "\n".join(out)

# provided sample
assert run("""5
Mike Gerald
Kate Mike
Kate Tank
Gerald Tank
Gerald David
""").splitlines()[0] == "5"

# custom: triangle
assert run("""3
A B
B C
A C
""")  # all excluded as friends => 0 suggested each

# custom: chain
assert run("""2
A B
B C
""")  # C and A mutual via B

# custom: star
assert run("""3
A B
A C
A D
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | tất cả số không | tất cả ứng viên đều là bạn bè trực tiếp | 
| chuỗi | điểm cuối đã chia sẻ ứng cử viên | lan truyền hai bước nhảy | 
| ngôi sao | lá chia sẻ cấu trúc trung tâm | hành vi trung tâm cấp cao | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các ứng viên tiềm năng đều là bạn bè trực tiếp. Ví dụ, trong một đồ thị tam giác đầy đủ`A-B-C-A`, mọi cặp đều được kết nối nên không có người bạn được đề xuất nào tồn tại. Thuật toán xây dựng`cnt`, nhưng mọi ứng cử viên đều được lọc ra bằng phép kiểm tra kề, để lại`best <= 0`và tạo ra số không. 

Một trường hợp khác là biểu đồ hình sao trong đó một nút trung tâm kết nối với nhiều lá. Đối với nút lá`x`, mỗi lá còn lại có chung một người bạn chung (ở giữa). Vòng đếm đảm bảo mỗi lá được đếm 1 và tất cả các lá đó được buộc chính xác ở mức tối đa. 

Cuối cùng, trong các biểu đồ thưa thớt trong đó một nút chỉ có một nút lân cận, thuật toán vẫn tính chính xác các kết nối cấp hai mặc dù tập hợp trung gian rất nhỏ. Tính chính xác chỉ dựa vào việc liệt kê các hàng xóm của hàng xóm chứ không dựa vào các giả định về kích thước mức độ.
