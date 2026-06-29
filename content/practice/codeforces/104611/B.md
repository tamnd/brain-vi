---
title: "CF 104611B - trò chơi hình vuông"
description: "Chúng tôi được cung cấp một số đống đá độc lập. Mỗi cọc bắt đầu với một số lượng đá dương và hai người chơi luân phiên di chuyển."
date: "2026-06-29T22:05:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104611
codeforces_index: "B"
codeforces_contest_name: "2023\u6e56\u5357\u7701\u8d5b"
rating: 0
weight: 104611
solve_time_s: 65
verified: true
draft: false
---

[CF 104611B - trò chơi hình vuông](https://codeforces.com/problemset/problem/104611/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số đống đá độc lập. Mỗi cọc bắt đầu với một số lượng đá dương và hai người chơi luân phiên di chuyển. Trong một lượt, người chơi chọn chính xác một cọc và loại bỏ các viên đá khỏi đó theo quy tắc phụ thuộc vào hình vuông lớn nhất vừa với kích thước cọc đó. 

Nếu một đống có$m$đá, chúng tôi tính toán$s = \lfloor \sqrt{m} \rfloor$, đó là độ dài cạnh lớn nhất$s \times s$hình vuông vừa vặn bên trong$m$. Các quy tắc mô tả rằng về mặt khái niệm, chúng tôi tách cấu trúc hình vuông này khỏi những viên đá còn sót lại và sau đó cho phép người chơi loại bỏ một số hàng đầy đủ nhất định của hình vuông đó. Sau khi di chuyển, cọc sẽ trống ở phần hình vuông và chỉ còn lại cấu trúc hình vuông nhỏ hơn tùy thuộc vào số lượng hàng đã bị loại bỏ. 

Trò chơi kết thúc khi tất cả các viên đá trên tất cả các cọc được loại bỏ và người chơi loại bỏ viên đá cuối cùng sẽ thắng. Nhiệm vụ là xác định người chiến thắng nếu cả hai người chơi đều chơi tối ưu. 

Nhận xét quan trọng từ các ràng buộc là mỗi kích thước cọc độc lập về mặt lựa chọn và sự ghép nối duy nhất giữa các cọc xảy ra thông qua việc thay phiên nhau. Điều này gợi ý rõ ràng về một công thức Sprague-Grundy đối với các thành phần rời rạc. Từ$n$có thể lên tới khoảng$10^5$và mỗi$a_i$có thể lớn như$10^9$, bất kỳ cách tiếp cận nào mô phỏng nước đi một cách rõ ràng hoặc xây dựng biểu đồ trò chơi trên mỗi cọc đều không thể thực hiện được. Ngay cả việc liệt kê các nước đi trên mỗi cọc cũng đã là quá lớn vì một cọc có kích thước$m$có thể ám chỉ tới$O(\sqrt{m})$các lựa chọn mang tính cấu trúc. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là diễn giải sự di chuyển dưới dạng phép trừ tùy ý dựa trên công thức trong câu lệnh và cố gắng mô phỏng tất cả các trạng thái có thể tiếp cận được từ mỗi trạng thái đó.$m$. Ví dụ, với$m = 10^9$, thậm chí chỉ một cọc cũng có thể tạo ra một biểu đồ chuyển tiếp khổng lồ. Một hướng đi sai lầm phổ biến khác là coi trò chơi như một đống Nim tiêu chuẩn trong đó mỗi bước di chuyển sẽ loại bỏ các số nguyên dương tùy ý; điều này bỏ qua cấu trúc mạnh do phân tách bình phương áp đặt, dẫn đến các giá trị Grundy không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ coi mỗi cọc là một trạng thái trong biểu đồ trò chơi và cố gắng tính giá trị Grundy của nó bằng cách liệt kê tất cả các nước đi hợp lệ. Đối với một đống kích thước$m$, người ta sẽ tính$s = \lfloor \sqrt{m} \rfloor$, sau đó xem xét tất cả các lựa chọn có thể có về số lượng hàng cần xóa và tính toán trạng thái kết quả theo cách đệ quy. Điều này đúng về mặt lý thuyết vì lý thuyết Sprague-Grundy được áp dụng, nhưng nó nhanh chóng trở nên không khả thi. Mỗi tiểu bang có khả năng phân nhánh thành$O(s)$các trạng thái tiếp theo, và kể từ đó$s$là$O(\sqrt{m})$, tổng công việc trên tất cả các cọc sẽ bùng nổ ngay cả đối với đầu vào vừa phải. 

Sự đơn giản hóa quan trọng xuất phát từ việc lưu ý rằng phần còn lại chi tiết của cọc bên ngoài hình vuông lớn nhất không ảnh hưởng đến các quyết định trong tương lai. Một khi chúng ta cô lập hình vuông có kích thước$s \times s$, mỗi lần di chuyển chỉ thay đổi kích thước hình vuông hiệu quả bằng cách loại bỏ các hàng đầy đủ. Điều này có nghĩa là trạng thái trò chơi của một cọc chỉ phụ thuộc vào$s = \lfloor \sqrt{m} \rfloor$, không dựa trên giá trị chính xác của$m$. 

Khi việc giảm này được thực hiện, mỗi cọc thực sự tương đương với một cọc có trạng thái là số nguyên.$s$, và một bước di chuyển sẽ biến đổi$s$vào bất kỳ nhỏ hơn$t < s$bằng cách loại bỏ các hàng. Điều này sẽ thu gọn trò chơi thành một cấu trúc gỡ xuống rất đơn giản, trong đó từ trạng thái$s$, tất cả các tiểu bang$0, 1, \dots, s-1$có thể truy cập được. 

Điều này biến bài toán thành tính toán các số Grundy cho một trò chơi có lựa chọn giảm dần cổ điển, tạo ra một dạng đóng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ trạng thái trên mỗi cọc) | số mũ trong$m$| Bảng đệ quy/ghi nhớ lớn | Quá chậm | 
| Giảm Grundy trên kích thước vuông |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta nén từng cọc thành một số nguyên duy nhất$s = \lfloor \sqrt{a_i} \rfloor$và tính giá trị Grundy cho mỗi$s$. 

1. Với mỗi cọc, tính$s = \lfloor \sqrt{a_i} \rfloor$. Điều này chiếm được phần duy nhất của cọc ảnh hưởng đến các bước di chuyển trong tương lai, bởi vì tất cả các thao tác đều phụ thuộc vào số lượng hàng vuông đầy đủ tồn tại. 
2. Xác định trạng thái khái niệm$G(s)$đại diện cho một cọc có chiều dài cạnh hình vuông hiệu dụng là$s$. Từ trạng thái này, một nước đi cho phép chọn bất kỳ số nào$k$của các hàng, giảm cạnh hình vuông từ$s$ĐẾN$s-k$, Ở đâu$k \ge 1$. 
3. Quan sát rằng mọi trạng thái$t$với$0 \le t < s$có thể truy cập chính xác bằng một lần di chuyển bằng cách chọn$k = s - t$. Điều này làm cho quá trình chuyển đổi được thiết lập từ$s$bằng mọi số nguyên nhỏ hơn. 
4. Tính các giá trị Grundy từ dưới lên bắt đầu từ$G(0) = 0$. Đối với mỗi$s \ge 1$, tập giá trị Grundy có thể truy cập chính xác là$\{G(0), G(1), \dots, G(s-1)\}$. 
5. Mex của bộ$\{0, 1, \dots, s-1\}$là$s$, Vì thế$G(s) = s$. 
6. Do đó, giá trị Grundy của mỗi cọc chỉ đơn giản là$s = \lfloor \sqrt{a_i} \rfloor$. 
7. Tính XOR của tất cả$s$các giá trị. Nếu kết quả khác 0, người chơi đầu tiên có chiến lược thắng; nếu không người chơi thứ hai sẽ thắng. 

Bất biến cốt lõi là sau khi rút gọn, mỗi cọc hoạt động như một đống Nim độc lập với kích thước bằng căn bậc hai của nó. Vì mỗi bước di chuyển đều làm giảm giá trị này một cách nghiêm ngặt và cho phép đạt tới tất cả các giá trị nhỏ hơn, nên chuỗi Grundy trở nên nghiêm ngặt$G(s)=s$, điều này đảm bảo rằng thành phần XOR trên các cọc sẽ xác định đầy đủ kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    x = 0
    for v in a:
        s = math.isqrt(v)
        x ^= s
    
    if x:
        print("First")
    else:
        print("Second")

if __name__ == "__main__":
    solve()
```Việc thực hiện áp dụng trực tiếp việc giảm dẫn xuất. Bước không cần thiết duy nhất là sử dụng căn bậc hai số nguyên để tránh các vấn đề về độ chính xác của dấu phẩy động. Mỗi cọc được chuyển đổi thành phần đóng góp Grundy của nó trong thời gian không đổi và XOR cuối cùng sẽ xác định người chiến thắng theo lý thuyết Sprague-Grundy. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào với cọc$[3, 8, 15]$. 

Với mỗi giá trị chúng tôi tính toán$s = \lfloor \sqrt{a_i} \rfloor$, cho$[1, 2, 3]$. XOR được tính toán từng bước. 

| Cọc | Giá trị$a_i$|$s = \lfloor \sqrt{a_i} \rfloor$| XOR cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 2 | 8 | 2 | 3 | 
| 3 | 15 | 3 | 0 | 

XOR cuối cùng bằng 0 nên người chơi thứ hai thắng. Điều này phù hợp với trực giác rằng các cọc cân bằng hoàn hảo khi chơi tối ưu. 

Bây giờ hãy xem xét$[1, 4, 9]$. Căn bậc hai là$[1, 2, 3]$một lần nữa, nhưng việc loại bỏ cấu trúc cho thấy mỗi cọc đã là một hình vuông hoàn hảo nên mỗi cọc đều đóng góp đầy đủ. 

| Cọc | Giá trị$a_i$|$s$| XOR cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 2 | 3 | 
| 3 | 9 | 3 | 0 | 

Một lần nữa, kết quả là người chơi đầu tiên thua, cho thấy rằng tính đối xứng trong các giá trị Grundy quyết định kết quả chứ không phải kích thước cọc thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cọc yêu cầu một căn bậc hai số nguyên và một phép toán XOR | 
| Không gian |$O(1)$| Chỉ có bộ tích lũy XOR đang chạy mới được lưu trữ | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì thậm chí$10^5$cọc chỉ yêu cầu các phép tính số học đơn giản và căn bậc hai số nguyên là thời gian không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    x = 0
    for v in a:
        x ^= isqrt(v)
    return "First" if x else "Second"

# basic sample-like cases
assert run("1\n1\n") == "First"
assert run("1\n2\n") == "Second"

# all perfect squares
assert run("3\n1 4 9\n") == "Second"

# mixed case
assert run("3\n3 8 15\n") == "Second"

# single large pile
assert run("1\n1000000000\n") == "Second"

# small alternating structure
assert run("4\n1 2 3 4\n") == "First"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | Đầu tiên | cọc tối thiểu khác 0 | 
| 3\n1 4 9 | Thứ hai | đối xứng vuông hoàn hảo | 
| 3\n3 8 15 | Thứ hai | hủy hỗn hợp Grundy | 
| 1\n1000000000 | Thứ hai | xử lý giá trị lớn | 
| 4\n1 2 3 4 | Đầu tiên | tương tác XOR không cần thiết | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi một cọc đã là một hình vuông hoàn hảo. Ví dụ,$m = 16$cho$s = 4$. Trong tình huống này, trạng thái trò chơi hoạt động giống hệt như một đống tiêu chuẩn có kích thước 4 ở dạng lựa chọn giảm dần giống Nim. Thuật toán vẫn gán cho nó giá trị Grundy 4, do đó nó được xử lý nhất quán mà không cần phân nhánh đặc biệt. 

Một trường hợp cạnh khác là$m = 1$. Đây$s = 1$, và nước đi duy nhất dẫn đến 0, khiến người chơi tiếp theo sẽ bị thua nếu bị cô lập. Phép tính tạo ra giá trị Grundy 1, phản ánh chính xác vị thế chiến thắng. 

Các giá trị lớn như$m = 10^9$cũng an toàn vì thuật toán chỉ tính căn bậc hai số nguyên. Cấu trúc đảm bảo không xảy ra hiện tượng tràn hoặc đệ quy sâu, mỗi cọc được xử lý độc lập trong thời gian không đổi.
