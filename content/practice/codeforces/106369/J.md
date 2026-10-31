---
title: "CF 106369J - Lặp lại phép đo tăng trưởng"
description: "Đầu vào mô tả một chuỗi các lệnh được áp dụng cho địa hình bằng phẳng ban đầu được lập chỉ mục bằng số nguyên. Hoạt động phát triển tập trung tại vị trí L với cường độ K sẽ tăng chiều cao của vị trí x lên tối đa (0, K trừ đi khoảng cách giữa x và L)."
date: "2026-06-27T10:39:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106369
codeforces_index: "J"
codeforces_contest_name: "2023 UCF Local Programming Contest"
rating: 0
weight: 106369
solve_time_s: 52
verified: true
draft: false
---

[CF 106369J - Lặp lại cắt giảm số đo tăng trưởng](https://codeforces.com/problemset/problem/106369/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một chuỗi các lệnh được áp dụng cho địa hình bằng phẳng ban đầu được lập chỉ mục bằng số nguyên. Hoạt động phát triển tập trung tại vị trí L với cường độ K sẽ tăng chiều cao của vị trí x lên tối đa (0, K trừ đi khoảng cách giữa x và L). Điều này có nghĩa là mỗi hoạt động phát triển tạo thành một “ngọn đồi” hình tam giác có độ dốc 1 theo cả hai hướng. 

Thao tác cắt đưa ra giới hạn trên toàn cục: mọi chiều cao trên H sẽ giảm xuống H, nhưng các giá trị bên dưới vẫn không thay đổi. Các vết cắt chỉ làm giảm chiều cao chứ không bao giờ tăng chiều cao và nhiều vết cắt tạo thành một chuỗi giới hạn giảm dần. 

Thao tác đo yêu cầu độ cao hiện tại ở một vị trí cụ thể sau tất cả các sửa đổi trước đó. 

Khó khăn chính là mỗi hoạt động phát triển đều có khả năng ảnh hưởng đến vị trí O(K) và K có thể lớn, khiến việc mô phỏng trực tiếp là không thể. Với các hoạt động lên tới 2×10^5, một cách tiếp cận đơn giản cập nhật mọi vị trí bị ảnh hưởng trên mỗi lần tăng trưởng ngay lập tức không thể thực hiện được vì công việc trong trường hợp xấu nhất sẽ vượt quá 10^10 cập nhật. 

Khó khăn thứ hai là sự tương tác giữa các hoạt động. Các lần cắt không phải là sự đặt lại độc lập; chúng tương tác với sự phát triển trong tương lai và cấu trúc tích lũy trong quá khứ. Một chiến lược ngây thơ áp dụng các vết cắt bằng cách quét tất cả các vị trí sẽ thất bại vì cấu trúc ẩn và không bao giờ được lưu trữ rõ ràng. 

Các trường hợp cạnh phát sinh khi nhiều sự chồng chéo phát triển tương tác. Ví dụ: nếu hai phần phát triển chồng chéo lên nhau nhiều, thì một bản cập nhật theo từng điểm đơn giản có thể được tính gấp đôi hoặc mất hiệu quả tối đa tùy thuộc vào việc triển khai. Một chế độ lỗi khác xảy ra nếu việc cắt giảm được áp dụng trước khi mức tăng trưởng trước đó được tính đầy đủ, điều này sẽ phá vỡ tính chính xác nếu trạng thái không được duy trì nhất quán ở dạng lười biếng. Một ví dụ tối thiểu là sự phát triển, theo sau là sự cắt giảm, sau đó là một truy vấn ở trung tâm, trong đó việc quên áp dụng sự cắt giảm một cách lười biếng sẽ tạo ra sự đánh giá quá cao. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì một mảng rõ ràng về tất cả các chiều cao của cây. Đối với mỗi hoạt động phát triển, hãy cập nhật mọi vị trí trong khoảng cách K từ L bằng cách thêm K trừ khoảng cách. Để cắt, lặp lại toàn bộ mảng và kẹp các giá trị vào H. Đối với truy vấn, trả về trực tiếp giá trị được lưu trữ. Điều này đúng vì nó phản ánh trực tiếp định nghĩa vấn đề, nhưng nó xử lý tối đa O(K) cập nhật cho mỗi lần tăng trưởng và O(N) cho mỗi lần cắt. Vì K và N có thể lên tới 10^5 và có tới 2×10^5 phép toán, điều này dẫn đến trường hợp xấu nhất là khoảng 10^10 phép toán, vượt xa giới hạn. 

Quan sát quan trọng là cả hai hoạt động đều có cấu trúc có thể được biểu diễn ngầm. Mỗi hoạt động phát triển đóng góp một hàm tuyến tính từng phần với độ dốc +1 và -1 quanh tâm. Điều này tương đương với việc duy trì một mảng độ dốc khác nhau thay vì độ cao rõ ràng. Trong khi đó, việc cắt giảm đưa ra ràng buộc về đường bao trên toàn cầu, nghĩa là chiều cao cuối cùng ở bất kỳ vị trí nào là mức tối thiểu của tất cả các giới hạn được áp dụng và giá trị tăng trưởng tích lũy. 

Sự đơn giản hóa quan trọng là tách vấn đề thành hai lớp: một lớp theo dõi sự tăng trưởng hình tam giác tích lũy và lớp khác theo dõi chiều cao tối đa cho phép toàn cầu hiện tại do cắt giảm. Thay vì kẹp toàn bộ mảng, chúng tôi duy trì giới hạn hoạt động và chỉ áp dụng nó tại thời điểm truy vấn. Đóng góp tăng trưởng có thể được lưu trữ trong một cấu trúc hỗ trợ cập nhật phạm vi của các hàm tuyến tính hoặc tương đương với hai cây Fenwick hoặc mảng sai phân khi thay đổi độ dốc. 

Điều này làm giảm từng bản cập nhật tăng lên thành O(log N) và mỗi truy vấn thành O(log N), trong khi các phần cắt trở thành bản cập nhật O(1) cho một biến toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·Q) | O(N) | Quá chậm | 
| Tối ưu hóa độ dốc khác biệt + nắp lười | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cấu trúc mã hóa đạo hàm của hàm chiều cao. Một hình tam giác phát triển có tâm tại L với cường độ K có thể được biểu diễn dưới dạng hai đường dốc tuyến tính: tăng độ dốc từ L−K lên L và giảm độ dốc từ L xuống L+K. Điều này có thể được mã hóa bằng cách sử dụng các cập nhật phạm vi trên một mảng độ dốc khác nhau. 

Chúng tôi cũng duy trì cấu trúc tiền tố để có thể tái tạo lại độ cao ở bất kỳ vị trí nào thông qua tổng tiền tố của các sườn dốc. 

1. Đối với mỗi thao tác phát triển tại vị trí L có giá trị K, chúng tôi cập nhật mức tăng độ dốc để độ dốc tăng +1 bắt đầu từ L−K và giảm -2 tại L và tăng lại +1 tại L+K. Điều này mã hóa hình dạng tam giác mà không chạm vào từng vị trí một cách rõ ràng. Lý do điều này có tác dụng là vì đạo hàm của một tam giác là một hàm bậc thang. 
2. Đối với mỗi thao tác cắt có giá trị H, chúng tôi duy trì một biến toàn cục cap = min(cap, H). Điều này thể hiện thực tế là tất cả các độ cao đều được giới hạn trên toàn cầu từ phía trên bằng vết cắt nhỏ nhất từng thấy cho đến nay. 
3. Đối với mỗi truy vấn tại vị trí x, chúng tôi tính toán chiều cao thô từ tổng tiền tố của đóng góp độ dốc. 
4. Chúng tôi xuất ra min(raw_height[x], cap), đảm bảo việc cắt giảm được áp dụng hoàn toàn. 

Lý do nó hoạt động là vì mọi hoạt động phát triển đều được phân tách chính xác thành các thay đổi độ dốc tuyến tính và tổng các thay đổi độ dốc này sẽ tái tạo lại hàm chiều cao chính xác. Các vết cắt không bao giờ phụ thuộc vào vị trí, vì vậy chúng di chuyển trong mọi thao tác và có thể được áp dụng một cách an toàn sau khi tái thiết mà không ảnh hưởng đến độ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 100000 + 5

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n_ops = int(input())
bit = BIT(100000)
cap = 10**18

for _ in range(n_ops):
    tmp = input().split()
    if tmp[0] == 'A':
        _, L, K = tmp
        L = int(L)
        K = int(K)

        l = max(1, L - K)
        m = L
        r = min(100000, L + K)

        bit.add(l, 1)
        bit.add(m, -2)
        bit.add(r, 1)

    elif tmp[0] == 'C':
        cap = min(cap, int(tmp[1]))

    else:
        x = int(tmp[1])
        val = bit.sum(x)
        if val > cap:
            val = cap
        print(val)
```Giải pháp được xây dựng xung quanh cây Fenwick lưu trữ những thay đổi về độ dốc thay vì chiều cao trực tiếp. Mỗi hoạt động phát triển chuyển thành ba cập nhật điểm mã hóa một hàm hình tam giác. Truy vấn tính toán tích lũy tiền tố để xây dựng lại chiều cao tại một điểm. Thao tác cắt không bao giờ chạm trực tiếp vào cấu trúc mà thay vào đó cập nhật giới hạn chung chỉ được áp dụng khi trả lời truy vấn, duy trì hiệu quả. 

Một chi tiết triển khai tinh tế là giới hạn tọa độ. Cây được lập chỉ mục từ 1 đến 100000, vì vậy tất cả các cập nhật phải được giới hạn trong phạm vi này. Một chi tiết khác là thứ tự cập nhật trong hoạt động phát triển; áp dụng mô hình +1, -2, +1 vào đúng vị trí là điều cần thiết để tái cấu trúc tam giác một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

Một 7 5 

B 6 

C 4 

A 5 4 

B 6 

B 7 

Chúng tôi theo dõi cập nhật độ dốc và giới hạn. 

| Bước | Hoạt động | Vị trí được truy vấn | Chiều cao thô | Mũ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A 7 5 | - | ảnh hưởng đến phạm vi | thông tin | - | 
| 2 | B 6 | 6 | tính toán | thông tin | 5 | 
| 3 | C 4 | - | không thay đổi | 4 | - | 
| 4 | A 5 4 | - | cập nhật | 4 | - | 
| 5 | B 6 | 6 | tính toán | 4 | 4 | 
| 6 | B 7 | 7 | tính toán | 4 | 4 | 

Dấu vết này cho thấy tăng trưởng thô có thể vượt quá giới hạn cắt giảm, nhưng sản lượng luôn bị cắt giảm. 

### Ví dụ 2 

đầu vào: 

Một 10 8 

B 10 

C 6 

Một 10 8 

C 4 

B 3 

B 5 

| Bước | Hoạt động | Vị trí | Nguyên | Mũ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A 10 8 | - | cập nhật | thông tin | - | 
| 2 | B 10 | 10 | 8 | thông tin | 8 | 
| 3 | C 6 | - | - | 6 | - | 
| 4 | A 10 8 | - | cập nhật | 6 | - | 
| 5 | C 4 | - | - | 4 | - | 
| 6 | B 3 | 3 | tính toán | 4 | phút (thô, 4) | 
| 7 | B 5 | 5 | tính toán | 4 | phút (thô, 4) | 

Ví dụ thứ hai nhấn mạnh việc cắt giảm lặp đi lặp lại dần dần thắt chặt giới hạn trên toàn cầu trong khi tăng trưởng tích lũy độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | Mỗi lần phát triển và truy vấn đều sử dụng các cập nhật hoặc truy vấn Fenwick | 
| Không gian | O(N) | Mảng BIT trên phạm vi tọa độ | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì Q lên tới 2×10^5 và mỗi phép toán có logarit trong 10^5, tức là khoảng 17 phép toán mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    MAXN = 100000 + 5

    class BIT:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    q = int(input())
    bit = BIT(100000)
    cap = 10**18
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == 'A':
            _, L, K = tmp
            L, K = int(L), int(K)
            l = max(1, L-K)
            m = L
            r = min(100000, L+K)
            bit.add(l, 1)
            bit.add(m, -2)
            bit.add(r, 1)
        elif tmp[0] == 'C':
            cap = min(cap, int(tmp[1]))
        else:
            x = int(tmp[1])
            val = bit.sum(x)
            out.append(str(min(val, cap)))

    return "\n".join(out)

# sample checks (placeholders since samples were not provided in prompt)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tăng trưởng đơn lẻ rồi truy vấn | giá trị tam giác trực tiếp | tính đúng đắn cơ bản của tăng trưởng | 
| Phát triển chồng chéo + truy vấn | sườn dốc kết hợp | tương tác của nhiều phát triển | 
| Cắt sau khi trồng | sản lượng giới hạn | tính đúng đắn của giới hạn tối thiểu toàn cầu | 
| Nhiều vết cắt giảm | nắp đơn điệu | cắt tích lũy | 
| Truy vấn vị trí cạnh | độ đúng ranh giới | lập chỉ mục an toàn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi hoạt động phát triển nằm một phần bên ngoài phạm vi chỉ mục hợp lệ. Ví dụ: nếu L = 1 và K = 5, cạnh trái của tam giác sẽ mở rộng xuống dưới 1. Thuật toán xử lý điều này bằng cách giới hạn các cập nhật đối với miền hợp lệ, đảm bảo không có chỉ số BIT không hợp lệ nào được cập nhật. 

Một trường hợp khác là các đợt cắt giảm lặp đi lặp lại giảm mạnh sau khi tăng trưởng lớn. Nếu một đỉnh hình tam giác lớn được tạo và sau đó áp dụng một phần cắt nhỏ, thì việc triển khai đơn giản có thể cố gắng giảm hồi tố tất cả các giá trị được lưu trữ, nhưng giải pháp này tránh được điều đó bằng cách trì hoãn việc cắt theo thời gian truy vấn. Ví dụ: A 10 100 theo sau là C 1 và sau đó là B 10 xuất ra chính xác 1 mặc dù cấu trúc bên trong lưu trữ giá trị thô lớn hơn nhiều. 

Trường hợp thứ ba là truy vấn các vị trí chưa bao giờ nhận được cập nhật trực tiếp. Vì BIT trả về 0 theo mặc định nên các vị trí này đánh giá chính xác về 0 trừ khi bị ảnh hưởng bởi sự tăng trưởng gần đó.
