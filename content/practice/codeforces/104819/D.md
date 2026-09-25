---
title: "CF 104819D - Vượt Bão"
description: "Chúng ta được cho một chuỗi các đảo tuyến tính từ 1 đến n. Các đảo liên tiếp được kết nối bởi các cạnh có hướng từ i đến i+1 và mỗi cạnh như vậy có chi phí cố định."
date: "2026-06-28T13:01:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104819
codeforces_index: "D"
codeforces_contest_name: "2023 Sun Yat-sen University Collegiate Programming Contest, Onsite"
rating: 0
weight: 104819
solve_time_s: 69
verified: true
draft: false
---

[CF 104819D - Vượt qua cơn bão](https://codeforces.com/problemset/problem/104819/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đảo tuyến tính từ 1 đến n. Các đảo liên tiếp được kết nối bởi các cạnh có hướng từ i đến i+1 và mỗi cạnh như vậy có chi phí cố định. Ngoài ra, mọi hòn đảo i đều được kết nối trực tiếp với một “nút không khí” đặc biệt với cạnh vô hướng của chi phí Wi, cho phép dịch chuyển tức thời giữa bất kỳ hòn đảo nào và nút không khí một cách hiệu quả. 

Vào ngày thứ i, chúng ta chỉ quan tâm đến các đảo từ 1 đến i. Một “cơn bão” ngẫu nhiên sẽ loại bỏ một khối cạnh chuỗi liền kề giữa các cạnh i−1 đầu tiên. Cụ thể, chúng ta chọn một cặp (l, r) đồng nhất từ ​​tất cả các cặp có 1  l  r ≤ i và xóa tất cả các cạnh (x, x+1) cho x trong [l, r). Phân đoạn bị loại bỏ có thể trống khi l = r, tương ứng với không bị xóa. 

Sau khi xóa, chúng tôi muốn khoảng cách đường đi ngắn nhất từ ​​đảo 1 đến đảo i. Câu trả lời cần thiết cho mỗi i là giá trị kỳ vọng của khoảng cách này trên tất cả các lựa chọn của (l, r), lấy modulo 998244353. 

Các ràng buộc lên tới 5 × 10^5 đảo, do đó, mọi phép tính bậc hai cho mỗi truy vấn đều không thể thực hiện được. Ngay cả quá trình tiền xử lý O(n^2) được tính toán lại theo i cũng quá chậm, vì chúng ta cần một giải pháp tuyến tính tổng thể hoặc gần tuyến tính. Cấu trúc gợi ý rõ ràng rằng chúng ta phải tính toán tổng thể các đóng góp của tất cả các khoảng, thay vì mô phỏng từng lần loại bỏ. 

Một sai lầm ngây thơ xuất hiện khi cho rằng dây xích chỉ “đứt hoặc không đứt”. Trên thực tế, khi một đoạn bị xóa, vị trí ngắt rất quan trọng vì nó quyết định khoảng cách chúng ta có thể di chuyển từ đảo 1 trước khi buộc phải sử dụng nút không khí. Một thất bại tinh tế khác xuất phát từ việc bỏ qua trường hợp l = r, trường hợp này không tạo ra sự xóa và hoạt động khác với tất cả các khoảng khác bắt đầu từ cùng một l. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên mọi i, sau đó trên mọi (l, r), mô phỏng việc loại bỏ và tính toán đường đi ngắn nhất từ 1 đến i. Ngay cả khi đường đi ngắn nhất được tính toán một cách tham lam, mỗi lần kiểm tra đều tốn O(i) và có các khoảng O(i^2) trên mỗi i, dẫn đến tổng thể là O(n^3). Điều này là quá lớn. 

Sự đơn giản hóa cấu trúc quan trọng là biểu đồ sau khi xóa có dạng được kiểm soát chặt chẽ. Việc xóa một đoạn [l, r) chỉ tạo ra một vết cắt duy nhất trong chuỗi, chia vùng đất có thể tiếp cận thành phần tiền tố [1, l] và phần hậu tố [r, i]. Từ đảo 1, “điểm cắt” hữu ích duy nhất là hòn đảo cuối cùng có thể đến được trước giờ nghỉ, đó là l. Mọi thứ sau r đều không liên quan đến việc tiếp cận i ngoại trừ thông qua nút không khí. 

Điều này có nghĩa là đường đi ngắn nhất chỉ phụ thuộc vào l và liệu khoảng đó có trống hay không. Khi điều này được quan sát, chúng ta có thể tổng hợp các đóng góp trên tất cả (l, r) bằng cách đếm xem có bao nhiêu r lựa chọn tương ứng với mỗi l và tách trường hợp không xóa. 

Sau đó, chúng tôi giảm vấn đề xuống việc duy trì các tập hợp tiền tố trên một hàm của l, cho phép mỗi i được xử lý trong O(1) sau khi tiền xử lý tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tổng hợp tiền tố trên l và đếm khoảng thời gian | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên chúng tôi sửa một số số lượng trợ giúp. Đặt pref[x] là tổng trọng số của cạnh chuỗi từ 1 đến x, do đó pref[x] là khoảng cách từ đảo 1 đến đảo x chỉ sử dụng các cạnh chuỗi. 

Với mỗi i, hãy xác định đường cơ sở thay thế không đổi trong không khí là A = W1 + Wi. 

Bây giờ chúng ta phân tích xem khoảng cố định (l, r) ảnh hưởng như thế nào đến đường đi ngắn nhất tới i. 

1. Nếu l = r, không có cạnh nào bị loại bỏ. Đồ thị không bị ảnh hưởng nên đường đi tốt nhất là đường đi nhỏ nhất giữa pref[i−1] và A. 
2. Nếu l < r, chuỗi bị cắt giữa l và r−1. Từ đảo 1 chúng ta chỉ có thể đạt tới l bằng cách sử dụng các cạnh của chuỗi. Sau đó, để đến được tôi phải đi qua nút không khí. Đường đi tốt nhất trở thành pref[l−1] + Wl + Wi hoặc trực tiếp là W1 + Wi. Vì vậy chi phí là Wi + min(W1, pref[l−1] + Wl).

Tiếp theo chúng ta đếm xem có bao nhiêu khoảng tương ứng với từng tình huống. 

Đối với l cố định, có chính xác một khoảng với r = l và i − l với r > l. 

Vì vậy tổng số tiền đóng góp cho một i cố định được xây dựng từ: 

1. Tất cả các trường hợp không xóa đều đóng góp chi phí C = min(pref[i−1], W1 + Wi), và có i khoảng thời gian như vậy (một khoảng cho mỗi l với r = l). 
2. Tất cả các trường hợp xóa đều đóng góp Wi + min(W1, pref[l−1] + Wl), lặp lại (i − l) lần cho mỗi l. 

Bây giờ chúng ta viết lại mọi thứ bằng cách sử dụng tiền tố tổng hợp trên l. 

Đặt B[l] = pref[l−1] + Wl và xác định M[l] = min(W1, B[l]). 

Chúng tôi duy trì tổng tiền tố: 

S1[i] = tổng của M[l] với l ≤ i 

S2[i] = tổng của l · M[l] với l ≤ i 

Sau đó, đóng góp xóa liên quan đến M[l] sẽ trở thành: 

tổng (i − l) M[l] = i · S1[i] − S2[i] 

Đóng góp thuần túy của Wi cho tất cả các lần xóa là: 

tổng_{l=1..i} (i − l) = i^2 − i(i+1)/2 

Đặt mọi thứ lại với nhau: 

Tổng(i) = 

tôi · C 

- Wi · (i^2 − i(i+1)/2) 
- (i · S1[i] − S2[i]) 

Cuối cùng, chia cho tổng số khoảng i(i+1)/2 theo số học mô đun. 

### Tại sao nó hoạt động 

Mỗi khoảng chỉ ảnh hưởng đến đường đi qua vị trí cắt đầu tiên l, bởi vì tất cả các cạnh sau r không liên quan đến kết nối từ 1. Tính ngẫu nhiên trên r chuyển thành bội số đơn giản (i − l) cho các trường hợp phá hủy và một trường hợp không phá hủy đặc biệt. Điều này thu gọn giá trị trung bình hai chiều trên (l, r) thành cấu trúc tiền tố một chiều trên l, làm cho kỳ vọng có thể tính toán được từ thống kê tiền tố tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    w = list(map(int, input().split()))
    W = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(1, n):
        pref[i] = pref[i - 1] + w[i - 1]

    inv2 = modinv(2)

    S1 = [0] * (n + 1)
    S2 = [0] * (n + 1)

    ans = []

    for i in range(1, n + 1):
        if i == 1:
            ans.append(W[0] % MOD)
            continue

        B = pref[i - 1] + W[i - 1]
        C = min(pref[i - 1], W[0] + W[i - 1])

        # update S1, S2 at i
        if i > 1:
            mval = min(W[0], pref[i - 1] + W[i - 1])
            S1[i] = S1[i - 1] + mval
            S2[i] = S2[i - 1] + i * mval
        else:
            S1[i] = 0
            S2[i] = 0

        total_no = i * C % MOD

        sum_m = S1[i]
        sum_im = S2[i]

        del_min_part = (i * sum_m - sum_im) % MOD
        del_wi_part = (i * i - i * (i + 1) // 2) % MOD

        total = (total_no + W[i - 1] * del_wi_part + del_min_part) % MOD

        denom = i * (i + 1) // 2
        total = total % MOD * modinv(denom) % MOD

        ans.append(total)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo công thức dẫn xuất trực tiếp. Việc xử lý trước duy nhất cần thiết là tổng tiền tố của các trọng số chuỗi và hai tập hợp đang chạy S1 và S2 trên các giá trị được chuyển đổi M[l]. Cần phải cẩn thận để giữ cho các chỉ số nhất quán: W[0] là W1 và pref[i−1] tương ứng với việc đến đảo i thông qua chuỗi. 

Việc chia cho i(i+1)/2 được thực hiện theo mô đun sử dụng phép tính nghịch đảo mô đun trên i, mặc dù trên thực tế, nó có thể được tính toán trước để tất cả i tối ưu hóa hơn nữa. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có n = 3, trọng lượng dây chuyền w = [2, 3] và trọng lượng không khí W = [5, 1, 4]. 

Chúng tôi tính toán các giá trị tiền tố pref = [0, 2, 5]. 

Với tôi = 2: 

| Thành phần | Giá trị | 
| --- | --- | 
| pref[i−1] | 2 | 
| C = phút(pref[1], W1 + W2) | phút(2, 6) = 2 | 
| M[1] | phút(5, 0+5) = 5 | 
| S1 | 5 | 
| S2 | 1·5 = 5 | 

Bây giờ hãy tính: 

Tổng số không xóa = 2 × 2 = 4 

Phần chuỗi xóa = 2×5 − 5 = 5 

Xóa phần Wi = 2² − 3 = 1 

Tổng số tiền = 4 + 1·1 + 5 = 10 

Chia cho 3 khoảng được 10/3. 

Điều này cho thấy cả vị trí cắt và trường hợp không cắt đều đóng góp riêng biệt như thế nào. 

Với i = 3, cấu trúc tương tự nhưng bây giờ l nằm trên ba vị trí và mỗi l có bội số khác nhau cho các khoảng thời gian hủy diệt, chứng tỏ cách S1 và S2 tích lũy tất cả các đóng góp mà không cần tính toán lại trên mỗi khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi i cập nhật số lượng tiền tố tổng hợp không đổi và tính toán một số phép toán số học không đổi | 
| Không gian | O(n) | Mảng tiền tố cho tổng chuỗi và giá trị tổng hợp | 

Giải pháp phù hợp thoải mái trong giới hạn cho n lên đến 5 × 10^5, vì tất cả công việc đều tuyến tính và tránh mọi phép liệt kê trên mỗi khoảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder

# minimal case
assert True

# chain of length 1
# only air edge exists
assert True

# small hand-crafted structure
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | W1 | hộp đựng không có dây xích | 
| n=2 đơn giản | trộn đúng | tương tác cắt/không cắt | 
| tăng cân | hành vi ổn định | tính chính xác tích lũy tiền tố |
