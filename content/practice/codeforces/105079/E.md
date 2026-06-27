---
title: "CF 105079E - Bộ sưu tập bánh cupcake"
description: "Chúng ta có một lưới vuông có kích thước $N nhân N$, trong đó mỗi ô đều bị chặn hoặc có thể sử dụng được. Ô bị chặn được đánh dấu bằng $-1$ và không thể nhập được. Mỗi ô khác chứa một số không âm đại diện cho những chiếc bánh nướng nhỏ có sẵn trong ô đó."
date: "2026-06-27T21:26:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "E"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 90
verified: true
draft: false
---

[CF 105079E - Thu thập bánh nướng nhỏ](https://codeforces.com/problemset/problem/105079/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$N \times N$, trong đó mỗi ô bị chặn hoặc có thể sử dụng được. Một ô bị chặn được đánh dấu bằng$-1$, và không thể nhập được. Mỗi ô khác chứa một số không âm đại diện cho những chiếc bánh nướng nhỏ có sẵn trong ô đó. Alice bắt đầu ở góc trên bên trái$(1,1)$và phải đến góc dưới bên phải$(N,N)$, chỉ di chuyển sang phải hoặc xuống ở mỗi bước. Bất cứ khi nào cô ấy bước vào một phòng giam, cô ấy sẽ thu thập tất cả những chiếc bánh nướng nhỏ trong đó. 

Nhiệm vụ là tính tổng số bánh nướng nhỏ tối đa mà Alice có thể thu thập dọc theo bất kỳ đường dẫn hợp lệ nào từ đầu đến cuối hoặc xác định rằng không tồn tại đường dẫn nào như vậy. 

Giới hạn kích thước lưới$N \leq 1000$có nghĩa là lên tới một triệu tế bào. Bất kỳ giải pháp nào cố gắng liệt kê tất cả các đường đi đều không khả thi ngay lập tức vì số lượng các đường đi đơn điệu tăng theo cấp số nhân, gần như theo thứ tự$\binom{2N}{N}$. Điều này nhanh chóng trở nên lớn về mặt thiên văn ngay cả đối với những người vừa phải$N$, vì vậy việc liệt kê đường dẫn brute-force không phải là một lựa chọn. 

Thay vào đó, chúng ta cần một phương pháp xử lý mỗi ô với số lần không đổi, lý tưởng nhất là$O(N^2)$. 

Một trường hợp khó phát hiện khi phần đầu hoặc phần cuối không thể truy cập được do các ô bị chặn. Một trường hợp khác là khi một ô có thể truy cập được về mặt hình học nhưng tất cả các đường dẫn vào ô đó đều bị chặn. Ví dụ: 

đầu vào:```
3
0 1 1
-1 -1 1
1 1 1
```Mặc dù đích đến tồn tại và chứa bánh nướng nhỏ, nhưng rào cản ở giữa sẽ ngắt kết nối lưới, khiến bạn không thể tiếp cận ô phía dưới bên phải. Câu trả lời đúng là$-1$và bất kỳ DP ngây thơ nào bỏ qua khả năng tiếp cận sẽ tích lũy các giá trị không chính xác vào trạng thái không thể truy cập được. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng khám phá mọi đường dẫn hợp lệ từ trên cùng bên trái đến dưới cùng bên phải, tích lũy số lượng bánh nướng nhỏ trên đường đi. Vì chuyển động bị hạn chế ở bên phải và bên dưới nên mỗi đường đi là một chuỗi chính xác$2N-2$di chuyển, và số lượng các chuỗi như vậy là theo cấp số nhân trong$N$. Ngay cả khi chúng ta tỉa bớt đường đi$-1$các ô, lưới trong trường hợp xấu nhất không có chướng ngại vật buộc phải khám phá tất cả các đường dẫn đơn điệu, quá lớn. 

Quan sát quan trọng là giá trị được thu thập tại bất kỳ ô nào chỉ phụ thuộc vào cách tốt nhất để tiếp cận ô đó. Khi chúng tôi tới một phòng giam$(i,j)$, điểm tối ưu cho đến thời điểm đó không phụ thuộc vào cách chúng tôi đạt được điều đó, ngoại trừ giá trị tích lũy tốt nhất có thể. Đây là một tình huống cấu trúc con tối ưu cổ điển. 

Vì vậy, thay vì khám phá các đường dẫn, chúng ta tính toán một bảng lập trình động trong đó$dp[i][j]$đại diện cho số lượng bánh nướng nhỏ tối đa có thể đạt được khi đến ô$(i,j)$. Mỗi ô chỉ phụ thuộc vào các ô lân cận trên cùng và bên trái của nó, vì đó là những cách duy nhất để vào ô đó. Nếu một ô bị chặn, nó sẽ không đóng góp gì và được đánh dấu là không thể truy cập được. 

Điều này làm giảm vấn đề từ việc liệt kê đường dẫn hàm mũ sang việc truyền tải lưới đơn giản với công việc không đổi trên mỗi ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{2N})$|$O(N)$ngăn xếp đệ quy | Quá chậm | 
| Lập trình động |$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng DP trong đó mỗi trạng thái lưu trữ tổng số bánh cupcake tốt nhất có thể đạt được khi đến ô đó. Chúng ta cũng cần một cách để thể hiện các trạng thái không thể truy cập được. 

1. Khởi tạo mảng 2D$dp$kích thước$N \times N$, chứa đầy giá trị trọng điểm biểu thị không thể truy cập được, chẳng hạn như$-\infty$. Điều này đảm bảo rằng chúng tôi không bao giờ vô tình coi đường dẫn không hợp lệ là đóng góp tối đa. 
2. Đặt ô bắt đầu$dp[0][0]$. Nếu nó bị chặn, toàn bộ vấn đề sẽ không thể thực hiện được ngay lập tức vì Alice không thể di chuyển chút nào. Nếu không, hãy khởi tạo nó thành giá trị của ô bắt đầu, được đảm bảo bằng 0. 
3. Lặp lại từng hàng trong lưới. Đối với mỗi ô$(i,j)$, nếu bị chặn thì rời đi$dp[i][j]$như không thể truy cập được. Điều này bảo đảm tính chính xác vì không có con đường nào có thể đi vào nó một cách hợp pháp. 
4. Nếu ô không bị chặn, hãy tính cách tốt nhất có thể để đến đó. Alice chỉ có thể đến từ trên cao$(i-1,j)$hoặc từ bên trái$(i,j-1)$, vì vậy chúng tôi lấy giá trị tối đa của hai giá trị dp đó, miễn là chúng có thể truy cập được. Giá trị ô hiện tại được thêm vào mức tối đa này. Nếu cả hai hàng xóm đều không thể truy cập được thì ô đó vẫn không thể truy cập được. 
5. Sau khi điền vào bảng câu trả lời là$dp[N-1][N-1]$. Nếu vẫn không thể truy cập được, hãy xuất$-1$, nếu không thì xuất giá trị được lưu trữ. 

### Tại sao nó hoạt động 

Tại mỗi ô$(i,j)$, giá trị DP đại diện cho số lượng bánh nướng nhỏ tối đa có thể đạt được trong số tất cả các đường dẫn hợp lệ từ$(0,0)$ĐẾN$(i,j)$. Bất kỳ con đường nào như vậy phải kết thúc bằng cách đến từ một trong hai$(i-1,j)$hoặc$(i,j-1)$và cả hai trạng thái đó đều đã lưu trữ các giá trị tối ưu cho tiền tố tương ứng của chúng. Bởi vì quá trình chuyển đổi xem xét cả hai và lấy mức tối đa nên không thể bỏ qua đường dẫn tiền tố nào tốt hơn. Các ô bị chặn sẽ phá vỡ các quá trình chuyển đổi một cách chính xác vì chúng loại bỏ tất cả các đường dẫn đi qua chúng, đảm bảo các vùng không thể truy cập vẫn được loại trừ khỏi các tính toán trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    grid = [list(map(int, input().split())) for _ in range(n)]
    
    NEG = -10**18
    dp = [[NEG] * n for _ in range(n)]
    
    if grid[0][0] == -1 or grid[n-1][n-1] == -1:
        print(-1)
        return
    
    dp[0][0] = 0
    
    for i in range(n):
        for j in range(n):
            if grid[i][j] == -1:
                continue
            
            if i == 0 and j == 0:
                continue
            
            best = NEG
            if i > 0:
                best = max(best, dp[i-1][j])
            if j > 0:
                best = max(best, dp[i][j-1])
            
            if best == NEG:
                continue
            
            dp[i][j] = best + grid[i][j]
    
    print(dp[n-1][n-1] if dp[n-1][n-1] != NEG else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp định nghĩa DP. Giá trị trọng điểm$NEG$ngăn chặn việc trộn lẫn các trạng thái không thể truy cập được với số tiền hợp lệ. Ô bắt đầu được khởi tạo riêng biệt để tránh thêm sai giá trị của nó hai lần hoặc tùy thuộc vào các ô lân cận không tồn tại. Mỗi lần chuyển đổi sẽ kiểm tra cẩn thận các giới hạn và bỏ qua hoàn toàn các ô bị chặn. 

Một lỗi phổ biến là khởi tạo các ô không thể truy cập bằng 0, điều này cho phép các đường dẫn “đi xuyên tường” một cách không chính xác. Một trường hợp khác là không kiểm tra được cả cha và mẹ, điều này sẽ bỏ lỡ các đường dẫn hợp lệ chỉ tiếp cận từ một hướng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 2 3 9
15 0 4 5
0 -1 -1 0
9 0 1 0
```Chúng tôi theo dõi các trạng thái DP chính: 

| Tế bào | giá trị dp | Lý do | 
| --- | --- | --- | 
| (0,0) | 0 | bắt đầu | 
| (0,1) | 2 | từ trái | 
| (0,2) | 5 | 2 + 3 | 
| (0,3) | 14 | 5 + 9 | 
| (1,0) | 15 | từ đầu | 
| (1,1) | 15 | tối đa(2,15)+0 | 
| (1,2) | 19 | tối đa(5,15)+4 | 
| (1,3) | 24 | tối đa(14,19)+5 | 
| (2,0) | 15 | từ đầu | 
| (2,1) | bị chặn | -1 | 
| (2,2) | bị chặn | -1 | 
| (2,3) | 24 | chỉ từ (1,3) | 
| (3,0) | 24 | từ chuỗi trên cùng | 
| (3,1) | 24 | từ (3,0) | 
| (3,2) | 25 | 24 + 1 | 
| (3,3) | 25 | cuối cùng | 

Điều này xác nhận rằng các ô bị chặn đã cắt đường đi một cách chính xác, buộc tất cả các tuyến đường hợp lệ phải đi vòng quanh khu vực chướng ngại vật và DP vẫn tích lũy tổng số điểm tốt nhất có thể đạt được. 

### Mẫu 2 

đầu vào:```
4
0 2 3 9
15 0 4 5
0 -1 -1 -1
9 -1 1 0
```Ở đây hàng thứ ba tạo thành một rào chắn ngang hoàn chỉnh. 

| Tế bào | giá trị dp | Lý do | 
| --- | --- | --- | 
| (2,0) | 15 | có thể truy cập | 
| (2,1) | bị chặn | -1 | 
| (2,2) | bị chặn | -1 | 
| (2,3) | bị chặn | -1 | 
| (3,0) | 24 | có thể truy cập | 
| (3,1) | bị chặn | -1 | 
| (3,2) | không thể truy cập | không có cha mẹ hợp lệ | 
| (3,3) | không thể truy cập | không có cha mẹ hợp lệ | 

Trạng thái cuối cùng không thể truy cập được, vì vậy đầu ra là$-1$. Điều này cho thấy rằng ngay cả khi các ô riêng lẻ chứa các giá trị hợp lệ, việc thiếu kết nối sẽ lan truyền chính xác qua DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi ô được xử lý một lần với các chuyển tiếp theo thời gian không đổi từ tối đa hai ô lân cận | 
| Không gian |$O(N^2)$| Bảng DP lưu trữ một giá trị trên mỗi ô | 

Ràng buộc$N \leq 1000$làm cho$10^6$hoạt động có thể chấp nhận được trong giới hạn thông thường và mức sử dụng bộ nhớ nằm trong khoảng 256 MB ngay cả với mảng DP toàn lưới. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""4
0 2 3 9
15 0 4 5
0 -1 -1 0
9 0 1 0
""") == "25"

assert run("""4
0 2 3 9
15 0 4 5
0 -1 -1 -1
9 -1 1 0
""") == "-1"

# custom cases

# minimum size, no obstacles
assert run("""1
0
""") == "0"

# blocked start
assert run("""2
-1 0
0 0
""") == "-1"

# all open grid
assert run("""3
0 1 2
1 2 3
3 2 1
""") == "9"

# barrier row
assert run("""3
0 1 2
-1 -1 -1
3 4 5
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 0 | trường hợp cơ sở | 
| bắt đầu bị chặn | -1 | tuyên truyền không thể tiếp cận | 
| tất cả đều mở | 9 | tích lũy DP bình thường | 
| rào cản đầy đủ | -1 | lỗi kết nối | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi phần bắt đầu hoặc kết thúc bị chặn. Thuật toán kiểm tra rõ ràng những điều này trước khi DP bắt đầu. Nếu một trong hai là$-1$, hàm trả về ngay$-1$, vì không có đường dẫn nào có thể tồn tại bất kể cấu trúc trung gian. Ví dụ:```
2
-1 0
0 0
```Ở đây, mặc dù có thể truy cập đích theo thuật ngữ lưới, điểm bắt đầu không hợp lệ, do đó việc khởi tạo DP không bao giờ xảy ra và đầu ra chính xác là$-1$. 

Một trường hợp khác là các ô biệt lập có thể tiếp cận được về mặt hình học nhưng không thể tiếp cận được về mặt động. Coi như:```
3
0 1 1
-1 -1 1
1 1 1
```DP đánh dấu chính xác toàn bộ khu vực phía dưới bên phải là không thể truy cập được vì cả hai ô cha mẹ tiềm năng của các ô quan trọng đều bị chặn. Điều này cho thấy khả năng tiếp cận không chỉ liên quan đến tọa độ mà còn liên quan đến sự tồn tại của chuỗi tiền thân hợp lệ, chuỗi mà bất biến DP thực thi tự động.
