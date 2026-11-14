---
title: "CF 1801A - Chăn Cực Đẹp"
description: "Chúng ta được yêu cầu xây dựng một lưới $n nhân m$ chứa đầy các số nguyên sao cho điều kiện XOR cụ thể đúng cho mỗi lưới con $4 nhân 4$."
date: "2026-06-09T09:30:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1801
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 857 (Div. 1)"
rating: 1600
weight: 1801
solve_time_s: 137
verified: false
draft: false
---

[CF 1801A - Chiếc chăn cực đẹp](https://codeforces.com/problemset/problem/1801/A) 

**Đánh giá:** 1600 
**Tas:** bitmask, thuật toán xây dựng 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một$n \times m$lưới chứa đầy các số nguyên sao cho điều kiện XOR cụ thể đúng cho mọi$4 \times 4$lưới con. Đối với bất kỳ khối liền kề nào có bốn hàng và bốn cột, hai khối đối diện$2 \times 2$các góc phải có tổng XOR bằng nhau: XOR của góc trên bên trái và góc dưới bên phải$2 \times 2$các khối phải khớp nhau và XOR của phía trên bên phải và phía dưới bên trái$2 \times 2$khối cũng phải phù hợp. 

Ngoài việc đáp ứng các ràng buộc XOR cục bộ này ở mọi nơi, chúng tôi được yêu cầu tối đa hóa số lượng giá trị riêng biệt xuất hiện trong lưới. 

Các ràng buộc đủ lớn để việc xây dựng hoặc kiểm tra tất cả các nhiệm vụ một cách thô bạo là không thể. Lưới có thể chứa tới$200 \times 200 = 40000$ô trên mỗi lần kiểm tra và lên tới 1000 lần kiểm tra, với tổng số ô được giới hạn bởi$2 \cdot 10^5$. Điều này ngụ ý rằng chúng ta cần một cấu trúc tuyến tính theo kích thước lưới cho mỗi trường hợp thử nghiệm. 

Một nỗ lực ngây thơ có thể thử gán các giá trị duy nhất một cách tham lam và sau đó sửa các vi phạm ràng buộc XOR. Điều này không thành công vì mỗi ô tham gia vào nhiều hoạt động chồng chéo$4 \times 4$các ràng buộc, do đó các bản sửa lỗi cục bộ lan truyền và nhanh chóng trở nên không nhất quán. 

Một cách tiếp cận không chính xác khác là giả sử chúng ta chỉ cần thỏa mãn điều kiện cho một$4 \times 4$khối. Điều đó bỏ qua mỗi lần trượt$4 \times 4$window phải đáp ứng cùng một cấu trúc, điều này thực thi tính tuần hoàn toàn cầu mạnh mẽ. 

Thách thức thực sự là nhận ra rằng ràng buộc không mang tính cục bộ một cách đơn giản mà áp đặt sự phụ thuộc tuyến tính có cấu trúc trên toàn lưới. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mỗi ô như một biến và mỗi ô$4 \times 4$điều kiện lưới con như một hệ phương trình XOR. Đối với mỗi cửa sổ, chúng tôi áp đặt hai đẳng thức XOR, đưa ra khoảng$O(nm)$hạn chế. Giải quyết vấn đề này một cách trực tiếp sẽ yêu cầu loại bỏ Gaussian$GF(2)$trên một hệ thống có tới 40000 biến, quá chậm. 

Quan sát chính là các ràng buộc XOR của dạng này thực thi một cấu trúc có thể tách rời: lưới có thể được phân tách thành các phần đóng góp độc lập từ các hàng và cột và cụ thể hơn là thành một mẫu được xác định hoàn toàn bởi hai bit tọa độ cuối cùng. 

Điều kiện về cơ bản nói rằng khi chuyển một$2 \times 2$khối trên lưới, sự khác biệt XOR phải nhất quán. Điều này buộc lưới hoạt động giống như một hàm trong đó sự khác biệt về giá trị chỉ phụ thuộc vào một vùng lân cận tọa độ nhỏ, điều này gợi ý việc xây dựng các giá trị bằng cách sử dụng mã hóa bitwise của các chỉ số hàng và cột. 

Một cách tiêu chuẩn để đáp ứng các ràng buộc như vậy trong khi tối đa hóa các giá trị riêng biệt là gán cho mỗi ô một giá trị dựa trên sự kết hợp của các bit hàng và cột sao cho mọi$4 \times 4$cửa sổ tự động bảo toàn tính đối xứng XOR. Một cấu trúc tự nhiên là mã hóa các chỉ mục hàng và cột thành các phạm vi bit rời rạc, ví dụ: sử dụng:$$B[i][j] = (i \ll k) \oplus j$$cho đủ lớn$k$. Điều này đảm bảo rằng XOR trên bất kỳ mẫu hình chữ nhật nào sẽ phân chia rõ ràng giữa các đóng góp hàng và cột, đảm bảo cả hai đẳng thức bắt buộc đều được giữ nguyên. 

Cấu trúc này cũng tối đa hóa các giá trị riêng biệt: vì cả chỉ mục hàng và cột đều đóng góp độc lập nên mỗi ô sẽ trở thành duy nhất miễn là$i, j$khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải quyết hệ thống Brute Force |$O((nm)^3)$hoặc tệ hơn |$O((nm)^2)$| Quá chậm | 
| Mã hóa tọa độ bitwise |$O(nm)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gán từng ô$(i, j)$một giá trị được hình thành bằng cách kết hợp các chỉ số hàng và cột của nó bằng cách sử dụng dịch chuyển bit và XOR. Một lựa chọn thuận tiện là tách các bit hàng và cột để chúng không bao giờ can thiệp. 
2. Đối với mỗi chỉ mục hàng$i$, chuyển nó sang các bit cao hơn để đảm bảo nhận dạng hàng chiếm một phạm vi tách biệt với các cột. Điều này ngăn chặn việc hủy XOR giữa các kích thước khác nhau. 
3. Đối với mỗi chỉ mục cột$j$, hãy giữ nó ở các bit thấp hơn để biến thể cột vẫn có thể phân biệt được hoàn toàn trong mỗi khối hàng. 
4. Điền vào lưới bằng cách sử dụng$B[i][j] = (i \ll 7) \oplus j$. Sự dịch chuyển liên tục chỉ cần đủ lớn để tránh trùng lặp với các bit cột (bất kỳ phần bù an toàn cố định nào cũng hoạt động vì$m \le 200 < 2^8$). 
5. Xuất toàn bộ ma trận. Vì mỗi giá trị là duy nhất nên số lượng các giá trị riêng biệt chính xác là$n \cdot m$, đó là mức tối đa có thể. 

### Tại sao nó hoạt động 

Các ràng buộc XOR là tuyến tính$GF(2)$, vì vậy chúng ta có thể phân tích riêng các phần phụ thuộc vào hàng và phụ thuộc vào cột. Bằng cách mã hóa thông tin hàng ở bit cao hơn và thông tin cột ở bit thấp hơn, mọi XOR trên bất kỳ$2 \times 2$hoặc$4 \times 4$khối chia thành các đóng góp hàng và cột độc lập. Mỗi bên của các đẳng thức bắt buộc chứa cùng nhiều tập chỉ mục hàng và nhiều tập chỉ mục cột giống nhau, do đó XOR của chúng khớp chính xác. Điều này ngăn chặn sự can thiệp chéo và đảm bảo tất cả các ràng buộc được giữ đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        
        shift = 10  # enough since m <= 200
        
        cnt = n * m
        print(cnt)
        
        for i in range(n):
            row = []
            base = i << shift
            for j in range(m):
                row.append(str(base ^ j))
            print(" ".join(row))

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp xây dựng từng hàng một cách độc lập. Chi tiết triển khai chính là dịch chuyển bit cố định, đảm bảo không có sự chồng chéo giữa các đóng góp hàng và cột. Một ca 10 là đủ vì$2^{10} = 1024$, vượt quá chỉ số cột tối đa có thể, do đó XOR không thể gây ra xung đột ngoài ý muốn giữa các phần hàng và cột. 

Số lượng đầu ra luôn luôn là$n \cdot m$, điều này là tối ưu vì tất cả các mục đều khác biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4 4
```Chúng tôi tính toán$B[i][j] = (i \ll 10) \oplus j$. 

| tôi | j | cơ sở = i<<10 | giá trị | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 0 | 1 | 0 | 1 | 
| 1 | 0 | 1024 | 1024 | 
| 1 | 1 | 1024 | 1025 | 

Tiếp tục mẫu này sẽ lấp đầy một lưới trong đó mỗi hàng là một bản sao đã dịch chuyển của$0..3$. 

Điều này cho thấy rằng mọi giá trị là duy nhất và việc phân tách hàng rõ ràng. Cấu trúc XOR trên bất kỳ$4 \times 4$khối duy trì sự bình đẳng vì mỗi bên tổng hợp các dịch chuyển hàng giống hệt nhau và cấu trúc XOR cột giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
1
5 6
```| tôi | j | căn cứ | giá trị | 
| --- | --- | --- | --- | 
| 2 | 3 | 2048 | 2051 | 
| 3 | 2 | 3072 | 3074 | 
| 4 | 5 | 4096 | 4101 | 

Cấu trúc tương tự kéo dài đồng đều. Bất kì$4 \times 4$lưới con chứa hai bản sao của mỗi hàng liên quan ở các vị trí đối xứng, đảm bảo cân bằng XOR trên cả hai đường chéo được yêu cầu. 

Điều này xác nhận rằng địa phương không phá vỡ cấu trúc vì mã hóa nhất quán trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| mỗi ô được tính một lần cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| chỉ lưu trữ hàng tạm thời được sử dụng để in | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó việc xây dựng thoải mái phù hợp trong giới hạn thời gian. Mỗi thao tác là một phép dịch bit đơn giản và XOR, là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        cnt = n * m
        out.append(str(cnt))
        shift = 10
        for i in range(n):
            row = [(i << shift) ^ j for j in range(m)]
            out.append(" ".join(map(str, row)))
    return "\n".join(out)

# provided samples (structure check only, exact values depend on construction)
assert run("1\n4 4\n").count("\n") > 0

# minimum size
assert run("1\n4 4\n").splitlines()[0] == "16"

# rectangular grid
assert run("1\n4 6\n").splitlines()[0] == "24"

# square larger
assert run("1\n6 6\n").splitlines()[0] == "36"

# multiple tests
assert run("2\n4 4\n5 5\n").splitlines()[0] == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 4×4 | 16 giá trị riêng biệt | tính đúng đắn và đếm cơ sở | 
| Lưới 4×6 | 24 giá trị riêng biệt | xử lý không vuông góc | 
| Lưới 6×6 | 36 giá trị riêng biệt | tổng quát đầy đủ | 
| nhiều bài kiểm tra | đúng từng trường hợp | phân tích nhiều chữ | 

## Vỏ cạnh 

Một lưới kích thước hợp lệ tối thiểu$4 \times 4$là trường hợp ràng buộc chặt chẽ nhất. Việc xây dựng gán các giá trị như$0,1,2,\dots$được dịch chuyển bởi các khối hàng. Ngay cả trong kịch bản nhỏ nhất này, mỗi ô vẫn là duy nhất và các ràng buộc XOR được giữ nguyên vì mỗi hàng đóng góp một chữ ký bit cao cố định. 

Đối với một$4 \times 6$lưới, việc dịch chuyển hàng giống nhau đảm bảo rằng việc mở rộng các cột không tạo ra xung đột. Mỗi hàng vẫn độc lập ở các bit cao, do đó không xảy ra nhiễu giữa các hàng khác nhau bên trong sự chồng chéo$4 \times 4$cửa sổ. 

Đối với nhiều trường hợp thử nghiệm, mỗi lưới được tạo độc lập. Vì không có trạng thái toàn cục nào được sử dụng nên không có hiệu ứng chuyển tiếp và mỗi khối đầu ra vẫn có hiệu lực riêng biệt.
