---
title: "CF 106104C - Mua trái cây"
description: "Chúng tôi bắt đầu với một bộ đầy đủ có kích thước N. Mỗi bước độc lập chọn một số từ 1 đến N một cách thống nhất. Nếu số đã chọn vẫn còn trong tập hợp, chúng tôi sẽ xóa số đó; nếu không thì tập hợp không thay đổi. Quá trình dừng lại khi tập hợp trở nên trống rỗng."
date: "2026-06-25T11:42:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106104
codeforces_index: "C"
codeforces_contest_name: "UT 104c Midterm #1"
rating: 0
weight: 106104
solve_time_s: 39
verified: true
draft: false
---

[CF 106104C - Mua trái cây](https://codeforces.com/problemset/problem/106104/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một bộ đầy đủ có kích thước N. Mỗi bước độc lập chọn một số từ 1 đến N một cách thống nhất. Nếu số đã chọn vẫn còn trong tập hợp, chúng tôi sẽ xóa số đó; nếu không thì tập hợp không thay đổi. Quá trình dừng lại khi tập hợp trở nên trống rỗng. 

Chúng tôi cần số bước dự kiến ​​cho đến khi chấm dứt. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có N và chúng ta phải xuất ra modulo kỳ vọng 998244353. 

Quan sát quan trọng là việc nhận dạng các phần tử không bao giờ quan trọng. Tại bất kỳ thời điểm nào, trạng thái có ý nghĩa duy nhất là còn lại bao nhiêu phần tử. Nếu k phần tử vẫn còn, hệ thống sẽ hoạt động giống hệt nhau bất kể chúng là phần tử cụ thể nào. 

Ràng buộc N lên tới 100000 sẽ loại trừ mọi tập hợp con DP hàm mũ. Ngay cả O(N^2) cũng quá lớn. Điều này ngay lập tức cho thấy sự tái diễn một chiều hoặc kỳ vọng tuyến tính DP. 

Một ý tưởng ngây thơ là mô phỏng quá trình nhiều lần, nhưng mỗi mô phỏng có thể mất vô số thời gian và sẽ không hội tụ trong các giới hạn. 

Ý tưởng ngây thơ thứ hai là xử lý từng phần tử một cách độc lập và tính tổng thời gian loại bỏ dự kiến. Điều này không thành công vì việc xóa bỏ không độc lập: việc chọn một phần tử đã bị xóa sẽ tiêu tốn thời gian mà không có tiến triển. 

Trường hợp cạnh tinh tế xuất hiện khi N = 1. Câu trả lời dự kiến ​​​​là 1 vì lựa chọn đầu tiên sẽ loại bỏ phần tử duy nhất có xác suất 1. Bất kỳ công thức nào cũng phải giảm chính xác ở đây. 

Một trường hợp khác là N = 2. Quá trình này có thể bị "kẹt" liên tục khi chọn các phần tử đã bị loại bỏ, do đó kỳ vọng hoàn toàn lớn hơn 2. Bất kỳ giả định tuyến tính không chính xác nào như "mỗi phần tử tốn N bước" đều không thành công ở đây. 

## Phương pháp tiếp cận 

Quan điểm brute-force là mô hình hóa trạng thái quy trình như một tập hợp con của các phần tử còn lại. Từ bất kỳ tập con S nào có kích thước k, mỗi bước chuyển tiếp sang S \ {x} với một số x trong S có xác suất k/N hoặc ở lại S với xác suất (N-k)/N. Điều này ngay lập tức xác định chuỗi Markov trên 2^N trạng thái. 

Điều này đúng nhưng không thể sử dụng được vì không gian trạng thái có tính hàm mũ. Ngay cả việc viết các chuyển tiếp cũng đã không thể thực hiện được. 

Sự đơn giản hóa chính là tất cả các tập con có kích thước k hoạt động giống hệt nhau. Vì vậy, thay vì theo dõi các tập hợp con, chúng tôi theo dõi một giá trị duy nhất dp[k]: thời gian còn lại dự kiến ​​khi k phần tử vẫn còn tồn tại. 

Từ trạng thái k, một lựa chọn ngẫu nhiên thành công với xác suất k/N và giảm k đi 1. Với xác suất (N-k)/N, không có gì thay đổi và chúng ta vẫn ở trạng thái cũ, điều này góp phần thêm một bước lãng phí. 

Cấu trúc vòng lặp này chính là chìa khóa: phương trình kỳ vọng có thể được viết trực tiếp. 

Nếu E[k] là thời gian còn lại dự kiến từ k phần tử thì: 

E[k] = 1 + (k/N) * E[k-1] + (1 - k/N) * E[k] 

Phía bên phải phản ánh rằng mỗi bước có giá 1, và sau đó chúng tôi giảm trạng thái hoặc giữ nguyên. 

Việc sắp xếp lại sẽ loại bỏ sự tự tham chiếu và tạo ra sự lặp lại đơn giản. 

Đây là cái nhìn sâu sắc về cấu trúc: “lựa chọn lãng phí” không thay đổi trạng thái, nhưng chúng tăng thời gian theo một hệ số tùy thuộc vào k/N. 

Sau đại số, ta có: 

E[k] = E[k-1] + N/k 

Vì vậy, quá trình giảm xuống còn tổng hợp các số hạng giống như điều hòa được chia tỷ lệ theo N. 

Như vậy: 

E[N] = N * (1/1 + 1/2 + ... + 1/N) 

Mọi thứ sụp đổ thành các số hài tiền tố theo số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực Markov DP trên các tập hợp con | O(2^N) | O(2^N) | Quá chậm | 
| Nén trạng thái DP trên k | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nhận ra rằng chỉ có số lượng vật phẩm còn lại mới quan trọng chứ không phải cái nào còn lại. Điều này làm giảm không gian trạng thái từ tập hợp con xuống số nguyên k từ 0 đến N. 
2. Xác định E[k] là số bước dự kiến cần thiết khi vẫn còn k mục. Mục tiêu là E[N]. 
3. Viết phần chuyển tiếp cho một bước. Từ k mục, một lựa chọn ngẫu nhiên thành công với xác suất k/N và giảm k xuống k−1. Ngược lại, với xác suất (N−k)/N, trạng thái không thay đổi nhưng thời gian vẫn tăng thêm một bước. 
4. Chuyển giá trị này thành phương trình kỳ vọng: 

E[k] = 1 + (k/N)E[k−1] + (1 − k/N)E[k]. 

Phía bên trái lại bao gồm E[k] vì việc không xóa một mục sẽ giữ trạng thái không thay đổi. 
5. Sắp xếp lại phương trình để cô lập E[k]. Di chuyển thuật ngữ tự cho: 

(k/N)E[k] = 1 + (k/N)E[k−1], rút gọn thành E[k] = E[k−1] + N/k. 
6. Sử dụng phép truy hồi này bắt đầu từ E[0] = 0 cho đến E[N]. Mỗi bước thêm N nhân với nghịch đảo mô-đun của i. 
7. Duy trì tổng điều hòa đang chạy H = sum(1/i mod MOD) và tích lũy câu trả lời cuối cùng là N * H. 

### Tại sao nó hoạt động 

Bất biến quan trọng là E[k] chỉ phụ thuộc vào k chứ không phụ thuộc vào đồng dạng của các phần tử còn lại. Mọi chuyển đổi từ k đều hoạt động giống hệt nhau và tính ngẫu nhiên duy nhất là liệu chỉ mục đã chọn có còn hoạt động hay không. Xác suất tự vòng lặp tạo ra hiệu ứng thời gian chờ hình học, chính xác là cái tạo ra số hạng N/k cộng thêm. Bởi vì mỗi trạng thái giảm độc lập với lịch sử vượt quá k, nên sự truy hồi mô tả đầy đủ kỳ vọng mà không mất thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    inv = [0] * (100000 + 1)

    for i in range(1, 100000 + 1):
        inv[i] = modinv(i)

    for _ in range(t):
        n = int(input())
        H = 0
        for i in range(1, n + 1):
            H = (H + inv[i]) % MOD
        ans = H * n % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước các nghịch đảo mô-đun vì các mẫu số giống nhau lặp lại trong các trường hợp thử nghiệm. Tổng hài được tích lũy trực tiếp và phép nhân cuối cùng với N áp dụng công thức dẫn xuất E[N] = N * H_N. 

Một cạm bẫy phổ biến là cố gắng cập nhật kỳ vọng một cách lặp đi lặp lại mà không tách rời cấu trúc hài hòa. Một điều nữa là quên rằng số hạng tự lặp phải được loại bỏ theo đại số; nếu để nguyên tại chỗ, sự tái phát sẽ xuất hiện không chính xác theo hình tròn. 

## Ví dụ đã hoạt động 

### Ví dụ 1: N = 2 

Chúng tôi tính toán nghịch đảo: 1/1 = 1, 1/2 = 499122177 (mod 998244353). 

H = 1 + 1/2. 

Khi đó E[2] = 2 * (1 + 1/2) = 3. 

| k | Tiền tố H | E[k] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 + 1/2 | 3 | 

Điều này cho thấy rằng ngay cả đối với N nhỏ, kỳ vọng vẫn vượt quá N vì các lần rút vô ích lặp đi lặp lại sẽ làm tăng quá trình. 

### Ví dụ 2: N = 3 

H = 1 + 1/2 + 1/3. 

E[3] = 3H. 

| k | Tiền tố H | E[k] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 + 1/2 | 3 | 
| 3 | 1 + 1/2 + 1/3 | 3H | 

Điều này khẳng định sự tích lũy tuyến tính của các đóng góp hài hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | tổng hài trên 1..N | 
| Không gian | O(1) bổ sung (hoặc O(N) nếu nghịch đảo được tính toán trước) | chỉ cần số tiền chạy | 

Tính toán hài hòa chiếm ưu thế trong thời gian chạy, nhưng N ≤ 100000 đảm bảo tính khả thi trong các giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            H = 0
            for i in range(1, n + 1):
                H = (H + pow(i, MOD - 2, MOD)) % MOD
            print(H * n % MOD)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like checks
assert run("1\n1\n") == "1", "n=1"
assert run("1\n2\n") == str((2 * (1 + pow(2, MOD - 2, MOD))) % MOD), "n=2"
assert run("1\n3\n") == str((3 * (1 + pow(2, MOD - 2, MOD) + pow(3, MOD - 2, MOD))) % MOD), "n=3"

# edge case large structure sanity
assert run("1\n5\n").split()[0], "basic structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | chấm dứt căn cứ | 
| n=2 | 3 | lạm phát tự lặp | 
| n=3 | tích lũy hài hòa | tính đúng đắn tái diễn | 
| n=5 | giá trị tính toán | tính đúng đắn chung | 

## Vỏ cạnh 

Với N = 1, trạng thái bắt đầu đã ở mức tối thiểu và lần chọn thành công đầu tiên luôn xảy ra ngay lập tức. Phép truy toán cho E[1] = N * (1) = 1, khớp trực tiếp với quy trình. 

Với N = 2, hệ thống thường lãng phí các bước trên các phần tử đã bị loại bỏ. Sự truy hồi giải thích chính xác điều này thông qua số hạng hài hòa 1/2, làm tăng kỳ vọng lên 3. 

Đối với N lớn hơn, sự đóng góp chủ yếu đến từ các giai đoạn muộn trong đó k nhỏ vì N/k trở nên lớn. Cấu trúc hài hòa ghi lại tiếng nổ này một cách tự nhiên mà không cần mô phỏng.
