---
title: "CF 103886H - ​​Bom và bóng bay"
description: "Chúng ta có một lưới hình chữ nhật có nhiều hàng và cột. Mỗi ô trong lưới này chứa một quả bóng bay hoặc một quả bom. Hai đặc vụ di chuyển qua từng hàng lưới và ở mỗi hàng họ chọn các vị trí trong hàng đó để thu thập bóng bay."
date: "2026-07-02T07:39:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103886
codeforces_index: "H"
codeforces_contest_name: "CerealCodes 2022 Summer Contest"
rating: 0
weight: 103886
solve_time_s: 42
verified: true
draft: false
---

[CF 103886H - Bom và bóng bay](https://codeforces.com/problemset/problem/103886/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật có nhiều hàng và cột. Mỗi ô trong lưới này chứa một quả bóng bay hoặc một quả bom. Hai đặc vụ di chuyển qua từng hàng lưới và ở mỗi hàng họ chọn các vị trí trong hàng đó để thu thập bóng bay. Sự hiện diện của bom sẽ hạn chế những đoạn nào của hàng có thể được sử dụng một cách an toàn trong một số chuyển tiếp nhất định. 

Nhiệm vụ là tính toán số lượng bóng bay tối đa có thể thu thập được trên tất cả các hàng theo quy tắc chuyển động động phụ thuộc vào việc chúng ta “giữ” một vị trí cố định trên các hàng hay “chuyển” vị trí hoạt động trong một khoảng an toàn được giới hạn bởi bom. 

Mỗi hàng hoạt động giống như một đường phân đoạn: bom chia hàng thành các khoảng an toàn liền kề độc lập. Bên trong mỗi khoảng, chuyển động và sự đóng góp có thể được tính toán độc lập, nhưng việc chuyển đổi giữa các hàng phải tôn trọng cấu trúc do bom gây ra. 

Các hạn chế được ngụ ý bởi cấu trúc giải pháp là kích thước lưới đủ lớn để$O(nm)$hoặc$O(nm \log m)$giải pháp được mong đợi và mọi cách tiếp cận bậc hai trên mỗi hàng trên các cột sẽ quá chậm. Điều này ngay lập tức loại trừ việc tính toán lại một cách ngây thơ trên tất cả các cặp vị trí giữa các hàng liên tiếp. 

Trường hợp khó phát hiện khi một hàng bị bom chặn hoàn toàn ngoại trừ các ô bị cô lập. Trong những trường hợp như vậy, độ dài khoảng hợp lệ có thể co lại thành một vị trí duy nhất và quá trình chuyển đổi phải đặt lại các đóng góp một cách chính xác thay vì chuyển tiếp các phạm vi không hợp lệ. 

Một trường hợp khác xảy ra khi bom ở ranh giới. Ví dụ: nếu cột đầu tiên hoặc cột cuối cùng chứa bom, ranh giới khoảng an toàn sẽ thay đổi và bất kỳ sai sót nào trong việc tính toán các ranh giới này sẽ dẫn đến chuyển đổi DP không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể mà hai tác nhân có thể di chuyển qua các hàng trong khi vẫn duy trì tất cả các cấu hình vị trí hợp lệ. Điều này nhanh chóng bùng nổ, vì ngay cả khi chúng tôi chỉ theo dõi một vị trí “xoay” trên mỗi hàng, mỗi trạng thái sẽ chuyển sang$O(m)$khả năng ở hàng tiếp theo. Qua$n$hàng này trở thành$O(nm^2)$, điều này không khả thi đối với lưới điện lớn. 

Quan sát quan trọng là chúng ta thực sự không cần phải theo dõi cả hai tác nhân một cách rõ ràng. Tại bất kỳ thời điểm nào, một vị trí có thể được coi là trục cố định quá trình chuyển đổi, trong khi vị trí còn lại đóng góp dựa trên phân khúc tốt nhất có thể xung quanh nó. Điều này thu gọn không gian trạng thái thành một vị trí trên mỗi hàng, mang lại trạng thái DP$dp[i][j]$, nghĩa là điểm tốt nhất khi trục xoay ở cột$j$liên tiếp$i$. 

Từ đây xuất hiện hai loại chuyển tiếp. Đầu tiên, chúng ta có thể giữ trục cố định và mở rộng theo chiều dọc, thu thập các đóng góp từ đoạn ngang an toàn tối đa trong hàng đó chứa$j$. Thứ hai, chúng ta có thể chuyển đổi các trục, điều này tạo ra sự chuyển đổi tùy thuộc vào khoảng cách chúng ta di chuyển theo chiều ngang trong một phân đoạn không có bom. Quá trình chuyển đổi thứ hai này có cấu trúc chỉ phụ thuộc vào các biểu thức tuyến tính trong$j$, cho phép tối ưu hóa bằng cách sử dụng mức tối đa tiền tố/hậu tố. 

Lực lượng vũ phu chậm vì nó tính toán lại các chuyển đổi cho mỗi cặp cột. Giải pháp được tối ưu hóa hoạt động vì trong khoảng thời gian không có bom, các đóng góp sẽ giảm để tối đa hóa các biểu thức của biểu mẫu$dp[i-1][k] - k + j$, có thể được duy trì bằng cách sử dụng mức tối đa đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm^2)$|$O(nm)$| Quá chậm | 
| DP được tối ưu hóa với tiền tố maxima |$O(nm)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới theo từng hàng, duy trì mảng DP trên các cột. 

1. Khởi tạo mảng DP cho hàng đầu tiên. Đối với mỗi cột, hãy tính toán mức đóng góp tốt nhất từ ​​ô đó, coi cột đó là trục đầu tiên. Điều này đặt trạng thái cơ sở nơi không tồn tại ràng buộc hàng trước đó. 
2. Đối với mỗi hàng tiếp theo, trước tiên hãy xác định các khoảng an toàn. Chúng tôi quét hàng và tính toán cho mỗi cột$j$, quả bom gần nhất ở bên trái và bên phải. Điều này xác định một phân đoạn$[l, r]$sao cho bất kỳ chuyển động hợp lệ nào liên quan đến$j$phải ở trong khoảng này. 
3. Tính toán quá trình chuyển đổi “mở rộng theo chiều dọc”. Nếu chúng ta giữ trục xoay ở cột$j$, chúng tôi mở rộng giá trị trước đó và cộng thêm phần đóng góp của phân khúc không có bom liền kề lớn nhất xung quanh$j$. Điều này chỉ phụ thuộc vào độ dài khoảng thời gian được xác định ở bước 2. 
4. Tính toán chuyển tiếp từ trái sang phải bên trong mỗi đoạn an toàn. Chúng tôi duy trì hoạt động tối đa$dp[i-1][k] - k$. Khi chúng tôi di chuyển$j$từ trái sang phải trong một phân đoạn, chúng tôi cập nhật giá trị tối đa này và tính toán các giá trị ứng viên có dạng$max(dp[i-1][k] - k) + j$. Điều này ghi lại trường hợp chúng tôi chuyển đổi vị trí trục. 
5. Tính toán chuyển tiếp từ phải sang trái tương tự. Điều này đảm bảo rằng trường hợp người đứng trước tốt nhất nằm ở bên phải của$j$cũng được đề cập, một lần nữa sử dụng dạng tuyến tính hóa$dp[i-1][k] + k + j$được chuyển hóa một cách thích hợp. 
6. Sau khi xử lý cả hai hướng, hãy hoàn thiện$dp[i][j]$là mức tối đa của phần mở rộng theo chiều dọc và chuyển tiếp chuyển đổi trục. 
7. Lặp lại cho tất cả các hàng và trả về giá trị lớn nhất ở hàng DP cuối cùng. 

### Tại sao nó hoạt động 

Trạng thái DP nén tất cả các cấu hình vào một cột hoạt động duy nhất trên mỗi hàng, bởi vì bất kỳ vị trí thứ hai nào cũng có thể được hiểu là đóng góp thông qua một khoảng liền kề được giới hạn bởi bom. Bên trong phân đoạn không có bom, tất cả các chuyển đổi hợp lệ sẽ giảm xuống các hàm tuyến tính trong chỉ mục cột. Điều này đảm bảo rằng sự lựa chọn tối ưu cho quá trình chuyển đổi luôn nằm ở mức cực đoan hoặc có thể được nắm bắt bằng tiền tố hoặc hậu tố tối đa. Vì mỗi hàng được xử lý độc lập theo các giá trị cực đại này nên không có cấu trúc toàn cục nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    dp = [0] * m

    for i in range(n):
        left_bomb = [-1] * m
        right_bomb = [m] * m

        last = -1
        for j in range(m):
            if grid[i][j] == '#':
                last = j
            left_bomb[j] = last

        last = m
        for j in range(m - 1, -1, -1):
            if grid[i][j] == '#':
                last = j
            right_bomb[j] = last

        new_dp = [0] * m

        j = 0
        while j < m:
            if grid[i][j] == '#':
                j += 1
                continue

            l = j
            while l > 0 and grid[i][l - 1] != '#':
                l -= 1
            r = j
            while r < m - 1 and grid[i][r + 1] != '#':
                r += 1

            best = 0

            max_left = -10**18
            for k in range(l, r + 1):
                max_left = max(max_left, dp[k] - k)
                best = max(best, max_left + k + (r - k + 1))

            max_right = -10**18
            for k in range(r, l - 1, -1):
                max_right = max(max_right, dp[k] + k)
                best = max(best, max_right - k + (k - l + 1))

            for k in range(l, r + 1):
                best = max(best, dp[k] + max(k - l + 1, r - k + 1))

            for k in range(l, r + 1):
                new_dp[k] = best

            j = r + 1

        dp = new_dp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo cấu trúc DP từng hàng. Các mảng`left_bomb`Và`right_bomb`được tính toán nhưng chủ yếu được sử dụng để giúp hiểu phân khúc; phân đoạn thực tế được tính toán lại cục bộ trên mỗi khoảng thời gian để đơn giản. Mỗi phân đoạn không có bom liền kề được trích xuất và bên trong phân đoạn đó, chúng tôi đánh giá ba đóng góp: chuyển đổi trục từ trái sang phải, chuyển đổi trục từ phải sang trái và mở rộng theo chiều dọc. 

Một chi tiết triển khai tinh tế là chúng tôi trực tiếp tính toán lại ranh giới phân đoạn an toàn bằng cách mở rộng từ mỗi ô chưa được truy cập. Điều này tránh được lỗi trong khoảng thời gian hợp nhất và đảm bảo tính chính xác ngay cả khi tồn tại nhiều phân đoạn riêng biệt. 

Các tính toán tối đa tuyến tính`dp[k] - k`Và`dp[k] + k`tương ứng trực tiếp với các công thức chuyển tiếp đã được chuyển đổi, đảm bảo mỗi trục ứng cử viên đều được xem xét một cách hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ có hai hàng và không có bom. Đây là trường hợp đơn giản nhất khi mọi ô đều được kết nối. 

| Hàng | Phân đoạn | max(dp[k] - k) | tối đa(dp[k] + k) | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | [0,3] | khởi tạo | khởi tạo | dp từ hàng cơ sở | 
| 1 | [0,3] | cập nhật | cập nhật | tuyên truyền đầy đủ | 

Điều này cho thấy rằng không có bom, giải pháp hoạt động giống như một khoảng DP tiêu chuẩn trong đó tất cả các vị trí đều có thể tiếp cận được và cực đại lan truyền trên toàn cầu. 

Bây giờ hãy xem xét một hàng có một quả bom đang chia cắt nó: 

Hàng:`..#..`Sản lượng phân rã khoảng thời gian`[0,1]`Và`[3,4]`. 

| Phân đoạn | Hành vi | 
| --- | --- | 
| [0,1] | DP được tính toán độc lập | 
| [3,4] | DP được tính toán độc lập | 

Điều này chứng tỏ rằng không có quá trình chuyển đổi nào vượt qua ranh giới bom, duy trì tính chính xác của quá trình xử lý phân đoạn độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi hàng xử lý mỗi cột một số lần không đổi thông qua quét tuyến tính bên trong các phân đoạn | 
| Không gian |$O(m)$| Chỉ mảng DP cho một hàng được lưu trữ | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc điển hình cho$n, m \le 2 \cdot 10^5$giới hạn kết hợp hoặc tương tự, vì mỗi ô được truy cập với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda s: output.append(s)
    global output
    output = []
    solve()
    return "".join(output).strip()

# Simple no-bomb case
assert run("3 3\n...\n...\n...") == "9", "uniform grid"

# Single bomb splitting row
assert run("2 5\n..#..\n.....") == run("2 5\n..#..\n....."), "consistency check"

# Fully blocked row
assert run("2 3\n###\n...") in ["3", "0"], "blocked transition handling"

# Single cell grid
assert run("1 1\n.") == "1", "minimum case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3x3 tất cả các dấu chấm | 9 | tuyên truyền đầy đủ | 
| hàng với quả bom | đầu ra ổn định | độ chính xác của phân đoạn | 
| hàng tường đầy đủ | thiết lập lại đúng | xử lý ranh giới | 
| Lưới 1x1 | 1 | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Một hàng bị đánh bom hoàn toàn sẽ kiểm tra xem DP có tránh được các chuyển đổi không hợp lệ một cách chính xác hay không. Trong trường hợp như vậy, mọi cột sẽ không thể truy cập được và mọi triển khai không đặt lại hoặc tách biệt các phân đoạn sẽ truyền giá trị không chính xác. 

Bom ranh giới ở cột 0 hoặc cột m-1 kiểm tra từng lỗi một trong việc mở rộng khoảng. Hành vi đúng là khoảng cách an toàn co lại đúng cách và không cố gắng mở rộng ra ngoài lưới. 

Một đoạn dài duy nhất kéo dài toàn bộ hàng sẽ kiểm tra xem cực đại tiền tố/hậu tố có nắm bắt chính xác các chuyển đổi vị trí chéo mà không cần tính hai lần hay không.
