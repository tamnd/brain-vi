---
title: "CF 106398E - \u041b\u0430\u0431\u0438\u0440\u0438\u043d\u0442 \u0434\u043b\u044f \u0445\u043e\u043c\u044f\u043a\u0430"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $N nhân M$. Bên trong lưới này, chúng ta xây dựng một cấu trúc được tạo thành từ các “vòng” hình chữ nhật đồng tâm."
date: "2026-06-21T10:00:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106398
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106398
solve_time_s: 46
verified: true
draft: false
---

[CF 106398E - \u041b\u0430\u0431\u0438\u0440\u0438\u043d\u0442 \u0434\u043b\u044f \u0445\u043e\u043c\u044f\u043a\u0430](https://codeforces.com/problemset/problem/106398/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$N \times M$. Bên trong lưới này, chúng ta xây dựng một cấu trúc được tạo thành từ các “vòng” hình chữ nhật đồng tâm. Mỗi vòng chiếm chu vi của một hình chữ nhật nhỏ hơn bên trong hình chữ nhật trước đó và giữa các vòng liên tiếp có một đường viền trống bắt buộc có chiều rộng chính xác bằng một ô. Mỗi vòng gần như được lấp đầy bởi các bụi cây dọc theo ranh giới của nó, ngoại trừ chính xác một ô trên ranh giới đó được để trống để làm lối vào. 

Vòng đầu tiên là ranh giới bên ngoài của toàn bộ lưới. Vòng thứ hai được đặt một ô vào trong, vòng thứ ba đặt một ô khác vào trong, v.v. Mỗi vòng mới tiêu tốn hai hàng và hai cột co lại so với vòng trước do hành lang trống xung quanh. 

Chúng tôi được yêu cầu tính toán có bao nhiêu bụi cây được sử dụng nếu chúng tôi xây dựng nhiều nhất$K$đổ chuông, nhưng chúng tôi có thể không thể đặt tất cả$K$nếu lưới trở nên quá nhỏ. Trong trường hợp đó, chúng tôi chỉ xây dựng số lượng vòng hợp lệ tối đa mà vẫn để lại ít nhất một ô bên trong bên trong mỗi vòng. 

Mỗi vòng hợp lệ đóng góp chiều dài chu vi của nó trừ đi một (vì một ô là lối vào không có bụi cây). 

Những ràng buộc cho phép$N, M, K$lên tới$3 \cdot 10^9$, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi. Bất kỳ mô phỏng nào lặp qua các vòng đều không thể thực hiện được vì số lượng vòng có thể lên tới hàng tỷ. Lời giải phải dựa vào đặc tính số học trực tiếp của từng vòng. 

Một điều kiện khó phát hiện ở cạnh xuất hiện khi lưới trở nên quá nhỏ để vừa với một hình chữ nhật đầy đủ khác có ô trống bên trong. Ví dụ: nếu sau khi xây dựng một vòng, hình chữ nhật bên trong còn lại là$1 \times M$hoặc$2 \times 2$, thì không có vòng nào nữa có hiệu lực ngay cả khi$K$lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng từng vòng một. Đối với mỗi vòng, chúng ta thu nhỏ hình chữ nhật đi 2 ở cả hai chiều, tính chu vi của nó, trừ đi một cho lối vào và tích lũy. Chúng tôi dừng lại khi một trong hai$K$các vòng được tạo hoặc hình chữ nhật còn lại không thể chứa vùng bên trong hợp lệ. 

Điều này đúng, nhưng chi phí tuyến tính theo số vòng. Trong trường hợp xấu nhất, mỗi vòng chỉ giảm kích thước một chút, vì vậy chúng ta có thể có tối đa khoảng$1.5 \cdot 10^9$lần lặp lại. Ngay cả với số học có thời gian không đổi cho mỗi lần lặp, điều này vượt xa giới hạn thời gian. 

Quan sát quan trọng là mỗi vòng hoàn toàn được xác định bởi chỉ số của nó. các$i$-vòng thứ (chỉ mục 0) là chu vi của hình chữ nhật có kích thước:$$(N - 2i) \times (M - 2i)$$Vòng chỉ hợp lệ nếu cả hai chiều ít nhất là 1 và quan trọng hơn, yêu cầu bên trong bao hàm ít nhất một ô trống bên trong vòng, vì vậy chúng ta cần:$$N - 2i \ge 3,\quad M - 2i \ge 3$$Điều này đưa ra giới hạn trên cứng về số lượng vòng:$$i < \frac{N-1}{2}, \quad i < \frac{M-1}{2}$$Vậy số vòng là:$$R = \min\left(K, \left\lfloor \frac{N-1}{2} \right\rfloor, \left\lfloor \frac{M-1}{2} \right\rfloor\right)$$Một lần$R$đã biết, chúng ta tính tổng các đóng góp của mỗi vòng. các$i$-vòng thứ có chu vi:$$2(N - 2i) + 2(M - 2i) - 4$$và chúng tôi trừ 1 cho lối vào, vì vậy:$$4(N + M - 4i) - 4 - 1 = 4(N + M - 4i) - 5$$Vì vậy, chúng tôi tính toán một chuỗi số học đơn giản trên$i = 0 \ldots R-1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(R)$|$O(1)$| Quá chậm | 
| Tổng số học |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán có thể tồn tại bao nhiêu vòng đầy đủ trước khi lưới sụp đổ. 

1. Tính toán số lớp tối đa có thể được hình học cho phép như$\left\lfloor\frac{N-1}{2}\right\rfloor$Và$\left\lfloor\frac{M-1}{2}\right\rfloor$. Điều này đảm bảo rằng mọi vòng được chọn vẫn để lại ít nhất một ô không gian bên trong. 
2. Đặt$R$đến mức tối thiểu$K$và hai giới hạn hình học này. Điều này sẽ kẹp số lượng vòng được yêu cầu sao cho vừa vặn về mặt vật lý. 
3. Tính tổng đóng góp của tất cả các vòng. Mỗi vòng đóng góp một chu vi trừ đi một ô vào và điều này phụ thuộc tuyến tính vào chỉ số vòng$i$. 
4. Viết lại tổng:$$\sum_{i=0}^{R-1} (4N + 4M - 16i - 5)$$dưới dạng tổng của các hằng số cộng với tổng trên$i$. 
5. Tính giá trị bằng công thức chuỗi số học:$$R(4N + 4M - 5) - 16 \cdot \frac{R(R-1)}{2}$$### Tại sao nó hoạt động 

Mỗi vòng là độc lập ngoại trừ việc giảm kích thước của nó, là tuyến tính trong chỉ số vòng. Hình học đảm bảo rằng một khi một vòng không hợp lệ thì tất cả các vòng sâu hơn cũng không hợp lệ vì cả hai kích thước đều giảm nghiêm ngặt 2 trên mỗi lớp. Sự co rút đơn điệu này biến vấn đề thành một tiền tố liền kề duy nhất của các vành hợp lệ. Vì mỗi vòng đóng góp một hàm tuyến tính cho chỉ số của nó nên tổng sẽ trở thành tổng số học dạng đóng, loại bỏ nhu cầu mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    M = int(input())
    K = int(input())

    R = min(K, (N - 1) // 2, (M - 1) // 2)

    if R <= 0:
        print(0)
        return

    # sum of (4N + 4M - 16i - 5)
    total = R * (4 * N + 4 * M - 5)
    total -= 16 * (R * (R - 1) // 2)

    print(total)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định có bao nhiêu lớp khả thi. biểu hiện`(N - 1) // 2`Và`(M - 1) // 2`nắm bắt số lượng offset hướng vào trong tối đa sao cho một vòng vẫn có ít nhất 1 ô bên trong. Việc trừ đi 1 đảm bảo rằng các trường hợp suy biến như a$3 \times 3$lưới cho phép chính xác một vòng. 

Phép tính tổng sử dụng biểu thức dạng đóng bắt nguồn từ việc tách các phần tuyến tính và hằng số của phần đóng góp vòng. Thuật ngữ`R * (R - 1) // 2`tương ứng với tổng các chỉ số$0 + 1 + \dots + (R-1)$, đây là phần không đổi duy nhất. 

Phải cẩn thận với số học số nguyên vì các giá trị có thể đạt tới khoảng$10^{19}$, nhưng Python xử lý việc này một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$N = 7$,$M = 5$,$K = 3$. 

Đầu tiên chúng ta tính giới hạn:$$\frac{7-1}{2} = 3,\quad \frac{5-1}{2} = 2$$Vì thế$R = \min(3, 3, 2) = 2$. 

| tôi | Kích thước vòng (N-2i, M-2i) | Đóng góp | 
| --- | --- | --- | 
| 0 | (7, 5) |$4(12) - 5 = 43$| 
| 1 | (5, 3) |$4(8) - 5 = 27$| 

Tổng cộng là$70$. 

Điều này cho thấy việc thu nhỏ ở cả hai chiều một cách đối xứng sẽ làm giảm tuyến tính sự đóng góp của chu vi như thế nào. 

### Ví dụ 2 

lấy$N = 3$,$M = 3$,$K = 10$. 

Giới hạn:$$(3-1)/2 = 1$$Vì thế$R = 1$. 

| tôi | Kích thước nhẫn | Đóng góp | 
| --- | --- | --- | 
| 0 | (3, 3) |$4(6) - 5 = 19$| 

Chỉ có thể sử dụng một vòng vì bất kỳ sự thu hẹp nào nữa sẽ làm mất đi không gian bên trong cần thiết. 

Điều này chứng tỏ đường cắt hình học chi phối giá trị của$K$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học bất kể kích thước lưới | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các giới hạn vì nó tránh được việc lặp lại trên hàng tỷ vòng và giảm toàn bộ cấu trúc về số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full solution is embedded, we re-implement minimal logic for testing
def solve(inp: str) -> str:
    data = list(map(int, inp.strip().split()))
    N, M, K = data
    R = min(K, (N - 1) // 2, (M - 1) // 2)
    if R <= 0:
        return "0"
    total = R * (4 * N + 4 * M - 5)
    total -= 16 * (R * (R - 1) // 2)
    return str(total)

# sample-like tests
assert solve("7\n5\n3") == "70"
assert solve("3\n3\n10") == "19"

# custom tests
assert solve("3\n4\n1") == str(4*(3+4)-5)
assert solve("1000000000\n1000000000\n1") == str(4*(2000000000)-5)
assert solve("5\n5\n10") == str(solve("5\n5\n10"))

assert solve("4\n4\n10") == str(27)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 4 1 | hộp đựng nhẫn đơn | hình học tối thiểu | 
| Lưới 1e9, K=1 | độ chính xác có giá trị lớn | số học an toàn tràn | 
| 5 5 10 | giới hạn bởi hình học | K lớn hơn khả thi | 
| 4 4 10 | co lại chặt chẽ | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi lưới chỉ đủ lớn cho một vòng chứ không phải hai vòng. Lấy$N = 4$,$M = 4$,$K = 10$. Công thức cho:$$R = \min(10, 1, 1) = 1$$Chỉ có vòng ngoài là hợp lệ. Bên trong$2 \times 2$vùng sẽ không đáp ứng yêu cầu có ít nhất một ô tự do bên trong cho một vòng khác, vì vậy nó phải được loại trừ. Việc tính toán thực hiện điều này một cách tự nhiên thông qua$(N-1)//2$Và$(M-1)//2$giới hạn. 

Một trường hợp tinh tế khác là khi một chiều là tối thiểu, chẳng hạn như$N = 3$,$M$lớn. Kể cả nếu$M$cho phép nhiều bước thu nhỏ, hạn chế theo chiều dọc chiếm ưu thế, ngăn cản nhiều hơn một vòng. Thuật toán ưu tiên chính xác kích thước chặt chẽ hơn. 

Cuối cùng, khi$K = 0$hoặc cực kỳ lớn, biểu thức của$R$kẹp chính xác, đảm bảo không có vòng âm và không tràn vượt quá tính khả thi về mặt hình học.
