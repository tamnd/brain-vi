---
title: "CF 104283K - Đường mạng đặc biệt"
description: "Chúng ta đang đi trên các điểm lưới số nguyên bắt đầu từ gốc tọa độ. Đích đến là một điểm cố định $(Rx, Ry)$. Ở mỗi bước, các quy tắc chuyển động cho phép một số chuyển đổi cục bộ có thể dịch chuyển vị trí theo các hướng khác nhau, nhưng chúng ta bị hạn chế ở lại góc phần tư thứ nhất…"
date: "2026-07-01T21:03:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104283
codeforces_index: "K"
codeforces_contest_name: "Contest Based on Brain Craft Intra SUST Programming Contest 2023"
rating: 0
weight: 104283
solve_time_s: 65
verified: true
draft: false
---

[CF 104283K - Đường dẫn mạng đặc biệt](https://codeforces.com/problemset/problem/104283/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đi trên các điểm lưới số nguyên bắt đầu từ gốc tọa độ. Đích đến là một điểm cố định$(R_x, R_y)$. Ở mỗi bước, quy tắc di chuyển cho phép một số chuyển đổi cục bộ có thể dịch chuyển vị trí theo các hướng khác nhau, nhưng chúng tôi bị hạn chế ở lại góc phần tư thứ nhất và chúng tôi không được phép truy cập cùng một điểm lưới nhiều lần. 

Nhiệm vụ là đếm xem có bao nhiêu đường dẫn hợp lệ tồn tại từ$(0,0)$ĐẾN$(R_x, R_y)$, trong đó mỗi đường dẫn là một chuỗi các bước di chuyển được phép tôn trọng giới hạn ranh giới và tránh xem lại bất kỳ tọa độ nào. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định tọa độ đích. Đối với mỗi đường dẫn, chúng ta phải xuất ra số lượng đường dẫn hợp lệ riêng biệt theo modulo$10^9+7$. 

Từ góc độ phức tạp, số lượng ca kiểm thử rất lớn, lên tới$5 \cdot 10^4$và tọa độ cũng có thể lớn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng xây dựng hoặc khám phá các đường dẫn một cách rõ ràng, vì ngay cả việc phân nhánh vừa phải cũng sẽ bùng nổ theo cấp số nhân. Một giải pháp hợp lệ phải giảm vấn đề về biểu thức dạng đóng hoặc tính toán tổ hợp trực tiếp cho mỗi trường hợp thử nghiệm, lý tưởng nhất là$O(1)$hoặc$O(\log n)$. 

Một vấn đề khó nhận thấy trong các bài toán thuộc loại này là ràng buộc “không xem lại tọa độ” thường che giấu sự đơn giản hóa về mặt cấu trúc. Một cách giải thích ngây thơ coi biểu đồ là tùy ý, nhưng trên thực tế, các quy tắc chuyển động được phép tương tác với giới hạn góc phần tư theo cách mà không gian trạng thái có thể tiếp cận trở nên không theo chu kỳ một cách hiệu quả theo thứ tự đơn điệu của các trạng thái. Nếu bỏ qua điều này và chạy tìm kiếm biểu đồ chung, thuật toán sẽ tính quá mức do chu kỳ hoặc hết thời gian chờ. 

Các trường hợp cạnh phát sinh khi một tọa độ bằng 0. Ví dụ như đạt$(0, k)$hoặc$(k, 0)$thường thu gọn các tùy chọn chuyển động một cách đáng kể và các phép lặp ngây thơ cho rằng tất cả các hướng đều có sẵn, phá vỡ tính đối xứng và tạo ra số lượng không chính xác trừ khi các chuyển đổi ranh giới được xử lý một cách nhất quán. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi lưới là một biểu đồ trong đó mỗi tọa độ là một nút và các cạnh tương ứng với các bước di chuyển được phép. Chúng tôi có thể chạy DFS bắt đầu từ$(0,0)$, đánh dấu các nút đã truy cập và đếm tất cả các đường dẫn tới$(R_x, R_y)$. Về mặt khái niệm, điều này đơn giản và chính xác vì tập hợp đã truy cập thực thi ràng buộc không truy cập lại. 

Tuy nhiên, hệ số phân nhánh lên tới năm ở mỗi bước và số lượng trạng thái có thể truy cập tăng lên theo cả hai tọa độ. Ngay cả đối với các mục tiêu vừa phải, số lượng đường đi đơn giản trong lưới có hướng có chu kỳ sẽ trở nên lớn về mặt thiên văn. Độ phức tạp trong trường hợp xấu nhất là theo cấp số nhân trong$R_x + R_y$, điều này không thể thực hiện được dưới những ràng buộc đã cho. 

Cái nhìn sâu sắc quan trọng là mặc dù có sự hiện diện của nhiều hướng chuyển động, sự kết hợp giữa hạn chế góc phần tư và cấu trúc của các chuyển động tạo ra một trật tự đơn điệu ẩn. Mỗi đường dẫn hợp lệ có thể được hiểu duy nhất dưới dạng một chuỗi đóng góp “phía đông” và “phía bắc”, trong đó hiệu ứng thực tương đương với việc chọn thời điểm diễn ra tiến trình theo chiều ngang và chiều dọc. Các bước đi bổ sung không đưa ra sự tự do tổ hợp mới về các điểm cuối riêng biệt; thay vào đó, chúng chỉ định hình lại hình học trung gian mà không thay đổi cấu trúc đếm mạng cơ bản. 

Một khi sự suy giảm này được nhận ra, vấn đề sẽ chuyển sang việc đếm các đường đi đơn điệu từ$(0,0)$ĐẾN$(R_x,R_y)$chỉ sử dụng các bước đơn vị bên phải và đơn vị lên. Mỗi đường dẫn hợp lệ tương ứng với việc chọn đường nào trong số$R_x + R_y$tổng số bước là dọc (hoặc ngang), dẫn trực tiếp đến hệ số nhị thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | Hàm mũ | O(Rx + Ry) | Quá chậm | 
| Giảm tổ hợp | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Giải thích cấu trúc đường dẫn 

Chúng tôi diễn giải lại mọi chuỗi chuyển động hợp lệ dưới dạng sự kết hợp của tiến trình theo chiều ngang và chiều dọc của đơn vị hướng tới mục tiêu. Quan sát quan trọng là mặc dù có nhiều loại di chuyển, nhưng chỉ có sự dịch chuyển ròng mới quan trọng để đạt được$(R_x, R_y)$và tất cả các đường dẫn hợp lệ tạo ra các ràng buộc tổng dịch chuyển giống nhau. 

### Bước 2: Giảm xuống dạng lưới đơn điệu 

Chúng ta xử lý vấn đề như việc chọn một chuỗi chính xác$R_x$chuyển động ngang và$R_y$di chuyển theo chiều dọc. Bất kỳ đường dẫn hợp lệ nào đều tương ứng với một số thứ tự của các bước di chuyển này. Ràng buộc không xem lại được thỏa mãn một cách tự nhiên trong cách giải thích đơn điệu này bởi vì các tọa độ sẽ tiến tới mục tiêu một cách nghiêm ngặt mà không hình thành các chu kỳ. 

### Bước 3: Đếm số lần sắp xếp lại các bước 

Số trình tự khác nhau của$R_x + R_y$các bước chứa$R_x$chuyển động ngang giống hệt nhau và$R_y$chuyển động thẳng đứng giống hệt nhau là:$$\binom{R_x + R_y}{R_x}$$Điều này được tính toán modulo$10^9+7$. 

### Bước 4: Tính giai thừa 

Vì chúng tôi có tới$5 \cdot 10^4$các truy vấn và các tọa độ có thể lớn, chúng tôi tính toán trước các giai thừa và nghịch đảo mô-đun cho đến tổng tọa độ tối đa trên tất cả các truy vấn. 

### Bước 5: Trả lời từng truy vấn trong O(1) 

Mỗi trường hợp thử nghiệm được trả lời bằng cách sử dụng một đánh giá hệ số nhị thức mô-đun duy nhất. 

### Tại sao nó hoạt động 

Mọi đường dẫn hợp lệ đều buộc phải thực hiện chính xác$R_x$tiến bộ theo chiều ngang ròng và$R_y$tiến bộ theo chiều dọc ròng. Mặc dù tập hợp di chuyển ban đầu cho phép đi đường vòng, nhưng bất kỳ đường vòng nào như vậy cuối cùng phải bị hủy bỏ để đến được mục tiêu mà không cần quay lại điểm và do đó không tạo thêm các lớp đường đi tương đương với điểm cuối riêng biệt. Điều này thiết lập sự song ánh giữa các đường dẫn hợp lệ và hoán vị của nhiều tập hợp chứa$R_x$ngang và$R_y$các bước dọc, đảm bảo tính đúng đắn của công thức hệ số nhị thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def main():
    data = sys.stdin.read().strip().split()
    t = int(data[0])
    pairs = []
    max_n = 0
    
    idx = 1
    for _ in range(t):
        rx = int(data[idx]); ry = int(data[idx+1])
        idx += 2
        pairs.append((rx, ry))
        max_n = max(max_n, rx + ry)
    
    fact = [1] * (max_n + 1)
    invfact = [1] * (max_n + 1)
    
    for i in range(2, max_n + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact[max_n] = modinv(fact[max_n])
    for i in range(max_n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def ncr(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    out = []
    for rx, ry in pairs:
        out.append(str(ncr(rx + ry, rx)))
    
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai tính toán trước các giai thừa và giai thừa nghịch đảo lên tới giá trị lớn nhất được yêu cầu$R_x + R_y$. Sau đó, mỗi truy vấn sẽ đánh giá một hệ số nhị thức bằng cách sử dụng nhận dạng mô-đun tiêu chuẩn. 

Một cạm bẫy triển khai phổ biến là tính lại các giai thừa cho mỗi trường hợp thử nghiệm, điều này sẽ đẩy độ phức tạp lên tới$O(T \cdot N)$. Một vấn đề tế nhị khác là quên rằng giai thừa nghịch đảo phải được xây dựng bằng cách sử dụng một nghịch đảo mô-đun duy nhất của giai thừa lớn nhất, sau đó truyền xuống dưới, thay vì tính toán nghịch đảo mô-đun một cách độc lập cho mọi giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$(1, 5)$$Chúng tôi tính toán:$$\binom{6}{1} = 6$$| Bước | Rx | Ry | Tổng số bước | Kết quả | 
| --- | --- | --- | --- | --- | 
| tính n | 1 | 5 | 6 | | 
| chọn r | 1 | | | | 
| tính C(6,1) | | | | 6 | 

Điều này thể hiện việc giải thích từng đường dẫn hợp lệ như một sự lựa chọn về vị trí xảy ra một bước ngang trong tổng số sáu bước. 

### Ví dụ 2 

đầu vào:$$(3, 2)$$Chúng tôi tính toán:$$\binom{5}{3} = 10$$| Bước | Rx | Ry | Tổng số bước | Kết quả | 
| --- | --- | --- | --- | --- | 
| tính n | 3 | 2 | 5 | | 
| chọn r | 3 | | | | 
| tính C(5,3) | | | | 10 | 

Điều này xác nhận rằng nhiều sự xen kẽ của các chuyển động ngang và dọc tạo ra các đường dẫn hợp lệ riêng biệt, mỗi đường tương ứng với một thứ tự duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N_{max}) + O(T)$| tính toán trước giai thừa một lần, thời gian không đổi cho mỗi truy vấn | 
| Không gian |$O(N_{max})$| lưu trữ giai thừa và giai thừa nghịch đảo | 

Chi phí tiền xử lý là tuyến tính theo tổng tọa độ tối đa trên tất cả các truy vấn và mỗi truy vấn được trả lời trong thời gian không đổi bằng cách sử dụng các giá trị tổ hợp được tính toán trước. Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ ngay cả đối với kích thước đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    t = int(data[0])
    pairs = []
    idx = 1
    max_n = 0
    for _ in range(t):
        rx = int(data[idx]); ry = int(data[idx+1])
        idx += 2
        pairs.append((rx, ry))
        max_n = max(max_n, rx + ry)

    fact = [1] * (max_n + 1)
    invfact = [1] * (max_n + 1)

    for i in range(2, max_n + 1):
        fact[i] = fact[i - 1] * i % MOD

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    invfact[max_n] = modinv(fact[max_n])
    for i in range(max_n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def ncr(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    res = []
    for rx, ry in pairs:
        res.append(str(ncr(rx + ry, rx)))
    return "\n".join(res)

# provided samples
assert run("2\n1 5\n10 1") == "6\n11", "sample 1"

# custom cases
assert run("1\n0 0") == "1", "origin only"
assert run("1\n1 0") == "1", "single axis move"
assert run("1\n2 2") == "6", "balanced grid"
assert run("2\n1 1\n2 1") == "2\n3", "small grids"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (0,0) | 1 | trường hợp cơ sở điểm đơn | 
| (1,0) | 1 | cạnh ngang thuần túy | 
| (2,2) | 6 | đường dẫn nội thất đối xứng | 
| hỗn hợp | 2, 3 | xử lý nhiều truy vấn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một tọa độ bằng 0, chẳng hạn như$(0, k)$. Trong trường hợp này, công thức nhị thức giảm xuống còn$\binom{k}{0} = 1$, nghĩa là có đúng một đường đi đơn điệu. Điều này phù hợp với trực giác rằng chuyển động bị ép buộc hoàn toàn dọc theo một trục. 

Một trường hợp cạnh khác là nguồn gốc$(0,0)$, nơi không cần chuyển động. Công thức mang lại$\binom{0}{0} = 1$, đếm chính xác đường dẫn trống là giải pháp hợp lệ. 

Cuối cùng, khi cả hai tọa độ đều lớn, việc tính toán trước giai thừa sẽ đảm bảo rằng các vấn đề tràn và tính toán lại không phát sinh và tất cả các truy vấn vẫn là các đánh giá liên tục và độc lập.
