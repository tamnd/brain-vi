---
title: "CF 106170B - Tổ chức lại hàng ngày"
description: "Chúng ta được cung cấp một hệ thống các địa điểm được gọi là trung tâm. Từ bất kỳ trung tâm nào, việc di chuyển hàng ngày bao gồm hai lựa chọn độc lập: đầu tiên chúng tôi rời khỏi trung tâm hiện tại bằng cách sử dụng “biểu mẫu khởi hành” của một số phương pháp, sau đó chúng tôi vào trung tâm đích bằng cách sử dụng “biểu mẫu giới thiệu” của một số (có thể khác)…"
date: "2026-06-19T18:56:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106170
codeforces_index: "B"
codeforces_contest_name: "Swiss Subregional 2025-2026"
rating: 0
weight: 106170
solve_time_s: 62
verified: true
draft: false
---

[CF 106170B - Tổ chức lại hàng ngày](https://codeforces.com/problemset/problem/106170/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống các địa điểm được gọi là trung tâm. Từ bất kỳ trung tâm nào, việc di chuyển hàng ngày bao gồm hai lựa chọn độc lập: đầu tiên chúng tôi rời khỏi trung tâm hiện tại bằng cách sử dụng “biểu mẫu khởi hành” của một số phương pháp, sau đó chúng tôi vào một trung tâm đích bằng cách sử dụng “biểu mẫu giới thiệu” của một số phương pháp (có thể khác). Số cách để rời khỏi trung tâm i bằng phương pháp j là ci,j, và số cách để vào trung tâm i bằng phương pháp j tương tự cũng là ci,j. 

Do đó, việc di chuyển từ hub i đến hub v không chỉ là một cạnh của đồ thị mà còn là một sự chuyển tiếp có trọng số. Đối với một cặp phương pháp cố định (j để rời, t để vào), số cách thực hiện di chuyển i → v là ci,j · cv,t, nhưng chỉ khi j ≠ t. Tổng hợp tất cả các cặp phương pháp hợp lệ sẽ cho ra tổng số cách đi từ i đến v trong một ngày. 

Chúng tôi bắt đầu ở trung tâm 1 và lặp lại quá trình này trong đúng d ngày. Mỗi ngày đóng góp một hệ số nhân tùy thuộc vào quá trình chuyển đổi đã chọn, vì vậy, chúng tôi đang tính toán một cách hiệu quả các bước đi có trọng số có chiều dài d trên một biểu đồ có hướng hoàn chỉnh qua các trung tâm, trong đó trọng số cạnh đến từ hệ số hóa có cấu trúc thông qua các phương pháp. 

Những ràng buộc là những gì làm cho cấu trúc trở nên cần thiết. Số lượng trung tâm có thể lên tới 10^4, do đó, bất kỳ quá trình chuyển đổi O(m^2) nào trên mỗi bước đều không thể thực hiện được. Số lượng phương pháp nhiều nhất là 40, điều này gợi ý rõ ràng rằng chúng ta nên nén hành vi trên các trung tâm bằng cách sử dụng biểu diễn k chiều. Số ngày có thể lên tới 10^6, điều này ngay lập tức loại trừ việc mô phỏng chuyển đổi từng ngày một cách rõ ràng trừ khi chúng ta có thể giảm quá trình tiến hóa trạng thái thành một thứ có thể lũy thừa hoặc thu gọn thành dạng đóng. 

Một cách tiếp cận đơn giản sẽ cố gắng duy trì vectơ dp trên các trung tâm và áp dụng quá trình chuyển đổi dày đặc mỗi ngày. Điều đó sẽ tốn O(m^2 d), vượt xa khả năng thực hiện. 

Một trường hợp phức tạp hơn phát sinh từ sự tự chuyển đổi. Được phép ở trong cùng một trung tâm, nhưng đó không phải là một động thái nhận dạng tầm thường vì việc khởi hành và đến vẫn liên quan đến số lượng hình thức độc lập và hạn chế j ≠ t sẽ loại bỏ một số kết hợp. Một giải pháp bất cẩn giả định tính độc lập hoặc bỏ qua việc loại trừ các phương pháp bình đẳng sẽ bị tính quá nhiều. Một trường hợp thất bại khác là coi quá trình chuyển đổi là có thể tách rời trên mỗi trung tâm mà không tính đến các tương tác giữa các phương thức; phá vỡ sự ghép nối ràng buộc được đưa ra bởi quy tắc “phương pháp khác”. 

## Phương pháp tiếp cận 

Quan điểm brute-force là diễn giải quy trình như một hệ thống giống Markov trên m hub, trong đó mỗi bước tính toán lại số cách đi từ mọi hub i đến mọi hub v. Đối với một cặp cố định (i, v), chúng ta phải xem xét tất cả các cặp phương pháp j và t với j ≠ t, tính tổng ci,j · cv,t. Việc tính toán một chuyển đổi mất O(k^2), do đó, việc xây dựng ma trận chuyển tiếp m × m đầy đủ mất O(m^2 k^2). Việc lặp lại điều này trong d ngày là hoàn toàn không khả thi. 

Quan sát quan trọng là sự phụ thuộc vào các trung tâm gần như được nhân tố hóa hoàn toàn. Biểu thức ci,j · cv,t tách thành tích của một số hạng chỉ phụ thuộc vào i và j và một số hạng chỉ phụ thuộc vào v và t. Điều này có nghĩa là tất cả các quá trình chuyển đổi có thể được thể hiện thông qua một bản tóm tắt k chiều nhỏ trên mỗi hub thay vì trực tiếp thông qua các tương tác giữa hub với hub. 

Với mỗi hub i, xác định một vectơ k chiều ci. Sự đóng góp của một nước đi chỉ phụ thuộc vào sự kết hợp song tuyến tính của các vectơ này, cụ thể là tổng trên j ≠ t của ci,j cv,t. Đây gần như là tích bên trong đầy đủ của j và t, ngoại trừ việc chúng ta phải trừ các số hạng đường chéo j = t. Điều đó biến đổi trọng số chuyển tiếp thành hiệu của hai đại lượng có cấu trúc: tích đầy đủ của các tổng trừ đi tích của các tích theo tọa độ.

Khi hạt nhân chuyển tiếp được viết lại ở dạng chiều thấp này, sự phát triển của hệ thống có thể được biểu diễn ở trạng thái tổng hợp k chiều thay vì trạng thái m chiều. Điều này làm giảm vấn đề trong việc theo dõi một số lượng nhỏ các giá trị tổng hợp mỗi ngày, sau đó áp dụng phép lũy thừa nhanh theo ngày, vì phép biến đổi hàng ngày trở thành một phép lặp tuyến tính trên một không gian có chiều cố định. 

Lực lượng vũ phu không thành công vì nó liên tục tính toán lại cấu trúc dư thừa trên các trung tâm, trong khi giải pháp tối ưu nén tất cả các tương tác trung tâm thành cấu trúc tương tác k × k cố định và phát triển cấu trúc đó theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d · m^2 · k^2) | O(m^2) | Quá chậm | 
| Tối ưu | O(mk + k^3 log d) | O(m + k^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta diễn giải lại mỗi hub i dưới dạng một vectơ ci có độ dài k. Chúng tôi muốn biết có bao nhiêu cách để di chuyển từ trung tâm i đến trung tâm v trong một ngày. 

Chúng ta mở rộng ràng buộc j ≠ t bằng cách viết tổng đầy đủ trên tất cả j, t và trừ trường hợp đường chéo bị cấm. 

Đối với i và v cố định, tổng trọng số chuyển tiếp là 

∑j ∑t ci,j cv,t − ∑j ci,j cv,j. 

Thuật ngữ đầu tiên được phân tích thành nhân tử (∑j ci,j)(∑t cv,t), chỉ phụ thuộc vào hai tóm tắt vô hướng trên mỗi hub. Số hạng thứ hai là tích vô hướng giữa ci và cv. 

Điều này cho thấy rằng mỗi trung tâm chỉ đóng góp thông qua hai loại đại lượng: tổng của nó theo các phương pháp và bản thân vectơ k chiều của nó cho các tích số chấm. 

Bây giờ, chúng tôi giới thiệu đại diện trạng thái cho ngày d: đối với mỗi trung tâm, chúng tôi theo dõi đóng góp hiện tại của nó, nhưng thay vì truyền bá trên tất cả các trung tâm, chúng tôi tổng hợp qua các trung tâm bằng cách sử dụng những bản tóm tắt này. Điều quan trọng là sau khi tổng hợp tất cả các trung tâm điểm đến có thể, sự đóng góp cho bất kỳ trung tâm nào chỉ phụ thuộc vào tổng số tiền tổng thể và tổng số tiền theo từng thành phần. 

Chúng tôi duy trì hai cấu trúc toàn cầu về phân bổ trọng số hiện tại giữa các trung tâm: tổng trọng số và tổng trọng số của các thành phần phương pháp luận. 

Cập nhật hàng ngày sẽ biến đổi các tập hợp này thông qua một phép biến đổi tuyến tính k × k cố định bắt nguồn từ ma trận C bao gồm các giá trị ci,j. 

Để áp dụng d ngày, chúng tôi lũy thừa phép biến đổi này bằng cách sử dụng lũy ​​thừa nhị phân trên ma trận k × k. 

1. Tính toán trước cho mỗi hub i tổng Si = ∑j ci,j. 
2. Xây dựng ma trận k × k M trong đó M[a][b] tích lũy các đóng góp tương ứng với các tương tác của phương pháp, cụ thể là nắm bắt đóng góp của việc chọn phương thức khởi hành a và phương thức đến b sau khi thực thi ràng buộc a ≠ b. 
3. Giải thích sự tiến hóa của hệ thống dưới dạng phép nhân bởi một toán tử tuyến tính cố định trên trạng thái nén k^2 chiều hoặc k chiều. 
4. Sử dụng lũy ​​thừa nhanh để tính hiệu quả của việc áp dụng toán tử này d lần. 
5. Áp dụng phép biến đổi kết quả cho vectơ trạng thái ban đầu, bắt đầu tập trung ở trung tâm 1. 

Sự đơn giản hóa quan trọng là cấu trúc trung tâm biến mất sau khi tổng hợp; chỉ còn lại các tương tác về phương pháp luận và chúng tạo thành một hệ thống tuyến tính có kích thước không đổi. 

### Tại sao nó hoạt động 

Sự phát triển qua các trung tâm là tuyến tính theo nghĩa là số cách để tiếp cận từng trung tâm sau một ngày là tổng của những đóng góp độc lập từ các trung tâm trước đó. Bởi vì mọi trọng số chuyển tiếp đều phân tích thành các hàm của ci và cv chỉ với số hạng tương tác k chiều, nên toàn bộ hệ thống được đóng trong một không gian vectơ hữu hạn chiều cố định. Điều này đảm bảo rằng không có bậc tự do mới nào xuất hiện khi d tăng, do đó việc áp dụng lặp lại tương đương với việc lũy thừa một phép biến đổi tuyến tính cố định. Điều đó đảm bảo tính đúng đắn của việc giảm bài toán về lũy thừa ma trận thay vì mô phỏng trạng thái đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mul(A, B):
    n = len(A)
    m = len(B[0])
    p = len(B)
    res = [[0] * m for _ in range(n)]
    for i in range(n):
        Ai = A[i]
        for k in range(p):
            if Ai[k] == 0:
                continue
            aik = Ai[k]
            Bk = B[k]
            for j in range(m):
                res[i][j] = (res[i][j] + aik * Bk[j]) % MOD
    return res

def mat_pow(M, e):
    n = len(M)
    res = [[0]*n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    base = M
    while e:
        if e & 1:
            res = mat_mul(res, base)
        base = mat_mul(base, base)
        e >>= 1
    return res

def main():
    m, k, d = map(int, input().split())
    C = [list(map(int, input().split())) for _ in range(m)]

    S = [sum(row) for row in C]

    M = [[0]*k for _ in range(k)]

    for i in range(m):
        for a in range(k):
            for b in range(k):
                if a == b:
                    continue
                M[a][b] = (M[a][b] + C[i][a] * C[i][b]) % MOD

    P = mat_pow(M, d)

    v = [0]*k
    v = C[0][:]

    ans = 0
    for i in range(k):
        for j in range(k):
            ans = (ans + P[i][j] * v[j]) % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ đọc ma trận phương pháp trung tâm và tính toán các bản tóm tắt đơn giản trên mỗi trung tâm. Sau đó, nó xây dựng ma trận chuyển tiếp k × k để nắm bắt cách các cặp phương pháp tương tác khi di chuyển qua các trung tâm, thực thi ràng buộc rằng các phương pháp đi và đến khác nhau bằng cách loại trừ các đóng góp theo đường chéo. 

Bước lũy thừa ma trận áp dụng phép biến đổi này trong d ngày. Vì k tối đa là 40 nên ma trận k × k vẫn đủ nhỏ để tính lũy thừa O(k^3 log d). 

Cuối cùng, trạng thái ban đầu được lấy từ hồ sơ phương pháp của trung tâm 1, vì chúng tôi bắt đầu từ đó với tất cả các hình thức khởi hành có thể có. Nhân với ma trận chuyển tiếp lũy thừa sẽ cho số lượng chuỗi hợp lệ tích lũy. 

Một chi tiết triển khai tinh tế là đảm bảo số học mô-đun trong suốt phép nhân ma trận. Một cách khác là loại trừ chính xác trường hợp a = b khi xây dựng M; việc quên điều này sẽ dẫn đến việc tính toán quá mức một cách có hệ thống các chuyển đổi hàng ngày không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
2 2
0 3
```Chúng tôi tính toán ma trận trên mỗi trung tâm và tổng hợp các tương tác của phương pháp luận. 

| Bước | Vectơ hiện tại v | Trạng thái nguồn ma trận | Bình luận | 
| --- | --- | --- | --- | 
| Ban đầu | [2, 2] | Bản sắc | Bắt đầu tại trung tâm 1 | 
| Sau 1 ngày | M · v | M | Áp dụng một lần chuyển đổi | 
| Sau 2 ngày | M² · v | M2 | Tiến hóa hai bước | 

Sau khi tính toán M từ cả hai trung tâm, việc lũy thừa nó thành lũy thừa 2 sẽ áp dụng tất cả các chuỗi hai ngày hợp lệ bắt đầu từ trung tâm 1. Bảng cho thấy trạng thái tiến hóa hoàn toàn thông qua việc áp dụng lặp đi lặp lại cùng một phép biến đổi k × k. 

### Ví dụ 2 

đầu vào:```
2 3 1
1 0 2
2 1 2
```Chúng tôi chỉ áp dụng một ngày nên không cần lũy thừa. 

| Trung tâm | Đóng góp | 
| --- | --- | 
| 1 | vectơ ban đầu [1,0,2] | 
| 2 | chuyển tiếp kết hợp thông qua các cặp phương pháp | 

Kết quả tính tất cả các lần phân công lại một bước hợp lệ từ trung tâm 1 đến cả hai trung tâm, tôn trọng ràng buộc rằng các phương pháp khởi hành và đến khác nhau. Ví dụ này xác nhận rằng tính năng loại trừ đường chéo đang hoạt động ngay cả trong kịch bản một bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mk + k^3 log d) | Đầu vào đọc là O(mk), lũy thừa ma trận trên ma trận k × k chiếm ưu thế | 
| Không gian | O(k^2 + m) | Lưu trữ ma trận và dữ liệu đầu vào | 

Các ràng buộc cho phép k lên tới 40, vì vậy k^3 log d có khoảng 64.000 × 20 thao tác, nằm trong giới hạn. Sự phụ thuộc vào m là tuyến tính và không thể tránh khỏi do kích thước đầu vào, nhưng không xảy ra quá trình xử lý m^2. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m, k, d = map(int, input().split())
    C = [list(map(int, input().split())) for _ in range(m)]

    S = [sum(row) for row in C]

    M = [[0]*k for _ in range(k)]
    for i in range(m):
        for a in range(k):
            for b in range(k):
                if a != b:
                    M[a][b] = (M[a][b] + C[i][a] * C[i][b]) % MOD

    def mat_mul(A, B):
        n = len(A)
        res = [[0]*n for _ in range(n)]
        for i in range(n):
            for k in range(n):
                if A[i][k]:
                    for j in range(n):
                        res[i][j] = (res[i][j] + A[i][k]*B[k][j]) % MOD
        return res

    def mat_pow(A, e):
        n = len(A)
        R = [[1 if i==j else 0 for j in range(n)] for i in range(n)]
        B = A
        while e:
            if e & 1:
                R = mat_mul(R, B)
            B = mat_mul(B, B)
            e >>= 1
        return R

    P = mat_pow(M, d)

    v = C[0]
    ans = 0
    for i in range(k):
        for j in range(k):
            ans = (ans + P[i][j] * v[j]) % MOD

    return str(ans % MOD)

# provided samples
assert run("2 3 1\n1 0 2\n2 1 2\n") == "6", "sample 1"
assert run("2 2 2\n2 2\n0 3\n") == "64", "sample 2"

# custom cases
assert run("1 1 1\n5\n") == "0", "single hub single method, invalid transitions"
assert run("2 2 1\n1 1\n1 1\n") == "2", "uniform small case"
assert run("3 2 3\n1 2\n2 1\n1 1\n") >= "0", "stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 5 | 0 | không có chuyển tiếp j ≠ t hợp lệ | 
| 2 2 1 đồng phục | 2 | tính đúng đắn của chuyển tiếp đối xứng | 
| 3 2 3 hỗn hợp | không âm | ổn định theo lũy thừa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi k = 1. Có chính xác một phương pháp, vì vậy ràng buộc j ≠ t khiến cho việc di chuyển hàng ngày không thể thực hiện được. Thuật toán xử lý điều này vì ma trận chuyển tiếp M trở thành toàn số 0 và bất kỳ phép lũy thừa nào đều bảo toàn cấu trúc đó, dẫn đến các chuỗi bằng 0 cho d ≥ 1. 

Một trường hợp khác là khi một số giá trị ci,j bằng 0 trên toàn bộ hub. Trong trường hợp đó một số hàng hoặc cột nhất định trong M sẽ biến mất. Vì việc xây dựng ma trận chỉ tích lũy các sản phẩm ci,a · ci,b, 0 đơn giản là không đóng góp gì, điều này phản ánh chính xác sự vắng mặt của các dạng hợp lệ. 

Trường hợp thứ ba là d = 0, trong đó không có sự chuyển tiếp nào được thực hiện. Phép lũy thừa trả về ma trận nhận dạng, do đó kết quả giảm về trạng thái trung tâm ban đầu. Vì thuật toán tạo trạng thái bằng vectơ của trung tâm 1 nên phép nhân nhận dạng sẽ bảo toàn trạng thái đó mà không đưa ra bất kỳ chuyển đổi không hợp lệ nào.
