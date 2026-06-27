---
title: "CF 105093H - Năng lượng cầu vồng"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi tinh thể có một tập hợp các tinh thể, mỗi tinh thể có một màu sắc và giá trị bức xạ."
date: "2026-06-27T20:50:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "H"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 36
verified: true
draft: false
---

[CF 105093H - Năng lượng cầu vồng](https://codeforces.com/problemset/problem/105093/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi tinh thể có một tập hợp các tinh thể, mỗi tinh thể có một màu sắc và giá trị bức xạ. Chúng ta phải chọn nhiều nhất$m$tinh thể, nhưng trước khi sử dụng chúng, chúng ta được phép hợp nhất bất kỳ số lượng tinh thể cùng màu nào bằng cách thay thế liên tục hai trong số chúng bằng một tinh thể có bức xạ bằng tổng của hai tinh thể ban đầu. Hợp nhất không làm thay đổi màu sắc, chỉ tổng hợp bức xạ. 

Sau khi hợp nhất tất cả, mỗi màu còn lại đóng góp tối đa một tinh thể, bởi vì nếu vẫn còn hai tinh thể cùng màu thì điều đó không hợp lệ đối với thiết bị cuối cùng. Điểm cuối cùng là tích của các giá trị bức xạ của tất cả các tinh thể có màu sắc riêng biệt được chọn. Nhiệm vụ là nhặt lên$m$tinh thể ban đầu, hợp nhất chúng một cách tối ưu trong các màu sắc và tối đa hóa sản phẩm này. 

Ràng buộc về cấu trúc chính là$n \le 15$, vì vậy ngay cả các tập hợp con hàm mũ cũng có thể thực hiện được. Tuy nhiên, các giá trị bức xạ nhỏ và có tính chất nhân lên cho thấy các lựa chọn tham lam rõ ràng là không an toàn nếu không liệt kê các cấu hình. Nhỏ này$n$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng tối ưu hóa trên không gian trạng thái lớn hoặc thực hiện DP liên tục trên các tổng, nhưng lại khuyến khích mạnh mẽ việc liệt kê tập hợp con hoặc lập trình động bitmask. 

Một cạm bẫy tinh vi là hiểu lầm “chiếc ba lô có kích thước$m$" ràng buộc. Bạn không chọn màu sắc; bạn chọn các tinh thể ban đầu. Nhưng việc hợp nhất có nghĩa là việc chọn nhiều tinh thể cùng một màu sẽ biến chúng thành một vật thể mạnh hơn một cách hiệu quả. Ví dụ: chọn ba tinh thể màu 2 sẽ mang lại một tinh thể duy nhất có bức xạ tổng hợp và đóng góp chính xác một yếu tố vào sản phẩm cuối cùng. 

Một trường hợp tế nhị khác là việc lấy nhiều tinh thể màu hơn không phải lúc nào cũng có lợi trừ khi nó làm tăng sản phẩm sau khi hợp nhất. Ví dụ, hai tinh thể có bức xạ 1 và 1 trở thành 2, có thể cải thiện hoặc không cải thiện sản phẩm tùy thuộc vào các giá trị được chọn khác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là thử tối đa mọi tập hợp con tinh thể có kích thước$m$. Đối với mỗi tập hợp con, chúng tôi mô phỏng việc hợp nhất bằng cách nhóm theo màu sắc và tổng bức xạ cho mỗi nhóm. Sau đó, chúng tôi tính tích của tổng số kết quả trên mỗi màu. Điều này đúng vì nó trực tiếp mô hình hóa quy trình. Số tập con là$2^n$, và với$n \le 15$, đây nhiều nhất là 32768 tập con, đủ nhỏ. Tuy nhiên, chúng ta cũng cần thực thi ràng buộc về kích thước tập hợp con và tính toán việc nhóm cho mỗi tập hợp con, mang lại thêm$O(n)$yếu tố, vẫn ổn trong sự cô lập. 

Vấn đề tế nhị là chúng ta thực sự đang tính toán gấp đôi cấu trúc nếu không cẩn thận: thứ tự hợp nhất không quan trọng, chỉ quan trọng tổng số cho mỗi màu. Vì vậy, mỗi tập hợp con ánh xạ duy nhất tới nhiều tập hợp màu có trọng số tổng hợp. 

Quan sát quan trọng là vì$n$rất nhỏ, chúng ta không cần phải khéo léo với việc nén DP hay các chiến lược tham lam. Chúng tôi trực tiếp liệt kê các tập hợp con, tính tổng tổng hợp cho mỗi màu và đánh giá sản phẩm. “Ba lô có kích thước$m$” ràng buộc được xử lý đơn giản bằng cách bỏ qua các tập hợp con lớn hơn$m$. 

Điều này làm giảm vấn đề đối với việc liệt kê tập hợp con với đánh giá tuyến tính trên mỗi tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot n)$|$O(n)$| Đã chấp nhận | 
| Đánh giá Bitmask (tối ưu) |$O(2^n \cdot n)$|$O(n)$| Đã chấp nhận | 

Trong thực tế, cả hai đều giống hệt nhau; "tối ưu hóa" nhận ra rằng không cần cấu trúc nâng cao hơn. 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, đọc tất cả các tinh thể và lưu trữ chúng dưới dạng cặp màu và bức xạ. 
2. Lặp lại tất cả các mặt nạ bit từ$0$ĐẾN$2^n - 1$, trong đó mỗi bitmask đại diện cho một tập hợp con các tinh thể. Điều này mã hóa quyết định chúng ta chọn loại tinh thể nào trước khi hợp nhất. 
3. Đối với mỗi tập hợp con, hãy đếm xem có bao nhiêu tinh thể được chọn. Nếu điều này vượt quá$m$, hãy loại bỏ tập hợp con ngay lập tức vì nó vi phạm ràng buộc về ba lô. 
4. Xây dựng một mảng hoặc từ điển được lập chỉ mục theo màu sắc và tích lũy các giá trị bức xạ của tất cả các tinh thể đã chọn có màu đó. Điều này mô phỏng tất cả các phép hợp nhất được phép, vì việc hợp nhất trong một màu tương đương với việc tính tổng. 
5. Tính tích của tất cả các tổng màu khác 0. 
6. Theo dõi tích lớn nhất trên tất cả các tập hợp con hợp lệ. 

Tính chính xác phụ thuộc vào thực tế là khi một tập hợp con được cố định, việc hợp nhất mang tính quyết định: tất cả các tinh thể có màu nhất định sẽ sụp đổ thành một giá trị duy nhất bằng tổng của chúng. Không có cấu hình thay thế nào làm thay đổi kết quả. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình cuối cùng hợp lệ nào đều tương ứng với việc chọn một số tập hợp con của các tinh thể ban đầu, sau đó hợp nhất trong mỗi màu một cách tùy ý. Việc hợp nhất không làm thay đổi tổng bức xạ trên mỗi màu, chỉ thay đổi số lượng mục trên mỗi màu. Do đó, mỗi tập hợp con xác định duy nhất một sản phẩm cuối cùng và mọi giải pháp khả thi đều tương ứng với chính xác một tập hợp con. Việc sử dụng hết tất cả các tập hợp con đảm bảo chúng tôi kiểm tra mọi kết quả có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        c = []
        r = []
        for _ in range(n):
            ci, ri = map(int, input().split())
            c.append(ci)
            r.append(ri)

        ans = 0

        for mask in range(1 << n):
            if mask.bit_count() > m:
                continue

            color_sum = {}
            for i in range(n):
                if mask & (1 << i):
                    color_sum[c[i]] = color_sum.get(c[i], 0) + r[i]

            prod = 1
            for val in color_sum.values():
                prod *= val

            if prod > ans:
                ans = prod

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược liệt kê tập hợp con. các`bit_count()`kiểm tra thực thi sớm ràng buộc kích thước, cắt bớt các tập hợp con không hợp lệ. Từ điển`color_sum`tổng hợp bức xạ trên mỗi màu, tất cả các mô hình đều cho phép hợp nhất ngầm. 

Phép tính tích sẽ nhân tất cả các giá trị tổng hợp, bỏ qua các màu trống vì chúng không xuất hiện trong từ điển. Câu trả lời được cập nhật một cách tham lam trên tất cả các tập hợp con. 

Một điểm tinh tế là giá trị bức xạ riêng lẻ nhỏ nhưng sản phẩm có thể phát triển nhanh chóng. Các số nguyên chính xác tùy ý của Python ngăn ngừa sự cố tràn nên không cần xử lý thêm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với ba tinh thể:$(1, 3), (2, 2), (2, 2)$, Và$m = 3$. 

Chúng tôi liệt kê các tập hợp con: 

| mặt nạ | tinh thể đã chọn | tổng màu | sản phẩm | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 000 | không | {} | 1 | vâng | 
| 001 | (1,3) | {1:3} | 3 | vâng | 
| 010 | (2,2) | {2:2} | 2 | vâng | 
| 011 | (2,2),(1,3) | {1:3,2:2} | 6 | vâng | 
| 110 | (2,2),(2,2) | {2:4} | 4 | vâng | 
| 111 | tất cả | {1:3,2:4} | 12 | vâng | 

Tối đa là 12, đạt được bằng cách lấy tất cả các tinh thể và hợp nhất hai tinh thể màu 2. 

Dấu vết này cho thấy rằng việc hợp nhất không bao giờ là một điểm quyết định: một khi một tập hợp con đã được cố định thì việc tổng hợp sẽ mang tính quyết định. 

Bây giờ hãy xem xét$m = 2$trên cùng một đầu vào. 

| mặt nạ | đã chọn | kích thước | hợp lệ | sản phẩm | 
| --- | --- | --- | --- | --- | 
| 011 | (1,3),(2,2) | 2 | vâng | 6 | 
| 110 | (2,2),(2,2) | 2 | vâng | 4 | 

Giá trị tốt nhất là 6. Điều này chứng tỏ rằng việc giới hạn kích thước tập hợp con sẽ trực tiếp cắt bớt các cấu hình có lợi từ việc hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot 2^n \cdot n)$| Mỗi tập hợp con được liệt kê và đối với mỗi tập hợp con, chúng tôi quét tất cả các phần tử để tích lũy tổng theo từng màu | 
| Không gian |$O(n)$| Lưu trữ cho mảng đầu vào và tổng hợp màu tạm thời | 

Với$n \le 15$, số lượng tối đa của
