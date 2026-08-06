---
title: "CF 102483J - Cá cược Jinxed"
description: "Julia là một trong số nhiều người đặt cược. Điểm hiện tại của cô ấy ít nhất cũng lớn bằng những người khác. Sau mỗi trận đấu trong tương lai, cô ấy sao chép đa số dự đoán của những người đặt cược hiện có số điểm cao nhất trong số các đối thủ."
date: "2026-08-05T18:43:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "J"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 211
verified: true
draft: false
---

[CF 102483J - Cá cược Jinxed](https://codeforces.com/problemset/problem/102483/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Julia là một trong số nhiều người đặt cược. Điểm hiện tại của cô ấy ít nhất cũng lớn bằng những người khác. Sau mỗi trận đấu trong tương lai, cô ấy sao chép đa số dự đoán của những người đặt cược hiện có số điểm cao nhất trong số các đối thủ. Câu hỏi đặt ra là cô ấy được đảm bảo bao nhiêu trận đấu sắp tới sẽ không tụt lại phía sau bất kỳ đối thủ nào, ngay cả khi cược tương lai và kết quả trận đấu được chọn trái ngược. 

Thay vì lưu trữ điểm số trực tiếp, việc xem xét thâm hụt của mọi đối thủ từ Julia sẽ dễ dàng hơn. Nếu Julia có điểm`S`và một người đặt cược khác đã ghi điểm`x`, thâm hụt của họ là`S - x`. Sự thâm hụt của`0`có nghĩa là họ đang hòa và mức thâm hụt âm có nghĩa là Julia đã mất vị trí dẫn đầu. 

Có thể có tới`100000`người đặt cược, và điểm số có thể lớn bằng`10^16`. Việc mô phỏng từng trận đấu một là không thể vì câu trả lời cũng có thể xoay quanh`10^16`. Giải pháp phải bỏ qua những hành vi giống hệt nhau. 

Cơ cấu then chốt là chỉ những đối thủ có mức thâm hụt nhỏ nhất mới quan trọng. Họ là đương kim á quân. Những người còn lại ở xa hơn và có thể được coi là nhóm đi sau cuối cùng sẽ về nhì. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ giữ lại tất cả các điểm số và liên tục xác định những người dẫn đầu hiện tại, phiếu bầu đa số và kết quả tồi tệ nhất có thể xảy ra. Chi phí một trận đấu`O(n)`, bởi vì tất cả những người đặt cược có liên quan có thể cần phải được kiểm tra. Vì số lượng trận đấu có thể lớn bằng sự khác biệt về điểm số nên cách tiếp cận này có thể yêu cầu khoảng`10^21`hoạt động. 

Quan sát hữu ích là quá trình này diễn ra theo nhóm. Giả định`t`đối thủ đang hòa với tư cách là á quân hiện tại. Trong trường hợp xấu nhất, những`t`tất cả mọi người đều có thể đạt được điểm so với Julia ngoại trừ một vòng trong một chu kỳ ngắn. Độ dài của chu kỳ đó là:```
1 + floor(log2(t))
```Trong các hiệp đấu đó, mọi đối thủ khác đều bắt kịp trong toàn bộ chiều dài chu kỳ, trong khi người về nhì hiện tại đuổi kịp ít hơn một. Điều này có nghĩa là khoảng cách giữa đội nhì bảng hiện tại và nhóm tiếp theo giảm đúng một sau mỗi vòng đấu. 

Điều này cho phép chúng ta nhảy qua nhiều vòng cùng một lúc. Chúng tôi sắp xếp các khoản thâm hụt, xử lý các nhóm thâm hụt bằng nhau và liên tục hợp nhất nhóm tiếp theo khi khoảng cách giữa các nhóm biến mất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển mọi điểm số của đối thủ thành điểm thâm hụt của Julia và sắp xếp điểm thâm hụt. Khoảng cách nhỏ nhất là đối thủ gần nhất có thể vượt qua Julia. 
2. Giữ mức thâm hụt nhỏ nhất hiện tại`d`và số`t`của đối thủ có sự thâm hụt đó. Nhóm tiếp theo có mức thâm hụt lớn hơn`next_d`. 
3. Tính độ dài chu kỳ`1 + floor(log2(t))`. Trong một chu kỳ, nhóm hiện tại tiến gần hơn một bước tới Julia so với nhóm tiếp theo. 
4. Nếu khoảng cách đến nhóm tiếp theo lớn, hãy bỏ qua nhiều chu kỳ hoàn chỉnh cùng một lúc. Thêm số vòng đã bỏ qua vào câu trả lời và di chuyển cả hai nhóm lại gần nhau hơn. 
5. Khi nhóm hiện tại hợp nhất với nhóm tiếp theo, hãy tăng`t`và tiếp tục. Một nhóm lớn hơn có nghĩa là chu kỳ dài hơn và tốc độ bắt kịp khác nhau. 
6. Dừng lại khi mức thâm hụt nhỏ nhất trở nên âm. Số vòng hoàn thành trước thời điểm đó chính là đáp án. 

Điều bất biến là sau mỗi thao tác nén, các nhóm được lưu trữ sẽ thể hiện chính xác thứ tự tương đối giống như sau khi mô phỏng tất cả các kết quả khớp bị bỏ qua riêng lẻ. Thông tin duy nhất quan trọng là sự thâm hụt của mỗi nhóm và tốc độ tiếp cận Julia của mỗi nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    julia = p[0]
    deficits = [julia - x for x in p[1:]]
    deficits.sort()

    ans = 0
    i = 0
    m = n - 1

    while True:
        d = deficits[i]
        if d < 0:
            break

        j = i
        while j < m and deficits[j] == d:
            j += 1
        cnt = j - i

        step = cnt.bit_length() - 1
        cycle = step + 1

        if j == m:
            need = d + 1
            full = need // step
            rem = need % step
            ans += full * cycle
            if rem:
                ans += rem + 1
            break

        gap = deficits[j] - d

        if step == 0:
            take = gap
        else:
            take = min(gap, (d + 1 + step - 1) // step)

        if take < gap:
            need = d + 1
            full = need // step
            rem = need % step
            ans += full * cycle
            if rem:
                ans += rem + 1
            break

        ans += take * cycle
        shift = take * step

        for k in range(i, j):
            deficits[k] -= shift

        i = j

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này chuyển điểm số thành mức thâm hụt vì dấu hiệu của mức thâm hụt thể hiện trực tiếp liệu Julia có còn dẫn đầu hay không. Việc sắp xếp cho phép các giải nhì bằng nhau được xử lý cùng nhau.`bit_length() - 1`tính toán`floor(log2(cnt))`, yếu tố này quyết định một nhóm sẽ bắt kịp nhanh như thế nào. Thuật toán không bao giờ lặp lại trên các kết quả khớp riêng lẻ mà chỉ lặp lại trên các nhóm có mức độ thiếu hụt bằng nhau. 

Các giá trị lớn trong đầu vào yêu cầu số nguyên Python, nhưng Python tự động xử lý độ chính xác tùy ý. Điều kiện biên quan trọng sẽ dừng lại ngay lập tức khi mức thâm hụt tối thiểu trở thành số âm, vì đó là thời điểm đầu tiên Julia không còn đảm bảo sẽ dẫn đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; mỗi nhóm được xử lý một lần | 
| Không gian | O(n) | Mảng thâm hụt lưu trữ tất cả đối thủ | 

Các ràng buộc yêu cầu tránh mọi mô phỏng tỷ lệ thuận với số lượng kết quả khớp. Sắp xếp`100000`giá trị và sau đó thực hiện quét nhóm tuyến tính dễ dàng phù hợp trong giới hạn.
