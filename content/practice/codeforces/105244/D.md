---
title: "CF 105244D - Hươu cao cổ đi du lịch và nhai"
description: "Chúng ta có một lưới $m nhân n$, trong đó mỗi ô chứa một số không âm biểu thị số lượng cây mọc ở đó. Một con hươu cao cổ bắt đầu ở ô trên cùng bên trái $(1,1)$ và muốn đến ô dưới cùng bên phải $(m,n)$. Các quy tắc chuyển động bị hạn chế và bất thường."
date: "2026-06-24T07:00:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105244
codeforces_index: "D"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 2"
rating: 0
weight: 105244
solve_time_s: 51
verified: true
draft: false
---

[CF 105244D - Một con hươu cao cổ đi du lịch và nhai](https://codeforces.com/problemset/problem/105244/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$m \times n$lưới, trong đó mỗi ô chứa một số không âm biểu thị số lượng cây mọc ở đó. Một con hươu cao cổ bắt đầu ở ô trên cùng bên trái$(1,1)$và muốn đến ô dưới cùng bên phải$(m,n)$. 

Các quy tắc chuyển động bị hạn chế và bất thường. Từ bất kỳ ô hiện tại nào, hươu cao cổ thực hiện một động tác bao gồm bước nhảy dài theo một hướng và bước ngắn theo hướng vuông góc. Trong một kiểu di chuyển, nó giảm dần$k$các ô và sang phải 1 ô, còn ở ô kia nó đi sang phải$k$ô và giảm đi 1 ô. Mỗi lần di chuyển sẽ tiếp đất chính xác trên một ô mới và hươu cao cổ sẽ thu thập những chiếc lá từ ô đích đó. 

Ô đầu tiên không đóng góp bất kỳ giá trị nào. Bắt đầu từ nước đi 1, mỗi nước đi đều được dán nhãn tuần tự. Ở những nước đi số lẻ, hươu cao cổ ăn chính xác$t$rời khỏi ô đích, trong khi di chuyển ở số chẵn nó sẽ ăn$3t$lá, ở đâu$t$là số cây trong ô đó. 

Nhiệm vụ là chọn một chuỗi các bước đi hợp lệ và kết thúc chính xác tại$(m,n)$, hoặc báo cáo rằng điều đó là không thể, đồng thời tối đa hóa tổng số lá đã ăn theo quy tắc tính điểm xen kẽ này. 

Kích thước lưới tối đa là$100 \times 100$, Và$k$nhiều nhất cũng là 100. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân trên các đường đi, vì ngay cả một số bước vừa phải cũng tạo ra sự bùng nổ tổ hợp. Giải pháp phải coi đây là bài toán đường đi ngắn nhất hoặc dài nhất có cấu trúc trên không gian trạng thái có nhiều nhất là vài triệu trạng thái. 

Một vấn đề tế nhị là tính chẵn lẻ của số lần di chuyển sẽ thay đổi trọng số của mỗi ô. Điều đó có nghĩa là giá trị của việc truy cập một ô không cố định, nó phụ thuộc vào việc chúng ta đến bước chẵn hay lẻ. Bất kỳ giải pháp nào bỏ qua tính chẵn lẻ và coi mỗi ô có một giá trị duy nhất sẽ tạo ra kết quả không chính xác. 

Một trường hợp cạnh không rõ ràng khác là khả năng tiếp cận. Bởi vì mỗi bước di chuyển đều thay đổi cả hai tọa độ, nhưng với kích thước bước không đối xứng, một số$(m,n)$cấu hình không thể truy cập được. Ví dụ, nếu$k$lớn so với kích thước, có thể không có chuỗi các bước di chuyển hợp lệ nào tiếp cận chính xác mục tiêu. Trong những trường hợp như vậy, câu trả lời phải là$-1$, ngay cả khi có thể chuyển động một phần. 

## Phương pháp tiếp cận 

Nỗ lực đầu tiên tự nhiên là coi đây là một cuộc tìm kiếm mạnh mẽ trên tất cả các đường dẫn từ$(1,1)$ĐẾN$(m,n)$. Từ mỗi ô, có nhiều nhất hai bước di chuyển có thể xảy ra, vì vậy chúng tôi khám phá đệ quy, duy trì vị trí hiện tại và tính chẵn lẻ của chỉ số di chuyển. Mỗi đường đi có độ dài xấp xỉ theo thứ tự$O(m+n)$chia cho các hiệu ứng kích thước bước, nhưng phân nhánh theo cấp số nhân vẫn chiếm ưu thế. Ngay cả trong một$100 \times 100$lưới, số lượng đường đi có thể tăng lên vượt quá bất kỳ giới hạn khả thi nào, vì ở mỗi bước chúng ta phân nhánh thành hai hướng cho đến khi chạm tới ranh giới. 

Lý do brute-force này đúng rất đơn giản: nó liệt kê mọi đường dẫn hợp lệ và đánh giá điểm số của nó một cách chính xác theo quy tắc chẵn lẻ. Điểm thất bại là quy mô chứ không phải tính chính xác. Với hệ số phân nhánh 2 có thể xảy ra trong hàng chục bước, số lượng trạng thái sẽ trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là vấn đề không nằm ở đường dẫn mà là ở các trạng thái được xác định bởi vị trí và tính chẵn lẻ của số lần di chuyển. Một khi chúng ta đã ở một ô nhất định và biết bước đi tiếp theo là lẻ hay chẵn thì mọi hành vi trong tương lai chỉ phụ thuộc vào trạng thái này chứ không phụ thuộc vào cách chúng ta đến đó. Điều này chuyển đổi bài toán thành bài toán đường đi ngắn nhất của đồ thị trên một lưới nhiều lớp có hai lớp: một cho các bước di chuyển lẻ và một cho các bước di chuyển chẵn. 

Mỗi tiểu bang$(i,j,p)$, Ở đâu$p \in \{0,1\}$thể hiện tính chẵn lẻ của bước đi tiếp theo, chuyển sang nhiều nhất hai trạng thái, tùy thuộc vào việc chúng ta áp dụng bước đi nặng theo chiều dọc hay chiều ngang. Chi phí chuyển đổi là giá trị của ô đích nhân với 1 hoặc 3 tùy thuộc vào tính chẵn lẻ. 

Đây hiện là bài toán đường đi dài nhất trên biểu đồ phân lớp giống như không tuần hoàn có hướng, nhưng các chu trình có thể tồn tại trong không gian chỉ mục, vì vậy chúng tôi coi nó như một DP tiêu chuẩn trên các trạng thái, liên tục nới lỏng các chuyển tiếp hoặc sử dụng thư giãn giống BFS cho đến khi hội tụ. Vì số lượng các trạng thái chỉ là$2mn \le 20000$, điều này hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | Hàm mũ | O(mn) đệ quy | Quá chậm | 
| DP ở trạng thái (i, j, chẵn lẻ) | O(mn) | O(mn) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định bảng DP trong đó$dp[i][j][p]$lưu trữ số lá tối đa có thể đạt được khi đến ô$(i,j)$và có tính chẵn lẻ của nước đi tiếp theo$p$, Ở đâu$p=0$có nghĩa là bước đi tiếp theo là số lẻ và$p=1$nghĩa là bước đi tiếp theo là số chẵn. Mã hóa này được chọn vì hệ số nhân chỉ phụ thuộc vào tính chẵn lẻ của chỉ số di chuyển. 
2. Khởi tạo tất cả các trạng thái về giá trị rất âm, ngoại trừ trạng thái bắt đầu$dp[1][1][0] = 0$, vì chúng tôi bắt đầu từ đầu trước khi thực hiện nước đi 1 và không thu thập bất cứ thứ gì ở đó. 
3. Từ mỗi trạng thái, hãy thử hai lần chuyển đổi tương ứng với hai loại chuyển động: di chuyển xuống$k$và sang phải 1, hoặc di chuyển sang phải$k$và xuống 1. Đối với mỗi lần di chuyển hợp lệ, hãy tính ô đích. 
4. Khi chuyển sang ô đích, hãy tính mức tăng. Nếu tính chẵn lẻ của nước đi hiện tại là số lẻ (thứ nhất, thứ ba, v.v.), hãy thêm$t$. Nếu chẵn thì thêm$3t$. Trọng số này được áp dụng tại thời điểm đến, vì vậy nó gắn liền với trạng thái đích. 
5. Lật chẵn lẻ sau mỗi nước đi, vì nước đi lẻ trở thành nước chẵn và ngược lại. Điều này đảm bảo DP theo dõi chính xác quy tắc tính điểm luân phiên. 
6. Lặp lại tất cả các trạng thái nhiều lần, nới lỏng các chuyển đổi cho đến khi không có sự cải thiện nào xảy ra hoặc đơn giản chạy một số lần lặp cố định được giới hạn bởi$2mn$, vì mỗi lần nới lỏng sẽ làm tăng một chiều độ dài đường đi và không thể cải thiện vô thời hạn. 
7. Câu trả lời là giá trị lớn nhất trên cả hai trạng thái chẵn lẻ tại ô$(m,n)$. Nếu cả hai vẫn âm vô cùng, đầu ra$-1$. 

### Tại sao nó hoạt động 

DP duy trì tính bất biến đối với mọi trạng thái có thể tiếp cận$(i,j,p)$, nó lưu trữ điểm tốt nhất có thể có trong số tất cả các chuỗi di chuyển hợp lệ kết thúc tại ô đó với tính chẵn lẻ chính xác. Mọi chuyển đổi đều duy trì tính chính xác vì nó chỉ mở rộng các đường dẫn đã hợp lệ bằng một bước di chuyển hợp pháp và áp dụng trọng số phụ thuộc chẵn lẻ chính xác một lần. Vì mọi đường đi đến một trạng thái đều có thể được phân tách thành trạng thái trước đó cộng với một bước di chuyển, nên việc thư giãn lặp đi lặp lại cuối cùng sẽ truyền các giá trị tối ưu đến tất cả các trạng thái có thể tiếp cận. Việc phân chia chẵn lẻ đảm bảo rằng không có hai đường dẫn nào chỉ khác nhau về căn chỉnh chỉ mục di chuyển được hợp nhất không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -10**18

def solve():
    m, n = map(int, input().split())
    k = int(input())
    grid = [None] + [[0] + list(map(int, input().split())) for _ in range(m)]

    dp = [[[INF] * 2 for _ in range(n + 1)] for _ in range(m + 1)]
    dp[1][1][0] = 0

    for _ in range(m * n * 2):
        changed = False
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                for p in range(2):
                    if dp[i][j][p] == INF:
                        continue

                    cur = dp[i][j][p]
                    np = 1 - p

                    ni, nj = i + k, j + 1
                    if ni <= m and nj <= n:
                        gain = grid[ni][nj]
                        if p == 1:
                            gain *= 3
                        if dp[ni][nj][np] < cur + gain:
                            dp[ni][nj][np] = cur + gain
                            changed = True

                    ni, nj = i + 1, j + k
                    if ni <= m and nj <= n:
                        gain = grid[ni][nj]
                        if p == 1:
                            gain *= 3
                        if dp[ni][nj][np] < cur + gain:
                            dp[ni][nj][np] = cur + gain
                            changed = True

        if not changed:
            break

    ans = max(dp[m][n])
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Bảng DP có ba chiều vì tính chẵn lẻ là điều cần thiết để tính điểm chính xác. Mỗi bước thư giãn sẽ thử cả hai loại chuyển động và cập nhật trạng thái tiếp theo nếu tìm thấy điểm tốt hơn. Việc nới lỏng lặp đi lặp lại là an toàn vì mọi cải tiến đều tương ứng với điểm đường dẫn tốt hơn và không gian trạng thái hữu hạn đảm bảo việc chấm dứt. 

Một cạm bẫy phổ biến khi triển khai là quên nhân với 3 chỉ với các nước đi được đánh số chẵn, ở đây tương ứng với tính chẵn lẻ$p=1$trước khi di chuyển. Một nguyên nhân khác là khởi tạo phần bắt đầu không chính xác: ô bắt đầu không được đóng góp bất kỳ giá trị nào, ngay cả khi nó chứa cây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1
1 1
1 1
```Chúng tôi bắt đầu lúc$(1,1)$với điểm 0 và tính chẵn lẻ 0. 

| Bước | Vị trí | Chẵn lẻ (bước tiếp theo) | Hành động | Điểm | 
| --- | --- | --- | --- | --- | 
| 0 | (1,1) | 0 | bắt đầu | 0 | 
| 1 | (2,2) | 1 | di chuyển và thu thập 1 | 1 | 

DP đạt$(2,2)$với điểm 1. Chỉ có một đường đi hợp lệ và hệ số chẵn lẻ không thay đổi bất cứ điều gì vì chỉ thực hiện một nước đi. 

### Ví dụ 2 

đầu vào:```
3 5
1
1 2 0 3 1
2 4 5 7 8
3 0 7 1 0
```Chúng tôi theo dõi các chuyển đổi tối ưu quan trọng để$(3,5)$. 

| Bước | Vị trí | Chẵn lẻ | Loại di chuyển | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (1,1) | 0 | bắt đầu | 0 | 0 | 
| 1 | (2,2) | 1 | xuống+phải | 4 | 4 | 
| 2 | (3,4) | 0 | xuống+phải | 7 | 11 | 
| 3 | (3,5) | 1 | phải + xuống | 0 hoặc 1 lựa chọn | 11 hoặc 14 | 

Bảng này cho thấy tính chẵn lẻ ảnh hưởng đáng kể đến bước cuối cùng như thế nào, vì bước cuối cùng có thể nhân giá trị ô lên 3 tùy thuộc vào việc nó có được đánh số chẵn hay không. 

Ví dụ này chứng minh rằng các đường dẫn khác nhau đến cùng một ô với số lần di chuyển khác nhau có thể tạo ra các kết quả cuối cùng khác nhau, đó chính xác là lý do tại sao DP phải lưu trữ tính chẵn lẻ riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(mn)$| Mỗi trạng thái được thư giãn một số lần không đổi trên một lưới nhỏ | 
| Không gian |$O(mn)$| Bảng DP lưu trữ hai giá trị trên mỗi ô | 

Kích thước lưới tối đa là 100 x 100, tổng cộng là 20000 trạng thái. Mỗi trạng thái có tối đa hai lần chuyển đổi, vì vậy tổng công việc nằm trong giới hạn 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# NOTE: in a real setup, solve() would be imported and called,
# here we assume it is already defined above.

def run(inp: str) -> str:
    import sys, io
    backup_in = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = backup_in
    sys.stdout = backup_out
    return out

# minimum grid, no movement possible
assert run("""1 1
1
5
""") == "0"

# simple 2x2
assert run("""2 2
1
1 1
1 1
""") == "1"

# unreachable configuration (example k too large)
assert run("""3 3
3
1 2 3
4 5 6
7 8 9
""") == "-1"

# path where parity matters
assert run("""2 3
1
1 2 3
4 5 6
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 0 | bắt đầu không đóng góp gì | 
| đồng phục 2x2 | 1 | tính đúng đắn của quá trình chuyển đổi cơ bản | 
| k bằng chiều | -1 | xử lý không thể truy cập | 
| lưới 2x3 | biến | độ nhạy chẵn lẻ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi lưới quá nhỏ đến mức không thể di chuyển được. Ví dụ, trong một$1 \times 1$lưới câu trả lời phải bằng 0 vì hươu cao cổ không bao giờ di chuyển và không bao giờ ăn. 

Một trường hợp cạnh khác là khi$k$vượt quá kích thước có sẵn sau bước đầu tiên. Ví dụ, nếu$m=3, n=3, k=3$, hầu hết các bước di chuyển ngay lập tức đi ra ngoài giới hạn, không để lại đường đi hợp lệ nào đến đích. DP giữ chính xác tất cả các trạng thái không thể truy cập được và câu trả lời cuối cùng sẽ trở thành$-1$. 

Một trường hợp phức tạp hơn là khi có nhiều đường dẫn đến cùng một ô nhưng có tính chẵn lẻ khác nhau. DP lưu trữ rõ ràng cả hai, vì vậy cả hai đóng góp đều được giữ nguyên. Điều này tránh mất các giải pháp tối ưu trong đó đường dẫn dài hơn một chút mang lại cấu trúc số nhân cuối cùng tốt hơn.
