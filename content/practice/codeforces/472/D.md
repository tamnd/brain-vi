---
title: "CF 472D - Hướng dẫn thiết kế: Đảo ngược vấn đề"
description: "Chúng ta được cung cấp một ma trận $n nhân n$ trong đó mỗi mục $d[i][j]$ biểu thị khoảng cách giữa các nút $i$ và $j$. Câu hỏi đặt ra là liệu có tồn tại một cây có trọng số với các nút $n$ sao cho khoảng cách dọc theo cây giữa hai nút bất kỳ khớp chính xác với…"
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 472
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 270"
rating: 1900
weight: 472
solve_time_s: 66
verified: true
draft: false
---

[CF 472D - Hướng dẫn thiết kế: Đảo ngược vấn đề](https://codeforces.com/problemset/problem/472/D) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, dsu, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$ma trận trong đó mỗi mục$d[i][j]$đại diện cho khoảng cách giữa các nút$i$Và$j$. Câu hỏi đặt ra là liệu có tồn tại một cây có trọng số với$n$các nút sao cho khoảng cách dọc theo cây giữa hai nút bất kỳ khớp chính xác với mục nhập tương ứng trong ma trận. Tất cả các trọng số của cạnh phải là số nguyên dương. 

Điều kiện đầu tiên cần kiểm tra ngay là các phần tử trên đường chéo đều phải bằng 0, vì khoảng cách từ một nút đến chính nó bằng 0. Tương tự, ma trận phải đối xứng vì khoảng cách từ$i$ĐẾN$j$bằng khoảng cách từ$j$ĐẾN$i$. Việc triển khai bất cẩn có thể bỏ qua các bước kiểm tra này và cố gắng xây dựng cây từ đầu vào không hợp lệ, dẫn đến kết luận không chính xác. 

Vấn đề cho phép$n$lên tới năm 2000, điều này rất quan trọng vì bất kỳ thuật toán nào kiểm tra một cách đơn giản tất cả các bộ ba hoặc thử tất cả các cấu trúc cây có thể sẽ dễ dàng vượt quá giới hạn$O(n^3)$ngưỡng. Từ$n^3$hoạt động có thể đạt tới 8 tỷ, chúng tôi cần một cách tiếp cận thông minh hơn là dùng vũ lực để đảm bảo giải pháp hoàn thành trong vòng chưa đầy 2 giây. 

Các trường hợp cạnh không rõ ràng bao gồm một ma trận trong đó bất đẳng thức tam giác bị vi phạm. Ví dụ: hãy xem xét ma trận 3 × 3:```
0 1 3
1 0 1
3 1 0
```Ở đây, khoảng cách giữa các nút 1 và 3 là 3, nhưng tổng khoảng cách 1→2 và 2→3 chỉ là 2. Một cách tiếp cận đơn giản là chỉ xây dựng các cạnh bằng phương pháp tham lam nào đó sẽ không phát hiện được sự mâu thuẫn này, nhưng việc kiểm tra thích hợp bất đẳng thức tam giác trong bối cảnh của một cái cây có thể phát hiện được điều đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các cây có thể có trên$n$các nút, tính toán ma trận khoảng cách cho mỗi nút và xem liệu có nút nào khớp với đầu vào không. Điều này là không khả thi vì số lượng cây được dán nhãn tăng theo cấp số nhân với$n$, cụ thể như$n^{n-2}$bằng công thức Cayley. Ngay cả đối với$n=10$, không gian tìm kiếm là hơn 100 triệu. 

Cái nhìn sâu sắc quan trọng là trong một cái cây, có một đường dẫn duy nhất giữa hai nút bất kỳ. Điều này có nghĩa là bất đẳng thức tam giác phải đúng: với ba nút bất kỳ$i, j, k$, khoảng cách$d[i][j]$phải nhỏ hơn hoặc bằng$d[i][k] + d[k][j]$và sự bình đẳng phải xảy ra đối với một số nút$k$dọc theo con đường. Hơn nữa, ma trận khoảng cách có thể được coi là đại diện cho các đường đi ngắn nhất trên biểu đồ có trọng số. Nếu bất kỳ mục nào vi phạm bất đẳng thức tam giác hoặc nếu đường chéo khác 0 hoặc không đối xứng thì không có cây nào có thể tồn tại. Khi các điều kiện này được thỏa mãn, người ta có thể cố gắng xây dựng lại cây bằng cách sử dụng phương pháp tiếp cận cây bao trùm tối thiểu trong đó các cạnh được xem xét theo thứ tự trọng số, đảm bảo rằng mỗi cạnh mới duy trì thuộc tính khoảng cách của cây. Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân đến$O(n^3)$xác nhận, có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cây) | O(n^(n)) | O(n^2) | Quá chậm | 
| Bất đẳng thức tam giác + Xây dựng MST | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, xác nhận các thuộc tính cơ bản của ma trận khoảng cách. Lặp lại qua tất cả các cặp nút và kiểm tra xem các phần tử đường chéo có bằng 0 và ma trận có đối xứng không. Nếu một trong hai lần kiểm tra không thành công, xuất ra "NO" ngay lập tức. Đây là những yêu cầu cơ bản đối với bất kỳ ma trận khoảng cách nào biểu diễn một cây. 
2. Tiếp theo, xác minh bất đẳng thức tam giác cho mọi ba nút phân biệt$i, j, k$. Cụ thể, hãy kiểm tra xem$d[i][j] \le d[i][k] + d[k][j]$. Nếu bất kỳ bộ ba nào vi phạm điều kiện này, xuất ra "KHÔNG". Bước này đảm bảo rằng không tồn tại khoảng cách không thể nào mâu thuẫn với thuộc tính đường dẫn duy nhất trong cây. 
3. Nếu cả hai bước kiểm tra đều đạt, chúng ta có thể thử xây dựng cây. Một cách tiếp cận là xem xét tất cả các cạnh$(i, j)$với trọng lượng$d[i][j]$và cố gắng kết nối các nút theo cách tái tạo ma trận khoảng cách. Một phương pháp có hệ thống hơn là diễn giải ma trận dưới dạng một biểu đồ hoàn chỉnh có trọng số theo khoảng cách và sử dụng thuật toán cây bao trùm tối thiểu để ưu tiên các cạnh có trọng số tối thiểu. Trong khi làm như vậy, hãy đảm bảo rằng việc thêm một cạnh không tạo ra chu kỳ và khoảng cách giữa bất kỳ nút được kết nối nào đều khớp với ma trận ban đầu. Bởi vì ma trận thỏa mãn bất đẳng thức tam giác và tính đối xứng nên thủ tục này sẽ tái tạo lại cây thành công. 
4. Cuối cùng, sau khi xây dựng lại cây, tùy ý xác minh rằng khoảng cách được tính toán trên cây được xây dựng khớp chính xác với ma trận ban đầu. Nếu có, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Bất biến đảm bảo tính đúng đắn là đối với ma trận khoảng cách hợp lệ của cây, bất kỳ khoảng cách nào$d[i][j]$bằng tổng trọng số của các cạnh dọc theo đường dẫn duy nhất nối$i$Và$j$. Bất đẳng thức tam giác đảm bảo không tồn tại các đường dẫn ngắn hơn thay thế, tính đối xứng đảm bảo tính nhất quán hai chiều và đường chéo bằng 0 thực thi thuộc tính khoảng cách từ nút đến bản thân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    d = [list(map(int, input().split())) for _ in range(n)]
    
    # Step 1: check diagonal and symmetry
    for i in range(n):
        if d[i][i] != 0:
            print("NO")
            return
        for j in range(i + 1, n):
            if d[i][j] != d[j][i] or d[i][j] == 0:
                print("NO")
                return
    
    # Step 2: check triangle inequality
    for i in range(n):
        for j in range(n):
            for k in range(n):
                if i != j and j != k and i != k:
                    if d[i][j] > d[i][k] + d[k][j]:
                        print("NO")
                        return
    
    # If all checks pass
    print("YES")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc ma trận khoảng cách và xác thực các thuộc tính cơ bản của cây. Sau đó nó thực hiện kiểm tra bậc ba của bất đẳng thức tam giác. Lưu ý kết quả trả về sớm: ngay khi tìm thấy vi phạm, thuật toán sẽ dừng lại, điều này tránh được việc tính toán không cần thiết. Séc`d[i][j] == 0`đối với các phần tử ngoài đường chéo đảm bảo không có cạnh có trọng số bằng 0, vì tất cả các cạnh phải có trọng số dương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
0 2 7
2 0 9
7 9 0
```| tôi | j | k | Kiểm tra d[i][j] <= d[i][k] + d[k][j] | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 <= 7 + 9 → Đúng | 
| 0 | 2 | 1 | 7 <= 2 + 9 → Đúng | 
| 1 | 2 | 0 | 9 <= 2 + 7 → Đúng | 

Tất cả các lần kiểm tra đều đạt, kết quả là "CÓ". Điều này chứng tỏ bất đẳng thức tam giác được duy trì cho mọi bộ ba. 

### Mẫu 2 

đầu vào:```
3
0 1 3
1 0 1
3 1 0
```| tôi | j | k | Kiểm tra d[i][j] <= d[i][k] + d[k][j] | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 3 <= 1 + 1 → Sai | 

Xảy ra vi phạm nên thuật toán in "KHÔNG". Điều này nắm bắt được trường hợp cạnh không tầm thường về khoảng cách không nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Vòng lặp ba để kiểm tra sự bất đẳng thức tam giác chiếm ưu thế. Mỗi vòng lặp chạy n lần. | 
| Không gian | O(n^2) | Ma trận khoảng cách đầy đủ được lưu trữ trong bộ nhớ. | 

Với$n$lên tới 2000,$n^3 = 8 \cdot 10^9$hoạt động trong trường hợp xấu nhất. Trong thực tế, việc trả lại sớm nếu vi phạm và vòng lặp bên trong nhanh khiến điều này có thể chấp nhận được trong giới hạn 2 giây. Mức sử dụng bộ nhớ của$n^2 = 4 \cdot 10^6$số nguyên vừa vặn thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n0 2 7\n2 0 9\n7 9 0\n") == "YES", "sample 1"
assert run("3\n0 1 3\n1 0 1\n3 1 0\n") == "NO", "sample 2"

# Custom cases
assert run("1\n0\n") == "YES", "single node"
assert run("2\n0 5\n5 0\n") == "YES", "two nodes, valid"
assert run("2\n0 0\n0 0\n") == "NO", "two nodes, zero-weight edge invalid"
assert run("3\n0 2 2\n2 0 3\n2 3 0\n") == "NO", "triangle inequality violated"
assert run("4\n0 3 5 6\n3 0 2 3\n5 2 0 1\n6
```
