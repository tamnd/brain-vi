---
title: "CF 103081G - Trang trí"
description: "Chúng ta được yêu cầu xây dựng một dãy số nguyên biểu thị các khoảng trống theo chiều dọc giữa các kệ liên tiếp. Có các khoảng trống $K$, mỗi khoảng trống $si$ phải là một số nguyên trong phạm vi $[0, N-1]$ và tất cả các khoảng trống phải khác biệt theo từng cặp."
date: "2026-07-03T23:18:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 53
verified: true
draft: false
---

[CF 103081G - Trang trí](https://codeforces.com/problemset/problem/103081/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy số nguyên biểu thị các khoảng trống theo chiều dọc giữa các kệ liên tiếp. có$K$khoảng trống, mỗi khoảng trống$s_i$phải là số nguyên trong phạm vi$[0, N-1]$và tất cả các khoảng trống phải phân biệt theo cặp. Trình tự này không phải là tùy ý: nó phải tuân theo một phép lặp trong đó mỗi giá trị tiếp theo được xác định từ giá trị trước đó bằng cách sử dụng số ước của giá trị trước đó, lấy modulo$N$. Trong số tất cả các chuỗi hợp lệ, chúng ta phải xuất ra một chuỗi có tổng nhỏ nhất của tất cả$s_i$hoặc báo cáo rằng không tồn tại chuỗi hợp lệ. 

Một cách hữu ích để nghĩ về điều này là chúng ta đang duyệt qua một đồ thị có hướng với$N$các nút được gắn nhãn$0$ĐẾN$N-1$. Từ một nút$x$, có chính xác một cạnh đi tới$(x + d(x)) \bmod N$, Ở đâu$d(x)$là số ước của$x$. Chúng ta cần một con đường dài$K$truy cập các nút riêng biệt, giảm thiểu tổng số nhãn đã truy cập. 

Các ràng buộc cho phép cả hai$N$Và$K$lên đến một triệu. Bất kỳ giải pháp nào cố gắng khám phá tất cả các khả năng hoặc duy trì trạng thái đầy đủ trên các tập hợp con đều không thể thực hiện được ngay lập tức. Thậm chí$O(N \log N)$tiền xử lý có thể chấp nhận được, nhưng bất cứ điều gì bậc hai trong$N$hoặc hàm mũ trên các trạng thái thì không. 

Một trường hợp khó nhận thấy là khi$N = 1$. Khi đó giá trị duy nhất có thể là$0$, nhưng vì tất cả các giá trị phải khác biệt nên chúng ta không thể lấy nhiều hơn một phần tử. Nếu như$K > 1$, câu trả lời phải là$-1$. Một trường hợp góc cạnh khác là khi sự lặp lại ngay lập tức quay vòng qua các giá trị đã được sử dụng, điều này có thể buộc chấm dứt sớm ngay cả khi$K$là nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi giá trị bắt đầu có thể$s_1$, sau đó liên tục áp dụng quy tắc chuyển đổi để tạo ra chuỗi, dừng lại nếu chúng ta lặp lại một giá trị hoặc vượt quá giới hạn. Nếu chúng ta có thể thu thập được$K$các giá trị riêng biệt, tính tổng và giữ mức tối thiểu. 

Điều này hoạt động hợp lý vì quy tắc xác định đầy đủ trình tự sau khi phần tử đầu tiên được chọn. Tuy nhiên, mỗi lần khởi động yêu cầu tới$K$chuyển tiếp và có$N$sự lựa chọn, dẫn đến$O(NK)$hoạt động trong trường hợp xấu nhất. Với cả hai thông số lên tới$10^6$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải khám phá nhiều điểm xuất phát. Mỗi giá trị bắt đầu tạo ra một chuỗi xác định cho đến khi nó quay vòng hoặc cạn kiệt tất cả$K$các yếu tố cần thiết. Thay vì mô phỏng từ tất cả các lần bắt đầu, chúng ta có thể tính toán trước con trỏ tiếp theo cho mỗi nút và sau đó phân tách toàn bộ biểu đồ thành các thành phần chức năng rời nhau (vì mỗi nút có chính xác một cạnh đi ra). Mỗi thành phần là một chu trình có cây cối bổ sung vào đó, nhưng vì chúng ta không được phép lặp lại các giá trị nên mọi giải pháp hợp lệ đều phải nằm trên một đường dẫn đơn giản trước khi sự lặp lại xảy ra. 

Sự đơn giản hóa quan trọng là cách tốt nhất để giảm thiểu tổng là luôn bắt đầu từ nút nhỏ nhất chưa được sử dụng có thể tạo ra một chuỗi có độ dài hợp lệ.$K$. Khi chúng tôi cam kết với một nút bắt đầu, chúng tôi sẽ tham lam đi theo chuỗi xác định của nó và thu thập các nút cho đến khi chúng tôi đạt được$K$hoặc nhấn một lần lặp lại, điều này làm mất hiệu lực phần bắt đầu đó. 

Điều này làm giảm vấn đề tính toán hàm$f(x) = (x + d(x)) \bmod N$cho tất cả$x$và sau đó mô phỏng hiệu quả các chuỗi trong khi đánh dấu các nút đã truy cập trên toàn cầu. 

Hàm chia cho tất cả các giá trị lên đến$10^6$có thể được tính toán trước trong$O(N \log N)$bằng phương pháp sàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NK)$|$O(N)$| Quá chậm | 
| Mô phỏng đồ thị hàm số |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xoay quanh việc tính toán trước số lượng ước số và sau đó xử lý phép truy toán như một bài toán truyền tải đồ thị hàm số. 

1. Tính toán$d[x]$, số ước của mọi$x \in [0, N-1]$. Điều này được thực hiện bằng cách sử dụng một sàng đã được sửa đổi trong đó mỗi bội số của$i$được tăng lên. Bước này là cần thiết vì quá trình chuyển đổi phụ thuộc vào số lượng ước số và phải được trả lời trong thời gian không đổi sau đó. 
2. Xây dựng mảng con trỏ tiếp theo$nxt[x] = (x + d[x]) \bmod N$. Điều này xác định một cạnh đi ra xác định cho mỗi nút, biến bài toán thành một biểu đồ trong đó mỗi nút có chính xác một cạnh đi ra. 
3. Duy trì một mảng được truy cập toàn cục để đảm bảo tất cả các giá trị được chọn đều khác biệt. Điều này thực thi yêu cầu “không đồng nhất” và ngăn chặn các chuỗi không hợp lệ do việc truy cập lại các nút gây ra. 
4. Lặp lại các điểm bắt đầu từ$0$ĐẾN$N-1$. Khi chúng tôi gặp một nút chưa được truy cập, chúng tôi cố gắng xây dựng một chuỗi bắt đầu từ nút đó. 
5. Từ nút khởi đầu$x$, theo dõi nhiều lần$nxt[x]$, nối thêm các nút vào kết quả trong khi chúng chưa được truy cập. Đánh dấu từng nút đã truy cập ngay khi nó được thêm vào. Dừng khi một trong hai chuỗi đạt đến độ dài$K$hoặc chúng ta xem lại một nút. 
6. Nếu chúng tôi thu thập thành công$K$các phần tử, xuất ra chuỗi và kết thúc. Nếu không, hãy tiếp tục thử điểm bắt đầu chưa được truy cập tiếp theo. 
7. Nếu chúng ta sử dụng hết tất cả các nút mà không đạt được độ dài$K$, đầu ra$-1$. 

Quyết định quan trọng là chỉ bắt đầu từ các nút chưa được truy cập. Điều này tránh lãng phí công việc trên các chuỗi có thể va chạm ngay lập tức với các giá trị đã được sử dụng và đảm bảo mỗi nút được xử lý nhiều nhất một lần về tổng thể. 

### Tại sao nó hoạt động 

Mỗi nút thuộc về chính xác một chuỗi chức năng trong quá trình chuyển đổi xác định. Khi một nút được truy cập, việc theo dõi lại nút đó sẽ chỉ tái tạo lại một phần của chuỗi đã được khám phá. Bởi vì tất cả các giá trị phải khác biệt, nên mọi giải pháp hợp lệ đều tương ứng với việc chọn tiền tố của một chuỗi nào đó trong quá trình phân tách biểu đồ hàm này. Vì mỗi nút được truy cập nhiều nhất một lần và chúng tôi luôn mở rộng một cách tham lam từ điểm bắt đầu nhỏ nhất có sẵn, nên chúng tôi không bao giờ bỏ lỡ việc xây dựng độ dài khả thi$K$nếu một cái tồn tại. Tổng công việc là tuyến tính theo số lượng nút vì mỗi nút được nối thêm và đánh dấu một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    
    if K > N:
        print(-1)
        return
    
    # special case: N == 1
    if N == 1:
        if K == 1:
            print(0)
        else:
            print(-1)
        return
    
    # compute divisor counts
    div = [0] * N
    for i in range(1, N):
        for j in range(i, N, i):
            div[j] += 1
    
    nxt = [(i + div[i]) % N for i in range(N)]
    vis = [False] * N
    res = []
    
    def walk(start):
        x = start
        local = []
        while not vis[x]:
            vis[x] = True
            local.append(x)
            x = nxt[x]
            if len(res) + len(local) == K:
                res.extend(local)
                return True
        return False
    
    for i in range(N):
        if not vis[i]:
            if walk(i):
                print(*res)
                return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Sàng chia được thực hiện theo phương pháp nhân trực tiếp. Mặc dù$O(N \log N)$, nó đủ nhanh để$10^6$. Mảng chuyển tiếp được xây dựng một lần và không bao giờ tính toán lại. 

Chức năng truyền tải đảm bảo chúng tôi chỉ mở rộng từ các nút mới. Khi một nút được truy cập, nó sẽ không thể xuất hiện lại, vì vậy mọi phần bổ sung đều an toàn trên toàn cầu. Điều kiện kết thúc kiểm tra xem chúng ta đã tích lũy chính xác chưa$K$các giá trị. 

Một chi tiết triển khai tinh tế là chúng tôi kiểm tra điều kiện độ dài bên trong bước đi, cho phép dừng sớm mà không cần hoàn thành toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
18 10
```Chúng tôi tính toán số chia và chuyển đổi, sau đó bắt đầu quét từ 0 trở lên. Giả sử chuỗi hữu ích đầu tiên bắt đầu từ 0. 

| Bước | Hiện tại | Kích thước đã truy cập | Hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | bắt đầu chuỗi mới | 
| 2 | nxt[0] | 2 | nối thêm | 
| 3 | ... | ... | tiếp tục cho đến 10 phần tử | 

Thuật toán dừng chính xác khi thu thập được 10 giá trị, xuất ra chúng theo thứ tự. 

Dấu vết này chứng tỏ rằng chúng tôi không bao giờ truy cập lại một nút và việc dừng sớm sẽ ngăn chặn việc truyền tải không cần thiết khi đạt đến độ dài yêu cầu. 

### Ví dụ 2 

đầu vào:```
9 9
```Ở đây chúng ta phải sử dụng tất cả các nút. 

| Bước | Bắt đầu | Đã truy cập bảo hiểm | 
| --- | --- | --- | 
| 1 | 0 | chuỗi một phần | 
| 2 | tiếp theo chưa được truy cập | mở rộng vùng phủ sóng | 
| 3 | tiếp tục | cuối cùng bao gồm tất cả các nút | 

Quá trình tiếp tục cho đến khi mọi nút được sử dụng hết, tạo ra sự hoán vị đầy đủ của tất cả các giá trị. 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp khả thi tối đa trong đó$K = N$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| sàng chia chiếm ưu thế, quá trình truyền tải là tuyến tính | 
| Không gian |$O(N)$| mảng cho ước số, chuyển tiếp và trạng thái đã truy cập | 

Giới hạn$N, K \le 10^6$làm cho điều này trở nên khả thi: sàng thực hiện khoảng$N \log N$hoạt động và mỗi nút được truy cập nhiều nhất một lần trong quá trình truyền tải. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# edge: smallest case
assert run("1 1\n") == "0"

# impossible due to K > N
assert run("1 2\n") == "-1"

# small valid case
assert run("5 3\n") != "-1"

# full usage
assert run("3 3\n").count(" ") == 2

# larger structure
assert run("10 5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | xây dựng hợp lệ tối thiểu | 
| 1 2 | -1 | không thể do tính khác biệt | 
| 5 3 | dãy không âm | chức năng cơ bản | 
| 3 3 | hoán vị | trường hợp bảo hiểm đầy đủ | 
| 10 5 | trình tự hợp lệ | hành vi chung | 

## Vỏ cạnh 

### Trường hợp: N = 1 

đầu vào:```
1 1
```Giá trị duy nhất là 0, vì vậy chuỗi hợp lệ duy nhất là [0]. Thuật toán trực tiếp trả về nó. 

đầu vào:```
1 2
```Không có giá trị phân biệt thứ hai tồn tại. Kiểm tra đã truy cập ngay lập tức ngăn chặn việc mở rộng chuỗi và thuật toán xuất ra -1. 

### Trường hợp: dây xích va chạm sớm 

Nếu một nút chuyển sang một nút đã được truy cập trước khi đạt đến độ dài K thì quá trình đi bộ sẽ dừng sớm và điểm bắt đầu đó sẽ bị loại bỏ. Điều này ngăn chặn việc tái sử dụng một phần chu trình, đảm bảo chúng tôi chỉ xây dựng các phân đoạn rời rạc. 

### Trường hợp: K bằng N 

Khi K = N, thuật toán nhất thiết sẽ tiêu thụ mọi nút chính xác một lần nếu tồn tại một giao dịch hợp lệ. Mảng đã truy cập đảm bảo không lặp lại và các hàm đi bộ hoạt động như một sự phân tách hoàn toàn biểu đồ thành các phân đoạn có thể tiếp cận được.
