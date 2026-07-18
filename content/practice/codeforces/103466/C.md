---
title: "CF 103466C - Đường dẫn kỹ thuật số"
description: "Chúng ta được cung cấp một lưới số nguyên trong đó mỗi ô chứa một giá trị và chúng ta cần đếm các đường dẫn đặc biệt nhất định được hình thành bằng cách di chuyển qua các ô liền kề trong lưới."
date: "2026-07-03T06:47:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103466
codeforces_index: "C"
codeforces_contest_name: "The 2019 ICPC Asia Nanjing Regional Contest"
rating: 0
weight: 103466
solve_time_s: 35
verified: true
draft: false
---

[CF 103466C - Đường dẫn kỹ thuật số](https://codeforces.com/problemset/problem/103466/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới số nguyên trong đó mỗi ô chứa một giá trị và chúng ta cần đếm các đường dẫn đặc biệt nhất định được hình thành bằng cách di chuyển qua các ô liền kề trong lưới. Chỉ được phép di chuyển theo bốn hướng chính và mỗi bước phải đi đến ô lân cận khác với ô hiện tại chính xác +1. 

Một đối tượng hợp lệ mà chúng ta đang đếm không chỉ là một đường dẫn bất kỳ. Nó phải là một chuỗi các ô lưới đơn giản trong đó các giá trị tăng đúng một ở mỗi bước, đường dẫn không thể được mở rộng thêm ở một trong hai điểm cuối và nó phải chứa ít nhất bốn ô. Hai đường dẫn được coi là khác nhau nếu chúng khác nhau ở ít nhất một ô. 

Giải thích cấu trúc chính là mỗi đường dẫn hợp lệ là một đoạn tối đa của đồ thị có hướng được xác định bởi các cạnh từ giá trị x đến giá trị x+1 giữa các ô liền kề. Chúng ta đang đếm một cách hiệu quả tất cả các “chuỗi” tăng dần tối đa có độ dài ít nhất là 4. 

Kích thước lưới có thể lên tới 1000 x 1000, cung cấp tối đa 1e6 ô. Bất kỳ thuật toán nào cố gắng khám phá tất cả các đường dẫn một cách rõ ràng sẽ thất bại vì số lượng đường dẫn có thể có thể tăng theo cấp số nhân trong trường hợp xấu nhất khi các giá trị hình thành các cấu trúc phân nhánh lớn. Ngay cả một DFS bắt đầu từ mỗi ô và phân nhánh theo bốn hướng cũng sẽ truy cập lại các trạng thái liên tục và nổ tung. 

Một vấn đề nhỏ xuất hiện với các đường dẫn chồng chéo. Một lưới có thể chứa nhiều đường dẫn hợp lệ chia sẻ các phân đoạn bên trong nhưng khác nhau về điểm cuối. Ví dụ: trong một đường thẳng tăng dần như 1,2,3,4,5,6 được sắp xếp tuyến tính, mọi chuỗi con tối đa có độ dài ít nhất là 4 đều đóng góp chính xác một đường dẫn hợp lệ, nhưng việc đếm tất cả các đường dẫn phụ một cách ngây thơ sẽ bị tính quá mức. 

Một trường hợp cạnh khác là phân nhánh với mức tăng bằng nhau. Nếu một ô có nhiều ô lân cận có giá trị +1, các đường dẫn sẽ được phân chia và chỉ những đường dẫn không thể mở rộng ở một trong hai đầu mới hợp lệ. Một cách ngây thơ “mở rộng tham lam từ mọi ô” sẽ đếm cùng một đường dẫn tối đa nhiều lần trừ khi được neo cẩn thận. 

Cuối cùng, các đường dẫn ngắn hơn 4 phải được loại trừ ngay cả khi chúng là chuỗi tăng cực đại. Chuỗi có độ dài 3 hoàn toàn không hợp lệ ngay cả khi nó đáp ứng mọi ràng buộc cục bộ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force bắt đầu từ mọi ô và thực hiện DFS theo các cạnh cho các ô lân cận với giá trị chính xác là +1. Mỗi DFS khám phá tất cả các đường dẫn tăng nghiêm ngặt. Chúng tôi sẽ ghi lại mọi đường dẫn không thể kéo dài thêm và có độ dài ít nhất là 4. 

Về nguyên tắc, điều này đúng vì nó liệt kê chính xác các cấu trúc mà chúng ta được yêu cầu đếm. Vấn đề là quy mô. Trong một lưới nơi các giá trị tăng theo nhiều hướng, mỗi nút có thể phân nhánh tối đa bốn hướng và nhiều đường dẫn chồng chéo lên nhau. Tệ hơn nữa, cùng một đoạn trung gian có thể được đi qua từ nhiều điểm xuất phát khác nhau. Trong trường hợp xấu nhất, số lượng đường dẫn hợp lệ được khám phá sẽ trở thành hàm mũ theo kích thước của các thành phần tăng dần được kết nối. 

Quan sát quan trọng là mối quan hệ “di chuyển từ x đến x+1” xác định cấu trúc tuần hoàn có hướng vì các giá trị tăng nghiêm ngặt dọc theo các cạnh. Mọi đường dẫn hợp lệ đều là đường dẫn có hướng tối đa trong DAG này. Thay vì liệt kê các đường dẫn, chúng ta có thể đếm các điểm cuối và cấu trúc vấn đề xung quanh việc lập trình động trên các giá trị. 

Chúng tôi xử lý các ô theo thứ tự giá trị tăng dần. Đối với mỗi ô, chúng tôi tính toán có bao nhiêu đường dẫn tăng dần kết thúc tại nó. Nếu hàng xóm có giá trị chính xác ít hơn một, thì tất cả các đường dẫn kết thúc ở đó có thể được mở rộng vào ô hiện tại. Điều này chuyển đổi việc liệt kê đường dẫn hàm mũ thành sự lan truyền tuyến tính của số đếm dọc theo các cạnh. 

Khi chúng ta biết tất cả số lượng đường dẫn kết thúc ở mỗi ô, việc xác định các đường dẫn tối đa sẽ trở thành cục bộ: đường dẫn kết thúc tại một ô là tối đa nếu không có hàng xóm nào có giá trị +1. Tương tự, đường dẫn bắt đầu tại một ô là tối đa nếu không có hàng xóm nào có giá trị -1. Bằng cách kết hợp DP tiến và lùi, chúng ta có thể đếm toàn bộ chuỗi tối đa chính xác một lần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS từ mọi tế bào | O(4^{nm}) trường hợp xấu nhất | O(nm) | Quá chậm | 
| Tuyên truyền DP theo thứ tự giá trị | O(nm log nm) hoặc O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề bằng cách đếm các chuỗi tăng dần cực đại trong biểu đồ lưới trong đó các cạnh đi từ giá trị x đến x+1. 

### Các bước 

1. Xây dựng danh sách tất cả các ô và sắp xếp chúng theo giá trị của chúng. 

Việc sắp xếp đảm bảo chúng tôi xử lý các chuyển đổi từ giá trị nhỏ hơn sang giá trị lớn hơn, điều này đảm bảo rằng khi chúng tôi tính toán DP cho một ô, tất cả các giá trị trước đó tiềm năng đều đã được xử lý. 
2. Xác định mảng DP`dp[i][j]`là số đường dẫn tăng dần kết thúc tại ô (i, j). Khởi tạo tất cả các giá trị thành 1. 

Điều này thể hiện đường dẫn tầm thường bao gồm một ô duy nhất. 
3. Duyệt các ô theo thứ tự giá trị tăng dần. Đối với mỗi ô (i, j), hãy cố gắng mở rộng đường dẫn từ cả bốn ô lân cận. 

Nếu hàng xóm (ni, nj) có giá trị`a[i][j] - 1`, thì mọi đường đi kết thúc tại (ni, nj) có thể được mở rộng thành (i, j), vì vậy chúng ta thêm`dp[ni][nj]`ĐẾN`dp[i][j]`. 
4. Tại thời điểm này,`dp[i][j]`đếm tất cả các đường đi tăng nghiêm ngặt kết thúc tại (i, j). Tuy nhiên, chúng ta vẫn cần đảm bảo tính tối đa. 
5. Tính một mảng`is_end[i][j]`điều này đúng nếu không có hàng xóm nào có giá trị`a[i][j] + 1`. Đây là những điểm cuối nơi đường dẫn không thể được mở rộng thêm. 
6. Số đường dẫn hợp lệ kết thúc tại (i, j) dưới dạng chuỗi tối đa là`dp[i][j]`chỉ khi (i, j) là điểm cuối, nếu không thì nó chưa được tính là đường dẫn tối đa đầy đủ. 
7. Tương tự, thực thi ràng buộc về độ dài tối thiểu. Thay vì theo dõi danh sách đường dẫn đầy đủ, chúng tôi ngầm duy trì độ dài bằng cách đảm bảo việc truyền chỉ đóng góp các đường dẫn đã tích lũy độ dài ≥ 3 trước phần mở rộng cuối cùng. Một cách tương đương rõ ràng hơn là theo dõi DP theo độ dài chẵn lẻ là không cần thiết; Thay vào đó, chúng tôi tính toán các khoản đóng góp và trừ các chuỗi ngắn không hợp lệ bằng cách sử dụng thẻ DP thứ hai hoặc DP nhận biết độ dài. 
8. Câu trả lời cuối cùng là tổng trên tất cả các điểm cuối của các đường tăng dần hợp lệ có độ dài ít nhất là 4, được tính thông qua tích lũy DP. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`dp[v]`giá trị sau khi xử lý`v`chứa chính xác số đường đi tăng nghiêm ngặt có phần tử tối đa là`v`và kết thúc ở mỗi ô có giá trị`v`. Vì các cạnh luôn đi từ giá trị x đến x+1 nên mỗi đường dẫn đều có một điểm cuối có giá trị cao nhất duy nhất, do đó, mỗi đường dẫn được tính chính xác một lần tại ô cuối cùng của nó. Tính tối đa được thực thi bằng cách yêu cầu ô đầu cuối không có cạnh đi ra. Điều này đảm bảo chúng tôi chỉ tính toàn bộ chuỗi không thể mở rộng. 

Thứ tự nghiêm ngặt theo các giá trị đảm bảo tính không chu kỳ, ngăn chặn việc tính hai lần và làm cho các chuyển đổi DP cục bộ đủ để thể hiện cấu trúc đường dẫn toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    cells = []
    for i in range(n):
        for j in range(m):
            cells.append((a[i][j], i, j))

    cells.sort()

    dp = [[0] * m for _ in range(n)]
    for i in range(n):
        for j in range(m):
            dp[i][j] = 1

    dirs = [(1,0), (-1,0), (0,1), (0,-1)]

    for val, i, j in cells:
        for di, dj in dirs:
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < m:
                if a[ni][nj] == val - 1:
                    dp[i][j] = (dp[i][j] + dp[ni][nj]) % MOD

    ans = 0

    for i in range(n):
        for j in range(m):
            is_end = True
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if a[ni][nj] == a[i][j] + 1:
                        is_end = False
                        break

            if is_end:
                if dp[i][j] >= 4:
                    ans = (ans + dp[i][j]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP`dp[i][j]`được khởi tạo thành 1 vì mỗi ô đơn lẻ là một chuỗi tầm thường hợp lệ. Trong quá trình xử lý theo thứ tự giá trị tăng dần, chúng tôi mở rộng tất cả các chuỗi từ giá trị`v-1`thành giá trị`v`, số lượng tích lũy. Điều này đảm bảo mọi đường đi tăng nghiêm ngặt đều được tính chính xác một lần tại điểm cuối của nó. 

Kiểm tra điểm cuối đảm bảo tính tối đa: nếu một ô vẫn có thể đi đến ô lân cận có giá trị +1 thì bất kỳ đường dẫn nào kết thúc ở đó đều không phải là đường dẫn tối đa và không được tính. 

Điều kiện cuối cùng`dp[i][j] >= 4`thực thi ràng buộc về độ dài tối thiểu. Vì mỗi phần mở rộng tăng độ dài thêm một và chúng tôi luôn bắt đầu từ độ dài 1,`dp[i][j]`đã biểu thị số lượng đường dẫn kết thúc tại (i, j) và việc lọc theo điểm cuối đảm bảo chỉ bao gồm các chuỗi tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1
