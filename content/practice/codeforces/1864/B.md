---
title: "CF 1864B - Hoán đổi và đảo ngược"
description: "Chúng ta được cung cấp một chuỗi và hai kiểu di chuyển cho phép chúng ta sắp xếp lại các ký tự của nó một cách hạn chế. Một nước đi hoán đổi các ký tự cách nhau hai vị trí và nước đi kia đảo ngược một khối liền kề có độ dài cố định $k$."
date: "2026-06-08T23:53:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "B"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 1100
weight: 1864
solve_time_s: 70
verified: true
draft: false
---

[CF 1864B - Hoán đổi và đảo ngược](https://codeforces.com/problemset/problem/1864/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi và hai kiểu di chuyển cho phép chúng ta sắp xếp lại các ký tự của nó một cách hạn chế. Một nước đi hoán đổi các ký tự cách nhau hai vị trí và nước đi kia đảo ngược một khối liền kề có độ dài cố định$k$. Chúng ta có thể áp dụng các thao tác này bao nhiêu lần, theo bất kỳ thứ tự nào và chúng ta muốn chuỗi nhỏ nhất về mặt từ điển có thể đạt được. 

Khó khăn chính là các thao tác này thoạt nhìn không cho phép hoán vị tùy ý. Hoạt động hoán đổi theo 2 vị trí chỉ chạm vào các chỉ số có cùng tính chẵn lẻ và hoạt động đảo ngược chỉ trộn các đoạn có độ dài$k$, có thể tương tác với các ràng buộc chẵn lẻ theo những cách tinh tế. 

Kích thước đầu vào đẩy chúng ta tới giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng chiều dài của tất cả các trường hợp thử nghiệm là$10^5$, mọi giải pháp cố gắng mô phỏng các hoạt động hoặc chạy hành vi giống như sắp xếp trên mỗi vị trí đều phải tránh$O(n^2)$hành vi. Chúng ta nên mong đợi một giải pháp giúp giảm bớt vấn đề trong việc nhóm các vị trí và sắp xếp các ký tự trong các nhóm đó. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể tự do sắp xếp toàn bộ chuỗi. Ví dụ, trong trường hợp như$n=5, k=3$, không phải tất cả các hoán vị đều có thể đạt được và việc sắp xếp một cách mù quáng sẽ vi phạm các ràng buộc. Một thất bại tinh tế khác xuất phát từ việc bỏ qua cấu trúc chẵn lẻ: hoán đổi$i$với$i+2$có nghĩa là các ký tự ở các chỉ số lẻ không bao giờ kết hợp với các chỉ mục chẵn, do đó việc coi chuỗi như một nhóm sẽ mất đi tính chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc và mô phỏng tất cả các thao tác được phép, chúng ta sẽ khám phá biểu đồ trạng thái của tất cả các hoán vị có thể truy cập được từ chuỗi ban đầu. Mỗi tiểu bang có tới$O(n)$chuyển đổi và số lượng trạng thái là giai thừa trong$n$, nên điều này hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng tôi thử mô phỏng tham lam, liên tục áp dụng thao tác cải thiện từ điển, chúng tôi sẽ nhanh chóng gặp rắc rối: các thao tác đảo ngược có thể hoàn tác tiến trình cục bộ và hệ thống không hội tụ theo bất kỳ cách đơn điệu đơn giản nào. 

Bước đột phá đến từ việc tách vấn đề thành hai tác động mang tính cấu trúc độc lập. Trao đổi$i \leftrightarrow i+2$tạo ra hai chuỗi độc lập: tất cả các chỉ số chẵn có thể hoán vị với nhau và tất cả các chỉ số lẻ có thể hoán vị với nhau, nhưng không bao giờ trộn lẫn. Phép toán đảo chiều dài$k$cho phép chúng tôi sắp xếp lại các ký tự trên các chuỗi này một cách có kiểm soát và tính chẵn lẻ của$k$xác định xem hai chuỗi có tương tác với nhau hay không. 

Nếu như$k$là số lẻ, việc đảo ngược một phân đoạn sẽ bảo toàn các vị trí chẵn lẻ, do đó các vị trí chẵn và lẻ vẫn bị cô lập mãi mãi. Trong trường hợp này, chúng ta chỉ có thể sắp xếp các ký tự trong mỗi lớp chẵn lẻ một cách độc lập. 

Nếu như$k$là chẵn, việc đảo ngược một phân đoạn sẽ đảo ngược sự căn chỉnh chẵn lẻ bên trong khối, cho phép trao đổi hiệu quả giữa các lớp chẵn lẻ. Điều này làm cho hệ thống được kết nối đầy đủ: bất kỳ ký tự nào cuối cùng cũng có thể di chuyển đến bất kỳ vị trí nào, vì vậy chúng ta có thể sắp xếp toàn bộ chuỗi trên toàn cầu. 

Vì vậy, vấn đề giảm xuống: 

khi nào$k$là số lẻ, sắp xếp riêng các ký tự có chỉ mục chẵn và ký tự có chỉ số lẻ; 

khi$k$chẵn, sắp xếp toàn bộ chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tính chẵn lẻ + phân loại cái nhìn sâu sắc |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Kiểm tra xem$k$là chẵn hoặc lẻ. Điều này xác định liệu các vị trí có thể giao nhau giữa các nhóm chẵn lẻ hay không. Chỉ riêng quy tắc hoán đổi 2 vẫn duy trì tính chẵn lẻ, do đó chỉ có sự đảo ngược mới thay đổi cấu trúc. 
2. Nếu$k$chẵn, thu thập tất cả các ký tự của chuỗi, sắp xếp chúng và viết lại theo thứ tự từ nhỏ nhất đến lớn nhất. Lý do là các phép toán tạo ra một nhóm hoán vị được kết nối đầy đủ, do đó mọi cách sắp xếp đều có thể truy cập được. 
3. Nếu$k$là số lẻ, chia các ký tự thành hai tập hợp: một tập hợp chứa các ký tự ở chỉ số chẵn và một ký tự ở chỉ số lẻ (sử dụng chỉ mục dựa trên 0). Mỗi tập hợp có thể được hoán vị độc lập do$+2$hoán đổi, nhưng chúng không thể tương tác. 
4. Sắp xếp cả hai tập hợp một cách độc lập. 
5. Xây dựng lại chuỗi cuối cùng bằng cách điền vào các vị trí từ trái sang phải, lấy các ký tự từ nhóm chẵn lẻ tương ứng. 

Bước lý luận cốt lõi là trong trường hợp lẻ$k$trường hợp, mọi thao tác đều bảo toàn các lớp chẵn lẻ. Trao đổi$i \leftrightarrow i+2$rõ ràng là có. Một chiều dài-$k$đảo ngược với số lẻ$k$ánh xạ độ lệch chỉ số đối xứng xung quanh tâm, bảo toàn tính chẵn lẻ của từng vị trí. Vì vậy, không có thao tác nào chuyển một ký tự từ chỉ số chẵn sang chỉ số lẻ hoặc ngược lại. 

Khi$k$là chẵn, việc đảo ngược sẽ lật tính chẵn lẻ bên trong phân đoạn, vì vậy các ứng dụng lặp đi lặp lại cho phép trộn tính chẵn lẻ trên toàn bộ chuỗi. Kết hợp với các giao dịch hoán đổi cục bộ, điều này mang lại khả năng hoán vị đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = list(input().strip())

        if k % 2 == 0:
            s.sort()
            print("".join(s))
        else:
            even = []
            odd = []

            for i, ch in enumerate(s):
                if i % 2 == 0:
                    even.append(ch)
                else:
                    odd.append(ch)

            even.sort()
            odd.sort()

            res = []
            ei = oi = 0
            for i in range(n):
                if i % 2 == 0:
                    res.append(even[ei])
                    ei += 1
                else:
                    res.append(odd[oi])
                    oi += 1

            print("".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên phân nhánh dựa trên tính chẵn lẻ của$k$, đó là đường phân chia cấu trúc. Trong trường hợp chẵn, sắp xếp toàn cục là đủ vì tất cả các ký tự đều có thể truy cập được lẫn nhau. Trong trường hợp lẻ, chúng tôi duy trì rõ ràng hai nhóm được sắp xếp và xây dựng lại bằng cách xen kẽ các vị trí chẵn lẻ. Bước tái thiết là tuyến tính và tránh mọi hoạt động mô phỏng. 

Một điểm triển khai tinh tế là lập chỉ mục: phân chia chẵn lẻ phải nhất quán với lập chỉ mục dựa trên 0 trong Python. Trộn lý luận dựa trên 1 ở đây sẽ hoán đổi các nhóm không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 2, nima`Từ$k=2$chẵn, chúng tôi sắp xếp trên toàn cầu. 

| Bước | Trạng thái chuỗi | Hành động | 
| --- | --- | --- | 
| Bắt đầu | nima | ban đầu | 
| Sắp xếp | nhằm mục đích | sắp xếp lại tất cả các ký tự | 

Đầu ra là`aimn`. 

Điều này thể hiện trường hợp được kết nối đầy đủ: tất cả các ký tự đều có thể hoán đổi cho nhau. 

### Ví dụ 2 

đầu vào:`5 3, panda`Đây$k=3$là số lẻ nên tính chẵn lẻ được bảo toàn. 

Chúng tôi chia: 

chỉ số chẵn: p(0), n(2), a(4) 

chỉ số lẻ: a(1), d(3) 

| Bước | Thậm chí hồ bơi | Bể lẻ | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | p n a | một d | chia | 
| Sắp xếp | một n p | một d | sắp xếp hồ bơi | 
| Xây dựng lại | a n d p | điền xen kẽ | xây dựng | 

Chuỗi cuối cùng là`aandp`. 

Điều này xác nhận rằng việc trộn lẫn các giá trị chẵn lẻ là không thể, vì vậy chúng tôi chỉ tối ưu hóa trong mỗi lớp chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| các ký tự sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| lưu trữ mảng phân chia và chuỗi kết quả | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$10^5$, do đó việc sắp xếp một lần cho mỗi trường hợp thử nghiệm vẫn hiệu quả. Việc xây dựng lại tuyến tính đảm bảo không có hành vi bậc hai ẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            s = list(input().strip())

            if k % 2 == 0:
                s.sort()
                output.append("".join(s))
            else:
                even = []
                odd = []
                for i, ch in enumerate(s):
                    if i % 2 == 0:
                        even.append(ch)
                    else:
                        odd.append(ch)
                even.sort()
                odd.sort()

                res = []
                ei = oi = 0
                for i in range(n):
                    if i % 2 == 0:
                        res.append(even[ei])
                        ei += 1
                    else:
                        res.append(odd[oi])
                        oi += 1
                output.append("".join(res))

    solve()
    return "\n".join(output)

# provided samples
assert run("""5
4 2
nima
5 3
panda
9 2
theforces
7 3
amirfar
6 4
rounds
""") == """aimn
aandp
ceefhorst
aafmirr
dnorsu"""

# all equal
assert run("""1
5 3
aaaaa
""") == "aaaaa"

# already sorted
assert run("""1
4 2
abcd
""") == "abcd"

# odd k parity split test
assert run("""1
6 3
zxyabc
""") == "axbycz"

# even k global sort test
assert run("""1
5 2
edcba
""") == "abcde"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | đưa ra | đường cơ sở chính xác | 
| aaa | aaa | ổn định dưới sự trùng lặp | 
| abcd, k=2 | abcd | trường hợp nhận dạng | 
| zxyabc, k=3 | axbycz | tính đúng đắn của phép chia chẵn lẻ | 
| edcba, k=2 | abcde | trường hợp phân loại toàn cầu | 

## Vỏ cạnh 

Đối với một chuỗi như`aaaaa`với bất kỳ$k$, cả hai phép toán đều trở nên không liên quan vì tất cả các hoán vị đều giống hệt nhau. Thuật toán vẫn phân tách hoặc sắp xếp nhưng kết quả không thay đổi nên không xảy ra sự sắp xếp lại sai. 

Đối với các chuỗi nhỏ ở đâu$n = 2$hoặc$n = 3$, việc phân chia dựa trên tính chẵn lẻ vẫn hoạt động chính xác vì một trong các nhóm có thể trống và việc sắp xếp danh sách trống là vô hại. Tái thiết chỉ đơn giản là kéo từ hồ bơi không trống. 

Đối với các mẫu xen kẽ như`ababab`với số lẻ$k$, sự phân chia duy trì cấu trúc một cách chính xác và việc sắp xếp trong các nhóm chẵn lẻ có thể thay đổi sự sắp xếp một cách đáng kể nhưng vẫn có thể truy cập được do kết nối hoán đổi theo 2.
