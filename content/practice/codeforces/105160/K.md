---
title: "CF 105160K - \u73af\u5f62\u6570\u7ec4(dễ)"
description: "Nhiệm vụ này mô tả một cách xác định để gán các số cho lưới n x m. Hãy tưởng tượng bắt đầu với một ma trận trống và viết các số nguyên bắt đầu từ 1, tăng dần từng số một, trong khi luôn đi dọc theo ranh giới bên ngoài của vùng chưa lấp đầy còn lại theo đường xoắn ốc theo chiều kim đồng hồ."
date: "2026-06-27T11:02:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "K"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 52
verified: true
draft: false
---

[CF 105160K - \u73af\u5f62\u6570\u7ec4(easy)](https://codeforces.com/problemset/problem/105160/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả một cách xác định để gán các số cho lưới n x m. Hãy tưởng tượng bắt đầu với một ma trận trống và viết các số nguyên bắt đầu từ 1, tăng dần từng số một, trong khi luôn đi dọc theo ranh giới bên ngoài của vùng chưa lấp đầy còn lại theo đường xoắn ốc theo chiều kim đồng hồ. Khi lớp bên ngoài được lấp đầy, quá trình tiếp tục vào bên trong với ranh giới bên trong tiếp theo, lặp lại cho đến khi mỗi ô được gán một giá trị. 

Đầu vào chỉ cung cấp kích thước của lưới. Đầu ra là ma trận được xây dựng hoàn chỉnh sau khi quá trình điền hình xoắn ốc này hoàn tất. 

Các ràng buộc cho phép n và m lên tới 1000, do đó ma trận có thể chứa tới một triệu ô. Bất kỳ giải pháp nào chạm vào từng ô một số lần không đổi đều có thể chấp nhận được. Một giải pháp cố gắng mô phỏng chuyển động không hiệu quả trên mỗi bước với chi phí quét hoặc đệ quy lặp đi lặp lại sẽ vẫn chỉ vượt qua trong Python nếu nó hoàn toàn tuyến tính về số lượng ô. Bất cứ điều gì tệ hơn O(nm) đều có nguy cơ bị hết thời gian vì một triệu thao tác đã gần đạt giới hạn trên đối với giải pháp Python 1 giây. 

Một lỗi thường gặp xuất hiện khi xử lý trường hợp một hàng hoặc một cột. Ví dụ: nếu n bằng 1 và m bằng 5, kết quả mong đợi chỉ đơn giản là một chuỗi thẳng từ trái sang phải. Việc triển khai theo hình xoắn ốc ngây thơ luôn cố gắng đi qua cả bốn hướng mà không kiểm tra ranh giới sẽ ghi đè các giá trị hoặc lập chỉ mục ngoài phạm vi. 

Một vấn đề tế nhị khác nảy sinh khi các ranh giới thu hẹp gặp nhau. Hãy xem xét một lưới 3 x 3. Sau khi hoàn thành lớp ngoài, ô trung tâm còn lại phải được điền đúng một lần. Điều kiện kết thúc không chính xác thường dẫn đến việc bỏ qua ô cuối cùng này hoặc ghi nó hai lần. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng từng bước chuyển động xoắn ốc, theo dõi các ô đã ghé thăm và liên tục cố gắng di chuyển sang phải, xuống, sang trái hoặc lên tùy theo hướng. Mỗi lần di chuyển sẽ kiểm tra xem ô tiếp theo có nằm trong giới hạn và không được truy cập hay không. Nếu không, hướng sẽ thay đổi. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa của quy trình. Tuy nhiên, mỗi bước bao gồm nhiều lần kiểm tra và có khả năng lặp lại các nỗ lực để tìm ra hướng đi hợp lệ. Mặc dù mỗi bước trong số n lần m vẫn thực hiện công việc liên tục, nhưng chi phí Python từ việc kiểm tra hướng và tra cứu đã truy cập khiến nó nằm ở ranh giới nhưng vẫn có thể chấp nhận được. Sự kém hiệu quả thực sự đến từ việc quản lý các cấu trúc đã ghé thăm và kiểm tra điều kiện lặp đi lặp lại trong một vòng lặp chặt chẽ. 

Một cách sạch hơn sẽ tránh hoàn toàn việc "đi bộ". Thay vì di chuyển từng bước, chúng tôi duy trì bốn ranh giới mô tả hình chữ nhật còn lại hiện tại: hàng trên cùng, hàng dưới cùng, cột bên trái và cột bên phải. Ở mỗi giai đoạn, chúng ta tô đầy một cạnh của hình chữ nhật còn lại trong một lần quét, sau đó thu nhỏ ranh giới đó vào trong. Điều này hiệu quả vì cấu trúc xoắn ốc đảm bảo rằng mỗi lớp là một đường viền hình chữ nhật hoàn chỉnh và sau khi được lấp đầy, nó không bao giờ cần phải xem lại. 

Điều này biến đổi quá trình từ mô phỏng chuyển động theo từng tế bào sang lấp đầy từng lớp có cấu trúc, loại bỏ logic định hướng không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bước mô phỏng với kiểm tra đã truy cập | O(nm) | O(nm) | Các hằng số được chấp nhận nhưng nặng hơn | 
| Truyền tải lớp ranh giới | O(nm) | O(1) bổ sung (không bao gồm đầu ra) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì bốn con trỏ mô tả hình chữ nhật chưa được tô màu hiện tại: trên, dưới, trái và phải. Chúng tôi cũng duy trì một bộ đếm để đặt số tiếp theo.

1. Khởi tạo ma trận với các giá trị trống và đặt bộ đếm thành 1. Các ranh giới bắt đầu bằng top = 0, Bottom = n - 1, left = 0, right = m - 1. 
2. Trong khi ranh giới trên vẫn ở dưới hoặc bằng đáy và ranh giới bên trái vẫn ở dưới hoặc bằng bên phải, chúng ta lấp đầy một lớp đầy đủ của hình xoắn ốc. 
3. Di chuyển từ trái sang phải dọc theo hàng trên cùng hiện tại, gán các giá trị liên tiếp. Sau khi hoàn thành thì tăng top vì hàng đó đã được sử dụng hết. 
4. Di chuyển từ trên xuống dưới dọc theo cột bên phải hiện tại, gán giá trị. Sau khi làm xong thì giảm sang phải vì cột đó đã được sử dụng hết. 
5. Nếu đỉnh vẫn ở dưới hoặc bằng đáy thì di chuyển từ phải sang trái dọc theo hàng dưới cùng. Điều kiện này là cần thiết vì ma trận có thể thu gọn thành một hàng còn lại. Sau khi điền, giảm đáy. 
6. Nếu bên trái vẫn ở dưới hoặc bằng bên phải thì di chuyển từ dưới lên trên dọc theo cột bên trái. Điều kiện này xử lý trường hợp chỉ còn lại một cột. Sau khi điền, tăng sang trái. 
7. Tiếp tục cho đến khi tất cả các ranh giới giao nhau, nghĩa là mọi ô đều đã được lấp đầy. 

### Tại sao nó hoạt động 

Mỗi lần lặp sẽ làm cạn kiệt hoàn toàn ranh giới bên ngoài của ma trận con còn lại mà không bỏ qua hoặc chồng chéo các ô. Sau mỗi lần quét định hướng, ranh giới tương ứng sẽ bị loại bỏ vĩnh viễn khỏi việc xem xét, do đó không có ô nào được xem lại. Điều bất biến là tất cả các ô bên ngoài hình chữ nhật hiện tại đã được điền theo đúng thứ tự xoắn ốc và hình chữ nhật hiện tại luôn thể hiện chính xác vùng chưa được gán còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    mat = [[0] * m for _ in range(n)]
    
    top, bottom = 0, n - 1
    left, right = 0, m - 1
    val = 1
    target = n * m
    
    while top <= bottom and left <= right:
        for j in range(left, right + 1):
            mat[top][j] = val
            val += 1
        top += 1
        
        for i in range(top, bottom + 1):
            mat[i][right] = val
            val += 1
        right -= 1
        
        if top <= bottom:
            for j in range(right, left - 1, -1):
                mat[bottom][j] = val
                val += 1
            bottom -= 1
        
        if left <= right:
            for i in range(bottom, top - 1, -1):
                mat[i][left] = val
                val += 1
            left += 1
    
    out = []
    for i in range(n):
        out.append(" ".join(map(str, mat[i])))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo chiến lược thu hẹp ranh giới một cách trực tiếp. Ma trận được phân bổ trước nên phép gán là O(1). Biến val tăng chính xác một lần trên mỗi ô, đảm bảo tiến trình tuyến tính nghiêm ngặt từ 1 đến n lần m. 

Việc kiểm tra có điều kiện trước khi điền vào hàng dưới cùng và cột bên trái sẽ ngăn chặn việc ghi đè trong các trường hợp suy biến, chẳng hạn như một hàng hoặc cột còn lại, đây là nguyên nhân phổ biến nhất của việc triển khai không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào là lưới 4 x 5. 

| Bước | Đầu trang | Dưới cùng | Trái | Đúng | Hành động | Chìa khóa điền | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | 4 | hàng trên cùng từ trái sang phải | 1 đến 5 | 
| 2 | 1 | 3 | 0 | 4 | cột bên phải từ trên xuống dưới | 6 đến 8 | 
| 3 | 1 | 2 | 0 | 3 | hàng dưới cùng từ phải sang trái | 9 đến 12 | 
| 4 | 2 | 2 | 0 | 3 | cột bên trái từ dưới lên trên | 13 | 
| 5 | 2 | 2 | 1 | 3 | hàng trên cùng của lớp bên trong | 14 đến 17 | 
| 6 | 3 | 2 | 1 | 3 | cột bên phải (không có giá trị sau khi thu nhỏ dừng sớm) | xử lý theo điều kiện | 
| 7 | cuối cùng | cuối cùng | cuối cùng | cuối cùng | hàng bên trong cuối cùng | hoàn thành 18 đến 20 | 

Dấu vết này xác nhận rằng mỗi lớp được tiêu thụ hoàn toàn trước khi di chuyển vào trong và vùng trung tâm được chạm tới đúng một lần. 

### Ví dụ 2 

Nhập lưới 1 x 4. 

| Bước | Đầu trang | Dưới cùng | Trái | Đúng | Hành động | Chìa khóa điền | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 3 | một hàng từ trái sang phải | 1 đến 4 | 

Trường hợp này thực hiện điều kiện một hàng. Sau lần vượt qua đầu tiên, phần trên trở nên lớn hơn phần dưới, ngăn chặn bất kỳ sự di chuyển theo chiều dọc nào. Thuật toán dừng chính xác mà không cố gắng thực hiện các chuyển động không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được chỉ định chính xác một lần trong một lần duyệt xoắn ốc | 
| Không gian | O(nm) | Ma trận lưu trữ tất cả các giá trị đầu ra | 

Các ràng buộc cho phép tối đa một triệu ô và mỗi ô được ghi chính xác một lần. Điều này giúp thời gian chạy thoải mái trong giới hạn dành cho Python với giới hạn 1 giây khi được triển khai với các vòng lặp đơn giản và đầu ra được đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("4 5\n") == "\n".join([
"1 2 3 4 5",
"14 15 16 17 6",
"13 20 19 18 7",
"12 11 10 9 8"
]), "sample 1"

# minimum case
assert run("1 1\n") == "1"

# single row
assert run("1 5\n") == "1 2 3 4 5"

# single column
assert run("4 1\n") == "1\n2\n3\n4"

# square small
assert run("3 3\n") == "\n".join([
"1 2 3",
"8 9 4",
"7 6 5"
])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | độ chính xác ma trận tối thiểu | 
| 1 5 | 1 2 3 4 5 | hành vi ranh giới một hàng | 
| 4 1 | trình tự dọc | hành vi ranh giới một cột | 
| 3 3 | hình vuông xoắn ốc | độ chính xác của lớp bên trong | 

## Vỏ cạnh 

Ma trận 1 x m được xử lý hoàn toàn bằng lần quét ngang đầu tiên. Vì phần trên lớn hơn phần dưới ngay sau lần quét đó, nên không xảy ra quá trình truyền tải theo chiều dọc hoặc ngược lại, ngăn cản việc truy cập chỉ mục không hợp lệ. 

Ma trận m x 1 hoạt động đối xứng. Lần quét dọc đầu tiên sẽ lấp đầy toàn bộ cột và các cập nhật ranh giới sẽ ngăn chặn mọi đường đi ngang. 

Một hình vuông nhỏ có chiều lẻ chẳng hạn như 3 x 3 đảm bảo rằng sau khi hoàn thành chu trình bên ngoài, ô trung tâm vẫn nằm trong phạm vi ranh giới hợp lệ cho đúng một lần lặp. Việc kiểm tra ranh giới của thuật toán cho phép nó được lấp đầy như một phần của lớp ô đơn cuối cùng mà không bị trùng lặp hoặc bỏ sót.
