---
title: "CF 300B - Xe khách"
description: "Chúng ta được yêu cầu chia n học sinh thành các đội có đúng ba người. Một số sinh viên yêu cầu rõ ràng được ở cùng đội với một số sinh viên khác. Những yêu cầu này là của nhau, vì vậy nếu học sinh 2 muốn lập nhóm với học sinh 5 thì học sinh 5 cũng muốn lập nhóm với học sinh 2."
date: "2026-06-05T18:28:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 300
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 181 (Div. 2)"
rating: 1500
weight: 300
solve_time_s: 131
verified: false
draft: false
---

[CF 300B - Huấn luyện viên](https://codeforces.com/problemset/problem/300/B) 

**Đánh giá:** 1500 
**Tags:** bạo lực, dfs và tương tự, đồ thị 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia`n`học sinh thành các đội có đúng ba người. Một số sinh viên yêu cầu rõ ràng được ở cùng đội với một số sinh viên khác. Các yêu cầu này là của nhau, vì vậy nếu học sinh 2 muốn lập nhóm với học sinh 5 thì học sinh 5 cũng muốn học sinh 2. Nhiệm vụ của chúng ta là xếp học sinh thành các nhóm sinh ba sao cho tôn trọng tất cả các yêu cầu đó. Nếu điều đó là không thể, chúng ta phải xuất ra`-1`. Nếu không, chúng ta nên in các đội thực tế. 

Đầu vào bao gồm`n`(chia hết cho 3) và`m`cặp sinh viên muốn ở bên nhau. Đầu ra là một trong hai`-1`nếu không có thỏa thuận hợp lệ nào tồn tại, hoặc`n/3`dòng gồm ba số nguyên, mỗi số đại diện cho một đội. 

Các hạn chế là nhỏ:`n ≤ 48`Và`m ≤ n(n-1)/2`. Điều này cho thấy rằng bất kỳ thuật toán nào có độ phức tạp lên tới khoảng`O(n^2)`sẽ thoải mái chạy trong vòng 2 giây. Vì vậy, chúng tôi có thể xem xét các phương pháp liên quan đến việc quét tất cả học sinh, xây dựng nhóm hoặc mô phỏng nhóm mà không cần lo lắng về việc tối ưu hóa cho các tập dữ liệu lớn. 

Một trường hợp quan trọng xảy ra khi học sinh có những yêu cầu trái ngược nhau buộc phải thành lập một nhóm lớn hơn ba người. Ví dụ: nếu học sinh 1, 2, 3 và 4 đều yêu cầu vào cùng một đội (thông qua một chuỗi yêu cầu) thì không thể thành lập một nhóm có đúng ba người. Một trường hợp khác là khi một số học sinh bị cô lập và phải kết hợp với những học sinh khác để tạo thành một đội hoàn chỉnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách phân chia học sinh có thể thành các nhóm ba người và kiểm tra xem tất cả các yêu cầu có được đáp ứng hay không. Cách tiếp cận này đúng về mặt lý thuyết nhưng không khả thi trong thực tế. Số cách phân vùng`n`học sinh chia thành nhóm 3 em theo thứ tự`(n!) / ((3!)^(n/3) * (n/3)!)`. Vì`n = 48`, cái này lớn về mặt thiên văn và không thể liệt kê được. 

Cách tiếp cận tối ưu quan sát thấy rằng các yêu cầu xác định các thành phần được kết nối trong biểu đồ: mỗi học sinh là một nút và một cạnh thể hiện yêu cầu "muốn ở bên nhau". Mỗi thành phần được kết nối phải hoàn toàn phù hợp với một hoặc nhiều nhóm. Nếu thành phần nào có nhiều hơn ba học sinh thì không thể chia thành các đội ba người vì các yêu cầu là của nhau và phải được tôn trọng. Các thành phần có kích thước 1 hoặc 2 có thể được kết hợp để tạo thành các nhóm. 

Cái nhìn sâu sắc là phân loại các thành phần theo kích thước. Các thành phần cỡ 3 tạo thành các đội hoàn chỉnh ngay lập tức. Các thành phần cỡ 2 cần thêm một học sinh từ các thành phần cỡ 1. Các thành phần có kích thước 1 có thể được nhóm thành ba nhóm để tạo thành một nhóm. Nếu tại bất kỳ thời điểm nào các con số không khớp nhau thì không có giải pháp hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Các thành phần được kết nối + Phân công nhóm tham lam | O(n^2) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn học sinh và yêu cầu của họ dưới dạng biểu đồ vô hướng sử dụng danh sách kề. Mỗi học sinh là một nút, mỗi học sinh yêu cầu một cạnh hai chiều. Điều này cho phép chúng tôi tìm các nhóm được kết nối một cách hiệu quả. 
2. Duyệt qua biểu đồ bằng Tìm kiếm theo chiều sâu hoặc Tìm kiếm theo chiều rộng để xác định tất cả các thành phần được kết nối. Mỗi thành phần đại diện cho một nhóm sinh viên phải ở cùng một đội. Lưu trữ kích thước của từng thành phần và danh sách sinh viên trong đó. 
3. Nếu bất kỳ thành phần nào có kích thước lớn hơn 3, hãy xuất ra`-1`và chấm dứt. Thành phần như vậy không thể được chia thành các nhóm có đúng ba người trong khi vẫn bảo toàn các yêu cầu. 
4. Phân loại các thành phần thành ba nhóm: cỡ 3, cỡ 2, cỡ 1. Các thành phần cỡ 3 tạo thành một đội hoàn chỉnh. Các thành phần thuộc cỡ 2 yêu cầu một học sinh từ nhóm cỡ 1 để hoàn thành một nhóm. Các thành phần có kích thước 1 có thể được kết hợp thành ba phần để tạo thành các nhóm hoàn chỉnh. 
5. Nếu số lượng học sinh cỡ 1 không đủ để ghép với các thành phần cỡ 2 hoặc không chia hết cho 3 sau khi ghép nối, hãy xuất ra`-1`. 
6. Tập hợp các đội theo thứ tự sau: đầu tiên, các nhóm gồm các thành phần cỡ 3, sau đó ghép từng thành phần cỡ 2 với một học sinh cỡ 1, và cuối cùng nhóm các học sinh cỡ 1 còn lại thành ba nhóm. 
7. Xuất ra tất cả các đội. Mỗi đội được in thành ba mã số học sinh. 

Tại sao nó hoạt động: Việc phân tách dựa trên biểu đồ đảm bảo rằng tất cả các yêu cầu chung đều được tôn trọng. Các thành phần lớn hơn ba sẽ bị từ chối vì chúng không thể vừa với một nhóm duy nhất. Việc kết hợp một cách tham lam các thành phần cỡ 2 với các học sinh cỡ 1 và nhóm các học sinh cỡ 1 còn lại thành ba nhóm đảm bảo rằng tất cả học sinh được bao gồm chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    
    for _ in range(m):
        a, b = map(int, input().split())
        adj[a-1].append(b-1)
        adj[b-1].append(a-1)

    visited = [False]*n
    components = []

    def dfs(u, comp):
        visited[u] = True
        comp.append(u)
        for v in adj[u]:
            if not visited[v]:
                dfs(v, comp)

    for i in range(n):
        if not visited[i]:
            comp = []
            dfs(i, comp)
            if len(comp) > 3:
                print(-1)
                return
            components.append(comp)

    ones, twos, threes = [], [], []
    for c in components:
        if len(c) == 1:
            ones.append(c[0])
        elif len(c) == 2:
            twos.append(c)
        else:
            threes.append(c)

    if len(ones) < len(twos):
        print(-1)
        return

    teams = []
    for t in threes:
        teams.append(t)
    for i, t in enumerate(twos):
        teams.append(t + [ones[i]])
    remaining_ones = ones[len(twos):]

    if len(remaining_ones) % 3 != 0:
        print(-1)
        return

    for i in range(0, len(remaining_ones), 3):
        teams.append(remaining_ones[i:i+3])

    for team in teams:
        print(' '.join(str(x+1) for x in team))

if __name__ == "__main__":
    main()
```Đầu tiên, mã xây dựng biểu đồ và tìm các thành phần được kết nối bằng DFS. Các thành phần lớn hơn ba kích hoạt ngay lập tức`-1`. Các thành phần được chia theo quy mô, sau đó các nhóm được xây dựng một cách tham lam. Việc lập chỉ mục cẩn thận đảm bảo rằng học sinh được chỉ định chính xác một lần và tất cả các thành phần cỡ 2 đều được hoàn thành với học sinh cỡ 1. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 0
```| Bước | Linh kiện | Những cái | Hai | Ba | Đội | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [[0],[1],[2]] | [0,1,2] | [] | [] | [] | 
| Lắp ráp | [[0,1,2]] | [] | [] | [] | [0,1,2] | 

Tất cả học sinh đều bị cô lập. Họ được nhóm lại thành một đội gồm ba người. 

**Đầu vào mẫu 2**```
6 2
1 2
3 4
```| Bước | Linh kiện | Những cái | Hai | Ba | Đội | 
| --- | --- | --- | --- | --- | --- | 
| DFS ban đầu | [[0,1],[2,3],[4],[5]] | [4,5] | [[0,1],[2,3]] | [] | [] | 
| Lắp ráp | - | [4,5] | [[0,1],[2,3]] | [] | [[0,1,4],[2,3,5]] | 

Hai cái được hoàn thành với những cái để tạo thành các đội đầy đủ. Đầu ra là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | DFS đi qua mỗi cạnh một lần; n 48, do đó, ngay cả việc kiểm tra kề O(n^2) cũng được. | 
| Không gian | O(n + m) | Danh sách kề lưu trữ các cạnh; mảng đã truy cập và danh sách thành phần lưu trữ n phần tử. | 

Với n ≤ 48 và m ≤ n(n-1)/2, cả thời gian và bộ nhớ đều ở dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3 0\n") in ["1 2 3","3 2 1"], "sample 1"

# Custom cases
assert run("6 2\n1 2\n3 4\n") in ["1 2 5\n3 4 6","1 2 6\n3 4 5"], "size-2 pairing"
assert run("3 1\n1 2\n") == "1 2 3", "2 wants together, 1 alone"
assert run("3 1\n1 2\n") == "1 2 3", "size-2 + single"
assert run("6 1\n1 2\n") in ["1 2 3\n4 5 6","1 2 4\n3 5
```
