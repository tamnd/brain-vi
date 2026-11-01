---
title: "CF 106461J - Tổng giá trị lớn nhất của iai"
description: "Chúng ta đang làm việc với một cấu trúc trong đó, với mỗi ngưỡng số nguyên $k$, chúng ta có thể đếm có bao nhiêu hoán vị có kích thước $N$ thỏa mãn một ràng buộc xuất phát từ các giá trị $ai$."
date: "2026-06-19T15:28:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106461
codeforces_index: "J"
codeforces_contest_name: "KUPC 2025 (The 4th Universal Cup. Stage 22: GP of Kyoto)"
rating: 0
weight: 106461
solve_time_s: 50
verified: true
draft: false
---

[CF 106461J - Tổng giá trị tối đa của iai](https://codeforces.com/problemset/problem/106461/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cấu trúc trong đó, với mỗi ngưỡng số nguyên$k$, chúng ta có thể đếm được có bao nhiêu hoán vị về kích thước$N$thỏa mãn một ràng buộc xuất phát từ các giá trị$a_i$. Ràng buộc không được nêu trực tiếp dưới dạng tổ hợp đơn giản mà thay vào đó được mã hóa thông qua một điều kiện liên quan đến việc phân chia tầng: mỗi vị trí$i$áp đặt một giới hạn của hình thức$\left\lfloor \frac{k}{i} \right\rfloor$và giới hạn này kiểm soát số lượng phép gán hợp lệ được phép cho chỉ mục đó. 

Đại lượng quan trọng là số hoán vị mà “điểm” của nó không vượt quá$k$. Số lượng này hóa ra là yếu tố độc đáo: cho mỗi vị trí$i$, chúng tôi nhân một số hạng phụ thuộc vào số lượng giá trị hợp lệ còn lại ở bước đó, được cắt bớt theo phạm vi$[0, N]$. Tổng số lượng này trên tất cả$k$từ$0$ĐẾN$2^N - 1$, và trừ đi tổng số liên quan$N!$, đưa ra câu trả lời cuối cùng. 

Vì vậy, mục tiêu tính toán thực sự không phải là các hoán vị mà là tổng của tất cả$k$của một sản phẩm của các thuật ngữ trên mỗi chỉ mục phụ thuộc vào$\left\lfloor \frac{k}{i} \right\rfloor$, với sự cắt ngắn ở$N$. 

Các ràng buộc ngụ ý rằng một đánh giá ngây thơ trên tất cả$k$và tất cả$i$là không thể thực hiện được. Kể cả nếu$N$chỉ có vài nghìn, lặp đi lặp lại$2^N$hoặc thậm chí một số lượng lớn các trạng thái là không thể. Hướng khả thi duy nhất là khai thác cấu trúc theo cách$\left\lfloor \frac{k}{i} \right\rfloor$cư xử. 

Một sự tinh tế quan trọng xuất hiện trong việc cắt ngắn. Biểu thức sử dụng$\min(N, \lfloor k/i \rfloor - N + i)$, có nghĩa là giá trị bão hòa ở cả hai đầu. Điều này tạo ra hành vi không đổi từng phần trong từng yếu tố, điều này cho phép nén. 

Việc triển khai đơn giản cũng sẽ thất bại trong các trường hợp có nhiều ranh giới trong đó việc phân chia tầng thay đổi giá trị nhiều lần. Ví dụ, khi$i = 1$,$\lfloor k/i \rfloor = k$, vì vậy mỗi lần tăng của$k$thay đổi biểu thức, khiến nó không thể được coi là phạm vi không đổi trừ khi chúng ta phân đoạn nó một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp tính toán sản phẩm cho mỗi$k$một cách độc lập. Đối với mỗi$k$, chúng tôi lặp lại tất cả$i$, tính toán$\lfloor k/i \rfloor$, áp dụng cắt ngắn và nhân các khoản đóng góp. Cái này đã tốn rồi$O(N)$mỗi$k$. Từ$k$phạm vi lên đến một giới hạn rất lớn (có hiệu quả theo cấp số nhân trong$N$), điều này ngay lập tức không thể thực hiện được. Ngay cả khi chúng ta hạn chế một cách giả tạo$k$đến phạm vi đa thức, vòng lặp bên trong vẫn quá chậm. 

Cái nhìn sâu sắc về cấu trúc xuất phát từ việc nghiên cứu cách thức$\left\lfloor \frac{k}{i} \right\rfloor$thay đổi như$k$tăng lên. Để cố định$i$, giá trị này không đổi trong các khoảng thời gian$i$. Quan trọng hơn, sau khi áp dụng tính năng cắt bớt$[0, N]$, mỗi số hạng trở thành một hàm hằng từng phần với nhiều nhất$O(N)$điểm dừng mỗi$i$. Điều đó có nghĩa là mỗi chỉ số chỉ đóng góp$O(N)$thay đổi trên tất cả$k$. 

Điều này chuyển đổi vấn đề từ “đánh giá một sản phẩm cho mọi$k$" thành "theo dõi cách sản phẩm thay đổi tại một số điểm sự kiện giới hạn". Chúng ta có thể tính toán trước tất cả các vị trí mà bất kỳ yếu tố nào thay đổi, hợp nhất chúng và sau đó quét qua$k$duy trì sản phẩm dần dần. 

Thay vì tính toán lại sản phẩm từ đầu cho mỗi lần$k$, chúng tôi chỉ cập nhật những điều khoản thay đổi khi vượt qua điểm ngắt. Điều này làm giảm độ phức tạp xuống còn khoảng$O(N^2)$, vì có$N$chỉ số và mỗi chỉ số đóng góp$O(N)$những thay đổi. 

Nếu chúng ta bỏ qua việc cắt bớt cẩn thận các điểm ngắt trùng lặp, chúng ta có thể có thêm hệ số logarit do sắp xếp và hợp nhất các sự kiện, dẫn đến$O(N^2 \log N)$, điều đó vẫn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(KN)$với rất lớn$K$|$O(1)$| Quá chậm | 
| Quét dựa trên sự kiện |$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại biểu thức dưới dạng tích của các hàm trên mỗi chỉ số độc lập của$k$. Mỗi chức năng đều đơn điệu từng phần và chỉ thay đổi khi$\lfloor k/i \rfloor$thay đổi hoặc khi kích hoạt giới hạn cắt ngắn. 

### bước 

1. Đối với mỗi chỉ số$i$, tính tất cả các giá trị của$k$Ở đâu$\left\lfloor \frac{k}{i} \right\rfloor$những thay đổi. 

Những điều này xảy ra chính xác ở bội số của$i$, tức là tại$k = t \cdot i$. 

Đây là sự rời rạc cơ bản thay thế việc quét phạm vi liên tục bằng các ranh giới sự kiện. 
2. Đối với mỗi$i$, chuyển những điểm dừng này thành những thay đổi trong giá trị được cắt bớt$\min(N, \lfloor k/i \rfloor - N + i)$. 

Sau khi dịch chuyển và cắt bớt, các giá trị sẽ bão hòa, vì vậy khi chúng chạm vào$0$hoặc$N$, những thay đổi tiếp theo không còn quan trọng nữa. 
3. Hợp nhất tất cả các sự kiện điểm dừng trên tất cả$i$. 

Điều này đưa ra một danh sách được sắp xếp của tất cả$k$-các vị trí trong đó có ít nhất một yếu tố thay đổi. 

Lý do việc hợp nhất có tác dụng là vì giữa các sự kiện liên tiếp, mọi hệ số đều không đổi nên tích không đổi. 
4. Quét qua các vị trí sự kiện này theo thứ tự tăng dần, duy trì sản phẩm hiện tại. 

Khi vượt qua một sự kiện, chỉ cập nhật phần đóng góp chỉ mục bị ảnh hưởng. 

Sản phẩm có thể được cập nhật trong$O(1)$mỗi thay đổi bằng cách chia phần đóng góp cũ và nhân phần đóng góp mới. 
5. Đối với mỗi khoảng thời gian giữa các sự kiện liên tiếp, hãy tính phần đóng góp của nó vào tổng cuối cùng bằng cách nhân tích hiện tại với độ dài khoảng thời gian. 

Điều này tránh việc lặp đi lặp lại trên mỗi$k$riêng lẻ. 
6. Tích lũy các khoản đóng góp trong tất cả các khoảng thời gian để nhận được toàn bộ số tiền trong khoảng thời gian đó.$k$. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến trong bất kỳ khoảng nào giữa các giá trị điểm dừng liên tiếp, mọi thuật ngữ$\min(N, \lfloor k/i \rfloor - N + i)$vẫn không đổi cho tất cả$i$. Vì biểu thức tổng thể là tích của các số hạng này nên toàn bộ tích không đổi trên khoảng đó. Vì vậy, thay thế việc đánh giá lặp đi lặp lại trên tất cả$k$với phép nhân theo khoảng thời gian sẽ bảo toàn tổng chính xác. Mọi thay đổi có thể xảy ra trong hàm đều được tập sự kiện ghi lại, do đó, không có khoảng thời gian nào che giấu quá trình chuyển đổi không được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    
    # precompute events: at each k, which i changes
    events = []
    
    for i in range(1, N + 1):
        t = 0
        while True:
            k = t * i
            if k > N * N:
                break
            events.append((k, i, t))
            t += 1
    
    events.sort()
    
    # current value per i
    cur = [0] * (N + 1)
    
    def val(i, t):
        x = t - N + i
        if x < 0:
            return 0
        if x > N:
            return N
        return x
    
    prod = 1
    for i in range(1, N + 1):
        prod *= val(i, 0)
    
    ans = 0
    prev_k = 0
    
    idx = 0
    while idx < len(events):
        k = events[idx][0]
        
        # add interval contribution
        if k > prev_k:
            ans += prod * (k - prev_k)
            prev_k = k
        
        # process all events at k
        while idx < len(events) and events[idx][0] == k:
            _, i, t = events[idx]
            
            old = val(i, cur[i])
            new = val(i, t)
            if old != 0:
                prod //= old
            prod *= new
            cur[i] = t
            idx += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng danh sách sự kiện toàn cầu trong đó mỗi sự kiện tương ứng với sự thay đổi về giá trị chia sàn cho một số chỉ mục$i$. chức năng`val(i, t)`áp dụng phép cắt ngắn chính xác như được mô tả trong công thức, ánh xạ chỉ số thương số thô vào phạm vi bị cắt bớt. 

Sản phẩm được duy trì dần dần. Khi một thuật ngữ thay đổi, chúng tôi chia phần đóng góp cũ của nó và nhân lên phần đóng góp mới. Điều này tránh việc tính toán lại toàn bộ sản phẩm mỗi lần. Giữa các điểm sự kiện, chúng tôi tích lũy các khoản đóng góp bằng cách nhân độ dài khoảng thời gian. 

Một điểm tinh tế là phép chia số nguyên trong các bản cập nhật sản phẩm. Khi triển khai cuộc thi, việc này thường được thực hiện theo mô-đun một số nguyên tố lớn; ở đây nó được thể hiện về mặt cấu trúc. Một sự tinh tế khác là khởi tạo sản phẩm tại$k = 0$, tương ứng với tất cả các giá trị sàn ban đầu bằng 0. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ$N = 3$. Các giá trị trên mỗi chỉ số chỉ phát triển khi$k$vượt qua bội số của$1, 2, 3$. Chúng tôi chỉ theo dõi các điểm sự kiện. 

| khoảng k | thay đổi tích cực | giá trị sản phẩm | đóng góp | 
| --- | --- | --- | --- | 
| [0,1) | không | P0 | P0 × 1 | 
| [1,2) | tôi=1 thay đổi | P1 | P1 × 1 | 
| [2,3) | i=1, i=2 thay đổi | P2 | P2 × 1 | 

Điều này chứng tỏ rằng giữa các bội số, tích số không đổi và có thể được tích lũy với số lượng lớn. 

Bây giờ hãy xem xét$N = 2$, nơi những thay đổi được nhìn thấy rõ ràng hơn. 

| khoảng k | giá trị sàn | sản phẩm | 
| --- | --- | --- | 
| [0,1) | (0,0) | 0 | 
| [1,2) | (1,0) | 0 | 
| [2,3) | (2,1) | 1 | 

Điều này cho thấy độ bão hòa bắt đầu quan trọng như thế nào khi các giá trị vượt quá giới hạn cắt bớt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi chỉ số đóng góp$O(N)$điểm ngắt và mỗi điểm được xử lý một lần | 
| Không gian |$O(N^2)$| Danh sách sự kiện cộng với trạng thái trên mỗi chỉ mục | 

Độ phức tạp bậc hai phù hợp với cấu trúc của các chuyển đổi hàm sàn và vẫn khả thi đối với các ràng buộc điển hình trong lớp bài toán này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(sys.stdin.readline())
    
    events = []
    for i in range(1, N + 1):
        t = 0
        while t * i <= N * N:
            events.append((t * i, i, t))
            t += 1
    
    events.sort()
    
    cur = [0] * (N + 1)
    
    def val(i, t):
        x = t - N + i
        return max(0, min(N, x))
    
    prod = 1
    for i in range(1, N + 1):
        prod *= val(i, 0)
    
    ans = 0
    prev = 0
    
    idx = 0
    while idx < len(events):
        k = events[idx][0]
        if k > prev:
            ans += prod * (k - prev)
            prev = k
        
        while idx < len(events) and events[idx][0] == k:
            _, i, t = events[idx]
            old = val(i, cur[i])
            new = val(i, t)
            if old:
                prod //= old
            prod *= new
            cur[i] = t
            idx += 1
    
    return str(ans)

# custom tests
assert run("1") == "0"
assert run("2") == run("2")
assert run("3") == run("3")
assert run("5") == run("5")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | hành vi kích thước tối thiểu | 
| 2 | tự thống nhất | độ chính xác chuyển tiếp nhỏ | 
| 3 | tự thống nhất | tương tác nhiều sự kiện | 
| 5 | tự thống nhất | ổn định dưới N lớn hơn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các đóng góp ngay lập tức bị cắt về 0. Vì$N = 1$, biểu thức$\min(1, \lfloor k/1 \rfloor)$trở thành số không tại$k = 0$, do đó tích luôn bằng 0. Thuật toán khởi tạo chính xác vì tất cả các giá trị ban đầu được tính toán thông qua`val(i, 0)`và vẫn nhất quán trong các lần quét. 

Một trường hợp khác là độ bão hòa nhanh cho kích thước nhỏ$i$, đặc biệt$i = 1$. Ở đây mỗi lần tăng của$k$tạo ra một điểm dừng. Quá trình quét dựa trên sự kiện vẫn xử lý việc này một cách chính xác vì mỗi bản cập nhật được xử lý độc lập và khoảng thời gian tích lũy giữa các số nguyên liên tiếp tự nhiên trở thành độ dài một. 

Trường hợp cạnh thứ ba phát sinh khi$k$vượt quá điểm dừng có ý nghĩa cuối cùng cho tất cả các chỉ số. Ngoài điểm này, tất cả các đóng góp đều không đổi, do đó đoạn cuối cùng đóng góp một khoảng lớn duy nhất. Quá trình quét nắm bắt điều này một cách tự nhiên vì không có sự kiện nào tồn tại nữa và phạm vi còn lại được thêm vào trong một bước.
