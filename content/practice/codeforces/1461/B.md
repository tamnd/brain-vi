---
title: "CF 1461B - Tìm cây vân sam"
description: "Chúng ta được cung cấp một lưới gồm hai loại ô: ô chứa đầy được đánh dấu bằng và ô trống được đánh dấu bằng .. Bên trong lưới này, chúng ta muốn đếm xem có bao nhiêu “hình dạng cây vân sam” hợp lệ tồn tại."
date: "2026-06-11T02:21:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1461
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 689 (Div. 2, based on Zed Code Competition)"
rating: 1400
weight: 1461
solve_time_s: 191
verified: false
draft: false
---

[CF 1461B - Tìm cây vân sam](https://codeforces.com/problemset/problem/1461/B) 

**Đánh giá:** 1400 
**Tags:** vũ lực, dp, thực hiện 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới gồm hai loại ô: các ô chứa đầy được đánh dấu bằng`*`và các ô trống được đánh dấu bằng`.`. Bên trong lưới này, chúng tôi muốn đếm xem có bao nhiêu “hình dạng vân sam” hợp lệ. Cây vân sam luôn là một hình tam giác phát triển hướng xuống: nó có một ô trên cùng, sau đó là một hàng rộng hơn ngay bên dưới, rồi một hàng thậm chí còn rộng hơn bên dưới, v.v. Mỗi hàng tiếp theo mở rộng chính xác một ô ở bên trái và một ô ở bên phải và mọi ô bên trong hình tam giác này phải chứa`*`. 

Chính xác hơn, nếu chúng ta chọn một ô làm đỉnh của cây vân sam, chúng ta có thể cố gắng kéo dài từng hàng xuống dưới. Ở độ sâu`i`, cây vân sam đòi hỏi một đoạn chiều dài nằm ngang liên tục`2i - 1`tập trung dưới gốc và tất cả các ô này phải được`*`. Mỗi tiền tố hợp lệ của cấu trúc như vậy được tính là một cây vân sam riêng biệt, do đó, một vị trí có thể đóng góp nhiều cây vân sam có chiều cao khác nhau. 

Nhiệm vụ là tính tổng số cây vân sam hợp lệ trên tất cả các vị trí trên cùng có thể có trong lưới, tính tổng theo tất cả các độ cao. 

Các ràng buộc cho phép tổng số lên tới 500 x 500 ô trên mỗi lô thử nghiệm, điều này cho thấy rằng giải pháp O(nm√) hoặc O(nm²) có thể được chấp nhận, nhưng phải tránh bất kỳ kích thước vượt quá khối nào. Nỗ lực trực tiếp để mở rộng một tam giác từ từng ô theo từng bước có nguy cơ lặp lại nhiều bước kiểm tra chồng chéo. 

Một trường hợp cạnh tinh tế phát sinh khi một ô`*`nhưng không thể kéo dài đến độ cao 2 do ranh giới hoặc thiếu sao. Một cách tiếp cận đơn giản chỉ kiểm tra xem mỗi hàng có hợp lệ độc lập hay không có thể đếm không chính xác các hình dạng không liền kề. 

Ví dụ, hãy xem xét:```
.*.
***
```Cấu trúc đáy ở giữa chỉ cho phép những cây vân sam nhỏ. Hàng trên cùng`*`không tạo thành một hình tam giác lớn dù hàng thứ hai đã đầy. Bất kỳ cách tiếp cận nào không thực thi các ràng buộc mở rộng đối xứng sẽ bị tính quá mức. 

Một trường hợp cạnh khác là lưới được lấp đầy. Trong những trường hợp như vậy, câu trả lời tăng lên nhanh chóng vì mọi ô đều trở thành một nghiệm hợp lệ cho nhiều độ cao, vì vậy thuật toán phải tích lũy các đóng góp một cách hiệu quả thay vì tính toán lại từng tam giác một cách rõ ràng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực bắt đầu từ mọi tế bào`(i, j)`và cố gắng trồng một cây vân sam hướng xuống dưới. Về chiều cao`1`, chúng ta chỉ cần kiểm tra`(i, j)`. Về chiều cao`2`, chúng tôi kiểm tra`(i+1, j-1)`bởi vì`(i+1, j+1)`, về chiều cao`3`, đoạn rộng hơn tiếp theo, v.v. cho đến khi chúng ta chạm đến ranh giới hoặc một`.`. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của cây vân sam. Tuy nhiên, trong trường hợp xấu nhất của lưới toàn sao, mỗi ô có thể mở rộng tới độ cao O(n) và mỗi lần mở rộng tốn O(n), tạo ra hành vi O(n³). Với n = 500 thì tốc độ này quá chậm. 

Quan sát quan trọng là một cây vân sam có chiều cao`h`kết thúc ở một tế bào chỉ phụ thuộc vào khả năng mở rộng một chiều cao nhỏ hơn`h-1`ngay phía trên nó, cộng với việc hàng hiện tại có chứa đủ các ngôi sao liên tiếp được căn giữa chính xác hay không. Điều này tạo ra một cấu trúc con chồng chéo: khi chúng ta biết một cây vân sam có thể kết thúc ở mỗi ô lớn đến mức nào, chúng ta có thể tái sử dụng thông tin đó để xây dựng những cấu trúc lớn hơn. 

Chúng tôi xác định`dp[i][j]`là chiều cao tối đa của một cây vân sam có đỉnh ở`(i, j)`. Nếu một tế bào`*`, chiều cao cơ sở của nó ít nhất là 1. Để mở rộng nó, chúng ta cần cả hai hàng xóm thấp hơn theo đường chéo để hỗ trợ một cây vân sam nhỏ hơn, vì mỗi cấp độ mở rộng một cách đối xứng. Điều này biến bài toán thành một phép tính lập trình động trong đó mỗi trạng thái chỉ phụ thuộc vào hàng bên dưới. 

Chúng tôi tính toán từ dưới lên trên, đảm bảo rằng khi xử lý một ô, tất cả thông tin về hàng tiếp theo đều đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| DP từ dưới lên | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng DP`dp`với tất cả số không, ở đâu`dp[i][j]`sẽ lưu trữ chiều cao vân sam tối đa bắt đầu từ`(i, j)`. Nếu như`grid[i][j]`là`*`, đặt nó thành 1 làm trường hợp cơ bản. Điều này phản ánh rằng mỗi ngôi sao ít nhất là một cây vân sam tầm thường có chiều cao bằng 1. 
2. Duyệt lưới từ dưới lên trên, vì mỗi ô phụ thuộc vào các hàng bên dưới nó. Thứ tự này đảm bảo rằng khi chúng ta tính toán`dp[i][j]`, tất cả các giá trị cần thiết trong hàng`i+1`đã có sẵn. 
3. Đối với mỗi ô`(i, j)`có chứa`*`, cố gắng mở rộng một cây vân sam. Việc gia hạn chỉ có thể thực hiện được nếu cả hai`(i+1, j-1)`Và`(i+1, j+1)`là hợp lệ và có thể hỗ trợ một chiều cao ít nhất`k-1`. Điều này đảm bảo cấu trúc hình tam giác vẫn liên tục và tập trung. 
4. Đặt`dp[i][j]`ĐẾN`1 + min(dp[i+1][j-1], dp[i+1][j+1])`khi có thể gia hạn. Mức tối thiểu đảm bảo rằng cây vân sam không thể phát triển vượt quá nhánh hỗ trợ yếu nhất. 
5. Tích lũy câu trả lời bằng cách tính tổng tất cả các giá trị trong`dp`. Mỗi`dp[i][j]`đếm xem có bao nhiêu cây vân sam bắt đầu từ lúc`(i, j)`trên tất cả các độ cao hợp lệ. 

### Tại sao nó hoạt động 

Mỗi cây vân sam được xác định duy nhất bởi ô trên cùng và chiều cao của nó. Sự lặp lại đảm bảo rằng một chiều cao phù hợp`h`tồn tại ở`(i, j)`khi và chỉ nếu cả hai mẫu con của chiều cao`h-1`tồn tại ở các đường chéo con cần thiết ở hàng tiếp theo. Điều này phản ánh chính xác định nghĩa hình học của tam giác. Vì chúng tôi xây dựng từ dưới lên trên nên mọi phần phụ thuộc đều đã được giải quyết khi cần, do đó, không có cấu hình hợp lệ nào bị bỏ qua và không được phép có phần mở rộng không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    
    dp = [[0] * m for _ in range(n)]
    
    ans = 0
    
    for i in range(n - 1, -1, -1):
        for j in range(m):
            if grid[i][j] == '*':
                if i == n - 1:
                    dp[i][j] = 1
                else:
                    left = dp[i + 1][j - 1] if j - 1 >= 0 else 0
                    right = dp[i + 1][j + 1] if j + 1 < m else 0
                    dp[i][j] = 1 + min(left, right)
                ans += dp[i][j]
    
    print(ans)

t = int(input())
for _ in range(t):
    solve()
```Bảng DP được điền từ dưới lên để mọi ô có thể dựa vào các giá trị đã được tính toán ở hàng bên dưới. Việc kiểm tra ranh giới ngăn chặn việc truy cập theo đường chéo không hợp lệ, điều này rất quan trọng vì các cây vân sam không thể mở rộng ra ngoài lưới. 

Câu trả lời cuối cùng là tổng của tất cả các giá trị dp, vì mỗi cấp độ của mỗi cây vân sam đóng góp một cấu trúc hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
.*.
***
```Chúng tôi tính toán từ dưới lên: 

| Tế bào | giá trị dp | Lý do | 
| --- | --- | --- | 
| (2,1) | 1 | sao đơn | 
| (2,0) | 1 | sao đơn | 
| (2,2) | 1 | sao đơn | 
| (1,1) | 1 | không thể kéo dài xuống dưới | 
| (1,0) | 1 | lá | 
| (1,2) | 1 | lá | 

Đáp án = 5. 

Điều này xác nhận rằng ngay cả khi tồn tại một hàng đầy đủ, phần mở rộng vẫn bị giới hạn do thiếu cấu trúc đường chéo ở trên. 

### Ví dụ 2 

đầu vào:```
4 5
.***.
*****
*****
*.*.*
```Ở đây, các cột trung tâm tạo thành các hình tam giác lớn chồng lên nhau. 

Ở hàng dưới cùng, chỉ các ngôi sao biệt lập mới đóng góp chiều cao 1. Di chuyển lên trên, giá trị dp tăng lên ở những nơi tồn tại hỗ trợ đường chéo và co lại khi các khoảng trống phá vỡ tính đối xứng. 

Sự tích lũy cho thấy mỗi gốc hợp lệ đóng góp nhiều cây vân sam lồng nhau như thế nào, xác minh rằng dp đếm tất cả các chiều cao một cách chính xác thay vì chỉ những chiều cao tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần với các chuyển tiếp O(1) | 
| Không gian | O(nm) | Bảng DP lưu trữ chiều cao trên mỗi ô | 

Kích thước lưới tối đa là 500 x 500 trên tất cả các trường hợp thử nghiệm, tổng cộng là 250.000 ô. Giải pháp O(nm) dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ đủ nhỏ cho một bảng DP số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        dp = [[0] * m for _ in range(n)]
        ans = 0
        for i in range(n - 1, -1, -1):
            for j in range(m):
                if grid[i][j] == '*':
                    if i == n - 1:
                        dp[i][j] = 1
                    else:
                        left = dp[i + 1][j - 1] if j - 1 >= 0 else 0
                        right = dp[i + 1][j + 1] if j + 1 < m else 0
                        dp[i][j] = 1 + min(left, right)
                    ans += dp[i][j]
        print(ans)

    t = int(input())
    for _ in range(t):
        solve()

    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
2 3
.*.
***
2 3
.*.
**.
4 5
.***.
*****
*****
*.*.*
5 7
..*.*..
.*****.
*******
.*****.
..*.*..
""") == """5
3
23
34"""

# single star
assert run("""1
1 1
*
""") == "1"

# empty grid
assert run("""1
2 2
..
..
""") == "0"

# full grid small
assert run("""1
3 3
***
***
***
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 sao | 1 | trường hợp tối thiểu | 
| tất cả các dấu chấm | 0 | không có cây vân sam | 
| đầy đủ 3x3 | 10 | nhiều cây vân sam lồng nhau | 

## Vỏ cạnh 

Một lưới ô đơn với`*`là cây vân sam hợp lệ đơn giản nhất. Thuật toán gán giá trị dp 1 và đếm nó một lần, khớp trực tiếp với định nghĩa vì không thể mở rộng. 

Một lưới hoàn toàn trống tạo ra số 0 ở mọi nơi vì dp vẫn bằng 0 đối với tất cả các ô. Không có chuyển tiếp nào được kích hoạt, vì vậy tổng chính xác bằng 0. 

Một lưới đầy đủ thể hiện hành vi tăng trưởng. Mỗi ô ở các hàng trên tích lũy các giá trị dp lớn vì cả hai ô con chéo luôn tồn tại, tạo ra cấu trúc tam giác cực đại. Sự lặp lại đếm chính xác mọi chiều cao tiền tố ở mọi điểm gốc, đó là nơi việc triển khai đơn giản thường đếm dưới hoặc vượt quá bằng cách chỉ xem xét chiều cao tối đa thay vì tất cả các chiều cao hợp lệ.
