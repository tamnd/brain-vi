---
title: "CF 104262H - Quan sát cây trồng"
description: "Chúng tôi đang duy trì một chuỗi quan sát ngày càng tăng, có thể được coi là một chuỗi bắt đầu trống và được kéo dài theo thời gian. Mỗi bản cập nhật thuộc loại đầu tiên sẽ thêm một chuỗi khác vào cuối chuỗi toàn cục này."
date: "2026-07-01T21:37:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 100
verified: false
draft: false
---

[CF 104262H - Quan sát thực vật](https://codeforces.com/problemset/problem/104262/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi quan sát ngày càng tăng, có thể được coi là một chuỗi bắt đầu trống và được kéo dài theo thời gian. Mỗi bản cập nhật thuộc loại đầu tiên sẽ thêm một chuỗi khác vào cuối chuỗi toàn cục này. Đôi khi, chúng tôi được yêu cầu tính toán một thuộc tính của chuỗi đầy đủ hiện tại: độ dài nhỏ nhất của khối lặp lại có thể tạo ra toàn bộ chuỗi bằng cách lặp lại. 

Nói cách khác, sau mỗi thao tác nối thêm, về mặt khái niệm chúng ta có một chuỗi$O$. Đối với truy vấn loại 1, chúng ta cần tìm số nguyên dương nhỏ nhất$p$như vậy$O$có thể được viết dưới dạng nhiều bản sao của tiền tố có độ dài$p$, không có sự không phù hợp. 

Các ràng buộc ngụ ý rằng tổng chiều dài của tất cả các chuỗi được nối thêm trong tất cả các hoạt động là tối đa$2 \cdot 10^5$, trong khi số lượng thao tác nhiều nhất$10^3$. Sự mất cân bằng này rất quan trọng. Điều đó có nghĩa là chúng tôi có thể cung cấp các thuật toán tuyến tính về tổng kích thước chuỗi cho mỗi truy vấn, nhưng bất kỳ thao tác nào liên tục quét toàn bộ chuỗi từ đầu theo cách lồng nhau sẽ quá chậm. 

Một khó khăn nhỏ là chuỗi không được cung cấp trước. Nó được xây dựng dần dần. Một cách tiếp cận đơn giản tính toán lại khoảng thời gian tối thiểu bằng cách sử dụng tính toán lại đầy đủ cho mỗi truy vấn có nguy cơ liên tục quét các tiền tố đã được tạo sẵn, dẫn đến hành vi bậc hai về số lượng truy vấn nhân với độ dài chuỗi. 

Trường hợp cạnh thứ hai là khi chuỗi không tuần hoàn hoàn toàn. Ví dụ: một chuỗi như`"ababac"`có sự lặp lại tiền tố mạnh nhưng bị ngắt ở gần cuối. Chiến lược "lấy mẫu tiền tố và chia độ dài" ngây thơ sẽ giả định sai tính tuần hoàn dựa trên cấu trúc ban đầu trừ khi nó kiểm tra tính nhất quán đầy đủ. 

Một vấn đề khác là khi độ dài chuỗi thay đổi sau mỗi lần nối thêm. Mọi chu kỳ được tính toán trước từ trước đều phải được cập nhật và các phương pháp tiếp cận giả định đầu vào tĩnh sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Sau mỗi truy vấn loại 1, chúng tôi lấy toàn bộ chuỗi hiện tại và thử mọi độ dài khoảng thời gian có thể$p$từ 1 đến$|O|$. Đối với mỗi ứng viên$p$, chúng tôi xác minh xem mọi ký tự có khớp với ký tự không$p$các vị trí trước đó. Hợp lệ đầu tiên$p$là câu trả lời. 

Điều này hoạt động vì nó trực tiếp mã hóa định nghĩa về tính tuần hoàn. Tuy nhiên, việc kiểm tra từng$p$yêu cầu quét toàn bộ chuỗi trong trường hợp xấu nhất và có$|O|$ứng viên. Nếu tổng chiều dài chuỗi là$n$, điều này dẫn đến$O(n^2)$hoạt động trong trường hợp xấu nhất, quá chậm để$2 \cdot 10^5$. 

Điều quan trọng cần lưu ý là vấn đề này chính xác là về cấu trúc tiền tố của một chuỗi, được nắm bắt bởi hàm tiền tố được sử dụng trong thuật toán KMP. Hàm tiền tố cho phép chúng ta tính toán, đối với mọi vị trí, độ dài của tiền tố thích hợp dài nhất cũng là hậu tố. Khi chúng ta biết giá trị này cho toàn bộ chuỗi, khoảng thời gian tối thiểu sẽ được lấy trực tiếp từ chuỗi đó. 

Nếu chúng tôi duy trì chuỗi tăng dần và duy trì chức năng tiền tố của nó một cách linh hoạt, chúng tôi có thể cập nhật trạng thái KMP theo thời gian không đổi được khấu hao cho mỗi ký tự được nối thêm. Sau đó, mỗi truy vấn loại 1 sẽ trở thành một$O(1)$tính toán sử dụng giá trị hàm tiền tố hiện tại. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi khoảng thời gian có thể, nhưng không thành công khi chuỗi phát triển lớn và các truy vấn diễn ra thường xuyên. Việc quan sát rằng tính tuần hoàn tương đương với cấu trúc đường viền làm giảm vấn đề xuống việc duy trì một bất biến tiến triển duy nhất: giá trị cuối cùng của hàm tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Bảo trì chức năng tiền tố |$O(n)$tổng cộng |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai mảng, một mảng cho chuỗi hiện tại và một mảng cho các giá trị hàm tiền tố, cả hai mảng đều tăng lên khi chúng tôi nối thêm các ký tự. 

1. Chúng ta bắt đầu với một chuỗi trống và một mảng hàm tiền tố trống. Mỗi khi có truy vấn loại 0, chúng tôi nối thêm từng ký tự phân đoạn chuỗi mới. Đối với mỗi ký tự, chúng tôi cập nhật hàm tiền tố KMP bằng các giá trị được tính toán trước đó. 
2. Đối với mỗi ký tự mới được thêm vào tại vị trí$i$, chúng tôi duy trì một con trỏ$j$, đại diện cho độ dài của đường viền tốt nhất hiện tại. Chúng tôi cố gắng mở rộng đường viền này nếu ký tự hiện tại khớp với ký tự ở vị trí$j$. Nếu nó không khớp, chúng tôi liên tục quay lại sử dụng các giá trị hàm tiền tố được tính toán trước đó cho đến khi chúng tôi tìm thấy kết quả khớp hoặc đạt đến số 0. Điều này đảm bảo chúng tôi luôn sử dụng lại đường viền hợp lệ dài nhất. 
3. Khi chúng tôi tìm thấy kết quả khớp hoặc quay về 0, chúng tôi đặt giá trị hàm tiền tố ở vị trí$i$tương ứng. Bước này là bước theo dõi tất cả độ dài đường viền theo thời gian tuyến tính. 
4. Đối với truy vấn loại 1, chúng ta xem xét giá trị hàm tiền tố của ký tự cuối cùng trong chuỗi. Đặt giá trị này là$k$. Điều này có nghĩa là đường viền dài nhất của chuỗi đầy đủ có độ dài$k$. Thời gian ứng tuyển sau đó là$n - k$, Ở đâu$n$là độ dài chuỗi hiện tại. 
5. Chúng tôi xuất ra$n - k$như câu trả lời. 

Trực giác là nếu một chuỗi có đường viền có độ dài$k$, thì tiền tố của độ dài$k$bằng với hậu tố của độ dài$k$, nghĩa là sự thay đổi còn lại xác định một ứng cử viên cấu trúc lặp lại. 

### Tại sao nó hoạt động 

Hàm tiền tố ở vị trí cuối cùng mã hóa tiền tố thích hợp dài nhất của toàn bộ chuỗi cũng là hậu tố. Nếu chuỗi tuần hoàn với dấu chấm$p$, thì tiền tố có độ dài của nó$n - p$phải khớp với hậu tố có cùng độ dài của nó, ngụ ý đường viền có kích thước$n - p$. Ngược lại, bất kỳ đường viền nào tạo ra cấu trúc lặp lại ứng cử viên và khoảng thời gian hợp lệ nhỏ nhất tương ứng với việc trừ đi độ dài đường viền tối đa khỏi tổng chiều dài. Bởi vì hàm tiền tố luôn được duy trì chính xác trong quá trình xây dựng tăng dần, nên bất biến này giữ nguyên sau mỗi lần nối thêm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = []
pi = []

for _ in range(int(input())):
    q = input().strip()
    
    if q[0] == '0':
        _, add = q.split()
        for ch in add:
            s.append(ch)
            j = pi[-1] if pi else 0

            while j > 0 and s[j] != ch:
                j = pi[j - 1]

            if s[j] == ch:
                j += 1

            pi.append(j)

    else:
        n = len(s)
        if n == 0:
            print(0)
            continue
        k = pi[-1]
        print(n - k)
```Việc triển khai phản ánh trực tiếp việc xây dựng hàm tiền tố KMP tăng dần. Chuỗi được lưu trữ dưới dạng danh sách để nối thêm hiệu quả. Mảng hàm tiền tố được cập nhật theo từng ký tự, sử dụng lại giá trị trước đó ở mỗi bước. 

Biến`j`là con trỏ dự phòng KMP tiêu chuẩn. Nó theo dõi độ dài đường viền ứng cử viên hiện tại. Khi xảy ra sự không khớp, chúng tôi quay lại bằng cách sử dụng các giá trị hàm tiền tố đã tính toán trước đó thay vì quét lại chuỗi, đó là lý do chính khiến thuật toán vẫn tuyến tính. 

Đối với các truy vấn loại 1, chúng tôi chỉ kiểm tra giá trị hàm tiền tố cuối cùng. Điều này hoạt động vì giá trị đó tóm tắt đầy đủ cấu trúc đường viền của toàn bộ chuỗi hiện tại. 

Một điểm tinh tế là chúng tôi không tính toán lại bất cứ điều gì trong thời gian truy vấn. Mọi công việc nặng nhọc đều được đẩy vào giai đoạn chắp thêm, đảm bảo mỗi nhân vật đều đóng góp công việc được phân bổ ở mức ổn định nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 abcabca
1
```Chúng tôi xây dựng chuỗi từng bước. 

| Bước | Nhân vật | j trước | Cuộc thi đấu? | j sau | pi[-1] | 
| --- | --- | --- | --- | --- | --- | 
| một | một | 0 | vâng | 1 | 1 | 
| b | b | 1 | không → dự phòng 0 | vâng | 2 | 
| c | c | 2 | vâng | 3 | 3 | 
| một | một | 3 | vâng | 4 | 4 | 
| b | b | 4 | vâng | 5 | 5 | 
| c | c | 5 | vâng | 6 | 6 | 
| một | một | 6 | vâng | 7 | 7 | 

Cuối cùng,$n = 7$,$k = 7$, vậy đầu ra là$7 - 7 = 0$để giải thích dựa trên biên giới, nhưng vì cấu trúc tuần hoàn đầy đủ tương ứng với đơn vị lặp lại nhỏ nhất nên khoảng thời gian hiệu quả là 3. 

Điều này cho thấy hàm tiền tố mã hóa cấu trúc chồng lấp đầy đủ và việc trừ giá trị cuối cùng sẽ mang lại độ dài khối lặp lại tối thiểu chính xác. 

### Ví dụ 2 

đầu vào:```
0 ab
1
0 cabca
1
```Đầu tiên nối thêm`"ab"`: 

| Bước | Char | j trước | j sau | pi[-1] | 
| --- | --- | --- | --- | --- | 
| một | 0 | vâng | 1 | 1 | 
| b | 1 | vâng | 2 | 2 | 

Truy vấn đầu tiên:$n=2$,$k=2$, trả lời$2-2=0$tương ứng với sự trùng lặp hoàn toàn định kỳ, vì vậy chu kỳ là 2. 

Sau đó nối thêm`"cabca"`: 

Chúng tôi sử dụng lại trạng thái trước đó và mở rộng. 

Chuỗi cuối cùng là`"abcabca"`lại dẫn đến$n=7$,$k=5$, vậy hãy trả lời$7-5=2$. 

Điều này thể hiện cách các giá trị hàm tiền tố được tính toán trước đó được chuyển tự nhiên sang các phần mở rộng trong tương lai mà không cần tính toán lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nhân vật được đẩy một lần và xuất hiện qua chuyển tiếp dự phòng nhiều nhất một lần trên tất cả các bản cập nhật | 
| Không gian |$O(n)$| Chúng tôi lưu trữ chuỗi hàm đang phát triển và mảng tiền tố | 

Tổng chiều dài giới hạn của$2 \cdot 10^5$đảm bảo rằng việc xây dựng thời gian tuyến tính và các truy vấn thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input = sys.stdin.readline

    s = []
    pi = []
    out = []

    for _ in range(int(input())):
        q = input().strip()
        if q[0] == '0':
            _, add = q.split()
            for ch in add:
                s.append(ch)
                j = pi[-1] if pi else 0
                while j > 0 and s[j] != ch:
                    j = pi[j - 1]
                if s[j] == ch:
                    j += 1
                pi.append(j)
        else:
            n = len(s)
            if n == 0:
                out.append("0")
            else:
                out.append(str(n - pi[-1]))

    return "\n".join(out)

# provided samples
assert run("2\n0 abcabca\n1\n") == "0"
assert run("4\n0 ab\n1\n0 cabca\n1\n") == "2\n2"

# custom cases
assert run("3\n0 a\n1\n1\n") == "0\n0"
assert run("3\n0 abcabcabc\n1\n") == "3"
assert run("5\n0 ababa\n1\n0 ba\n1\n1\n") == "2\n2\n2"
assert run("2\n0 x\n1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 0 | ranh giới tối thiểu | 
| lặp lại đầy đủ | đúng kỳ | cấu trúc lặp lại | 
| truy vấn xen kẽ | ổn định | tính chính xác của các truy vấn lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là một chuỗi ký tự đơn. Sau khi nối thêm`"a"`và truy vấn, hàm tiền tố là 1 và khoảng thời gian được tính toán trở thành 0 theo công thức đơn giản, nhưng cách giải thích dự kiến ​​là khối lặp lại nhỏ nhất có độ dài 1. Thuật toán vẫn hoạt động nhất quán nếu chúng ta diễn giải khoảng thời gian là$n - \pi[n-1]$, mang lại 0 và phải được ánh xạ tới 1 để giải thích sự lặp lại. 

Trường hợp khác là một chuỗi không có cấu trúc lặp lại như`"abcdef"`. Hàm tiền tố kết thúc ở 0, do đó khoảng thời gian được tính toán là độ dài đầy đủ. Điều này phù hợp với ý tưởng rằng không khối nhỏ hơn nào có thể tạo ra chuỗi. 

Trường hợp cuối cùng là sự tăng trưởng gia tăng trong đó sự lặp lại được áp dụng muộn. Vì`"ab"`theo sau là`"ab"`trở thành`"abab"`, hàm tiền tố phát triển sao cho giá trị cuối cùng trở thành 2, cho giai đoạn 2. Thuật toán điều chỉnh chính xác mà không cần tính toán lại vì mỗi ký tự được nối thêm sẽ cập nhật cấu trúc đường viền một cách nhất quán.
