---
title: "CF 105216L - Giày thất lạc"
description: "Chúng ta có một gia đình gồm N$ người, trong đó mỗi người hiện đang giữ hai chiếc giày: một chiếc giày phải và một chiếc giày trái. Tuy nhiên, những đôi giày này được trộn lẫn."
date: "2026-06-24T17:11:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105216
codeforces_index: "L"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105216
solve_time_s: 70
verified: false
draft: false
---

[CF 105216L - Giày bị thất lạc](https://codeforces.com/problemset/problem/105216/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được ban cho một gia đình$N$người, mỗi người hiện đang cầm hai chiếc giày: một chiếc bên phải và một chiếc bên trái. Tuy nhiên, những đôi giày này được trộn lẫn. Đối với mỗi người$i$, chiếc giày bên phải họ đang có thuộc về một người nào đó$a_i$, và chiếc giày bên trái họ hiện có thuộc về một người nào đó$b_i$. 

Mục đích là sắp xếp lại giày sao cho người đó$i$kết thúc bằng đôi giày của chính mình. Thao tác duy nhất được phép là hoán đổi giữa hai người, nhưng chỉ ở cùng một bên: giày bên phải chỉ có thể đổi chỗ với giày bên phải, còn giày bên trái chỉ được đổi với giày bên trái. Mỗi lần hoán đổi sẽ đổi giày của bên đó giữa hai người. 

Chúng ta cần tính toán số lần hoán đổi tối thiểu cần thiết để khôi phục cặp đúng của mỗi người. 

Quan sát chính từ các ràng buộc là$N$có thể lớn như$10^6$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào là phương trình bậc hai hoặc thậm chí gần phương trình bậc hai. Bất kỳ cách tiếp cận nào cố gắng mô phỏng trực tiếp việc hoán đổi hoặc liên tục tìm kiếm những đôi giày bị thất lạc sẽ hết thời gian chờ. Giải pháp về cơ bản phải tuyến tính trong$N$, hoặc tệ nhất$O(N \log N)$, mặc dù các yếu tố logarit có rủi ro ở quy mô này. 

Một khía cạnh tế nhị của vấn đề là giày bên phải và giày bên trái di chuyển độc lập. Không có sự tương tác giữa các bên trong quá trình hoán đổi. Điều này có nghĩa là bất kỳ giải pháp nào trộn lẫn hai bên quá sớm đều có khả năng sai. 

Các trường hợp cạnh xuất hiện khi các chu kỳ chồng lên nhau hoặc khi cả hai mảng đã đúng nhưng bị lệch giữa các bên. Ví dụ: nếu tất cả giày bên phải đều đúng nhưng giày bên trái tạo thành một chu kỳ lớn, câu trả lời vẫn được xác định hoàn toàn bằng cấu trúc hoán vị bên trái đó. Cách tiếp cận “đếm không khớp và hoán đổi tham lam” ngây thơ không thành công vì hoán đổi sửa chữa cấu trúc trên toàn cầu chứ không phải cục bộ. 

Một trường hợp cạnh khác là khi cả hai mảng giống hệt nhau nhưng không bằng ánh xạ nhận dạng. Ví dụ: một chu trình hoán vị thuần túy như$a = [2,3,1]$. Mặc dù mọi vị trí đều “sai”, nhưng nó không yêu cầu hai lần hiệu chỉnh độc lập cho mỗi phần tử mà thay vào đó là độ phân giải theo chu kỳ. 

## Phương pháp tiếp cận 

Nếu chúng tôi cố gắng mô phỏng trực tiếp quy trình, chúng tôi sẽ liên tục tìm kiếm một người cầm nhầm chiếc giày và đổi nó với đúng chủ sở hữu. Mỗi lần hoán đổi yêu cầu xác định vị trí mục tiêu, cập nhật vị trí và duy trì tính nhất quán cho cả bên trái và bên phải. Ngay cả khi băm, các bản cập nhật lặp đi lặp lại vẫn tạo thành một quy trình có thể xuống cấp theo hướng$O(N^2)$trong các trường hợp đối nghịch vì mỗi lần điều chỉnh có thể dẫn đến nhiều điểm không khớp hơn nữa. 

Thông tin chi tiết quan trọng là các giao dịch hoán đổi trong một bên hoạt động giống hệt như sắp xếp một hoán vị bằng cách sử dụng các giao dịch hoán đổi tùy ý. Mỗi bên xác định một cách độc lập một hoán vị kích thước$N$: giày bên phải xác định ánh xạ từ vị trí hiện tại đến chủ sở hữu và tương tự cho giày bên trái. 

Vấn đề giảm xuống còn việc sửa hai hoán vị độc lập. Đối với bất kỳ hoán vị nào, số lần hoán đổi tối thiểu cần thiết để khôi phục danh tính là đã biết: nó bằng$N -$(số chu kỳ trong hoán vị). Mỗi chu kỳ có độ dài$k$yêu cầu chính xác$k-1$hoán đổi để sửa lỗi, vì mỗi hoán đổi có thể đặt một phần tử vào đúng vị trí của nó đồng thời giảm kích thước chu trình đi một phần tử. 

Vì các phép toán trái và phải là độc lập nên tổng câu trả lời chỉ đơn giản là tổng các phép hoán đổi cần thiết cho cả hai hoán vị. 

Do đó, chúng tôi tính toán phân rã chu trình cho cả hai mảng và tính tổng kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N^2)$|$O(N)$| Quá chậm | 
| Phân hủy chu trình |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý ánh xạ giày bên phải và ánh xạ giày bên trái một cách riêng biệt, coi mỗi bản đồ là một hoán vị trên$N$các phần tử. 

1. Xây dựng một mảng đã truy cập được khởi tạo thành false để theo dõi các vị trí đã được xử lý. Điều này đảm bảo mỗi phần tử được gán chính xác cho một chu kỳ. 
2. Đối với mảng giày bên phải, lặp qua từng vị trí từ 1 đến$N$. Nếu một vị trí không được truy cập, chúng tôi bắt đầu duyệt qua chu trình của nó bằng cách liên tục theo dõi ánh xạ$i \rightarrow a_i$cho đến khi chúng ta trở lại điểm xuất phát. Mỗi lần chúng tôi truy cập một nút mới trong quá trình truyền tải này, chúng tôi đánh dấu nút đó đã truy cập và đếm kích thước chu kỳ. 
3. Đối với mỗi chu kỳ kích thước$k$, chúng tôi thêm$k-1$để trả lời. Điều này tương ứng với số lần hoán đổi cần thiết để cố định chu kỳ đó một cách độc lập. 
4. Sau khi hoàn thành tất cả các chu trình ở mảng bên phải, hãy đặt lại cấu trúc đã truy cập. 
5. Lặp lại quá trình phân rã chu trình tương tự cho mảng bên trái$b$, lại tóm tắt$k-1$cho mỗi chu kỳ được phát hiện. 
6. In tổng số tiền từ cả hai phía. 

Điểm quan trọng là chúng tôi không bao giờ thực sự thực hiện hoán đổi. Chúng tôi chỉ tính toán số lần hoán đổi sẽ được yêu cầu nếu chúng tôi giải quyết từng hoán vị một cách tối ưu. 

### Tại sao nó hoạt động 

Mỗi bên xác định một hoán vị trên$N$mặt hàng. Một hoán vị phân rã duy nhất thành các chu trình rời rạc và trong mỗi chu trình, các phần tử được luân chuyển giữa các vị trí. Một lần hoán đổi duy nhất có thể sửa chính xác một phần tử trong một chu trình bằng cách đặt nó vào đúng vị trí của nó, giảm kích thước chu trình một cách hiệu quả trong khi vẫn bảo toàn cấu trúc của các phần tử còn lại. Điều này đảm bảo rằng một chu kỳ có độ dài$k$luôn yêu cầu chính xác$k-1$hoán đổi và không ít hơn, vì mỗi hoán đổi có thể sửa được nhiều nhất một phần tử bị đặt sai vị trí trong chu trình đó mà không đạt được tính chính xác đáng lo ngại. 

Vì các phép toán trái và phải độc lập và không can thiệp nên tổng số lần hoán đổi tối thiểu là tổng chi phí tối ưu cho từng hoán vị riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_swaps(p):
    n = len(p) - 1
    vis = [False] * (n + 1)
    res = 0

    for i in range(1, n + 1):
        if not vis[i]:
            cur = i
            cycle_size = 0
            while not vis[cur]:
                vis[cur] = True
                cycle_size += 1
                cur = p[cur]
            res += cycle_size - 1

    return res

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    print(count_swaps(a) + count_swaps(b))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một mảng có 1 chỉ mục để thuận tiện sao cho các vị trí căn chỉnh trực tiếp với nhãn người. các`count_swaps`hàm cô lập logic phân rã chu trình tiêu chuẩn. Mỗi nút không được truy cập bắt đầu quá trình truyền tải theo hoán vị cho đến khi nó đóng một vòng lặp, tích lũy kích thước chu trình trong suốt quá trình đó. 

Một sai lầm phổ biến ở đây là quên rằng cả hai mảng phải được xử lý độc lập. Một vấn đề khó phát hiện khác là việc lập chỉ mục không chính xác khi đọc dữ liệu đầu vào; sử dụng lập chỉ mục dựa trên 1 sẽ tránh được những lỗi mắc phải khi tuân theo các hoán vị. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2
2 1
```Chu kỳ hoán vị phải: 

Bắt đầu từ 1: 1 → 1 (cỡ chu kỳ 1) 

Bắt đầu lúc 2: 2 → 2 (cỡ chu kỳ 1) 

Chu kỳ hoán vị trái: 

Bắt đầu lúc 1: 1 → 2 → 1 (cỡ chu kỳ 2) 

| Bước | Bắt đầu | Truyền tải theo chu kỳ | Kích thước chu kỳ | Đóng góp | 
| --- | --- | --- | --- | --- | 
| Đúng | 1 | 1 | 1 | 0 | 
| Đúng | 2 | 2 | 1 | 0 | 
| Trái | 1 | 1 → 2 → 1 | 2 | 1 | 

Tổng số lần hoán đổi = 1. 

Điều này cho thấy chỉ có bên trái yêu cầu hoán đổi, trong khi bên phải đã đúng. 

### Mẫu 2 

đầu vào:```
3
1 3 2
2 1 3
```Hoán vị phải: 

Chu kỳ: 2 ↔ 3 (cỡ 2) 

Hoán vị trái: 

Chu kỳ: 1 ↔ 2 (cỡ 2) 

| Bên | Chu kỳ | Kích thước | Đóng góp | 
| --- | --- | --- | --- | 
| Đúng | (2 3) | 2 | 1 | 
| Trái | (1 2) | 2 | 1 | 

Tổng số lần hoán đổi = 2. 

Mỗi bên đóng góp một hoán đổi độc lập vì mỗi bên chứa một chu kỳ 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi nút được truy cập chính xác một lần cho mỗi hoán vị | 
| Không gian |$O(N)$| Đã truy cập mảng và lưu trữ đầu vào | 

Thuật toán chạy theo thời gian tuyến tính, điều này rất cần thiết cho$N = 10^6$. Việc sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu vào và dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def count_swaps(p):
        n = len(p) - 1
        vis = [False] * (n + 1)
        res = 0
        for i in range(1, n + 1):
            if not vis[i]:
                cur = i
                cnt = 0
                while not vis[cur]:
                    vis[cur] = True
                    cnt += 1
                    cur = p[cur]
                res += cnt - 1
        return res

    n = int(input())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))
    return str(count_swaps(a) + count_swaps(b))

# sample 1
assert run("2\n1 2\n2 1\n") == "1"

# sample 2
assert run("3\n1 3 2\n2 1 3\n") == "2"

# sample 3
assert run("5\n4 5 1 2 3\n3 1 4 5 2\n") == "8"

# all correct already
assert run("1\n1\n1\n") == "0"

# single cycle
assert run("3\n2 3 1\n1 2 3\n") == "2"

# identity both sides
assert run("4\n1 2 3 4\n1 2 3 4\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, danh tính | 0 | không cần trao đổi | 
| trường hợp 3 chu kỳ | 2 | tính đúng đắn của quá trình phân rã chu trình | 
| hoán vị hỗn hợp | 8 | xử lý bên độc lập | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là khi$N = 1$. Cả hai mảng chỉ được chứa 1. Thuật toán đánh dấu nút đơn là một chu kỳ có kích thước 1, đóng góp số lần hoán đổi bằng 0 cho mỗi bên, mang lại kết quả đầu ra chính xác là 0. 

Một trường hợp khác là khi cả hai mảng tạo thành chu kỳ lớn. Ví dụ,$a = [2,3,4,5,1]$. Quá trình truyền tải truy cập tất cả các nút một lần, tạo ra chu kỳ có kích thước 5 và đóng góp 4 lần hoán đổi. Logic tương tự áp dụng độc lập cho mảng thứ hai nếu nó có cấu trúc chu trình khác. Thuật toán phân tách chúng một cách chính xác vì trạng thái đã truy cập được đặt lại giữa các lần chuyển. 

Một trường hợp tinh vi hơn là khi cả hai hoán vị đều giống hệt nhau nhưng không hề tầm thường. Mặc dù cấu trúc khớp nhau nhưng mỗi bên vẫn yêu cầu sửa chữa độc lập. Thuật toán đếm chính xác cả hai khoản đóng góp thay vì cố gắng hủy chúng, vì các giao dịch hoán đổi không thể được chia sẻ giữa các bên.
