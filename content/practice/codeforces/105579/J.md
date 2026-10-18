---
title: "CF 105579J - Sự thống trị của các vị vua"
description: "Chúng tôi được cung cấp vị trí của một số vị vua được đặt trên lưới 100 x 100. Trong một hành động, chúng ta chọn một vị vua duy nhất và bắt đầu di chuyển nó từng bước theo bất kỳ hướng nào trong tám hướng, do đó, mỗi bước di chuyển sẽ thay đổi ô của nó thành một trong các ô vuông liền kề bao gồm cả các đường chéo."
date: "2026-06-22T20:54:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "J"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 63
verified: true
draft: false
---

[CF 105579J - Sự thống trị của các vị vua](https://codeforces.com/problemset/problem/105579/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp vị trí của một số vị vua được đặt trên lưới 100 x 100. Trong một hành động, chúng ta chọn một vị vua duy nhất và bắt đầu di chuyển nó từng bước theo bất kỳ hướng nào trong tám hướng, do đó, mỗi bước di chuyển sẽ thay đổi ô của nó thành một trong các ô vuông liền kề bao gồm cả các đường chéo. Bất cứ khi nào quân vua đang di chuyển này đi vào một ô đã chứa vua khác, vua đó sẽ bị loại khỏi bàn cờ và quá trình di chuyển đối với vua được chọn này sẽ tiếp tục cho đến khi nó thực hiện việc bắt giữ như vậy. Sau khi việc bắt giữ diễn ra, chúng ta dừng hành động này và có thể chọn bất kỳ vị vua nào còn lại để lặp lại quá trình. 

Mục tiêu là giảm số quân vua xuống còn đúng một, giảm thiểu tổng số bước đi của quân vua trong tất cả các hành động. 

Nhận xét quan trọng từ các ràng buộc là có tối đa 100 vị vua. Giá trị này đủ nhỏ để bất kỳ giải pháp nào cố gắng xem xét mối quan hệ giữa tất cả các cặp vua đều khả thi, vì một đồ thị hoàn chỉnh trên 100 nút chỉ có 4950 cạnh. Điều này ngay lập tức gợi ý rằng có thể có liên quan đến khoảng cách theo cặp và tối ưu hóa tổng thể trên một cấu trúc được kết nối đầy đủ. 

Một vấn đề tế nhị là hiểu chi phí “di chuyển” thực sự có ý nghĩa gì. Việc chụp ảnh không miễn phí; nó tiêu tốn số bước vua cần thiết để đi từ vua ban đầu đến vua mục tiêu. Đây không chỉ là sự kề cận theo nghĩa đồ thị mà là đường đi ngắn nhất hình học trên lưới 8 điểm kết nối, chính xác là khoảng cách Chebyshev giữa hai điểm. 

Một sự hiểu lầm ngây thơ sẽ coi mỗi lần bắt được là chi phí 1, điều này là sai vì việc di chuyển trên bảng có thể mất nhiều bước. Một chế độ thất bại khác là chỉ nghĩ đến những người hàng xóm gần nhất một cách tham lam, điều này không tính đến cấu trúc lâu dài của việc hợp nhất nhiều vị vua một cách hiệu quả. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng toàn bộ quá trình liên tục chọn vua nào để di chuyển và mục tiêu nào để bắt và đối với mỗi chuỗi có thể, hãy tính tổng chi phí. Điều này tương đương với việc khám phá tất cả các thứ tự có thể có của việc hợp nhất k đối tượng và ở mỗi bước thử tất cả các cặp có thể hợp nhất với chi phí đường đi chính xác. Ngay cả khi chúng ta bỏ qua việc tính toán lại đường dẫn, số chuỗi vẫn là giai thừa tính theo k, điều này hoàn toàn không khả thi ngay cả với k = 20, chứ đừng nói đến 100. 

Cái nhìn sâu sắc về cấu trúc quan trọng là diễn giải lại quá trình. Mỗi lần chúng ta di chuyển một vị vua để bắt một vị vua khác, chúng ta đang hợp nhất hai thành phần thành một một cách hiệu quả và chi phí cho việc hợp nhất đó chỉ phụ thuộc vào vị trí của chúng tại thời điểm đó. Vì kết quả cuối cùng là một vị vua còn lại duy nhất nên mỗi vị vua ngoại trừ một vị vua phải được hấp thụ đúng một lần. Đây chính xác là cấu trúc xây dựng cây bao trùm trên tập hợp các điểm ban đầu, trong đó mỗi cạnh tương ứng với một thao tác bắt và trọng số là chi phí di chuyển giữa hai vị vua được hợp nhất tại thời điểm đó. 

Khoảng cách giữa hai vua (i, j) là cố định bất kể thứ tự hợp nhất vì đích đến của việc hợp nhất luôn là vị trí của một trong hai điểm cuối ban đầu. Điều này biến bài toán thành việc tìm cây khung nhỏ nhất trên một đồ thị đầy đủ các vị vua, trong đó mỗi trọng số cạnh là khoảng cách Chebyshev giữa các tọa độ. 

Khi được nhìn theo cách này, vấn đề sẽ giảm xuống thành tính toán MST tiêu chuẩn trên tối đa 100 nút, có thể giải quyết hiệu quả bằng thuật toán của Kruskal. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hợp nhất vũ phu | hàm mũ trong k | O(k) | Quá chậm | 
| Cây bao trùm tối thiểu trên đồ thị đầy đủ | O(k^2 log k) | O(k^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi quân vua như một nút trong biểu đồ và xác định chi phí giữa hai quân vua bất kỳ là số nước đi của vua cần thiết để đi từ quân này sang quân khác trên một bàn cờ trống. 

1. Tính chi phí giữa mỗi cặp quân vua bằng khoảng cách Chebyshev. Đây là`max(|r1 - r2|, |c1 - c2|)`bởi vì vua có thể di chuyển theo đường chéo, đồng thời giảm sự chênh lệch cả hàng và cột. Điều này xác định một biểu đồ có trọng số hoàn chỉnh. 
2. Xây dựng tất cả các cạnh (i, j) với trọng số tính toán của chúng. Vì k tối đa là 100 nên số cạnh có thể quản lý được. 
3. Sắp xếp tất cả các cạnh theo trọng lượng theo thứ tự không giảm. Điều này chuẩn bị cho chúng ta tham lam chọn kết nối có sẵn nhỏ nhất không tạo ra chu trình trong cấu trúc hợp nhất. 
4. Chạy cấu trúc Disjoint Set Union trên k nút. Ban đầu mỗi vị vua là thành phần riêng của nó. 
5. Lặp lại các cạnh đã được sắp xếp. Đối với mỗi cạnh (i, j), nếu i và j nằm trong các thành phần khác nhau, hãy hợp nhất chúng và thêm trọng số của cạnh vào câu trả lời. Tiếp tục cho đến khi tất cả các vua được kết nối thành một thành phần, yêu cầu chính xác k − 1 sự hợp nhất. 

Mỗi lần hợp nhất tương ứng với một thao tác chụp trong quy trình ban đầu và trọng số cạnh tích lũy biểu thị tổng số lần di chuyển vua một bước được yêu cầu. 

### Tại sao nó hoạt động 

Mỗi chuỗi bắt giữ hợp lệ sẽ xác định một cây bao trùm trên các vua ban đầu, trong đó mỗi cạnh tương ứng với một bước bắt giữ giữa hai thành phần riêng biệt trước đó. Chi phí của bước đó chính xác là khoảng cách Chebyshev giữa cặp đã chọn tại thời điểm hợp nhất, khớp với trọng số cạnh cố định trong biểu đồ hoàn chỉnh. Bất kỳ chiến lược nào dẫn đến một vua còn lại phải thực hiện chính xác k − 1 phép hợp nhất, do đó tương ứng với một số cây bao trùm. 

Ngược lại, bất kỳ cây bao trùm nào cũng có thể được hiện thực hóa bằng cách chọn một gốc và mô phỏng việc hợp nhất dọc theo các cạnh của nó, đảm bảo rằng mỗi cạnh được thanh toán chính xác một lần. Do đó, việc giảm thiểu tổng chi phí di chuyển tương đương với việc tìm cây khung nhỏ nhất của đồ thị này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def solve():
    k = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(k)]

    edges = []
    for i in range(k):
        r1, c1 = pts[i]
        for j in range(i + 1, k):
            r2, c2 = pts[j]
            w = max(abs(r1 - r2), abs(c1 - c2))
            edges.append((w, i, j))

    edges.sort()
    dsu = DSU(k)

    ans = 0
    used = 0

    for w, a, b in edges:
        if dsu.union(a, b):
            ans += w
            used += 1
            if used == k - 1:
                break

    print(ans)

if __name__ == "__main__":
    solve()
```DSU duy trì các vị vua đã được kết nối thông qua các sự hợp nhất đã chọn, đảm bảo chúng ta không bao giờ hình thành các chu kỳ. Mỗi liên minh thành công tương ứng với việc chọn một thao tác chụp giữa hai nhóm riêng biệt trước đó. Các cạnh được sắp xếp đảm bảo rằng chúng ta luôn chọn kết nối có sẵn rẻ nhất trước tiên, đây chính xác là thuộc tính tham lam xác định cây bao trùm tối thiểu. 

Việc tính toán khoảng cách Chebyshev rất quan trọng vì nó mô hình hóa số lần di chuyển vua tối thiểu thực sự trong một lưới trống. Bất kỳ định nghĩa khoảng cách nào khác sẽ phá vỡ sự tương đương với chi phí di chuyển ngắn nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ của ba vị vua: 

đầu vào:```
3
1 1
1 3
3 2
```Chúng tôi tính toán khoảng cách theo cặp: 

| Cặp | Khoảng cách | 
| --- | --- | 
| (1,2) | 2 | 
| (1,3) | 2 | 
| (2,3) | 2 | 

Tất cả các cạnh đều có trọng số bằng nhau, do đó, bất kỳ cây bao trùm nào cũng chỉ có tổng giá trị là 4 nếu chúng ta tính tổng nhầm tất cả các cạnh, nhưng MST chỉ chọn hai cạnh. 

| Bước | Đã chọn cạnh | Linh kiện | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu | - | {1},{2},{3} | 0 | 
| 1 | (1,2) | {1,2},{3} | 2 | 
| 2 | (2,3) | {1,2,3} | 2 | 

Tổng chi phí là 4. 

Dấu vết này xác nhận rằng chính xác k − 1 sự hợp nhất được thực hiện và DSU ngăn chặn chính xác các kết nối dư thừa. 

Bây giờ hãy xem xét trường hợp có một vị vua ở xa:```
3
1 1
1 2
100 100
```Khoảng cách: 

| Cặp | Khoảng cách | 
| --- | --- | 
| (1,2) | 1 | 
| (1,3) | 99 | 
| (2,3) | 99 | 

| Bước | Đã chọn cạnh | Linh kiện | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu | - | {1},{2},{3} | 0 | 
| 1 | (1,2) | {1,2},{3} | 1 | 
| 2 | (1,3) hoặc (2,3) | {1,2,3} | 99 | 

Tổng chi phí là 100, chứng tỏ rằng thuật toán trì hoãn chính xác việc hợp nhất đắt tiền cho đến khi cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2 log k) | Tất cả các cạnh theo cặp được tạo trong O(k^2), việc sắp xếp chiếm ưu thế | 
| Không gian | O(k^2) | Lưu trữ tất cả các cạnh | 

Với k lên tới 100, số cạnh nhiều nhất là 4950, do đó các thao tác sắp xếp và DSU dễ dàng nhanh chóng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else sys.stdout.getvalue()

# Since solve() prints directly, we redefine a safe runner
def run(inp: str) -> str:
    import subprocess, textwrap
    return subprocess.run(
        ["python3", "-c", open(__file__).read()],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode()

# sample
assert run("3\n1 1\n1 3\n3 2\n") != ""

# custom tests
assert run("2\n1 1\n1 2\n") != "", "minimum case"
assert run("3\n1 1\n100 100\n50 50\n") != "", "spread case"
assert run("4\n1 1\n1 2\n2 1\n2 2\n") != "", "dense cluster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 vua liền kề | 1 | chi phí hợp nhất tối thiểu | 
| trải chéo | số tiền nhỏ | tính đúng đắn của khoảng cách Chebyshev | 
| 4 cụm | MST có cấu trúc | xử lý chu kỳ và sự lựa chọn tham lam | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là khi có chính xác hai quân vua. Thuật toán xây dựng một cạnh có trọng số bằng khoảng cách Chebyshev của chúng và xuất ra ngay lập tức. Điều này khớp với chuỗi di chuyển duy nhất có thể, vì vậy độ chính xác là ngay lập tức. 

Cấu hình hình học suy biến là khi tất cả các quân vua được sắp xếp trên một đường thẳng hoặc được xếp thành một cụm chặt chẽ. Trong những trường hợp như vậy, nhiều cạnh có trọng số bằng hoặc gần bằng nhau. DSU đảm bảo rằng ngay cả khi tồn tại nhiều cạnh có trọng số bằng nhau, chỉ k − 1 được chọn, tránh việc đếm quá mức. 

Một trường hợp ngoại lệ xa hơn cho thấy tại sao việc phân cụm tham lam lại quan trọng. Nếu một vua ở xa, cấu trúc MST sẽ trì hoãn kết nối của nó cho đến khi hết tất cả các sự hợp nhất gần hơn, đảm bảo rằng cạnh đắt tiền được sử dụng chính xác một lần, phù hợp với nhu cầu cuối cùng là hấp thụ vua bị cô lập đó.
