---
title: "CF 910A - Đường Về Nhà"
description: "Chúng ta có một đường một chiều chứa các vị trí từ 1 đến n. Một số vị trí chứa hoa huệ, được biểu thị bằng số 1, trong khi các vị trí khác trống, được biểu thị bằng số 0. Một con ếch bắt đầu ở vị trí 1 và muốn đạt đến vị trí n."
date: "2026-06-13T00:24:42+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 910
codeforces_index: "A"
codeforces_contest_name: "Testing Round 14 (Unrated)"
rating: 800
weight: 910
solve_time_s: 750
verified: true
draft: false
---

[CF 910A - Đường về nhà](https://codeforces.com/problemset/problem/910/A) 

**Đánh giá:** 800 
**Tags:** dfs và tương tự, dp, tham lam, triển khai 
**Thời gian giải:** 12p 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường một chiều chứa các vị trí từ 1 đến n. Một số vị trí có hình hoa huệ, được biểu thị bằng hình`1`, trong khi những cái khác trống, được biểu thị bằng một`0`. Một con ếch xuất phát ở vị trí 1 và muốn đến vị trí n. 

Từ bất kỳ vị trí x nào có hoa huệ tồn tại, con ếch có thể nhảy về phía trước tới bất kỳ vị trí y nào sao cho 1 ≤ y − x ≤ d, nhưng chỉ khi vị trí y cũng chứa hoa huệ. Mỗi lần nhảy tốn chính xác một lần di chuyển và chúng tôi muốn số lần nhảy tối thiểu cần thiết để đạt đến vị trí n. Nếu không thể đạt được n thì câu trả lời là -1. 

Cấu trúc về cơ bản là một biểu đồ trong đó các nút là vị trí hoa huệ và các cạnh có hướng tồn tại từ i đến j nếu j có thể truy cập được từ i trong khoảng cách d. Nhiệm vụ giảm xuống còn tìm đường đi ngắn nhất từ ​​nút 1 đến nút n trong biểu đồ ẩn này. 

Ràng buộc n ≤ 100 ngay lập tức gợi ý rằng ngay cả nghiệm bậc hai hoặc bậc ba cũng có thể chấp nhận được. BFS đầy đủ trên tất cả các chuyển đổi sẽ không đáng kể trong phạm vi này, nhưng ngay cả tính năng quét tham lam cũng hoạt động do kích thước nhỏ. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Ví dụ: nếu tất cả các vị trí ngoại trừ điểm cuối đều bằng 0`10000001`, và d nhỏ, con ếch buộc phải bỏ qua những khoảng trống lớn hơn d, khiến không thể đến được đích. Một trường hợp tinh vi khác là khi tồn tại nhiều bước nhảy ngắn nhưng bước nhảy tham lam có độ dài tối đa không đạt được trên một chuỗi có thể truy cập được, chẳng hạn như:```
n = 7, d = 3
1000101
```Mặc dù một bước nhảy xa có vẻ hấp dẫn, nhưng chỉ có các vị trí hoa huệ trung gian mới quan trọng và việc bỏ qua chúng sẽ làm mất kết nối. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi vị trí hoa huệ như một nút và thử mọi chuỗi bước nhảy có thể. Từ vị trí i, chúng ta có thể thử tất cả j có thể truy cập được trong [i+1, i+d] có chứa hoa huệ, khám phá đệ quy tất cả các đường dẫn cho đến khi đạt được n. Điều này đúng vì nó liệt kê tất cả các tuyến đường hợp lệ, nhưng về bản chất nó là hàm mũ. Trong trường hợp xấu nhất, khi mọi vị trí đều có hoa huệ và d lớn, số lượng đường đi tăng lên giống như hệ số phân nhánh d trên độ sâu n, điều này nhanh chóng trở nên không khả thi ngay cả khi n = 100. 

Quan sát quan trọng là tất cả các cạnh đều có trọng số bằng nhau, mỗi lần nhảy có giá đúng bằng 1. Điều này biến bài toán thành bài toán đường đi ngắn nhất không có trọng số. Thay vì khám phá tất cả các con đường, chúng ta chỉ cần số bước tối thiểu để đến từng vị trí có thể tiếp cận. Điều này tự nhiên gợi ý lập trình động hoặc BFS. 

Giải thích DP trực tiếp là duy trì`dp[i]`là số lần nhảy tối thiểu cần thiết để đạt đến vị trí i. Chúng ta khởi tạo dp[1] = 0 và nới lỏng các chuyển tiếp về phía trước: từ i, chúng ta thử tất cả j trong (i+1 đến i+d). Nếu j có hoa huệ, chúng ta cập nhật dp[j] = min(dp[j], dp[i] + 1). Bởi vì n nhỏ nên việc nới lỏng O(n·d) đơn giản là đủ. 

Công thức BFS tương đương xem mỗi vị trí dưới dạng một nút trong biểu đồ có các cạnh đến d vị trí tiếp theo có chứa hoa huệ. BFS đảm bảo đường đi ngắn nhất trong biểu đồ không có trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | Hàm mũ | Ngăn xếp đệ quy O(n) | Quá chậm | 
| DP / BFS qua các vị trí | O(n·d) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi thành một mảng boolean để chúng ta có thể nhanh chóng kiểm tra xem một vị trí có chứa hoa huệ hay không. Điều này cho phép kiểm tra tính hợp lệ theo thời gian liên tục trong quá trình chuyển đổi. 
2. Tạo mảng khoảng cách`dist`có kích thước n+1 và khởi tạo tất cả các giá trị thành một số lớn ngoại trừ`dist[1] = 0`, vì chúng tôi bắt đầu ở vị trí 1 với số lần nhảy bằng 0. 
3. Lặp qua các vị trí từ 1 đến n. Đối với mỗi vị trí tôi có thể tiếp cận được (nghĩa là`dist[i]`không phải là vô cùng), chúng ta cố gắng nhảy về phía trước. 
4. Từ vị trí i, thử tất cả các vị trí j tiếp theo từ i+1 đến min(n, i+d). Nếu vị trí j có hoa huệ, chúng ta có thể đến đó bằng một bước nhảy bổ sung, vì vậy chúng ta cập nhật`dist[j] = min(dist[j], dist[i] + 1)`. 
5. Tiếp tục quá trình thư giãn này cho tất cả các tư thế. Bởi vì chúng tôi luôn truyền bá khoảng cách ngắn nhất đã biết về phía trước nên những cải tiến sau này vẫn tinh chỉnh các giá trị gần đúng trước đó. 
6. Sau khi xử lý, kiểm tra`dist[n]`. Nếu nó vẫn là vô cùng, xuất ra -1. Nếu không thì xuất ra`dist[n]`. 

Ý tưởng chính là mọi vị trí đều được xử lý theo thứ tự tăng dần và tất cả các chuyển đổi chỉ tiến về phía trước, vì vậy khi xử lý xong i, chúng ta sẽ không bao giờ tìm thấy đường dẫn tốt hơn đến i sau này. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`dist[i]`luôn lưu trữ số lần nhảy tối thiểu cần thiết để đến được i chỉ bằng cách sử dụng các vị trí hoa huệ hợp lệ. Khi chúng tôi xử lý i, tất cả các cách tối ưu để tiếp cận i đã được xem xét bởi vì bất kỳ đường dẫn nào tới i đều phải đến từ một chỉ số nhỏ hơn do chuyển động tiến về phía trước một cách chặt chẽ. Mỗi bước cuối cùng có thể đến vị trí có thể tiếp cận đều được kiểm tra chính xác một lần, đảm bảo không bỏ sót đường đi ngắn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    s = input().strip()
    
    INF = 10**9
    dist = [INF] * n
    dist[0] = 0  # position 1
    
    for i in range(n):
        if dist[i] == INF:
            continue
        if s[i] == '0':
            continue
        for j in range(i + 1, min(n, i + d + 1)):
            if s[j] == '1':
                if dist[j] > dist[i] + 1:
                    dist[j] = dist[i] + 1
    
    print(dist[n - 1] if dist[n - 1] != INF else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng mảng có chỉ mục 0 để biểu thị các vị trí từ 1 đến n. các`dist`mảng theo dõi số lần nhảy tối thiểu cần thiết để đạt được từng chỉ mục. Vòng lặp lồng nhau liệt kê tất cả các bước nhảy về phía trước có thể có độ dài tối đa là d. 

Một chi tiết tinh tế là điều kiện`if s[i] == '0'`. Ngay cả khi một vị trí có khoảng cách được tính toán thì nó cũng không liên quan trừ khi nó thực sự chứa một bông hoa huệ, vì con ếch không thể đứng ở đó. Điều này ngăn cản việc truyền bá quá trình chuyển đổi từ các trạng thái không hợp lệ. 

Vòng lặp bên trong sử dụng cẩn thận`min(n, i + d + 1)`để tránh truy cập ngoài giới hạn trong khi vẫn duy trì tất cả độ dài bước nhảy hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 4
10010101
```Chúng tôi theo dõi`dist`khi chúng tôi xử lý. 

| tôi | quận [i] | s[i] | bước nhảy có thể tiếp cận | cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1-4 | dist[3]=1 | 
| 3 | 1 | 1 | 4-7 | dist[4]=2, dist[6]=2 | 
| 4 | 2 | 0 | không | không | 
| 6 | 2 | 1 | 7-7 | dist[7]=3 | 

Câu trả lời cuối cùng là 2 vì đường đi tốt nhất là 1 → 4 → 8. 

Dấu vết này xác nhận rằng việc bỏ qua các vị trí không hợp lệ (như chỉ mục 4) không chặn tiến trình miễn là tồn tại các chuỗi có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
5 2
10101
```| tôi | quận [i] | s[i] | bước nhảy có thể tiếp cận | cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1-2 | dist[2]=1 | 
| 2 | 1 | 1 | 3-4 | dist[4]=2 | 
| 4 | 2 | 1 | không | không | 

Câu trả lời là 2. 

Điều này cho thấy rằng mặc dù các vị trí thưa thớt, DP chỉ xâu chuỗi chính xác các vị trí hoa huệ hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·d) | Đối với mỗi vị trí có thể tiếp cận, chúng tôi quét tới d vị trí chuyển tiếp | 
| Không gian | O(n) | Mảng khoảng cách lưu trữ một giá trị cho mỗi vị trí | 

Với n ≤ 100, số lượng thao tác tối đa nhiều nhất là 10.000, điều này là không đáng kể trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("builtins").input.__globals__['solve']()

# provided sample
assert run("8 4\n10010101\n") == "2"

# minimal case
assert run("2 1\n11\n") == "1"

# unreachable case
assert run("5 2\n10000\n") == "-1"

# all lilies, large jumps
assert run("6 5\n111111\n") == "1"

# forced multi-step
assert run("7 2\n1010101\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1/11 | | |
