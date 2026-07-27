---
title: "CF 102780H - Trận đấu nam"
description: "Chúng ta có một chồng bút chì chứa N bút chì màu. Hai người chơi luân phiên nhau và trong mỗi lượt, một người chơi loại bỏ chính xác 1, 5 hoặc 13 bút chì màu. Người chơi nào lấy được chiếc bút chì còn lại cuối cùng sẽ thắng ngay lập tức."
date: "2026-07-27T20:14:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 60
verified: true
draft: false
---

[CF 102780H - Trận đấu nam](https://codeforces.com/problemset/problem/102780/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chồng bút chì chứa N bút chì màu. Hai người chơi luân phiên nhau và trong mỗi lượt, một người chơi loại bỏ chính xác 1, 5 hoặc 13 bút chì màu. Người chơi nào lấy được chiếc bút chì còn lại cuối cùng sẽ thắng ngay lập tức. Cả hai người chơi đều nắm rõ kích thước cọc hiện tại và luôn chọn nước đi sao cho tối đa hóa cơ hội chiến thắng. Nhiệm vụ là xác định xem người chơi thứ nhất hay thứ hai có chiến lược chiến thắng. 

Đầu vào chứa một số nguyên N, số lượng bút chì màu ban đầu. Kết quả là số người chơi có thể buộc phải thắng, trong đó người chơi 1 đi trước và người chơi 2 đi sau. 

Giới hạn trên của N là 10000. Giá trị này đủ nhỏ để có thể dễ dàng thực hiện được giải pháp quy hoạch động tuyến tính. Một giải pháp thử mọi chuỗi trò chơi có thể sẽ phát triển theo cấp số nhân vì mỗi vị trí có thể phân nhánh thành ba vị trí tiếp theo. Ngay cả cách tiếp cận bậc hai cũng không cần thiết vì mỗi kích thước cọc chỉ phụ thuộc vào một số kích thước cọc nhỏ hơn. 

Những trường hợp khó đều đến từ những vị trí gần cuối trận. Ví dụ: khi chỉ có một cây bút chì màu:```
1
```Đầu ra đúng là:```
1
```Người chơi có thể loại bỏ bút chì màu duy nhất và giành chiến thắng. Một giải pháp bất cẩn chỉ kiểm tra xem người chơi có thể lấy 5 hay 13 cây bút chì màu có thể đánh dấu nhầm là thua hay không. 

Một trường hợp ranh giới khác là:```
2
```Đầu ra đúng là:```
2
```Người chơi đầu tiên phải loại bỏ một bút màu, để lại một bút màu cho đối thủ. Người chơi thứ hai sau đó lấy cây bút chì cuối cùng. Một mô hình ngây thơ chỉ dựa trên kích thước di chuyển có thể bỏ sót rằng các trạng thái thua nhỏ được xác định bởi các trạng thái trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi trò chơi có thể. Với một số bút chì màu nhất định, chúng ta có thể thử đệ quy lấy 1, 5 hoặc 13 cây bút chì màu và xem liệu lựa chọn nào có dẫn đến việc đối phương thua cuộc hay không. Điều này đúng vì mọi nước đi có thể xảy ra đều được xem xét và một người chơi sẽ thắng chính xác khi họ có thể di chuyển đến trạng thái mà người chơi kia không thể buộc phải chiến thắng. 

Vấn đề với phương pháp này là số lượng trạng thái lặp lại. Ví dụ: khi phân tích một cọc có kích thước 10000, nhiều chuỗi di chuyển khác nhau cuối cùng đạt đến cùng một cọc có kích thước nhỏ hơn. Không lưu trữ kết quả, phép đệ quy sẽ khám phá đi khám phá lại các vị trí giống nhau. Số lượng các chuỗi di chuyển có thể xảy ra gần như theo cấp số nhân, khiến nó quá chậm. 

Nhận xét quan trọng là chỉ có số lượng bút chì màu hiện tại mới quan trọng. Lịch sử di chuyển không ảnh hưởng đến khả năng trong tương lai. Điều này có nghĩa là mỗi kích thước cọc là một trạng thái trò chơi riêng biệt và chúng ta chỉ cần biết trạng thái đó là thắng hay thua. 

Chúng tôi xác định trạng thái là chiến thắng nếu người chơi hiện tại có thể thực hiện một nước đi khiến đối thủ rơi vào trạng thái thua cuộc. Nếu không thì nhà nước sẽ thua. Bắt đầu từ các cột nhỏ nhất, chúng ta có thể tính toán tất cả các trạng thái lên đến N. Điều này chuyển đổi tìm kiếm đệ quy thành một quy trình lập trình động đơn giản. 

Phương pháp vũ lực có hiệu quả vì nó khám phá mọi tương lai có thể xảy ra, nhưng nó thất bại khi đạt được cùng kích thước cọc thông qua nhiều con đường khác nhau. Nhận xét rằng các trạng thái của trò chơi chỉ phụ thuộc vào số bút chì màu còn lại cho phép chúng ta giải quyết mọi trạng thái một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^N) | Độ sâu đệ quy O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng trong đó giá trị ở chỉ số i thể hiện liệu một chồng bút chì màu i có giành chiến thắng cho người chơi đến lượt hay không. Giá trị đúng có nghĩa là người chơi hiện tại có thể buộc phải thắng, trong khi giá trị sai có nghĩa là mọi nước đi cuối cùng đều thua. 
2. Đặt trạng thái cho bút chì màu không là mất. Người chơi bắt đầu lượt của mình mà không có bút chì màu sẽ không thể di chuyển, vì vậy trạng thái này là trường hợp cơ bản. 
3. Đối với mỗi cọc có kích thước từ 1 đến N, hãy kiểm tra ba bước di chuyển có thể xảy ra: loại bỏ 1, 5 hoặc 13 bút chì màu. Nếu có thể thực hiện một nước đi và để lại trạng thái thua cho đối thủ, hãy đánh dấu trạng thái hiện tại là thắng. 

Lý do điều này có tác dụng là vì người chơi chỉ cần một nước đi thành công. Nếu có thể khiến đối thủ rơi vào thế thua thì lối chơi tối ưu đảm bảo đối thủ không thể tránh khỏi thất bại. 

1. Sau khi tính toán trạng thái cho N bút chì màu, xuất ra người chơi 1 nếu trạng thái thắng và người chơi 2 nếu ngược lại. 

Tại sao nó hoạt động: 

Bất biến quy hoạch động là sau khi xử lý một cọc có kích thước i, giá trị được lưu trữ mô tả chính xác kết quả của trò chơi khi vẫn còn i bút chì màu. Trường hợp cơ bản là đúng vì không có bút chì màu nghĩa là người chơi trước đó đã thắng. Đối với mọi trạng thái khác, thuật toán sẽ kiểm tra tất cả các nước đi hợp pháp. Nếu bất kỳ nước đi nào đạt đến trạng thái thua, người chơi hiện tại sẽ thắng bằng cách chọn nước đi đó. Nếu không có nước đi nào như vậy tồn tại thì mọi nước đi có thể đều mang lại cho đối phương thế thắng, do đó trạng thái hiện tại là thua. Vì tất cả các trạng thái nhỏ hơn đều đã được biết khi tính toán một trạng thái nên mọi quyết định đều dựa trên thông tin chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    
    win = [False] * (n + 1)
    
    moves = [1, 5, 13]
    
    for i in range(1, n + 1):
        for move in moves:
            if i >= move and not win[i - move]:
                win[i] = True
                break
    
    print(1 if win[n] else 2)

if __name__ == "__main__":
    solve()
```Mảng`win`lưu trữ kết quả trò chơi cho mọi kích thước cọc từ 0 đến N. Nó bắt đầu với tất cả các giá trị được đặt thành thua vì một trạng thái chỉ trở thành thắng sau khi tìm thấy một nước đi hợp lệ dẫn đến trạng thái thua. 

Vòng lặp xử lý các trạng thái theo thứ tự tăng dần. Khi tính toán`win[i]`, mọi tiểu bang`win[i - move]`đã có giá trị cuối cùng vì cọc còn lại sau mỗi lần di chuyển luôn nhỏ hơn i. 

các`break`câu lệnh dừng việc kiểm tra nước đi khi nước đi thắng được tìm thấy. Không cần phải tìm kiếm thêm vì chỉ cần một phương án thắng là đủ. Kiểm tra ranh giới`i >= move`ngăn chặn việc truy cập các chỉ số tiêu cực không hợp lệ. 

Số nguyên Python đủ lớn cho vấn đề này và kích thước mảng tối đa chỉ là 10001 phần tử, do đó mức sử dụng bộ nhớ là tối thiểu. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1
```Dấu vết là: 

| Kích thước cọc hiện tại | Di chuyển đã kiểm tra | Trạng thái trước đó | giá trị chiến thắng | 
| --- | --- | --- | --- | 
| 1 | Xóa 1 | thắng[0] = Sai | Đúng | 

Nước đi duy nhất là loại bỏ cây bút chì cuối cùng và khiến đối thủ không còn cây bút chì màu nào. Do đối phương không có nước đi thắng nên người chơi 1 thắng. 

Bây giờ hãy xem xét:```
2
```Dấu vết là: 

| Kích thước cọc hiện tại | Di chuyển đã kiểm tra | Trạng thái trước đó | giá trị chiến thắng | 
| --- | --- | --- | --- | 
| 1 | Xóa 1 | thắng[0] = Sai | Đúng | 
| 2 | Xóa 1 | thắng[1] = Đúng | Sai | 

Đối với một chồng bút cỡ hai, việc loại bỏ một bút chì sẽ mang lại trạng thái chiến thắng cho đối thủ. Loại bỏ năm hoặc mười ba bút chì màu là không thể. Mỗi nước đi đều thua nên người chơi thứ 2 thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi kích thước cọc chỉ kiểm tra ba bước di chuyển có thể xảy ra. | 
| Không gian | O(N) | Mảng lưu trữ một kết quả boolean cho mỗi kích thước cọc. | 

Với N được giới hạn ở 10000, thuật toán chỉ thực hiện vài chục nghìn thao tác và dễ dàng phù hợp với giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    win = [False] * (n + 1)
    
    for i in range(1, n + 1):
        for move in (1, 5, 13):
            if i >= move and not win[i - move]:
                win[i] = True
                break
    
    ans = "1" if win[n] else "2"
    
    sys.stdin = old_stdin
    return ans

assert solve_input("1\n") == "1", "minimum case"
assert solve_input("2\n") == "2", "first losing position"
assert solve_input("5\n") == "1", "direct move of size five"
assert solve_input("13\n") == "1", "direct move of size thirteen"
assert solve_input("10000\n") in ("1", "2"), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Cọc nhỏ nhất có thể được lấy ngay lập tức. | 
| 2 | 2 | Trạng thái thua do để cho đối thủ đi nước đi cuối cùng. | 
| 5 | 1 | Trạng thái chiến thắng khi sử dụng nước đi được phép lớn hơn. | 
| 13 | 1 | Xử lý chính xác kích thước di chuyển lớn nhất. | 
| 10000 | 1 hoặc 2 | Giải pháp xử lý hạn chế tối đa. | 

## Vỏ cạnh 

Đối với trường hợp một bút chì màu:```
1
```Thuật toán bắt đầu với`win[0] = False`. Trong khi xử lý`i = 1`, loại bỏ một bút chì đạt`win[0]`, nghĩa là người chơi tiếp theo sẽ thua. Trạng thái hiện tại trở thành chiến thắng nên đầu ra là người chơi 1. 

Đối với trường hợp hai bút chì màu:```
2
```Người chơi đầu tiên chỉ có thể loại bỏ một bút chì màu. Điều này để lại một bút chì màu, và`win[1]`đã được biết là đúng. Vì nước đi duy nhất mang lại cho đối thủ trạng thái chiến thắng,`win[2]`vẫn sai và đầu ra là trình phát 2. 

Đối với một cọc khớp chính xác với một nước đi có sẵn:```
13
```Việc tính toán trạng thái cuối cùng đạt đến`i = 13`. Loại bỏ mười ba bút chì màu`win[0]`, vậy là nhà nước đang thắng. Thuật toán không yêu cầu xử lý đặc biệt đối với kích thước di chuyển vì chúng được đưa vào kiểm tra chuyển tiếp một cách tự nhiên. 

Đối với đầu vào tối đa:```
10000
```Thuật toán vẫn chỉ thực hiện 10000 phép tính trạng thái, mỗi phép tính có thể có ba lần chuyển đổi. Bất biến tương tự áp dụng trong toàn bộ phạm vi, do đó kết quả cuối cùng được tính toán mà không gặp vấn đề về độ sâu đệ quy hoặc tính toán quá mức.```

```Bạn có thể điều chỉnh độ sâu của bài xã luận, thêm thuật ngữ lý thuyết trò chơi hoặc làm cho nó gần hơn với phong cách biên tập của cuộc thi Codeforces nếu cần.
