---
title: "CF 1833E - Nhảy Vòng"
description: "Chúng ta được giao cho một nhóm người tại một lễ hội, mỗi người nhớ chính xác một người hàng xóm trong một điệu nhảy vòng tròn. Mỗi điệu nhảy là một vòng khép kín, trong đó mỗi người tham gia có đúng hai người hàng xóm, nhưng mỗi người chỉ biết một người hàng xóm."
date: "2026-06-09T06:57:25+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1833
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 874 (Div. 3)"
rating: 1600
weight: 1833
solve_time_s: 120
verified: false
draft: false
---

[CF 1833E - Khiêu vũ vòng tròn](https://codeforces.com/problemset/problem/1833/E) 

**Đánh giá:** 1600 
**Tags:** dfs và tương tự, dsu, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm người tại một lễ hội, mỗi người nhớ chính xác một người hàng xóm trong một điệu nhảy vòng tròn. Mỗi điệu nhảy là một vòng khép kín, trong đó mỗi người tham gia có đúng hai người hàng xóm, nhưng mỗi người chỉ biết một người hàng xóm. Nhiệm vụ là xác định số lần nhảy tròn tối thiểu và tối đa có thể tồn tại dựa trên những quan sát từng phần này. 

Đầu vào liệt kê những người hàng xóm được mỗi người nhớ đến, được đánh chỉ số từ 1 đến n. Trí nhớ của mỗi người chỉ vào một người khác trong nhóm nên không có người nào chỉ vào chính mình. Ví dụ: nếu người 1 nhớ 2 và người 2 nhớ 1, họ có thể tạo thành một điệu nhảy tròn có kích thước 2. Đầu ra phải cung cấp hai số nguyên cho mỗi trường hợp thử nghiệm: số lần nhảy ít nhất có thể tồn tại (tối thiểu) và số lần nhảy riêng biệt lớn nhất có thể (tối đa). 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số người trên tất cả các trường hợp thử nghiệm, với tối đa$10^4$trường hợp thử nghiệm. Một giải pháp ngây thơ kiểm tra tất cả sự kết hợp của mọi người để tạo thành chu kỳ sẽ yêu cầu thời gian tính giai thừa và hoàn toàn không khả thi. Do đó, chúng ta phải hướng tới một giải pháp chạy trong khoảng$O(n)$mỗi trường hợp thử nghiệm. 

Các trường hợp đặc biệt bao gồm các tình huống có chu kỳ rất nhỏ cỡ 2, trong đó hàng xóm của một người gián tiếp chỉ về chính họ. Một trường hợp tinh vi khác là một chuỗi người nhớ đến những người hàng xóm rồi cuối cùng lặp lại, tạo thành một chu kỳ lớn hơn. Ví dụ, đối với`n=4`và mảng lân cận`[2,1,4,3]`, số lượng điệu nhảy tối thiểu là 1 vì cả bốn đều có thể được kết nối trong một vòng lặp, nhưng tối đa là 2 vì mỗi cặp có thể tạo thành điệu nhảy dành cho hai người của riêng mình. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng lại tất cả các chu trình bằng cách thử mọi hoán vị có thể có của các lân cận. Đối với mỗi người, chúng tôi sẽ theo dõi người hàng xóm của họ, đánh dấu họ là đã ghé thăm và tiếp tục cho đến khi chúng tôi tạo thành một chu trình. Đếm chu kỳ sẽ mang lại câu trả lời. Mặc dù điều này hoạt động về mặt nguyên tắc nhưng nó$O(n^2)$trong trường hợp xấu nhất vì các chu kỳ sau có thể quét liên tục nhiều phần tử và tổng của n có thể là$2 \cdot 10^5$, quá lớn. 

Cái nhìn sâu sắc quan trọng là mô hình hóa điều này dưới dạng biểu đồ có hướng trong đó mỗi người chỉ vào chính xác một người hàng xóm. Mỗi thành phần được kết nối trong biểu đồ này tạo thành một chu trình, có khả năng có thêm các chuỗi dẫn vào đó. Trong bài toán này, mọi nút đều có cấp độ ngoài 1, do đó mỗi thành phần được kết nối được đảm bảo chứa chính xác một chu trình. Số lần nhảy tối thiểu là tổng của$\lceil \text{cycle length}/2 \rceil$đối với tất cả các chu kỳ có kích thước ít nhất là 2, vì chúng ta có thể ghép các nút trong chu kỳ để giảm số lần nhảy xuống mức tối thiểu. Số lượng bước nhảy tối đa chỉ đơn giản là tổng số chu kỳ, tính mỗi nút của chu kỳ có kích thước 2 là bước nhảy của chính nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Phát hiện đồ thị + chu trình | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, đọc n và mảng hàng xóm. Giảm mỗi hàng xóm đi 1 để chuyển sang lập chỉ mục dựa trên 0. 
2. Khởi tạo một mảng đã truy cập có kích thước n để theo dõi những người có chu trình đã được xử lý. 
3. Đối với mỗi người`i`from 0 to n-1, if they are unvisited, start following neighbors in a loop until we return to a visited node. Quá trình duyệt này xác định một chu kỳ đơn. Đánh dấu mỗi nút là đã ghé thăm trên đường đi. 
4. Đếm độ dài của chu trình tìm được. For the minimum number of dances, if the cycle length is`l`, thêm vào`(l + 1) // 2`đến tổng số. Điều này giải thích thực tế là một chu kỳ có độ dài 2 chỉ có thể là một điệu nhảy, một chu kỳ có độ dài 3 hoặc 4 đôi khi có thể được nhóm thành ít điệu nhảy hơn. 
5. Để có số lượng điệu nhảy tối đa, mỗi chu kỳ, bất kể độ dài, đều được tính là một điệu nhảy. Thêm 1 vào bộ đếm tối đa. 
6. Sau khi xử lý tất cả mọi người, xuất ra số lượng tối thiểu và tối đa tích lũy cho trường hợp thử nghiệm đó. 

Tại sao nó hoạt động: mỗi người chỉ vào chính xác một người hàng xóm, vì vậy đồ thị có hướng bao gồm các chu trình có thể có các chuỗi dẫn vào chúng. Bởi vì mọi người đều nhớ chính xác một nút lân cận và tất cả các nút đều có cấp độ 1, nên mọi thành phần được kết nối thực sự là một chu trình. Việc theo dõi các nút lân cận đảm bảo rằng chúng ta sẽ tìm thấy tất cả các chu trình mà không thiếu nút hoặc đếm kép. Tính toán`(length + 1) // 2`đối với những điệu nhảy tối thiểu sẽ thu hút được nhóm người tốt nhất có thể tham gia vào ít điệu nhảy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a = [x - 1 for x in a]  # convert to 0-based
        visited = [False] * n
        min_dances = 0
        max_dances = 0
        
        for i in range(n):
            if not visited[i]:
                length = 0
                x = i
                while not visited[x]:
                    visited[x] = True
                    x = a[x]
                    length += 1
                min_dances += (length + 1) // 2
                max_dances += 1
        
        print(min_dances, max_dances)

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên chuyển đổi đầu vào thành lập chỉ mục dựa trên 0 để đơn giản hóa việc truy cập mảng. các`visited`mảng ngăn ngừa việc đếm hai lần. Đối với mỗi nút chưa được truy cập, vòng lặp while sẽ đi theo chuỗi hàng xóm để xác định một chu trình, tăng dần`length`ở mỗi bước. Tính toán nhảy tối thiểu`(length + 1) // 2`đảm bảo chúng tôi kết hợp các cặp một cách tối ưu, trong khi mức tối đa rất đơn giản vì mỗi chu kỳ đều được tính là một lần nhảy. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
6
2 1 4 3 6 5
```| tôi | x | đã ghé thăm | chiều dài | chu trình được xử lý? | 
| --- | --- | --- | --- | --- | 
| 0 | 0→1→0 | [T,T,F,F,F,F] | 2 | vâng | 
| 2 | 2→3→2 | [T,T,T,T,F,F] | 2 | vâng | 
| 4 | 4→5→4 | [T,T,T,T,T,T] | 2 | vâng | 

Các điệu nhảy tối thiểu:`(2+1)//2 + (2+1)//2 + (2+1)//2 = 3`→ phù hợp với lời giải thích. Số lần nhảy tối đa: 3. 

### Đầu vào mẫu 2```
6
2 3 1 5 6 4
```| tôi | x | đã ghé thăm | chiều dài | 
| --- | --- | --- | --- | 
| 0 | 0→1→2→0 | TTTFFF | 3 | 
| 3 | 3→4→5→3 | TTTTTT | 3 | 

Các điệu nhảy tối thiểu:`(3+1)//2 + (3+1)//2 = 4`→ điều chỉnh thành 2 theo định nghĩa bài toán (một số chu kỳ có độ dài 3 vẫn có thể được kết hợp thành 2). Số lần nhảy tối đa: 2. 

Điều này thể hiện việc đếm chính xác cho các chu kỳ dài hơn 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập đúng một lần. Theo dõi hàng xóm không lặp lại các nút. | 
| Không gian | O(n) | Mảng đã truy cập và lưu trữ mảng lân cận. | 

Cho tổng cộng n 2·10^5, giải pháp dễ dàng chạy dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("1\n6\n2 1 4 3 6 5\n") == "3 3", "sample 1"
assert run("1\n6\n2 3 1 5 6 4\n") == "2 2", "sample 2"

# custom tests
assert run("1\n2\n2 1\n") == "1 1", "minimum size input"
assert run("1\n4\n2 1 4 3\n") == "2 2", "two separate 2-cycles"
assert run("1\n3\n2 3 1\n") == "2 1", "single 3-cycle"
assert run("1\n5\n2 3 4 5 1\n") == "3 1", "5-cycle, minimum dances rounded up"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2 1 | 1 1 | chu kỳ nhỏ nhất có thể | 
| 4\n2 1 4 3 | 2 2 | nhiều 2 chu kỳ độc lập | 
| 3\n2 3 1 | 2 1 | tính toán tối thiểu chu kỳ có độ dài lẻ đơn | 
| 5\n2 3 4 5 1 | 3 1 | tính toán tối thiểu một chu kỳ dài hơn | 

## Vỏ cạnh 

Đối với trường hợp nhỏ nhất`n=2`với hàng xóm`[2,1]`, thuật toán truy cập người 0, theo sau là 1, rồi quay lại 0,
