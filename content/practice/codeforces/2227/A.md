---
title: "CF 2227A - Koshary"
description: "Chúng ta được cung cấp một lưới bắt đầu từ điểm gốc $(0,0)$ và điểm đích $(x,y)$. Ở mỗi lần di chuyển, Yousef có thể tăng chính xác một tọa độ lên 2 bằng cách sử dụng một bước dài, di chuyển sang phải 2 hoặc lên 2."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2227
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1096 (Div. 3)"
rating: 0
weight: 2227
solve_time_s: 158
verified: false
draft: false
---

[CF 2227A - Koshary](https://codeforces.com/problemset/problem/2227/A) 

**Đánh giá:** - 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới bắt đầu từ điểm gốc$(0,0)$và một điểm mục tiêu$(x,y)$. Ở mỗi lần di chuyển, Yousef có thể tăng chính xác một tọa độ lên 2 bằng cách sử dụng một bước dài, di chuyển sang phải 2 hoặc lên 2. Ngoài ra, anh ta được phép sử dụng một chiêu thức đặc biệt duy nhất trong toàn bộ hành trình, một bước ngắn, tăng chính xác một tọa độ lên 1. 

Nhiệm vụ là xác định xem có tồn tại chuỗi chuyển động nào rơi chính xác vào$(x,y)$. 

Các ràng buộc rất nhỏ, với cả hai tọa độ tối đa là 10 và nhiều nhất là 100 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào chỉ phụ thuộc vào thời gian không đổi cho mỗi trường hợp thử nghiệm là đủ. Về nguyên tắc, ngay cả việc liệt kê đơn giản tất cả các đường dẫn cũng có thể khả thi vì không gian trạng thái rất nhỏ, nhưng việc tìm kiếm có hệ thống vẫn sẽ là chi phí không cần thiết so với đặc tính cấu trúc trực tiếp. 

Một trường hợp thất bại phổ biến đối với các phương pháp tiếp cận trực quan là xử lý vấn đề một cách tham lam, ví dụ bằng cách liên tục trừ 2 từ tọa độ lớn hơn hoặc các hướng xen kẽ. Những chiến lược như vậy có thể thất bại vì hạn chế thực sự duy nhất là tính nhất quán chẵn lẻ kết hợp với bước ngắn sử dụng một lần chứ không phải thứ tự thực hiện. 

Một ví dụ gây hiểu lầm cụ thể là$(1,1)$. Một nỗ lực ngây thơ có thể cố gắng sử dụng một bước ngắn cho một trong hai trục và sau đó hoàn thành phần còn lại, nhưng sau khi sử dụng một bước ngắn một lần, tọa độ còn lại cũng sẽ yêu cầu số gia lẻ, điều này là không thể chỉ với các bước có kích thước 2. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô hình hóa mỗi trạng thái như một vị trí$(a,b)$cùng với việc bước ngắn đã được sử dụng hay chưa. Từ mỗi trạng thái, chúng tôi phân nhánh thành nhiều nhất ba lần chuyển đổi: di chuyển$(a+2,b)$, di chuyển$(a,b+2)$, hoặc nếu không sử dụng, hãy di chuyển$(a+1,b)$hoặc$(a,b+1)$. Từ$x,y \le 10$, số lượng trạng thái có thể truy cập nhiều nhất là$11 \times 11 \times 2$và BFS sẽ chấm dứt nhanh chóng. Điều này đúng vì nó khám phá tất cả các chuỗi nước đi hợp pháp. 

Tuy nhiên, cách tiếp cận này có cấu trúc nặng nề hơn mức cần thiết. Quan sát quan trọng là mỗi bước di chuyển đều thay đổi chính xác một tọa độ và mỗi thay đổi là chẵn hoặc lẻ tùy thuộc vào việc đó là bước dài hay bước ngắn. Các bước dài duy trì tính chẵn lẻ, trong khi một bước ngắn lật tính chẵn lẻ của chính xác một tọa độ và chỉ có thể xảy ra một lần. 

Điều này làm giảm vấn đề về điều kiện khả thi chẵn lẻ. Nếu không sử dụng bước ngắn, cả hai tọa độ phải duy trì đều nhau trong suốt, do đó cả hai$x$Và$y$phải chẵn. Với đúng một bước ngắn, chính xác một tọa độ sẽ trở thành số lẻ trong khi tọa độ kia vẫn chẵn và tất cả các đóng góp còn lại đều là số gia chẵn. Do đó, cấu hình duy nhất không thể thực hiện được là khi cả hai tọa độ đều là số lẻ, vì điều đó sẽ yêu cầu hai lần lật chẵn lẻ độc lập. 

Phương pháp brute-force hoạt động vì nó khám phá tất cả các kết hợp một cách rõ ràng, nhưng nó thất bại về mặt rõ ràng và hiệu quả về mặt khái niệm so với phương pháp giảm tính chẵn lẻ, thu gọn tất cả các đường dẫn thành một điều kiện bất biến duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS vũ phu |$O(xy)$mỗi bài kiểm tra |$O(xy)$| Được chấp nhận nhưng không cần thiết | 
| Phân tích chẵn lẻ |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$x$Và$y$cho từng trường hợp thử nghiệm. Chúng đại diện cho sự dịch chuyển cần thiết dọc theo mỗi trục. 
2. Tính chẵn lẻ của cả hai tọa độ bằng cách kiểm tra xem mỗi tọa độ là chẵn hay lẻ. Đây là thuộc tính cấu trúc duy nhất quan trọng vì tất cả các bước di chuyển được phép đều thay đổi tọa độ theo gia số 1 hoặc 2. 
3. Nếu cả hai$x$Và$y$là số lẻ, xuất ra "NO". Điều này xuất phát từ thực tế là mỗi tọa độ bắt đầu từ 0, tức là số chẵn và các bước dài bảo toàn tính chẵn lẻ, do đó, để đạt được hai tọa độ lẻ sẽ cần hai lần lật chẵn lẻ độc lập, nhưng chỉ tồn tại một bước ngắn. 
4. Nếu không thì xuất ra "CÓ". Bất kỳ cấu hình còn lại nào cũng có thể được xây dựng bằng cách gán bước ngắn tùy chọn cho tọa độ cần tính chẵn lẻ lẻ, đồng thời sử dụng các bước dài để hoàn thành phần còn lại. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi bước dài bảo toàn tính chẵn lẻ của cả hai tọa độ, trong khi bước ngắn sẽ lật tính chẵn lẻ của chính xác một tọa độ và được sử dụng nhiều nhất một lần. Bắt đầu từ$(0,0)$, cả hai tọa độ đều chẵn. Vì vậy, sau tất cả các bước di chuyển, tính chẵn lẻ của$(x,y)$phải có thể truy cập được bằng 0 hoặc một lần lật chẵn lẻ. Mẫu chẵn lẻ duy nhất yêu cầu hai lần lật độc lập là$(\text{odd}, \text{odd})$, không thể truy cập được dưới sự ràng buộc của một bước ngắn. Bất biến này hoàn toàn đặc trưng cho tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        if x % 2 == 1 and y % 2 == 1:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai đọc từng trường hợp kiểm thử một cách độc lập và áp dụng trực tiếp điều kiện chẵn lẻ. Điểm tinh tế duy nhất là đảm bảo cả hai tọa độ đều được kiểm tra đồng thời, vì điều kiện không thể xảy ra yêu cầu cả hai tọa độ đều lẻ cùng một lúc. Không cần phải theo dõi số bước hoặc mô phỏng chuyển động, vì cấu trúc của các thao tác được phép giảm mọi thứ về logic chẵn lẻ. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$(x,y) = (2,4)$. Cả hai tọa độ đều bằng nhau nên không cần bước ngắn. Tính chẵn lẻ hai chiều vẫn nhất quán theo các bước dài lặp đi lặp lại. 

| Bước | x chẵn lẻ | y chẵn lẻ | Tiểu bang | 
| --- | --- | --- | --- | 
| bắt đầu | thậm chí | thậm chí | (0,0) | 
| sau những bước dài | thậm chí | thậm chí | (2,4) | 

Điều này xác nhận rằng có thể truy cập được các cấu hình chẵn. 

Bây giờ hãy xem xét$(x,y) = (1,1)$. 

| Bước | x chẵn lẻ | y chẵn lẻ | Tiểu bang | 
| --- | --- | --- | --- | 
| bắt đầu | thậm chí | thậm chí | (0,0) | 
| sau bước ngắn | lẻ | thậm chí hoặc thậm chí | lẻ tùy lựa chọn | 
| yêu cầu cuối cùng | lẻ | lẻ | không thể | 

Điều này cho thấy rằng bất kỳ tọa độ nào nhận được bước ngắn, tọa độ còn lại vẫn giữ nguyên xuyên suốt, khiến cho tính chẵn lẻ lẻ đồng thời không thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm được kiểm tra bằng các phép toán chẵn lẻ theo thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung ngoài một vài số nguyên | 

Giải pháp này là tối ưu cho các ràng buộc vì nó thực hiện một số phép toán số học và modulo cố định cho mỗi trường hợp thử nghiệm, nằm trong giới hạn cho$t \le 100$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        x, y = map(int, input().split())
        if x % 2 == 1 and y % 2 == 1:
            out.append("NO")
        else:
            out.append("YES")
    return "".join(out)

# provided samples (as concatenated style)
assert run("6\n1 1\n2 4\n6 5\n9 7\n2 10\n10 10\n") == "NOYESYESNOYESYES"

# minimum values
assert run("1\n1 2\n") == "YES"

# both odd case
assert run("1\n3 3\n") == "NO"

# all even large-ish
assert run("1\n10 10\n") == "YES"

# mixed parity
assert run("1\n8 7\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1,2) | CÓ | tọa độ lẻ duy nhất có thể truy cập được | 
| (3,3) | KHÔNG | cả hai điều không thể kỳ lạ | 
| (10,10) | CÓ | trường hợp biên chẵn | 
| (8,7) | CÓ | một trường hợp tọa độ lẻ | 

## Vỏ cạnh 

cho$(1,1)$, cả hai tọa độ đều lẻ. Thuật toán ngay lập tức đưa ra "KHÔNG" vì điều kiện phát hiện tính chẵn lẻ lẻ đồng thời. Bất kỳ nỗ lực xây dựng nào cũng sẽ yêu cầu chỉ định bước ngắn cho một trục, khiến trục kia không thể sửa được chỉ bằng cách sử dụng số gia tăng là 2. 

cho$(1,2)$, có đúng một tọa độ là số lẻ. Thuật toán xuất ra "CÓ". Một dấu vết cho thấy rằng việc sử dụng bước ngắn trên tọa độ x sẽ giúp thực hiện được:$(0,0) \rightarrow (1,0)$qua bước ngắn, sau đó lặp lại các bước dài$(1,2)$. 

| Bước | x | y | Hành động | 
| --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | bắt đầu | 
| 1 | 1 | 0 | bước ngắn trong x | 
| 2 | 1 | 2 | bước dài trong y | 

Điều này khẳng định rằng các trường hợp tọa độ lẻ luôn khả thi trong các phép toán được phép.
